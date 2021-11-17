---
title: "New Elixir 1.12 - The developer's point of view"
image: /assets/images/posts/elixir-1-12-released.png
excerpt: "
  The Elixir 1.12 version introduced some significant changes.
  The main one is the ability to create scripts and import dependencies via Mix.install.
  The interactive console can be more helpful in the software development, prototyping and debugging processes.
  "
date: 2021-06-16 07:00:00 +01:00
last_modified_at: 2021-06-16 07:00:00 +01:00
tags:
  - Elixir
  - code
---

  A month ago, a **new version of the Elixir language was released**.
  It received the designation of version 1.12.
  The latest version is a way to **enable Elixir to be used for more scripted applications**.

  In this post, I wanted to summary what new we got as developers.
  I am pleased with the direction of changes and the maturity of the language.
  There are no sudden version compatibility breaks.

## Changes, changes!

  The entire list of changes is available in the release description on [GitHub](https://github.com/elixir-lang/elixir/releases/tag/v1.12.0).
  Additionally, an official announcement was posted on the Elixir language [website](https://elixir-lang.org/blog/2021/05/19/elixir-v1-12-0-released/).
  Go ahead and check yourself!

  **Overall, you can look at the changes as Elixir opens up to more scripted use-cases.**
  It is no longer necessary to set up the entire project.
  A simple script is enough.
  **This change makes it possible to abandon Python or Ruby and unify the environment to one language.**
  It can be handy for DevOps teams or private applications.

### Mix.install is powerful

  In latest version the main novelty is **[Mix.install](https://hexdocs.pm/mix/1.12/Mix.html#install/2)**.
  The first parameter is the application list (as it is currently in the *mix.exs* settings).
  The second parameter, however, is additional options that may facilitate work.

  The ability to use the `force: true` option allows you to force the deletion of the cache.
  It will allow more friendly dependency updates without the hassle of deleting data from `_deps` directory.
  It can be beneficial when you're working on a dependency whose state changes dynamically.

  **Using the script is very simple.**
  The use of the **cache mechanism allows you to speed up performance and eliminate the need to download dependencies multiple times.**
  The build mechanism was prepared in a similar way to the one currently used.
  **The first time you run it, it will download, compile, and cache dependencies.**
  Each subsequent one will already be based on the cache to speed up the process.

### IEx improvements

  The interactive console has received two main novelties.
  The first is the **ability to use the pipe operator in the same way as in modules.**
  This will eliminate problems when pasting code (from the editor), when the pipe operator **started the line**.

  The second change is the **ability to check the parameters directly when using the function**.
  You will no longer need to delete the entire line to refer to help via `h Module_name.function_name`.

### Enum & System improvements

  Another significant change is **[System.trap_signal/3](https://hexdocs.pm/elixir/1.12/System.html#trap_signal/3) and the more effortless ability to capture signals**.
  Be careful when using this functionality as it has a significant impact on the system.

  **Our favorite Enum module will also receive changes.**
  This is for example [count_until/2](https://hexdocs.pm/elixir/1.12/Enum.html#count_until/2).
  This will allow you to check the constraints in the form: *in the enumerable at least / at most X items*.

  [zip_with/2](https://hexdocs.pm/elixir/1.12/Enum.html#zip_with/2) can also be used.
  This will make it easier to combine elements.
  Especially more than two sets, with going through the enumerable once and using the function defined by us.

### Erlang/OTP 24

  **After 10 years of development**, the Erlang/OTP 24 was also [released](https://blog.erlang.org/My-OTP-24-Highlights/)!
  It introduces **a lot of performance improvements that can have a significant impact on your application**.

  **The error reporting method is also great.**
  Instead of the error itself and forcing you to search the documentation, we can read the explanations directly from the result of the function execution.

  Compare the two erroneous implementations of the addition to the ETS.

  ```elixir
  Interactive Elixir (1.11.0)
  iex(1)> ets = :ets.new(:example, [])
  #Reference<0.3845811859.2669281281.223553>
  iex(2)> :ets.delete(ets)
  true
  iex(3)> :ets.insert(ets, :should_be_a_tuple)
  ** (ArgumentError) argument error
    (stdlib 3.15) :ets.insert(#Reference<0.3845811859.2669281281.223553>, :should_be_a_tuple)
  ```


  ```elixir
  Interactive Elixir (1.12.0)
  iex(1)> ets = :ets.new(:example, [])
  #Reference<0.105641012.1058144260.76455>
  iex(2)> :ets.delete(ets)
  true
  iex(3)> :ets.insert(ets, :should_be_a_tuple)
  ** (ArgumentError) errors were found at the given arguments:

    * 1st argument: the table identifier does not refer to an existing ETS table
    * 2nd argument: not a tuple

      (stdlib 3.15) :ets.insert(#Reference<0.105641012.1058144260.76455>, :should_be_a_tuple)
  ```

  The second uses the latest Elixir and Erlang versions.
  **You know immediately what's wrong.**
  The wrong type parameter was specified.
  This change is both changes in Erlang to indicate such errors, and on the Elixir side to display them correctly.

## Setup on your local machine

  If you want to test the changes yourself, use the following commands to install Elixir 1.12.1 (current the latest version) and Erlang/OTP 24.

  ```bash
  asdf plugin-update --all
  # Download and set Erlang/OTP 24 as default
  asdf install erlang 24.0.1
  asdf global erlang 24.0.1
  # Download and set Elixir 1.12 as default
  asdf install elixir 1.12.1-otp-24
  asdf global elixir 1.12.1-otp-24
  ```

  After installation, it is worth executing the command `mix local.rebar` to use the latest version of Rebar.

## Examples

  **As an example, you can download a list (in JSON format) of my public repositories on GitHub.**
  Just run the code below with the command `elixir github-repos.exs`

  ```elixir
  # github-repos.exs
  Mix.install([
    {:jason, ">= 1.0.0"},
    {:httpoison, "~> 1.8"}
  ])

  defmodule GithubRepos do
    def load(username) do
      "https://api.github.com/users/#{username}/repos"
      |> HTTPoison.get!()
      |> Map.get(:body)
      |> Jason.decode!()
      |> do_some_calc()
    end

    defp do_some_calc(data), do: data
  end

  "bartoszgorka"
  |> GithubRepos.load()
  |> IO.inspect()
  ```

  For more examples, check [Wojtek Mach](https://github.com/wojtekmach)'s repository: [mix_install_examples](https://github.com/wojtekmach/mix_install_examples).
  It will be even better if you prepare your script and share it in the comments.

## Summary

  **The Elixir 1.12 version introduced some significant changes.**
  The main one is the ability to create scripts and use dependencies via **Mix.install** in the same way as for entire projects.
  **Erlang/OTP 24** is a way to **improve application performance** and a new way to **describe errors**.

  The **interactive console can be more helpful** in the software development and debugging process.
  Especially the possibility of easier use of the pipe operator will allow for faster prototyping.
  Changes to the Enum can also make it easier to create a changes in your projects.

  The new version of the language is an excellent opportunity to test its capabilities.
  Share your scripts or applications that can be addressed by new Elixir 1.12.
