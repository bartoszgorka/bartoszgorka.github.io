---
title: "TIL: Correctly comparing dates in Elixir"
image: /assets/images/posts/comparing-dates-in-elixir.png
excerpt: >-
  Special functions are used to compare dates.
  With it, you can prepare a comparison based on semantics.
  Simple functions can solve your problems with incorrect results.
date: 2022-09-14 06:00:00 +00:00
last_modified_at: 2022-09-14 06:00:00 +00:00
tags:
  - Elixir
  - TIL
  - code
  - project
  - learning
  - tips
---

  Comparing objects with each other is always a threat.
  A prime example of this are **dates that you should never compare by math operations**.
  Special functions are used to compare them, thanks to which a **comparison based on semantics is possible**.

  It can be particularly troublesome when you are just starting your adventure with Elixir.
  You'll get used to it after a while.
  However, you have to be careful at first.

## Compare the dates

  If we want to compare two dates, we should never do something like this:

  ```elixir
  iex(1)> ~D[2022-03-14] > ~D[2022-01-20]
  false
  ```

  **As a result, we may get erroneous results, which is also the case here.**
  Fortunately, we receive appropriate information about the problem:

  ```
  warning: invalid comparison with struct literal %Date{year: 2022, month: 3, day: 14}.
  Comparison operators (>, <, >=, <=, min, and max) perform structural and not semantic comparison.
  Comparing with a struct literal is unlikely to give a meaningful result.
  Struct modules typically define a compare/2 function that can be used for semantic comparison
  ```

  The documentation can specify more about the [Structural comparison](https://hexdocs.pm/elixir/1.12/DateTime.html#compare/2).
  In short: **instead of comparing structures based on the importance of individual fields, they were compared field-by-field without any semantics**.

  Use `Date.compare/2` and` DateTime.compare/2` **in your code to avoid date comparison problems**.
