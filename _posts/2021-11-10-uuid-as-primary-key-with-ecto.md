---
title: "UUID as Primary key with Ecto"
excerpt: >-
  A simple example of how to introduce a Universally unique identifier (UUID) into your project.
  You just need to remember a small modification in the migration and the column type change.
date: 2021-11-10 07:00:00 +00:00
last_modified_at: 2021-11-10 07:00:00 +00:00
tags:
  - Elixir
  - code
  - TIL
---

  Ecto is almost an inseparable part of every Elixir project.
  A simple code allows you to achieve spectacular effects.

  **In this post, I would like to present a way to introduce a Universally unique identifier (UUID) to a project.**
  But let's start by using the default approach.

## Using Ecto.Schema

  **Without modifying Ecto's default behavior**, structures will use an `id` field of type `integer` as the table's primary key.

  **Using an integer type identifier can be problematic.**
  It is especially noticeable when we want the identifiers not to be consecutive numbers from the sequence.
  Sometimes a **security part can be compromised**.
  It can also reveal too much information about the system, for example, the number of users.

## Introducing the UUID

  To replace the integer type with the expected UUID, the migration files should be modified.
  Deactivating `primary_key` will allow no extra column to be added, automatically being an `id` of type integer.
  Instead, **we will introduce our column** of type `uuid` (the name can be anything, I used `id`).

  ```elixir
  # priv/repo/migrations/<date>_create_entries_table.exs
  defmodule MyProject.Repo.Migrations.CreateEntriesTable do
    use Ecto.Migration

    def change do
      create table(:entries, primary_key: false) do
        add(:id, :uuid, primary_key: true)
        add(:author, :string)
        add(:title, :string)

        ...
      end
    end
  end
  ```

  It is also worth making modifications to the structures.
  **Instead of using Ecto.Schema and duplicate module attributes, I used a custom module that eliminates code duplication.**

  ```elixir
  defmodule MyProject.Entry do
    use MyProject.Schema

    schema "entries" do
      field(:author, :string)
      field(:title, :string)

      ...
    end
  end
  ```

  An additional module defines the behavior for Ecto.
  Using the macro `__using__/1`, every time `use MyProject.Schema` will apply the proper settings.
  The `id` column, which is of type `binary_id`, was used as the primary key, and its value is automatically generated (database responsibility).

  **To use bindings between the objects** defined in `belongs_to`, it **is necessary to set** `@foreign_key_type`.
  It eliminates the mismatch problem of types `id` and `binary_id` (`id` is used by default).
  Instead of putting it in relation each time, it is worth making sure that it is prepared in one place.

  ```elixir
  defmodule MyProject.Schema do
    defmacro __using__(_) do
      quote do
        use Ecto.Schema

        @primary_key {:id, :binary_id, autogenerate: true}
        @foreign_key_type :binary_id
      end
    end
  end
  ```

## Summary

  As you can see, **introducing UUID into a project is not difficult**.
  You just need to remember a slight modification in the migration and the column type change.
  **It can be achieved by isolating the code into a separate module, which further simplifies things.**
  Now just use your module instead of the default Ecto.Schema for everything to work correctly.
