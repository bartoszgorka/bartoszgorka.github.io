---
title: "Empirical approach to refactoring"
image: /assets/images/posts/empirical-approach-to-refactoring.png
excerpt: >-
  We don't touch the code in our projects with equal frequency.
  The tool for automatically discovering candidates comes in handy by identifying the most frequently modified files with high cyclomatic complexity.
  Then we can focus on these files and make changes to work with better codebase.
date: 2021-07-14 07:00:00 +01:00
last_modified_at: 2021-07-14 07:00:00 +01:00
tags:
  - project
  - code
  - Elixir
---

  Developing a **project usually turns out to be more complicated than it seemed at the very beginning**.
  Often the assumptions made in the initial phase of development change.
  It is necessary to modify our project to meet business expectations systematically.

  As I indicated in [the entry](<{% post_url 2021-03-10-the-most-important-thing-in-the-project%}>), **the most important thing is to react to change**.
  The actual projects are complicated.
  After some time, we notice some additional dependencies, requirements, or have new business requirements.

## The power of refactoring

  To avoid getting stuck with project development, **it is crucial to understand the importance of refactoring**.
  Refactoring and technical debt must be appropriately represented.
  We should look at them as a missed opportunity to hit the market.
  More on this in [the entry](<{% post_url 2021-06-30-business-doesnt-understand-technical-debt%}>) about the understanding of technical debt.

  **There are two ways to approach refactoring.**
  The first is to separate the **changes as separate tasks** and implement them during the sprint.
  The second way is to integrate improvement into your daily work (implementing the **scout principle**).

  **The fact is, we don't touch the code in our projects with equal frequency.**
  Some files are touched once a week, while others, once written, hardly change at all.
  One could only focus on constantly changing files.
  However, this is a misconception of project needs.
  Frequently modified code may be a configuration file.
  It is also worth **focusing on the complexity of the solution**.

## Automatic selection of candidates for change

  Most developers are well aware of which modules need to be changed.
  However, sometimes there is a lack of objectivity in selecting a candidate for change.
  **The tool for automatically discovering candidates comes in handy by identifying the most frequently modified files with high cyclomatic complexity.**

  In Elixir language, we can use **[churn](https://github.com/patrykwozinski/churn) for automatic analysis**.
  Just add dependency `{:churn, "~> 0.1", only: :dev}` to file `mix.exs` to get the list of candidates for change.
  The current version works great for single applications.
  If you use an umbrella application, you will need a small workaround prepared under [issue](https://github.com/patrykwozinski/churn/issues/15).

  Also, for other languages (such as Ruby or PHP), there are tools that allow for the automatic evaluation of file variability and complexity.

## Summary

  If we refactor, there is a chance that we will be working in a better and better codebase.
  Sometimes, however, it is helpful to look from a different perspective.

  **Automatic solutions allow you to indicate modules that are worth special attention.**
  Then we can focus on these files and make changes that may save us from having to write a project from scratch in the future.
