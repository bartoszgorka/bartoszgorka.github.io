---
title: "TIL: Cancel the scheduled send_after message"
image: /assets/images/posts/til-cancel-scheduled-send-after-message.png
excerpt: >-
  Sending a message to yourself as a process is an easy way to schedule an action to retry.
  However, it is not always necessary to send this message.
  Find out how easy it is to cancel a scheduled message.
date: 2022-05-18 06:00:00 +00:00
last_modified_at: 2022-05-18 06:00:00 +00:00
tags:
  - Elixir
  - code
  - TIL
  - tips
  - learning
---

In production code, especially with GenServer, it is common to **execute action again after** a specific time.
**Many times this is just the process sending a message to itself.**
Most often it will be prepared as `Process.send_after(self(), :message_event, after_x_ms)`.

It may happen that after some time, the conditions will change.
Executing the planned action will not be needed or will even be incorrect.
**Have you ever wondered how you can cancel such a message?**

For this purpose, **we can use [Process.cancel_timer/2](https://hexdocs.pm/elixir/1.13/Process.html#cancel_timer/2)**.
It is **necessary to remember the references to the timer**.
We can use, for example, the state of GenServer.

**See how easy it can be presented in the code:**

```elixir
# inside schedule flow
timer_ref = Process.send_after(self(), :message_event, 5_000)

# inside cancel flow
Process.cancel_timer(timer_ref)
```

**A really simple solution and will cancel redundant processing.**
It probably can be helpful in your project too!
