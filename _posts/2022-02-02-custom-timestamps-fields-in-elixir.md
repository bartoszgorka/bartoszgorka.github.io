---
title: "TIL: Custom timestamps fields in Elixir"
image: /assets/images/posts/custom-timestamps-fields-in-elixir.png
excerpt: >-
  Ecto Schema is very easy to adapt to our needs.
  Besides changing the type of the primary key, you can also change the type and names of the fields created by the `timestamps()` macro.
date: 2022-02-02 07:00:00 +00:00
last_modified_at: 2022-02-02 07:00:00 +00:00
tags:
  - TIL
  - Elixir
  - code
---

I have already written many times about the ease of extending Ecto and changing the default behavior.
It is possible, for example primary key type used in [the schema]({% post_url 2021-11-10-uuid-as-primary-key-with-ecto %}).

However, this is not all!
Today I want to present a way to change the default names for fields created by the `timestamps()` macro.

## Own names and types

Sometimes `inserted_at` and `updated_at` are not very convenient, especially if an application should use `created_at` to support backward compatibility (with other systems).
It is also possible to eliminate the default use of `naive_datetime` and use `datetime`, which can be very useful for many applications.

Use the code to change the default behavior:

```elixir
timestamps(inserted_at: :created_at, updated_at: :updated_at, type: :datetime)
```

All options can be pre-configured by setting `@timestamps_opts` in our schema.
You can also check [the documentation](https://hexdocs.pm/ecto/Ecto.Schema.html#timestamps/1) to learn about additional possibilities.
