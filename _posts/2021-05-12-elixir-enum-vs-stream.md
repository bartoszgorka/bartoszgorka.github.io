---
title: "Manipulate enumerable: Enum vs. Stream"
image: /assets/images/posts/elixir-enum-vs-stream.png
excerpt: "
  Two ways of handling enumerables dominate Elixir language.
  Seemingly very similar to each other, offering a matching set of possibilities, but they behave entirely differently.
  "
date: 2021-05-12 07:00:00 +01:00
last_modified_at: 2021-05-12 07:00:00 +01:00
tags:
  - Elixir
  - code
---

  Two ways of handling enumerables dominate Elixir language.
  They are [Enum](https://hexdocs.pm/elixir/Enum.html) and [Stream](https://hexdocs.pm/elixir/Stream.html).
  Seemingly very similar to each other, offering a matching set of possibilities.

  They even have corresponding functions with the same name to make it easier to modify the code.
  **What makes them significantly different?**

## Lazy vs. Eager

  The most significant difference between modules is how the functions are executed.
  **Enum is eagerly evaluated, whereas Stream is evaluated lazily.**

  It means that **Stream will wait to evaluate the functions** until it is somehow explicitly called.
  On the other hand, **Enum will immediately process the function** and create intermediate results.

  Due to his laziness, **Stream is useful when working with large collections**.
  Enum module is good when your collections are small (memory size is small).
  As the data size increases, performance decreases.
  This is due to the need to produce intermediate results.
  Each step in the pipeline is a new collection!

## Example

  I prefer learning through examples.
  Check the simple comparison of how to handle the multiplication of numbers.
  It is just an outline of how it works.
  Simplicity allows you to focus on the operation of the Enum and Stream modules themselves.

#### Enum version

  ```elixir
  ["1", "5", "25"]
  |> Enum.map(&String.to_integer(&1))
  |> Enum.map(&IO.inspect(&1))
  |> Enum.map(&(&1 * 2))
  |> Enum.map(&IO.inspect(&1))
  |> Enum.map(&(&1 * 2))
  |> Enum.map(&IO.inspect(&1))

  1
  5
  25
  2
  10
  50
  4
  20
  100
  [4, 20, 100]
  ```

  Each item in the input list is processed according to the current line (2-7).
  **Intermediate results are generated for the next step.**

#### Stream version

  ```elixir
  ["1", "5", "25"]
  |> Stream.map(&String.to_integer(&1))
  |> Stream.map(&IO.inspect(&1))
  |> Stream.map(&(&1 * 2))
  |> Stream.map(&IO.inspect(&1))
  |> Stream.map(&(&1 * 2))
  |> Stream.map(&IO.inspect(&1))
  |> Enum.to_list()

  1
  2
  4
  5
  10
  20
  25
  50
  100
  [4, 20, 100]
  ```

  We can observe a different behavior with Stream module.
  **Instead of going straight to the next item in the input list, all the functions on lines 2-7 are performed on one item.**
  After completing all of them, we proceed to process the next element of the input list.

  We could effectively compose the streams and keeping them lazy.
  The computations are only performed when you call a function from the Enum module (line 8).
  In the absence of executing `Enum.to_list/1`, we would get the internal structure of the Stream module, similar to the one shown below.

  ```elixir
  #Stream<[
    enum: ["1", "5", "25"],
    funs: [#Function<47.104660160/1 in Stream.map/2>,
     #Function<47.104660160/1 in Stream.map/2>,
     #Function<47.104660160/1 in Stream.map/2>,
     #Function<47.104660160/1 in Stream.map/2>,
     #Function<47.104660160/1 in Stream.map/2>,
     #Function<47.104660160/1 in Stream.map/2>]
  ]>
  ```

  This structure contains the input data (`enum`) and performed functions (`funs`).

## Summary

  **Stream** is useful when **working with large collections.**
  Stream can help handle CSV parsing or other extensive data set operations.
  **For short lists can be slower than simply using Enum.**

  Enum module instead is good when your collections are small (memory size is small).
  **Most often, you will be executing the Enum module** but also understand the benefits of Stream.
