---
title: "The most important thing in the project"
image: /assets/images/posts/the-most-important-thing-in-the-project.png
excerpt: "
  No project is perfect from the start.
  The real problems are very complicated.
  While something seems simple and fits perfectly, it may not be.
  I want to present the most critical and unchanging aspect of each project.
  "
date: 2021-03-10 06:50:00 +01:00
last_modified_at: 2021-03-10 06:50:00 +01:00
tags:
  - project
  - management
---

  No project is perfect from the start.
  Each requires many changes to be successful.
  **The real problems are very complicated.**
  While something seems simple and fits perfectly, it may not be.

  I want to present **the most critical and unchanging aspect of each project**.
  I am talking about **the change**.
  Despite this, we are annoyed when the customer adjusts the requirements.
  **A programmer's job is to react to changes.**

## Change is everywhere

  Let's consider the **principles of good software**.
  I dare say that they were created precisely to respond to changes.

  For example, **DRY**[^dry] makes it clear: **avoid redundancy, reduce repetition, and use abstractions**.
  **Changes should be encapsulated** in a single, restricted place.
  They don't apply to the entire codebase, which makes it easy to change.

  [^dry]: [Don't repeat yourself](https://en.wikipedia.org/wiki/Don%27t_repeat_yourself)

  **SOLID**[^solid] also indicates the conscious creation of code ready for changes.
  Instead of rewriting the entire system, **the change is made in isolation**.
  I realize that often "one place" is not entirely true.
  Minimizing the impact is important here.

  [^solid]: [SOLID](https://en.wikipedia.org/wiki/SOLID)

  The responsibility of **testing** is to validate the impact of changes.
  **We want to eliminate side effects.**
  We don't want to screw up something else that **should be unrelated**.

  I like to suggest solutions instead of just creating problems.
  My suggestion:
  You can use **feature flags** to change system behavior.
  Testers can check the changes before the customer sees them.
  When everything is ok, we switch the settings, and the client can also see the new version.

## Requirements are changing

  A lot of people operate using **Agile** methodologies.
  The **Agile Manifesto**[^agile-principles] points out to react to changes instead of following a plan for the customer's competitive advantage.

  [^agile-principles]: [Principles behind the Agile Manifesto](https://agilemanifesto.org/principles.html)

  **Requirements will change.**
  We have to understand and **accept this**.
  We cannot be nervous.
  The client wants to achieve business value.
  We should think about the project as a way to achieve business value.

## Accept a better life

  We know that **changes are an integral part of the project**.
  We should also try to understand why the change is happening.
  The origin of the change may affect how it is made.
  Once we understand the project, we can deliver value better.

  **The only certain thing is change.**
  We should avoid code that is difficult to modify.
  Think with business how often a given functionality may change.
  Some modules (e.g., authorization) may change very rarely.
  Avoid complications when modules would be modified frequently.

## Summary

  We do not know the requirements that will appear in the future.
  So it seems logical to **choose simplicity instead of complicated schemes**.

  The most manageable code to change is the one in our heads.
  You can change a lot in the early stages.
  **Iterate, analyze, improve.**
  Then modify a code.
  Not earlier.
