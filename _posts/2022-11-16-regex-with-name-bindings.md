---
title: "TIL: Regex with name bindings"
image: /assets/images/posts/regex-with-name-bindings.png
excerpt: >-
  In Elixir, regular expressions can extract a lot of useful information.
  Using Regex and named_captures/3 function, we can parse the input data based on the prepared regex.
date: 2022-11-16 06:00:00 +00:00
last_modified_at: 2022-11-16 06:00:00 +00:00
tags:
  - Elixir
  - code
  - project
  - TIL
  - learning
  - tips
---

  Text processing is often part of computer systems.
  Sometimes it can be helpful to check regular expressions.

  **In Elixir, regular expressions can extract a lot of useful information.**
  It is also possible to use names.
  Thanks to this, **you don't have to worry about the order of matches**.
  It can be great for data verification and further use of data in the system.

  Example use Regex in your code:

  ```elixir
  iex(1)> Regex.named_captures(~r/a(?<name>b)c(?<order>d)/, "abcd")
  %{"name" => "b", "order" => "d"}

  iex(2)> Regex.named_captures(~r/a(?<name>b)c(?<order>d)/, "abcd", return: :binary)
  %{"name" => "b", "order" => "d"}

  iex(3)> Regex.named_captures(~r/a(?<name>b)c(?<order>d)/, "abcd", return: :index)
  %{"name" => {1, 1}, "order" => {3, 1}}
  ```

  Using [Regex.named_captures/3](https://hexdocs.pm/elixir/Regex.html#named_captures/3) **allows us to parse the input data based on the prepared regex.**
  By default, the result will be the text that matches (`return: :binary` is the default).
  However, by specifying `:index`, we can limit ourselves to verifying that the expression was successfully matched, obtaining the initial position and matched length.
