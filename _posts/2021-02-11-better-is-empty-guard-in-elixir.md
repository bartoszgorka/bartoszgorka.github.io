---
title: "Better is_empty Guard in Elixir"
excerpt: "
  We often want to protect ourselves against inappropriate data from the user.
  In Elixir, we will use a Guards for it.
  Its great advantage is the possibility of using it in pattern-matching.
  One of the most common uses is the need to check if we are getting empty input.
  "
date: 2021-02-11
last_modified_at: 2021-02-11T07:50:00+01:00
tags:
  - Elixir
---

  We often want to protect ourselves against inappropriate data from the user.
  In Elixir, we will use a Guards[^Guards] for it.
  Its great advantage is the possibility of using it in pattern-matching.
  One of the most common uses is the need to check if we are getting empty input.

  [^Guards]: [Elixir Guards](https://hexdocs.pm/elixir/guards.html) - security mechanism.

## Limitations

  **Not all expressions are allowed.**
  This is a **deliberate choice.**
  Elixir (and Erlang) can guarantee that nothing wrong will happen during the execution of the guards.
  It is also a **way to reduce mutations.**

  Also, due to its build at compile-time, the **compiler can optimize code and usage.**
  However, this means that **you cannot use dynamic code** (such as reading variables from the `Application` in runtime).

## Naive approach

  In places where we wait for an empty string or a `nil` value, we often use the expression:

  ```elixir
  def foo(input) when is_nil(input) or input == "" do
    ...
  end
  ```

  Writing this would be repetitive every time we need to check it.
  We may forget one of the parts, and boom 💥.
  However, we can extract this with a macro.
  The expressions `defguard`[^defguard] and `defguardp`[^defguardp] are used for this.

  [^defguard]: [Kernel.defguard/1](https://hexdocs.pm/elixir/Kernel.html#defguard/1)
  [^defguardp]: [Kernel.defguardp/1](https://hexdocs.pm/elixir/Kernel.html#defguardp/1)

## Better approach

  Using [Defining custom guard expressions](https://hexdocs.pm/elixir/guards.html#defining-custom-guard-expressions) chapter, we can prepare the module responsible for handling this case.

  ```elixir
  defmodule MyProject.Guards do
    defguard is_empty(input) when is_nil(input) or input == ""
  end
  ```

  Now you only need to use our new check via `when is_empty(input)`.
  Please remember to add `import MyProject.Guards` or `import MyProject.Guards, only: [is_empty: 1]`.

## Summary

  In this simple way, you can **make your code clean.**
  In the project that I am developing, this simplification introduced a **better understanding of the expectations** towards the input data.

  **What do you think about it?**
  Maybe you have your insights that you want to share?
  Traditionally, the comments section below is for you!
