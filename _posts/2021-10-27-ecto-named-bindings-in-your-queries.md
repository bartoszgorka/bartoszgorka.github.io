---
title: "Ecto Named Bindings in your queries"
excerpt: >-
  Positional bindings can be problematic due to the order when building large queries with many different functions.
  Named bindings can be of great help in our challenges.
  It is not necessary to know the position to be able to refer to the variable.
  Creating reusable and maintainable queries doesn't have to be complicated.
date: 2021-10-27 07:00:00 +01:00
last_modified_at: 2021-10-27 07:00:00 +01:00
tags:
  - Elixir
  - code
---

  Every software developer wants his code to be the best.
  One of the best practices in software development is the DRY (Don't repeat yourself) principle, which specifies not to duplicate code unnecessarily.
  It can be found very often in building complex queries from small components.

## Dynamic queries

  **Instead of writing very similar SQL queries many times, you can use the [dynamic query mechanism](<{% post_url 2021-09-15-dynamic-queries-in-ecto%}>) to significantly reduce the amount of code needed to complete the query.**
  When building queries, we can also make the code readable.
  Instead of reading the SQL code, **you only need to look at the well-defined function names to know what a given query should do**.

  For example, the code:
  ```elixir
  from(q in Blog)
  |> join(:inner, [blog], author in Author, on: blog.author_id == author.id)
  |> join(:inner, [_blog, author], tag in Tag, on: tag.author_id == author.id)
  |> Repo.all()
  ```

  can be replaced by:

  ```elixir
  from(q in Blog)
  |> with_author()
  |> with_tag()
  |> Repo.all()
  ```

### Positional bindings

  What we have shown in the sample code is the use of positional bindings.
  **Their huge disadvantage is the possibility of confusing the order when building large queries with many different functions.**

  What would happen if the `blog` and `author` replaced places?
  Why do we need to know the bindings order at all?
  Can't you create more reused code that is both easy to understand and maintain?

### Named bindings

  **Named bindings can be of great help in our challenges.**
  By entering them into our sample code, we can get the following query:

  ```elixir
  from(q in Blog)
  |> join(:inner, [blog], author in Author, as: :author, on: blog.author_id == author.id)
  |> join(:inner, [author: author], tag in Tag, as: :tag, on: tag.author_id == author.id)
  |> Repo.all()
  ```

  Please check the code and our use of the `as:` option.
  **Now it is not necessary to know the position to be able to refer to the variable.**
  If the position of the variables were to change, the query would still be valid.

## Summary

  As you can see, **creating reusable and maintainable queries doesn't have to be complicated**.
  Instead of complicated variable order management, **use named bindings to make your work easier**.

  When creating this post, I used the documentation of the Ecto.Query module.
  If you are looking for additional explanations or inspiration, refer to the chapters *[Positional bindings](https://hexdocs.pm/ecto/Ecto.Query.html#module-positional-bindings)*, *[Named bindings](https://hexdocs.pm/ecto/Ecto.Query.html#module-named-bindings)* or *[Bindingless operations](https://hexdocs.pm/ecto/Ecto.Query.html#module-bindingless-operations)* of this module.
