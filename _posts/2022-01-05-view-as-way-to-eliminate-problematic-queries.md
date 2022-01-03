---
title: "Using View as a way to simplify your system"
image: /assets/images/posts/using_view_as_a_way_to_simplify_your_system.png
excerpt: >-
  Many applications introduce some kind of status to make it easier to share data.
  However, more and more modules in your codebase need to be explicit about these assumptions.
  Views can eliminate the problematic SQL query.
  It's also great way to introduce well-defined entities.
date: 2022-01-05 07:00:00 +00:00
last_modified_at: 2022-01-05 07:00:00 +00:00
tags:
  - Elixir
  - code
  - Ecto
  - project
  - tips
---

  Many applications introduce some kind of status to make it easier to share data.
  **Seemingly the same data, in different states, may mean slightly different entities.**
  It can be, for example, the client's status (base, VIP, partner) or information whether the data has been soft-deleted.

  Let's use the latter to consider how this functionality can be approached.
  Technically, with soft-delete, the data is still available but not shown to users.
  However, it would not be very professional if it was possible to check this data.

## Status can be problematic

  There is a problem with the status field: **more modules in your codebase need to be explicit about these assumptions**.
  There is no abstraction to keep the changes within a limited number of modules.
  Technically, we can have one place that is used to read the data.
  However, often the database is queried in more than one place.
  **In this case, we must always have an appropriate SQL query (or record filtering) that will limit the data.**

  Anyone can get it wrong or forget these terms.
  Additionally, if we have many, it can be even easier done.
  It is also necessary to ensure the correct code within Ecto associations.

  For example, if you want to check active (not deleted) users, you should probably run the command:

  ```elixir
  def active(users_query) do
    from(user in users_query, where: is_nil(user.deleted_at))
  end
  ```

## View as a way to make the code a little more readable

  There are many ways to make life easier.
  One of them might be the use of views.
  **They will allow us to eliminate the** `where ...` **that appear everywhere.**

  **Views are also a great way to introduce well-defined entities.**
  They allow us to use a domain language.
  Instead of using the `User` module, we can use `ActiveUser`, which explicitly specifies who we are dealing with.

  The view itself is easy to manage and update.
  There are also no concerns about the performance degradation of our solution.
  Our view will work just like "manually" applied `where ...`.
  **Selecting from a view is exactly as fast or slow as running the underlying SQL statement**.
  You can quickly check that using `EXPLAIN ANALYZE`.

  If your data doesn't change very often, you can also use materialized view.
  It allows for additional acceleration of data checking.
  Instead of checking the conditions each time, the database will use this materialized view to read the data.

  Also, **remember that you can have multiple views based on the same data**.
  In my opinion, this can simplify the logic inside the application itself.
  It is also a way to transfer the responsibility for the restriction applied to the database.
  And that will always be done - the database engine guarantees it.

### Technical solution

  **Below I present how you can introduce views instead of duplicate data filtering.**
  It is effortless.
  To make changes, you just need to prepare a migration containing:

  ```elixir
  defmodule Project.Repo.Migrations.ViewInsteadOfSQLQuery do
    use Ecto.Migration

    def up do
      execute("CREATE VIEW <name> AS SELECT <columns> FROM <source> WHERE <your filters>;")
    end

    def down do
      execute("DROP VIEW IF EXISTS <name>")
    end
  end
  ```

  In the code, it is enough to duplicate the module (probably without some columns, such as the status one) and use the new `<name>` as the data source instead of `<source>`.
