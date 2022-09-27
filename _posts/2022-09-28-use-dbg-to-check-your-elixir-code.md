---
title: "Use dbg to check your Elixir code"
image: /assets/images/posts/use-dbg-to-check-your-elixir-code.png
excerpt: >-
  Debugging Elixir code has become even more accessible thanks to the `dbg` function.
  Thanks to the latest version, your work may become more pleasant and the search for errors even easier.
  Check how to apply new possibilities in your code.
  A simple one-line can facilitate your error analysis.
date: 2022-09-28 06:00:00 +00:00
last_modified_at: 2022-09-28 06:00:00 +00:00
tags:
  - Elixir
  - code
  - project
  - learning
  - tips
  - Phoenix
---

  The latest version of Elixir introduced many functionalities that can be very useful for a developer.
  **One of the most significant changes is the new macro that allows for better debugging your code behavior.**

  This post is a quick analysis of the possibilities offered by `dbg/2`.

### Kernel.dbg/2

  Macro allows us to analyze our code.
  Let's look at the following usage example:

  ```elixir
  def otp_already_sent?(email) do
    min_valid_datetime = DateTime.utc_now() |> Timex.shift(minutes: -@code_ttl_in_minutes)

    from(q in LoginOTP,
      where: q.email == ^email,
      where: q.generated_at >= ^min_valid_datetime,
      limit: 1
    )
    |> Repo.one()
    |> case do
      nil -> false
      %LoginOTP{} -> true
    end
    |> dbg() # <----- NEW CODE HERE
  end
  ```

  Putting the `dbg/2` call at the end, **we can get information about the execution of our software**:

  ```elixir
  # File location
  [(core 0.1.0) lib/core/otp_generator.ex:42: Core.OTPGenerator.otp_already_sent?/1]

  # First line
  from(q in LoginOTP,
    where: q.email == ^email,
    where: q.generated_at >= ^min_valid_datetime,
    limit: 1
  ) #=> #Ecto.Query<from l0 in Core.LoginOTP,
   where: l0.email == ^"test@example.com",
   where: l0.generated_at >= ^~U[2022-09-26 11:39:00.794321Z], limit: 1>

  # Second line
  |> Repo.one() #=> %Core.LoginOTP{
    __meta__: #Ecto.Schema.Metadata<:loaded, "login_otps">,
    id: "7375c128-0703-41ea-96b5-4d0af850094a",
    email: "test@example.com",
    code: "229436",
    generated_at: ~U[2022-09-26 11:54:00Z]
  }

  # Third line
  |> case do
    nil -> false
    %LoginOTP{} -> true
  end #=> true
  ```

  For the sake of clarity of the example above, I have introduced additional separations and indicated which line the result applies to.
  As you can see, after `# =>`, **we get the current binding with which the command was executed**.
  By itself, `dbg/2` **returns the value of the debugged code**, which can be extremely useful.

### Change in debugging

  **The introduced code testing capabilities may facilitate the work.**
  Instead of entering `|> IO.inspect(label: "Some step")` after each line, and you need to specify `|> dbg()` in one place, especially at the end of our pipeline.

  **You can customize the** `dbg/2` function to suit your needs.
  For this purpose, you can use the functionality **described in the documentation**: [Configuring the debug function](https://hexdocs.pm/elixir/Kernel.html#dbg/2-configuring-the-debug-function).
