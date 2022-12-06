---
title: "TIL: Only one server will run your migrations"
image: /assets/images/posts/ecto-migration-executed-once.png
excerpt: >-
  Ecto uses SHARE UPDATE EXCLUSIVE lock to ensure that only one instance is running a migration at a time and only once.
  Internally stored version and inserted at allows you to modify and improve created files.
date: 2022-12-07 06:00:00 +00:00
last_modified_at: 2022-12-07 06:00:00 +00:00
tags:
  - Elixir
  - Ecto
  - project
  - TIL
  - learning
  - explanation
---

  In my articles, I often discuss the possibilities offered by Ecto.
  I admire how it is well prepared and how many possibilities it offers.

  **This time I would like to touch briefly on how migrations work.**
  More precisely how, it is guaranteed that only one server will make changes to the database (with DB migrations).

## Understand migrations

  **Migrations are performed one file after the other.**
  Ecto uses the `schema_migrations` table, which stores all migrations that have already been executed.
  We can configure this table's name with the `:migration_source` in Ecto configuration [options](https://hexdocs.pm/ecto_sql/Ecto.Migration.html#module-repo-configuration).

  To ensure that the migration is performed once, Ecto will [lock](https://hexdocs.pm/ecto_sql/Ecto.Migration.html#module-repo-configuration) the `schema_migrations` table when running migrations.
  Based on docs, we can see that Ecto leverages this SHARE UPDATE EXCLUSIVE lock as a **way to ensure that only one instance is running a migration at a time and only once**.
  In the end, you will only have the migration applied once!

  The table stores the `version` and `inserted_at` columns.
  **There is no storing of checksums.**
  It is crucial because **it allows us to modify and improve already created files**.
