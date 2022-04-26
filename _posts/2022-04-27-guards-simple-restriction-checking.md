---
title: "Guards - simple restriction checking"
image: /assets/images/posts/guards-simple-restriction-checking.png
excerpt: >-
  Guards are a great way to check data limits.
  They are safe and can make the everyday work with the project more pleasant and, at the same time, limit the duplicated code.
  And it's all in pattern matching!
date: 2022-04-27 06:00:00 +00:00
last_modified_at: 2022-04-27 06:00:00 +00:00
tags:
  - Elixir
  - code
  - project
  - tips
---

  We often want to protect ourselves against inappropriate data from the user.
  Another example is the verification of certain assumptions to be able to perform a specific action.

  **Verification can be one of the steps inside the body of the function.**
  For example, we can use:

  ```elixir
    with :ok <- first_rule(data),
         :ok <- second_rule(data),
         ...
    do
         execute action
    end
  ```

  **In the Elixir, however, we have the option of using an additional security mechanism which is** [Guards](https://hexdocs.pm/elixir/master/patterns-and-guards.html#guards).
  You can know `is_map/1`,` is_integer/1`, and many other functions used to check data.
  **Guards allow you to use pattern matching and extend it with additional checks.**

### Limitations

  **Not all expressions are allowed.**
  This is a **conscious behavior for security reasons**.

  Elixir can guarantee that nothing wrong will happen during the execution of the guards.
  Also, due to its build at compile time, the **compiler can optimize code and usage.**
  However, this means that **you cannot use dynamic code**.

  Detailed information on what comparisons and functions are available can be found in [documentation](https://hexdocs.pm/elixir/master/patterns-and-guards.html#list-of-allowed-functions-and-operators).

### Examples

  Each project may have different assumptions and limitations, but some of the checks will often be identical.
  It can be, for example, checking whether the given string is an empty string or the `nil` value:

  ```elixir
  defmodule Project.Guards do
    defguard is_empty(input) when is_nil(input) or input == ""
  end
  ```

  You only need to use our new check via `when is_empty(input)`.
  Please remember to add `import Project.Guards` or `import Project.Guards, only: [is_empty: 1]`.

  The second example that will be interesting in terms of behavior might be checking if we are processing an empty map:

  ```elixir
  defmodule Project.Guards do
    defguard empty_map?(map) when map_size(map) == 0
  end
  ```

### Error handling

  The example above is excellent for discussing error handling for guards.
  In normal execution, if we passed parameters that are not `map()`, we would get the error:

  ```elixir
  ** (BadMapError) expected a map, got: nil
      :erlang.map_size(nil)
  ```

  **However, in the case of guards, any error is assumed to be the same as a failure to meet the assumptions:**

  ```elixir
  iex(1)> Project.Guards.empty_map?(nil)
  false

  iex(2)> Project.Guards.empty_map?(1)
  false

  iex(3)> Project.Guards.empty_map?([])
  false

  iex(4)> Project.Guards.empty_map?(%{website: "https://bartoszgorka.com"})
  false

  iex(5)> Project.Guards.empty_map?(%{})
  true
  ```

### Summary

  **Guards are a great way to check the basic assumptions of the project based on pattern matching.**
  They have some limitations as extra security and can significantly facilitate everyday work.
  **Especially when we have some utils module, sharing checks can reduce duplicated code.**
