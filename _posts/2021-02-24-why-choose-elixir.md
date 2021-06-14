---
title: "Why choose Elixir?"
excerpt: "
  Companies operate in an uncertain environment.
  Uncertain technology is certainly not something that will interest them.
  What makes companies decide to use Elixir language?
  "
date: 2021-02-24 07:00:00 +01:00
last_modified_at: 2021-02-24 07:00:00 +01:00
tags:
  - Elixir
  - management
---

  When I started my adventure with Elixir, it was even less popular than today.
  The latest results of [TIOBE](https://www.tiobe.com/tiobe-index/) and **Elixir's 48th position** made me wonder why it is worth being interested in this language at all.
  Instead of focusing on why a developer might choose this language, **I prefer to focus on companies.**

  Companies operate in an uncertain environment.
  Uncertain technology is certainly not something that will interest them.
  **What makes companies decide to use Elixir language?**

## Successful Companies

  Big companies decided to choose not very popular technology.
  And all this despite procedures, audits, or limitations.
  **WhatsApp, Pinterest, and Goldman Sachs** are just examples of the bigger players who chose **BEAM**[^beam].

  Many companies want to be great.
  So they focus on technologies that big players are betting on.
  However, **blindly following is one thing, but what does the language itself offer?**

  [^beam]: To be completely honest, most chose Erlang over Elixir. Elixir uses Erlang Virtual Machine and BEAM. It will probably not be too much of an abuse from my site.

## Employees and Syntax

  Without employees or contractors, it would be difficult to implement large projects.
  You can do it all by yourself.
  You can create games or operating systems alone, but **most of us will work in a team.**

  Elixir has a syntax similar to Ruby.
  This allows **lower entry level for developers who already know Ruby.**
  Companies can **easier recruit people to the project.**
  I migrated from PHP.
  Many of the people I spoke, started working in Elixir this way.

## Development speed

  Often, business starts with an MVP[^mvp] to see if the proposal makes sense at all.
  **Delivering business value is crucial.**
  Thanks to development tools and good documentation, you can work faster.

  [^mvp]: [Minimum viable product](https://en.wikipedia.org/wiki/Minimum_viable_product)

  The **functional nature** of language is also essential.
  **Immutability** also has an impact.
  It eliminates many problems.
  Our code can be more **thread-safety** which translates into **potentially easier scaling.**

  **Frameworks** such as [Phoenix](https://www.phoenixframework.org/) and [Ecto](https://hexdocs.pm/ecto/Ecto.html) also contribute to the use of language.
  **You don't have to create everything from scratch.**
  Also, hundreds of **dependencies** make the work easier.
  A prime example would be to use [Timex](https://hexdocs.pm/timex/Timex.html) to manipulate dates instead of handle it manually.

## Scalability

  A moment ago, we identified **scalability** as an advantage of language.
  It is worth noting the **handling of concurrency and process separation.**

  In the case of processes, we are dealing with light processes.
  If we want to have hundreds of processes, low memory and CPU consumption are essential.
  This **reduces the infrastructure costs and gives a better way to meet customers expectations.**

  **Less means more.**
  Money can go towards other aspects of the project or business.

## Stability and Security

  Other things may be stability and security.
  Elixir is based on Erlang and BEAM.
  **Erlang is a reliable technology**, due to the many **years of experience** he had to overcome.

  **Separated processes in the VM reduce downtime.**
  Even a few errors, unhandled requests (terrible error code 500) can be acceptable.
  One process will fail instead of the entire application.
  **Downtime is costly, so we want to avoid it** or at least minimize it.

  Again, immutability is useful.
  It is easier for us to protect ourselves from entering user data and changing the system.
  Better security can save us from catastrophe and loss of money.

## Extra

  The [author](https://www.linkedin.com/in/josevalim) of the language was a member of the Ruby Core Team.
  Based on Ruby's experience, critical weaknesses have been changed.
  Also, the compatibility with **Erlang NIFs**[^nifs] is a huge benefit.
  **Places that are critical to performance can use low-level solutions.**

  [^nifs]: [Native Implemented Functions](https://erlang.org/doc/tutorial/nif.html). Code implemented in C instead of Erlang to achieve better performance.

## Summary

  I presented what I think about Elixir and its features that may help business.
  Depending on the company's nature, the number of experienced employees, and openness to still young technology, the preferences may be different.
