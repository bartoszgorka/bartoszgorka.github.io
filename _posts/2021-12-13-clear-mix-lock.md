---
title: "TIL: How to remove unused dependencies from mix.lock"
image: /assets/images/posts/clear-mix-lock.png
excerpt: >-
  What if a dependency is no longer needed?
  One library less is potentially one place of conflict between different versions of dependencies less.
  You can do it with one command, and clear your lock file.
date: 2021-12-13 07:00:00 +00:00
last_modified_at: 2021-12-13 07:00:00 +00:00
tags:
  - Elixir
  - code
  - tips
  - TIL
---

  The Elixir environment supports the creation of small dependencies that can be used to solve some specific issues.
  **What if a given dependency is no longer needed?**

  Ideally, you should **remove it from the project entirely**.
  One library less is potentially one place of conflict between different versions of dependencies less.
  It is also an excellent step to **reduce the risk of out-of-date dependencies that can be an attack vector** on the system.

## Removal of unused dependencies

  You can use the command:

  ```elixir
  mix deps.clean --unused
  ```

  **This is useful, but it will only delete files in the deps directory.**
  The `mix.lock` entry itself will still be there, which means that when you re-download dependency, it will reappear.

  **If you want to completely (both from deps and lock file) remove unused dependencies, you should run this command:**

  ```elixir
  mix deps.clean --unlock --unused
  ```
