---
title: "Speed up your daily work with Elixir console tricks"
image: /assets/images/posts/speed-up-your-daily-work-with-elixir-console-tricks.png
excerpt: >-
  I would like to present some useful tricks to speed up your work.
  Elixir's interactive shell is a great tool to make your work comfortable.
  You don't need to use a browser to check function errors.
  You have everything available in the console!
date: 2022-03-23 07:00:00 +00:00
last_modified_at: 2022-03-23 07:00:00 +00:00
tags:
  - Elixir
  - code
  - project
  - teamwork
  - learning
  - tips
---

  Daily work with Elixir made me wonder if it could be a little more pleasant.
  Especially if you need to verify the documentation, the sequence of parameters, or possible errors.
  Based on discussions with friends, **I would like to present some valuable tricks that can speed up your work**.

  I inspire the list with my daily work.
  Please get in touch with me to complete this article together if you have additional suggestions.
  **Thanks to all the tips, I hope that you will be able to work more nicely.**

## IEx: Interactive Shell

  [Elixir's interactive shell](https://hexdocs.pm/iex/1.13/IEx.html) is a primary work tool.
  We have access to our defined modules when running in the context of a project (via `iex -S mix`).

### Help directly in IEx

  A must-have that you need to get out of this article.
  **Instead of switching from the console to your web browser to check the documentation for the built-in modules, you should use the help option.**

  ```elixir
  iex(1)> h String.to_existing_atom

  def to_existing_atom(string)

    @spec to_existing_atom(t()) :: atom()

  Converts a string to an existing atom.

  The maximum atom size is of 255 Unicode code points.

  Inlined by the compiler.

  ## Examples

      iex> _ = :my_atom
      iex> String.to_existing_atom("my_atom")
      :my_atom
  ```

  For example, the function `String.to_existing_atom/1` takes one argument, which is a string.
  As a result, we get an atom.
  In addition, we have information about the limitation in the form of a maximum of 255 Unicode code points that can be used.
  Another piece of information about compiler behavior allows us to understand the influence of functions on the performance of our operations.

  Besides the description, the best examples are.
  **The Elixir documentation is lovely support in your daily work.**

  The best thing is that **you can also find your own modules in the documentation**.
  You just need to run the project with `iex -S mix` to have access to the documentation of your project (used attributes `@doc` and `@moduledoc`).

### Autocomplete

  **Interactive shell allows the use of auto-completion of names.**
  It is extremely useful if you want to get something done quickly and doesn't want to type the name of each module and function manually.
  Try it yourself:

  ```elixir
  iex(1)> Str
  Stream      String      StringIO

  iex(1)> String.t
  to_atom/1             to_charlist/1         to_existing_atom/1
  to_float/1            to_integer/1          to_integer/2
  trim/1                trim/2                trim_leading/1
  trim_leading/2        trim_trailing/1       trim_trailing/2
  ```

  Auto-discovery is based on public functions.
  **Just press the tab, and you will get hints or automatic name completion.**

  If you use the annotations `@docs false` or `@impl true`, the functions will not be visible.

### Enable history

  **By default, the shell does not remember the actions performed in the previous run.**
  If we close the console, all executed commands must be re-entered.
  To be able to toggle with the up / down arrows and re-execute commands between runs, it is worth activating the history saving.

  It can be done by selecting the attributes:

  ```bash
  iex --erl "-kernel shell_history enabled"
  ```

  However, typing this command each time can be problematic.
  Instead, it is better to set the `ERL_AFLAGS` environment variable so you will use history each time.
  You can do it by specifying in the configuration files:

  ```bash
  export ERL_AFLAGS="-kernel shell_history enabled"
  ```

### Multiline expressions

  In previous versions of Elixir, there was a problem with quickly pasting the code directly into the console.
  It was especially troublesome when we used the pipeline operator.
  **We have received support in handling multiline expressions with one of the updates.**

  Suppose we paste the command starting with the binary operator (`|>`, `++` ...).
  In that case, **IEx will automatically use the result obtained in the previously executed expressions** (accessible with `v()`).

  ```elixir
  iex(1)> [1, 20, 5, 2]
  [1, 20, 5, 2]
  iex(2)> |> Enum.sort()
  [1, 2, 5, 20]
  ```

### Referencing values from previous expressions

  Being able to refer to previous results is especially useful for debugging code.
  It can also be **helpful when the computation is resource-intensive or time-consuming**.

  ```elixir
  iex(1)> params = %{"users" => 12, "test" => true}
  %{"test" => true, "users" => 12}

  iex(2)> # extra_work here
  nil

  iex(3)> v(1) |> Map.get("test")
  true
  ```

  When testing new features, it is often helpful to be able to use the previous results.
  We don't have to calculate everything again.
  And we don't always want to save everything as variables.

### Recompile without restarting

  We can make changes many times during local development and want to test them.
  **To save time, it's better to use the** `recompile` **command instead of restarting the console.**

  ```elixir
  iex(2)> recompile
  Compiling 1 file (.ex)
  :ok

  iex(3)> recompile
  :noop
  ```

  Suppose your project runs additional databases, uses message brokers, or has other actions during startup.
  In that case, you will feel a huge boost.
  Otherwise, the acceleration will be slower but still noticeable.
  I appreciate this functionality, especially when modifying existing code.
  **An additional bonus is access to previous calculations and the possibility of performing the code again.**

### .iex.exs file

  When IEx starts, it first checks for the existence of a local `.iex.exs`.
  It tries to run the global `~/.iex.exs` if not found local version.
  This special file can be compared to the configuration file and seeds.

  For example, **we can use it to import functions and introduce aliases to make our daily work more enjoyable**.

  ```elixir
  # Import some module
  import_if_available(MyProject.SomeModule)

  # Alias most used code
  alias MyProject.Repo

  # set variables
  super_admin = Repo.get_by(MyProject.Schemas.SuperAdmin, id: 1)

  # configure IEx
  IEx.configure(inspect: [limit: :infinity])
  ```

  That's it for me this time.
  **Good luck and enjoy your work!**
