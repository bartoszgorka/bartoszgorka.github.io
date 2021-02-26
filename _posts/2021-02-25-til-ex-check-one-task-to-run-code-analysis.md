---
title: "TIL: ex_check - one task to run code analysis"
excerpt: "
  Quick configuration, out-of-the-box solution.
  Simple, fast, and effortless way to manage checkers like *credo*, *sobelow* and *dialyzer*.
  Check *ex_check* dependency in your project.
  "
date: 2021-02-25 17:00:00 +0100
last_modified_at: 2021-02-25 17:00:00 +0100
tags:
  - Elixir
  - code
  - TIL
---

  Elixir has many interesting dependencies, which we can use in our projects.
  [ex_check](https://hex.pm/packages/ex_check) is one of them.

  It is characterized by **quick configuration.**
  **Out-of-the-box solution.**
  After adding to the dependencies list, it starts working.
  **Simple, fast, effortless.**
  Exactly how we love!

## Adoption

  The project is **constantly being developed**, mostly by [author](https://github.com/karolsluszniak).
  When I write this post, dependency downloaded: *2 960 latest (v0.14.0) version*, and *143 240 all time*.
  Is it **a lot of downloads.**
  That indicates use in the Elixir's community.

  **It allows for easy:**
  {: .margin-bottom-0p2-rem}
  * testing ([compile](https://hexdocs.pm/mix/Mix.Tasks.Compile.html), [ex_unit](https://hexdocs.pm/ex_unit/ExUnit.html))
  * code quality checking ([credo](https://hexdocs.pm/credo/overview.html), [dialyzer](https://hexdocs.pm/dialyxir/readme.html), [format](https://hexdocs.pm/mix/Mix.Tasks.Format.html))
  * security issues verification ([sobelow](https://hexdocs.pm/sobelow/readme.html))
  * documentation ([ex_doc](https://hexdocs.pm/ex_doc/readme.html))
  * retry random failures, also last failed tests.

## Use case

  **Works well for both local testing and CI.**
  However, in the case of CI, I would prefer to have the stages separately to control it better.

  **I could use it** about six months ago **when I implemented a whole set of checkers** to the project.
  It was difficult for everyone to use them.
  Ultimately, the team used CI checkers only (they were mandatory).
  The lack of local development meant that we had to wait longer for everything.

  As most pointed out - **so many of these aliases that too many.**
  **Finally, I prepared my own alias** but here are much more options.

## Setup

  Only these changes are required to be added to your project:

  ```elixir
  # mix.exs
  def project do
    [
      preferred_cli_env: [
        check: :test,
        credo: :test,
        dialyzer: :test,
        sobelow: :test
      ]
    ]
  end

  def deps do
    [
      {:ex_check, "~> 0.14.0", only: [:test], runtime: false},
      {:credo, ">= 0.0.0", only: [:test], runtime: false},
      {:dialyxir, ">= 0.0.0", only: [:test], runtime: false},
      {:ex_doc, ">= 0.0.0", only: [:dev, :test], runtime: false},
      {:sobelow, ">= 0.0.0", only: [:test], runtime: false}
    ]
  end
  ```

  ```elixir
  # .check.exs
  [
    tools: [
      {:compiler, env: %{"MIX_ENV" => "test"}},
      {:formatter, env: %{"MIX_ENV" => "test"}},
      {:ex_doc, env: %{"MIX_ENV" => "test"}}
    ]
  ]
  ```

## Source

  Code from the project's README.
  Check it on [ex_check on GitHub](https://github.com/karolsluszniak/ex_check).

  **What do you think about it?**
  Do you have any insights you want to share?
  The comments section below is for you!
