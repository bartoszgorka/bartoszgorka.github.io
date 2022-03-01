---
title: "Ecto Changeset for verifying parameters used in your API"
image: /assets/images/posts/ecto_changeset_for_verifying_parameters_used_in_your_api.png
excerpt: >-
  You can use Ecto to check any information from the user.
  Extra, we have another layer of security.
  Only the supported parameters are passed to the domain layer.
  Using Ecto to verify parameters from the user is not difficult and can be part of your API verification pipeline.
date: 2022-03-02 07:00:00 +00:00
last_modified_at: 2022-03-02 07:00:00 +00:00
tags:
  - Elixir
  - Phoenix
  - code
  - project
  - teamwork
  - tips
---

  Probably every project using a database will somehow verify the parameters provided by the user before passing them to the database.
  In Elixir projects, `Ecto.Changeset` is often used to check parameters.
  **The use of** `Ecto.Changeset` **is practically a standard because we have a unified method of checking parameters and handling errors.**

  **In this post, I would like to present how you can use Ecto to check any information from the user.**
  Especially at the controllers and API level, eliminating requests containing incorrect parameters as quickly as possible.

  You may wonder why responding quickly with an error message is so important.
  **If the parameters are incorrect, further processing, checking permissions, and performing the work are unreasonable.**
  Especially in the case of APIs that handle significant traffic, this can speed up the execution of requests and the return of information in the event of erroneous queries.

  Additionally, **we have another layer of security** for our application.
  **Only the supported parameters are passed to the domain layer.**
  All additional functions not used directly in a given action are ignored, which may be important for the security of the entire application (by calling other functions receiving parameters, there is no fear that the user will influence their process).

