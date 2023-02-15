---
title: "Customize outputs with Inspect protocol"
image: /assets/images/posts/customize-outputs-inspect-protocol.png
excerpt: >-
  The latest changes to the Inspect protocol can help you change the way data is presented.
  With the new `optional` option, you can only indicate information when the data has changed.
date: 2023-02-15 06:00:00 +00:00
last_modified_at: 2023-02-15 06:00:00 +00:00
tags:
  - Elixir
  - code
  - learning
  - explanation
  - tips
---

  I love to analyze changes that occur in programming languages that I use on a daily basis.
  Many of these changes can make your job much easier.

  **In this post, I would like to refer to the topic presented more than a year ago in the [Global customize for inspect data in Elixir]({% post_url 2021-12-08-global-customize-for-inspect-data-in-elixir %}).**
  I touched on a way to modify the representation of the data.
  This could be done by changing `default_inspect_fun/1`.

## Inspect protocol

  In Elixir we have **[Inspect](https://hexdocs.pm/elixir/1.14.3/Inspect.html) protocol to convert internal Elixir data structures for presentation.**
  This protocol can be used also in our structures.

### Exclude selected fields

  **If we want to present our structures omitting selected fields, the effect can be achieved by using** `except`.
  This allows us to indicate which fields we do not want to present.
  This can be convenient when hiding information such as a password or similar.

  **Thanks to this, we can also hide sensitive fields from the point of view of GDPR and all legal provisions.**

### Indication of what fields we want to present

  Using `only` **we can indicate which fields we want to be presented.**
  This has the advantage over the previously indicated `except` that we do not have to worry about updating everything after adding a new field.
  Unfortunately, with large structures, we have to balance and choose what suits us best with the volatility of objects in the system.

### Indication of changed fields only

  **With Elixir 1.14 we got another possibility.**
  Although it may seem impractical, **thanks to** `optional` **we can indicate the fields we want to be presented only if a value other than the default is indicated.**

  This is extremely useful as it allows you to reduce the number of fields presented.
  Only when information appears it will be presented to the user.

## Changing the way information is presented

  For our example we will use the `StructTest` module to show the differences between versions:

  ```elixir
  defmodule StructTestOnly do
    @derive {Inspect, only: [:id, :role]}
    defstruct [:id, :name, role: :owner]
  end

  iex(1)> %StructTestOnly{}
  #StructTestOnly<id: nil, role: :owner, ...>

  defmodule StructTestExcept do
    @derive {Inspect, except: [:id, :role]}
    defstruct [:id, :name, role: :owner]
  end

  iex(2)> %StructTestExcept{}
  #StructTestExcept<name: :nil, ...>

  defmodule StructTestOptional do
    @derive {Inspect, optional: [:role]}
    defstruct [:id, :name, role: :owner]
  end

  iex(3)> %StructTestOptional{}
  %StructTestOptional{id: nil, name: nil}
  ```

  It is worth emphasizing the **change in the last case**.
  **We no longer have a response as a** `String` **but as an Elixir structure!**
  This is because with `:only` or `:except` being used to restrict fields, the struct will be printed using the `#_struct_name<...>` notation, as the struct can no longer be copied and pasted as valid Elixir code.
