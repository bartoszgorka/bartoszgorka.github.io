---
title: "Ecto.Repo.stream/1 in large data processing"
excerpt: "
  The datasets can be too large to handle entirely in-memory, but we should do the processing.
  With Ecto.Repo.stream/1 we can process it in batches.
  Tested in practice solution can come in handy when dealing with CSV export, updating indexes, and much more.
  "
date: 2021-05-19 07:00:00 +01:00
last_modified_at: 2021-05-19 07:00:00 +01:00
tags:
  - Elixir
  - code
---

  Sometimes we need to process a lot of data.
  The datasets can be **too large to handle entirely in-memory**, but we should do the processing.
  It forces us to download some data, process it, and repeat the whole process.

  The presented solution will work as expected when we do not want to use the mechanisms offered by the database itself.
  **We can process data using Elixir language.**
  It is worth being aware that the process will often take a while.

## Ecto.Repo.stream/1 as our way to process data

  [Recently]({% post_url 2021-05-12-elixir-enum-vs-stream %}), I suggested a performance difference between Enum and Stream.
  A solution based on the Stream module will be used.
  **We can present our processing pipeline as follows:**

  ```elixir
  Ecto.Repo.transaction(
    fn ->
      some_data_source_query
      |> Ecto.Repo.stream()
      |> Stream.filter(&filter_data/1)
      |> Stream.map(&transform_data/1)
      |> Stream.each(&load_data/1)
      |> Stream.run()
    end,
    timeout: :infinity
  )
  ```

  `filter_data/1`, `transform_data/1` and `load_data/1` are your's function to manipulate loaded database's content.
  Based on the [documentation](https://hexdocs.pm/ecto/Ecto.Repo.html#c:stream/2), in PostgreSQL and MySQL, **we need to use a transaction**.
  Modifying the `timeout` option will allow us to **avoid disconnecting the connection** (default: [15 seconds](https://hexdocs.pm/ecto/Ecto.Repo.html#module-shared-options)).
  It is important because we assume that the processing may take many hours.

  The number of records received from the database in a single batch can be modified through the `max_rows` parameter (default: 500).
  As I mentioned recently, Stream requires you to start running explicitly.
  This is possible via [Stream.run/1](https://hexdocs.pm/elixir/Stream.html#run/1)

## Summary

  **The proposed solution has been tested in practice.**
  It can come in handy when you need to deal with:
  * migration of large amounts of data between different data schemas
  * building / updating index in ElasticSearch
  * CSV / PDF export - grouping large amounts of data
  * much more ...
