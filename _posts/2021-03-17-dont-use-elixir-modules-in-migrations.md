---
title: "Don't use Elixir modules in migrations"
excerpt: "
  Most of our projects use databases.
  We often do not realize that incorrectly used functionality may block our applications.
  Simple changes can save us from unpleasant consequences.
  "
date: 2021-03-17 08:10:00 +01:00
last_modified_at: 2021-03-17 08:10:00 +01:00
tags:
  - Elixir
  - code
---

  Most of our projects use databases.
  Using [Ecto](https://hexdocs.pm/ecto/Ecto.html) allows us to manage data migrations easily.
  **We often do not realize that incorrectly used functionality may block our applications.**

  In this post, I would like to present a problem that may occur.
  It is important to understand the problem and the consequences better.
  **Simple changes can save us from unpleasant consequences.**

## Understand migrations

  **Migrations are performed one file after the other.**
  Ecto uses the `schema_migrations` table, which stores all migrations that have already been executed.
  We can configure this table's name with the `:migration_source` in Ecto configuration option[^migration_source].

  [^migration_source]: [Ecto.Repo configuration](https://hexdocs.pm/ecto_sql/Ecto.Migration.html#module-repo-configuration)

  To ensure that the migration is performed once, Ecto will lock[^ecto_lock] the `schema_migrations` table when running migrations.
  This guarantees that **only one server can make changes at the same time**.

  [^ecto_lock]: [migration_lock in Ecto.Repo configuration](https://hexdocs.pm/ecto_sql/Ecto.Migration.html#module-repo-configuration)

  The table stores `version` and `inserted_at` columns.
  **There is no storing of checksums.**
  This is crucial because **it allows us to modify and improve already created files**.

## Problematic schemas

  The use of the Elixir code in migration files can be divided into two categories.
  When we use modules such as Enum or Map, there should be no problems.

  The greater risk occurs when we decide to use data structures: **structs or Ecto schemas**.
  We can **make our application unable to start!**

  Let's analyze a simple example.
  In our project, we have prepared the following migration:

  ```elixir
  # priv/repo/migrations/timestamp_create_logs_table.exs
  defmodule Project.Repo.Migrations.CreateLogsTable do
    use Ecto.Migration

    def change do
      create table(:logs) do
        add(:action, :string)
        add(:details, :text)
      end
    end
  end
  ```

  In our code, we can use structure:
  ```elixir
  defmodule Project.Schema.Log do
    use Ecto.Schema

    schema "logs" do
      field :action, :string
      field :details, :string
    end

    ...
  end
  ```

  Let's suppose we need to change data in a database.
  We want to mark the used earlier `update` action as `modified` now.
  We can prepare a simple migration:

  ```elixir
  # priv/repo/migrations/timestamp_modify_log_action.exs
  defmodule Project.Repo.Migrations.CreateLogsTable do
    use Ecto.Migration

    def up do
      import Ecto.Query, only: [from: 2]

      from(
        q in Project.Schema.Log,
        where: q.action == "update"
      )
      |> Project.Repo.update_all(set: [action: "modified"])
    end

    def down do
      ...
    end
  end
  ```

  The project is developing, but at some point, we recognize that our logs need to be transferred to a separate database.
  To keep things in order, we delete the table `logs` and structure `Project.Schema.Log` after moving the data.
  **Anyone who wants to run our migration code from zero state will get an error now.**

  ```elixir
  ** (UndefinedFunctionError) function Project.Schema.Log.__schema__/1 is undefined
     (module Project.Schema.Log is not available)
  ```

  **Migrations are blocked, CI cannot work.**
  It may be a not very extensive example, but it conveys an idea.
  Using structures in migrations is dangerous.

  **Changing the name or deleting the fields will cause problems.**
  Most likely just when we least expect it and have other important tasks.

  **How to improve invalid migration?**
  Just use [Ecto.Migration.execute/2](https://hexdocs.pm/ecto_sql/Ecto.Migration.html#execute/2)

  ```elixir
  execute("UPDATE logs SET action = 'modified' WHERE action = 'update';")
  ```

  Theoretically, both versions will lead to the same state.
  In the case of using data schemas, unfortunately, we close ourselves to changes.
  Removing the schematic from the project blocks migrations and the entire project because the module is unknown.

## Summary

  Changes in migrations are not tricky.
  **Instead of structures**, we should **use raw SQL**.
  This will eliminate problems when we want to run our application with a clean database or change fields in schemas.
  Simple changes today can save us from critical errors in the future.

  **What do you think about it?**
  Do you have any insights you want to share?
  The comments section below is for you!
