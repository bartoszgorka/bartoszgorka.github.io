---
title: "Elixir tap and then macros - life-saving helpers"
image: /assets/images/posts/elixir-tap-and-then-macros-life-saving-helpers.png
excerpt: >-
  Elixir 1.12 introduced two very useful macros.
  They can be life savings helpers in your codebase.
  Instead of write extra functions, you can have nice logging and executing actions based on given data.
date: 2022-02-16 07:00:00 +00:00
last_modified_at: 2022-02-16 07:00:00 +00:00
tags:
  - Elixir
  - tips
  - learning
  - project
  - code
---

  One of the last updates in Elixir (version 1.12, to be precise) introduced two very useful macros.
  They are [them/2](https://hexdocs.pm/elixir/main/Kernel.html#then/2) and [tap/2](https://hexdocs.pm/elixir/main/Kernel.html#tap/2).
  I would like to introduce how useful it can be to use these expressions in your code.

## Differences between macros

  **Both macros take exactly two arguments.**
  The first is the data, while the second is the function performed on the data.
  The difference is which information is to be returned as a result.

  In the case of `tap/2`, **the result** is the **data given as the first argument** of the call.
  The result of the passed function has no meaning; it is ignored.

  For `then/2`, the **result is calculated based on the function passed as the second argument**.

## Usage

  `tap/2` **is excellent for logging information** - log or track statements' execution flow before producing the final output.
  Useful, especially inside pipelines.

  ```elixir
  posts
  |> tap(&IO.inspect(&1, label: "Data")) # <#--- HERE
  |> preload_authors()
  |> tap(&IO.inspect(&1, label: "Preloaded")) # <#--- HERE
  |> send_notifications()
  ```

  `then/2` will come in handy when we **perform an action based on the data**.

  ```elixir
  balance
  |> Bank.calculate_fees()
  |> then(&Bank.calculate_credit_worthiness(:credit, &1, balance)) # <#--- HERE
  ```

  Both macros are **usable directly by name** as they are available in the `Kernel` module.
