---
title: "TIL: wrapping an execution in a tuple"
image: /assets/images/posts/til-wrapping-an-execution-in-a-tuple.png
excerpt: >-
  One of the advantages of Elixir is its openness.
  You can get more readable code with the command wrapped in tuple.
  You don't need to modify the response from functions itself.
date: 2022-03-16 07:00:00 +00:00
last_modified_at: 2022-03-16 07:00:00 +00:00
tags:
  - Elixir
  - code
  - tips
  - TIL
  - project
---

  In Elixir, it is very common to use blocks of code processed with `with`.
  We go through all the executions in the indicated order.
  **The first mismatch with the expected pattern ends the processing.**
  The expression handler then begins.

  Sometimes, however, there are problems in **how to indicate which case took place**.
  It is especially true **when multiple functions return similar responses**.
  See an example of checking access permissions:

  ```elixir
  with true <- is_active?(user),
       true <- can_use_premium_feature?(user),
       ...
  do
    ...
  else
    false -> {:error, "User deactivated or premium features disabled"}
    ...
  end
  ```

  In the situation indicated above, it is difficult to tell which of the functions `is_active?/1` or `can_use_premium_feature?/1` returned `false`.
  However, **by using tuple-wrapping, we can have better execution control**.
  Such code is much clearer when handling errors, and we can also indicate more precise information.
  Compare both solutions:

  ```elixir
  with {:active, true} <- {:active, is_active?(user)},
       {:can_use_premium, true} <- {:can_use_premium, can_use_premium_feature?(user)},
       ...
  do
    ...
  else
    {:active, false} ->
      {:error, "User deactivated"}

    {:can_use_premium, false} ->
      {:error, "Premium features disabled"}

    ...
  end
  ```

  **You don't need to modify the response from the function** `is_active?/1` nor `can_use_premium_feature?/1`.
  It can be super handy if the code is used in multiple places.