## "Normal" use of Ecto

  You probably associate Ecto only with a database and action on records in the database.
  You probably use Ecto changeset and schemas similar to the ones presented below:

  ```elixir
  defmodule Project.User do
    use Project.Schema
    import Ecto.Changeset

    schema "users" do
      field :username, :string, unique: true
      field :first_name, :string
      field :last_name, :string
      timestamps()
    end

    def changeset(user, attrs) do
      user
      |> cast(attrs, [:username, :first_name, :last_name])
      |> validate_required([:username, :first_name])
      ... some extra validations
    end
  end
  ```

  The resulting changeset is invalid if the parameters passed are incorrect (`changeset.valid? == false`).
  We can extract errors from the `errors` field (for example):

  ```elixir
  errors: [
    first_name: {"should be at most %{count} character(s)",
      [count: 255, validation: :length, kind: :min, type: :string]},
    last_name: {"should be at most %{count} character(s)",
      [count: 255, validation: :length, kind: :max, type: :string]},
    username: {"can't be blank", [validation: :required]}
  ]
  ```

  We can also use the functionality of [traverse_errors/2](https://hexdocs.pm/ecto/Ecto.Changeset.html#traverse_errors/2) to display the bugs better.
  In our case, this function will change and insert the value from the `count` field instead of the `%{count}` expression.

  ```elixir
  %{
    "first_name" => ["should be at most 255 character(s)"],
    "last_name" => ["should be at most 255 character(s)"],
    "username" => ["can't be blank"]
  }
  ```

## Verification of parameters on the API side

  I indicated above how you can "normally" use Ecto.
  **However, it is worth being aware that Ecto does not require a database at all.**
  It is a comprehensive way to verify all parameters, especially those coming from the user.

### Independent verification

  You can approach it a bit naively and verify each parameter independently:

  ```elixir
  # API controller

  defp validate_user_params(%{"first_name" => first_name, "last_name" => last_name, username and more...}) do
    with true <- String.length(first_name) in 5..255,
         true <- String.length(last_name) in 5..255,
         ...
    do
      :ok
    else
      false ->
        :error
    end
  end
  ```

  It is not a solution that I would like to maintain.
  Surely you have the same feeling.
  It will be challenging to control all the limitations in this case.
  Additionally, it would be wise to add error handling to clarify what is wrong (not currently available).

### Embedded schema

  The second way is to create structures for individual actions.
  The structure can be stored in a separate module to have validation in one place.

  ```elixir
  # API params validations

  embedded_schema do
    field(:username, :string)
    field(:first_name, :string)
    field(:last_name, :string)
  end

  defp changeset(attrs) do
    %__MODULE__{}
    |> cast(attrs, [:username, :first_name, :last_name])
    |> validate_required([:username, :first_name, :last_name])
    |> validate_length(:username, min: 5, max: 255)
    |> validate_length(:first_name, min: 5, max: 255)
    |> validate_length(:last_name, min: 5, max: 255)
  end

  def validate_params(params) do
    case changeset(params) do
      %Ecto.Changeset{valid?: false} = changeset ->
        {:error, changeset}

      %Ecto.Changeset{valid?: true, changes: changes} ->
        {:ok, changes}
    end
  end
  ```

  **This solution is much easier to maintain than previously presented.**
  It is enough that the controller uses the function `validate_params/1` to pass user parameters there.
  As a result, we will get a tuple indicating whether the parameters have been successfully verified.
  Additionally, the list of parameters will be limited to those used by the module (we get it from `changes` instead of just using `params`).

  **However, something is still missing here.**
  How to improve our code?

### Parameter validation as a helper for controllers

  **One of the ideas for better management of user parameters may be to prepare one generic module responsible for verification.**
  Then the controllers only need to specify which validations should be met.
  It will **eliminate a significant code duplication** and ensure that all operations are defined in one (well-tested) module.

  This can be prepared, for example as:

  ```elixir
  defmodule Web.ParamsValidator do
    def new(schema, params \\ %{}) do
      fields_with_types =
        for {field, [type | _options]} <- schema,
          into: %{},
          do: {field, type}

      required_fields =
        for {field, [_type | opts]} when is_list(opts) <- schema,
            opts[:required],
            do: field

      defaults =
        for {field, [_type | opts]} when is_list(opts) <- schema,
            into: %{},
            do: {field, opts[:default]}

      {defaults, fields_with_types}
      |> Ecto.Changeset.cast(params, Map.keys(fields_with_types))
      |> Ecto.Changeset.validate_required(required_fields)
    end

    def apply_custom_validations(%Ecto.Changeset{} = changeset, functions) do
      Enum.reduce(functions, changeset, fn fun, acc -> fun.(acc) end)
    end

    def validate(%Ecto.Changeset{} = changeset) do
      case Ecto.Changeset.apply_action(changeset, :insert) do
        {:ok, params} ->
          {:ok, params}

        {:error, error} ->
          {:error, error}
      end
    end
  end
  ```

  It will allow you to extract an action in the controller and use parameter verification.
  For example:

  ```elixir
  defp validate_index_params(params) do
    validations = [
      &Ecto.Changeset.validate_inclusion(&1, :records_per_page, [10, 20]),
      &Ecto.Changeset.validate_number(&1, :page, greater_than_or_equal_to: 1),
      &my_super_custom_check/1
    ]

    [
      page: [:integer, default: 1],
      records_per_page: [:integer, default: 20],
      query: [:string]
    ]
    |> Web.ParamsValidator.new(params)
    |> Web.ParamsValidator.apply_custom_validations(validations)
    |> Web.ParamsValidator.validate()
  end

  defp my_super_custom_check(changeset) do
    # TODO: verify data and return changeset with/without new errors
    changeset
  end

  def index(conn, params) do
    with {:ok, valid_params} <- validate_index_params(params),
      ... index actions
    do
      render data
    else
      {:error, %Ecto.Changeset{} = changeset} ->
        render_error(changeset)
    end
  end
  ```

  By passing the following parameters, a comprehensive error statement can be obtained:

  ```elixir
  %{"page" => -1, "records_per_page" => 2, "sort" => "none"}
  ```

  ```elixir
  {:error,
   #Ecto.Changeset<
     action: :insert,
     changes: %{page: -1, records_per_page: 2},
     errors: [
       page: {"must be greater than or equal to %{number}",
        [validation: :number, kind: :greater_than_or_equal_to, number: 1]},
       records_per_page: {"is invalid", [validation: :inclusion, enum: [10, 20]]}
     ],
     data: %{page: 1, query: nil, records_per_page: 20},
     valid?: false
   >}
  ```

  **As you can see, using Ecto to verify parameters from the user is not difficult.**
  You need to prepare a single module that will perform all the actions.
  In controllers you will focus only on indicating the validation.
  In addition, **all developers know what information can be expected in the next processing steps**.
  Therefore, it is also a **great way to share knowledge within the team and some kind of documentation** that does not become outdated very quickly.
