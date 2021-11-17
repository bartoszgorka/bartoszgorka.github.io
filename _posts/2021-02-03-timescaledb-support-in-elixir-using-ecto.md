---
title: "TimescaleDB support in Elixir using Ecto"
image: /assets/images/posts/timescaledb-support-in-elixir-using-ecto.png
excerpt: "Most of the projects collect a lot of data.
  It usually means a heavy loads on the database.
  What can we do to provide better request handling and lower access times?
  See how to introduce TimescaleDB to the project using Ecto."
date: 2021-02-03 15:50:00 +01:00
last_modified_at: 2021-02-03 15:50:00 +01:00
tags:
  - TimescaleDB
  - Elixir
  - Ecto
  - code
---

  Most of the projects we have the pleasure of creating collect a lot of data.
  It usually means a heavy load on the database, which we use for data persistence.
  Often our data is **time-series data**.
  But **what can we do** to provide better request handling and lower access times?

  I will present how you can integrate **Elixir** and **Ecto** with one, very popular extension for **PostgreSQL database** which is **TimescaleDB**.

## TimescaleDB and time-series data

  TimescaleDB is a very **popular extension** for PostgreSQL.
  It **allows better handling of time-series data.**

  But what does time-series data itself mean concerning other data?

  Time-series data collectively **represents changes in a process, system, or behavior over time.**
  Data should always has a time aspect by including timestamp.
  The vast **majority of data is append-only.**
  We are also most often interested in the latest data in the processing process.
  The frequency aspect of adding new data is not significant.

  **Time-series data can be found everywhere.**
  Monitoring computer systems, container metrics (CPU, free memory, net/disk IOPs), and application metrics (requests) are time-series data.
  Data from Internet of Things sensors, data from the application (user login, clicks), or even financial trading systems generate data from this category.

## Business use case

  **Technology and proposed solutions should match the business expectations and challenges of real projects.**
  In one of the projects that I was developing, data were collected regarding, among others, the availability of games (yep, also Cyberpunk 2077 sales summary).

  The original use of **PostgreSQL alone has proved insufficient.**
  Saving data typically time-series and operations on them away from the database (we need to obtain a large volume of data and processing in Elixir) did not work well.

  **TimescaleDB came to the rescue.**
  It was possible to process the data inside the database better.
  The use of time-divided tables allowed for better access to the latest data.
  Let's see below with code examples of how you can easily manage it from within Ecto.

## Ecto and TimescaleDB

  The considerable advantage of TimescaleDB is the **full SQL interface for all SQL natively supported by PostgreSQL.**
  So we can use [Postgrex](https://github.com/elixir-ecto/ecto#usage) and Ecto to communicate with the database.

  Let's begin by ensuring the extension is available in the database.

  ```bash
  mix ecto.gen.migration create_timescaledb_extension
  ```

  ```elixir
  # priv/repo/migrations/20210131105904_create_timescaledb_extension.exs

  defmodule Project.Repo.Migrations.CreateTimescaledbExtension do
    use Ecto.Migration

    def up do
      execute("CREATE EXTENSION IF NOT EXISTS timescaledb CASCADE")
    end

    def down do
      execute("DROP EXTENSION IF EXISTS timescaledb CASCADE")
    end
  end
  ```

  When you want to create a new table using the options introduced in the extension, use the code below.

  ```elixir
  # priv/repo/migrations/20210131110504_create_conditions_table.exs

  defmodule Project.Repo.Migrations.CreateConditionsTable do
    use Ecto.Migration

    def up do
      create table(:conditions, primary_key: false) do
        add(:time, :naive_datetime, null: false)
        add(:location, :string, null: false)
        add(:temperature, :decimal, null: false)
        add(:humidity, :decimal, null: false)

        timestamps()
      end

      execute("SELECT create_hypertable('conditions', 'time')")
    end

    def down do
      drop(table(:conditions))
    end
  end
  ```

  The table `conditions` will store the conditions (temperature and humidity) at a given location.
  Instead of the default `SERIAL` primary key, we will use the `naive_datetime` to split records based on that key.
  Executing the `create_hypertable` command requires `execute`, hence the separation into `up` and `down` to undo the migration.

## Summary

  As we can see, **using TimescaleDB is not complicated at all.**
  Depending on the project, we can gain a lot from its introduction to our system.
  I recommend you check out the [TimescaleDB vs. Relational Databases comparison](https://docs.timescale.com/latest/introduction/timescaledb-vs-postgres).
