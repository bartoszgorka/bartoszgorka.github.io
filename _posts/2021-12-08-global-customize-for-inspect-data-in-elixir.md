---
title: "Global customize for inspect data in Elixir"
image: /assets/images/posts/global-customize-for-inspect-data-in-elixir.png
excerpt: >-
  You may be surprised, but the behavior of the popular `inspect` can be easily modified.
  Maybe it will make your daily work easier and make it a bit more comfortable.
date: 2021-12-08 07:00:00 +00:00
last_modified_at: 2021-12-08 07:00:00 +00:00
tags:
  - Elixir
  - code
  - Phoenix
  - tips
---

  How well do you know the possibilities offered by the built-in Elixir mechanisms?
  **You may be surprised, but the behavior of the popular inspect can be easily modified.**

  I would like to present some important options.
  They will adjust the display to our needs.
  **Maybe it will make your daily work more accessible and make it a bit more comfortable.**

## List of integers as a list, not as a character

  How often have you worked with lists that contain a single integer?
  **How often did you expect to see a character in a field that should be on a list?**

  ```elixir
  iex(1)> [78]
  'N'
  iex(2)> [95]
  '_'
  iex(3)> [120]
  'x'
  ```

  When testing, it can be a problem, and you need to modify the data.
  However, **the problem should be fixed thanks to** `charlists: :as_lists`.

  ```elixir
  iex(1)> IEx.configure(inspect: [charlists: :as_lists])
  :ok
  iex(2)> [78]
  [78]
  iex(3)> [95]
  [95]
  iex(4)> [120]
  [120]

  iex(5)> inspect([120], charlists: :as_lists)
  "[120]"
  ```

## No more truncating content

  In order not to overwhelm with information, a limit is used.
  It allows you to limit the amount of information displayed in the terminal.
  It applies to the inspected items: for tuples, bitstrings, maps, lists, and any other collection of items.

  **The default is 50, which may not be enough in some cases.**
  You can change this (to any positive integer) or use `:infinity` **to not worry about the limit at all**.

  ```elixir
  iex(1)> 1..60 |> Enum.to_list
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
   23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42,
   43, 44, 45, 46, 47, 48, 49, 50, ...]

  iex(2)> IEx.configure(inspect: [limit: :infinity])
  :ok

  iex(3)> 1..60 |> Enum.to_list
  [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22,
   23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35, 36, 37, 38, 39, 40, 41, 42,
   43, 44, 45, 46, 47, 48, 49, 50, 51, 52, 53, 54, 55, 56, 57, 58, 59, 60]
   ```

## Your own formatting style

  **With the newest (released a few days ago) version of Elixir 1.13, it is possible to control the way information is displayed.**
  You can prepare your own implementation of [default_inspect_fun/1](https://hexdocs.pm/elixir/1.13.0/Inspect.Opts.html#default_inspect_fun/1), which **will change the default data formatting**.

  It can be an excellent way to hide sensitive information in your logs.
  **It will be one of the options to eliminate, for example, a hashed password.**

  It is worth highlighting the information provided directly in the description of the function.
  **Libraries are advised not to set their own settings for this feature as other applications must control these.**
  Instead, they should define their own modules with custom auditing implementations and ask the user to use them.

  ```elixir
  previous_fun = Inspect.Opts.default_inspect_fun()

  Inspect.Opts.default_inspect_fun(fn
    %{hashed_password: _} = map, opts ->
      previous_fun.(%{map | hashed_password: "[SECRET]"}, opts)

    value, opts ->
      previous_fun.(value, opts)
  end)
  ```
