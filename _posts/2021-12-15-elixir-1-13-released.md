---
title: "Elixir 1.13 released: The developer's point of view"
image: /assets/images/posts/elixir-1-13-released.png
excerpt: >-
  Let's check what has changed in the latest version of Elixir.
  Most interesting, from my perspective, will be improving compilation time.
  However, that's not all!
  New map support, information logging level and changes inside Inspect are waiting for you.
date: 2021-12-15 07:00:00 +00:00
last_modified_at: 2021-12-15 07:00:00 +00:00
tags:
  - Elixir
  - code
  - Phoenix
  - tips
---

  Recently, a new version of Elixir has been released!
  I think some interesting points can apply to your daily work.

  **I would like to present the developer's perspective on language changes in this post.**
  As I mentioned in the article about [version 1.12](<{% post_url 2021-06-16-elixir-1-12-released %}>), the language is becoming more and more stable.

## Preliminary assumptions

  I base my analysis on [Changelog](https://hexdocs.pm/elixir/1.13.0/changelog.html#v1-13-0-2021-12-03) and a summary article [Elixir v1.13 released](https://elixir-lang.org/blog/2021/12/03/elixir-v1-13-0-released/).

  **I decided to choose those points that can be the most important for daily work.**
  In the case of creating dependencies, your choices may be different.
  However, I encourage you to check what has changed.
  **As you can see, the changes mainly concern the API itself.**

  **From my perspective, the most significant changes concern the improvement of compilation time, and I encourage you to analyze this passage carefully.**

### Improve compilation times

  It is by far the **most significant change** in this version.
  More precisely, it has been presented as a series of the following changes:

  - *[Kernel] Improve compilation times by reducing the amount of copies of the AST across compiler processes*
  - *[mix compile.elixir] Do not recompile files if their modification time change but their contents are still the same and the .beam files are still on disk*
  - *[mix compile.elixir] Do not recompile all Elixir sources when Erlang modules change, only dependent ones*
  - *[mix compile.elixir] Do not recompile Elixir files if mix.exs changes, instead recompile only files using Mix.Project or trigger a recompilation if a compiler option changes*
  - *[mix compile.elixir] Only recompile needed files when a dependency is added, updated or removed*
  - *[mix compile.elixir] Only recompile needed files when a dependency is configured*

  In my opinion, this is the **most noticeable change since the introduction of automatic formatting (formatter)**.
  Build times for larger projects can be significantly improved.

  You may wonder why this is so crucial.
  Once one file is changed, the rest of the codebase also needs to be rebuilt.
  Until this change, Elixir did not care about the analysis of what had changed.
  A change to the module used inside may have resulted in the need to rebuild many other modules, which unfortunately took time.
  In the case of configuration files, even the entire project.

  A more detailed analysis of what has changed will be available with the new language version.
  **It will be helpful when switching between branches or rebase changes.**
  It can be a significant acceleration of everyday work and seen in even greater language adoption.

## Other important changes

  In the following sections, I enclose a summary of other important changes that may interest you.

#### [Inspect] Allow default inspect fun to be set globally with Inspect.Opts.default_inspect_fun/1

  **A great way to manipulate the way log data is presented.**
  Many systems allow access to logs less restrictive than access to the data in the database itself.
  It may be possible to **hide certain information, e.g., required by law (GDPR)**.

  You can read more in one of my previous posts: [Global customize for inspect data in Elixir](<{% post_url 2021-12-08-global-customize-for-inspect-data-in-elixir %}>).

#### [Map] Implement Map.filter/2, Map.reject/2 and Map.map/2

  A very interesting change that will allow **better map handling**.
  In the case of filter, we will only get those pairs from the map that will fulfill the filtering function.

  These functions are **specially adapted to maps, so their application is faster than the Enum module**.
  It is possible because **no intermediate list** is built.

#### [Kernel] Add power operator (**/2)

  This small change will allow us to reduce the number of direct uses of Erlang code in our projects.
  You can use `base ** exponent` instead of `:math.pow(base, exponent)`.

  It may not be too big of a change, but it is nice to use syntax supported by other languages.

#### [URI] URI.parse/1 is deprecated in favor of URI.new/1 and URI.new!/1

  In the case of projects that support connections to external services, it is possible that parse from the URI module was just used.
  The latest version introduces **soft deprecation due to the possibility of raising an exception when the URI is an invalid value**.
  It is worth considering code improvement to keep the way open to new versions.

#### [Logger] Add Logger.put_application_level/2

  Sets the login level for modules in a given application.

  It will take precedence over the levels you set, so it **can be used to increase or decrease the detail of certain parts of your project**.
  It can be a very good way to collect more information in case of problems in the production environment.

#### [Mix] Add Mix.installed?/0

  Instead of verifying that we receive a `nil`, it will be possible to directly check if `Mix` is available in a given context.
  It can help in scripting and avoid problems with the lack of this tool.
