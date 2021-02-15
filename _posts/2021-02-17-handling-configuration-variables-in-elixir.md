---
title: "Handling configuration variables in Elixir"
excerpt: "
  We have some environmental variables in almost every Elixir application.
  They change the behavior of the system.
  They even allow you to reduce variables to be hard-coded somewhere in the depths of the code.
  I will show how you can use environment variables better.
"
date: 2021-02-17
last_modified_at: 2021-02-17T08:05:00+01:00
tags:
  - Elixir
  - code
---

  We have some **environmental variables in almost every Elixir application.**
  They change the behavior of the system.
  They even allow you to **reduce variables hard-coded somewhere in the depths of the code.**

  Are you sure we use them well?
  Let me introduce you to something.
  **How can you even better use environment variables?**

## Traditional solution

  When we need to make use of environment variables, we often use [Application](https://hexdocs.pm/elixir/Application.html) or [System](https://hexdocs.pm/elixir/System.html) for this purpose.
  The first option assumes that **each application has its own environment.**
  An environment is a keyword list that maps atoms to terms.
  **The application's environment is not related to the operating system environment.**
  In the case of the second solution, we can use system variables.

  **It is recommended to use `Application` over `System`.**
  Especially due to the possibility of easy use of configuration files and [Config](https://hexdocs.pm/elixir/Config.html).

  How often does `Application.get_env/2` be used in your code?
  Or the entire processing pipeline to ensure that the value is of the correct type?

  I had a lot of it.
  Mostly when there was more and more code in the project.
  I also had "wonderful" `Utils` modules to provide type conversions.

## Dedicated config manager

  You can approach the issue of handling variables entirely differently.
  See the code proposal below.

  ```elixir
  defmodule YourApp.Env do
    @doc """
    Get a variable from System / Application and ensure the correct type.
    """
    @spec get!(atom()) :: term()
    def get!(key), do: resolve(Application.fetch_env!(:your_app, key))

    defp resolve({variable, :boolean}), do: System.get_env(variable) == "true"
    defp resolve({variable, :system}), do: System.get_env(variable)
    defp resolve({variable, default}), do: System.get_env(variable) || default
    defp resolve({variable, default, :int}), do: resolve({variable, default}) |> parse_int(default)
    defp resolve({variable, default, :boolean}) do
      case System.get_env(variable) do
        "true" -> true
        val when val in ["", nil] -> default
        _ -> false
      end
    end
    defp resolve(value), do: value

    defp parse_int(value, _default) when is_integer(value), do: value
    defp parse_int(value, default) when is_binary(value) do
      case Integer.parse(value) do
        {number, _} -> number
        :error -> default
      end
    end
  end
  ```

  To make it easier, let's define a new module `YourApp.Env`.
  Let it only provide one public function `get!/1`, and include support for getting system variables.
  Perhaps by presenting a configuration file, it will be easier to understand how to apply the code.

  ```elixir
  config :your_app,
    json_serializer: Jason,
    env: {"MIX_ENV", "dev"},
    use_ssl: {"USE_SSL", true, :boolean},
    homepage_url: {"HOME_PAGE_URL", :system},
    timeout_in_seconds: {"TIMEOUT_IN_SECONDS", 5, :int}
  ```

  By using tuple `{variable name, default value, type :int / :boolean or skip for :string}`, **we can easily guarantee that we get variables of an expected type.**
  This can be standardized by entering the type `:string` as the third element of the tuple.

  Now call `YourApp.Env.get!(:some_variable)` and get the data.
  **No extra handling. No need to think about the returned value or the default values.
  Simple and clear.**

## Summary

  I presented how we dealt with the problem of environmental variables.
  It may not be an ideal solution, but it is worth considering and adapting to your project.

  **What do you think about it?**
  Maybe you have your insights that you want to share?
  The comments section below is for you!
