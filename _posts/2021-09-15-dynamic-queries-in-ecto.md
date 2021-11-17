---
title: "Dynamic Queries in Ecto"
image: /assets/images/posts/dynamic-queries-in-ecto.png
excerpt: >-
  The macro Ecto Query dynamic/2 allows you to build query fragments and interpolate them into one large query.
  We get easy-to-manage query building in an accessible way.
  It allows you to control the parameters from the user and transparently create filtering of data stored in the database.
date: 2021-09-15 07:00:00 +01:00
last_modified_at: 2021-09-15 07:00:00 +01:00
tags:
  - Elixir
  - code
---

  Ecto was created to allow easy manipulation of data stored in various databases.
  Thanks to a well-thought-out API, you can build complex queries in a very readable way.

  **Execution security is ensured thanks to query precompilation.**
  It **reduces the risk** associated with parameters from the user, including the famous **SQL Injection** and `OR 1 = 1; UNAUTHORIZED CODE`.
  It also **ensures efficiency thanks to the preliminary preparation** of queries in the database.

  The code may be presented in the form of **keyword syntax**
  ```elixir
  from(
    q in Blog,
    where: q.score >= 4.5
    order_by: [desc: q.inserted_at]
  )
  ```

  or be in the form of a **pipe-based version**
  ```elixir
  Blog
  |> where([q], q.score >= 4.5)
  |> order_by([q], desc: q.inserted_at)
  ```

  **Such code has a limitation in dynamically creating queries.**
  For many applications, very sophisticated **filtering of data is required based on many parameters provided by the user.**
  Filtering is often an integral part of any project.
  Without the proper operation, users may not feel the pleasure of our solutions, as they have to dig through thousands of irrelevant data manually.
  The API provided by Ecto comes to the rescue again.

## Dynamic query mechanism

  The macro **[Ecto.Query.dynamic/2](https://hexdocs.pm/ecto/Ecto.Query.html#dynamic/2) allows you to build query fragments and interpolate them into one large query**.
  When we want to filter only when the user enters the data, we can have a code similar to:

  ```elixir
  query =
    if inserted_at = params["inserted_at"] do
      where(query, [q], q.inserted_at < ^inserted_at)
    else
      query
    end
  ```

  **However, when using** `dynamic`, **you can choose not to pass** `query` **and only create new components that are ultimately interpolated into a single query.**
  Additionally, it can come in handy the [Ecto.Query.field/2](https://hexdocs.pm/ecto/Ecto.Query.html#field/2), which will allow you to specify the field without hard-coding their name as we write the query.

  ```elixir
  def filter(params) do
    Blog
    |> order_by(^filter_order_by(params["order_by"]))
    |> where(^filter_where(params))
  end

  def filter_order_by("inserted_at_desc"),
    do: [desc: dynamic([q], q.inserted_at)]

  def filter_order_by("inserted_at"),
    do: [asc: dynamic([q], q.inserted_at)]

  def filter_order_by(_),
    do: []

  def filter_where(params) do
    Enum.reduce(params, dynamic(true), fn
      {"author", author}, dynamic ->
        dynamic([q], ^dynamic and q.author == ^author)

      {"category", category}, dynamic ->
        dynamic([q], ^dynamic and q.category == ^category)

      {"score_gt", score}, dynamic ->
        dynamic([q], ^dynamic and q.score >= ^score)

      {"inserted_at", inserted_at}, dynamic ->
        dynamic([q], ^dynamic and q.inserted_at > ^inserted_at)

      {_, _}, dynamic ->
        # Not a where parameter
        dynamic
    end)
  end
  ```

  Example based on technical documentation: [Building dynamic queries](https://hexdocs.pm/ecto/dynamic-queries.html#building-dynamic-queries).
  **The use of** `dynamic` **allows the code to be broken down into smaller functions.**
  Ultimately, depending on the user's parameters, the entire query will be composed of the components.

  ```elixir
  iex(1)> MyModule.filter(%{})
  Ecto.Query<from a0 in Blog, order_by: []>

  iex(2)> MyModule.filter(%{"order_by" => "inserted_at_desc"})
  Ecto.Query<from a0 in Blog, order_by: [desc: a0.inserted_at]>

  iex(3)> MyModule.filter(%{"order_by" => "inserted_at_desc", "author" => "Bartosz Górka"})
  Ecto.Query<from a0 in Blog,
    where: true and a0.author == ^"Bartosz Górka",
    order_by: [desc: a0.inserted_at]>

  iex(4)> MyModule.filter(%{
    "order_by" => "inserted_at_desc",
    "author" => "Bartosz Górka",
    "score_gt" => 4.5})

  Ecto.Query<from a0 in Blog,
    where: true and a0.author == ^"Bartosz Górka" and a0.score >= ^4.5,
    order_by: [desc: a0.inserted_at]>
  ```

  The query itself obtained in `iex (3)` is shown below.
  ```sql
  WHERE (TRUE AND (a0."author" = $1)) ORDER BY a0."inserted_at" DESC ["Bartosz Górka"]
  ```

  **We get easy-to-manage query building in an accessible way.**
  It **allows you to control the parameters from the user** and transparently create filtering of data stored in the database.

  **Dynamic query support can also be an excellent way to handle queries using commands promoted by MongoDB.**
  You can use the sample code included in the repository [pentacent/keila](https://github.com/pentacent/keila/blob/b8698694525e521963773b2b38e252cebca1fcd9/lib/keila/contacts/query.ex) on GitHub.
  It is also a good example of the production use of dynamic queries supported by Ecto.
