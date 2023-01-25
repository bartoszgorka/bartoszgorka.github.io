---
title: "Features of well-prepared tests"
image: /assets/images/posts/features-well-prepared-tests.png
excerpt: >-
  Software development is often a test creation process.
  To achieve success, it is worth considering what features good tests have.
  Well-written tests should document the implemented functionality and check the tested function well.
date: 2023-01-25 06:00:00 +00:00
last_modified_at: 2023-01-25 06:00:00 +00:00
tags:
  - management
  - project
  - teamwork
  - learning
  - tips
  - business
---

  Software development often involves analyzing individual cases and comparing the results with the expectations.
  **Good tests can increase the chance of project success and meeting business objectives.**

  **Tests can be a valuable source of knowledge about the project**, especially when onboarding new developers.
  It is worth considering what it means that the tests are of good quality.

  In this post, I would like to present some features that tests should have.
  Thanks to them, **they will be a good source of knowledge and guarantee the correct operation of the prepared software**.

## Correctness

  **The most important thing is correctness.**
  I do not mean that the assumptions and test assertions have been met.
  **The test should check the given function.**

  Thinking about the purpose of the test, we can **avoid half-tested functionalities**.
  This is often a problem when we introduce mocks that only partially reflect the behavior of the production environment.

  In addition, we can **avoid creating code handling invalid test flow**.
  I've often seen many redundant features that were only implemented because of tests.
  Most often, it was handling missing dependencies between objects, which will never occur in the production environment.
  In tests, it was possible due to the use of a factory and creating objects manually instead of according to the system's flow.

## Readability

  **Well-written tests should be documentation of the implemented functionality.**
  When preparing tests, we should consider what cases we want to handle.
  It's great when in our test set, we have prepared independent tests that handle individual errors and cases.

  To this point, we can also add the role of tests as documentation of our code and indicate how the prepared API should be used.

  Multiple testing languages and libraries allow you to **group tests into one feature-specific block**.
  Then, internally, we create separate tests to check a given case.

  If your test has many activities that prepare the data for the correct run of the test itself, it is worth separating them into a helper function or some setup block.
  Remember to keep it in moderation and not to secrete too much because it can hide the context important from the point of view of a given test.

## Completeness

  This point relates to those previously indicated.
  **Our tests should be complete.**
  It's often tempting to set up a simple test for the most straightforward case and skip the edge cases altogether.
  Eventually, an error occurs, forcing us to improve the code and cover the case with an appropriate test.

  To check this point, you can often use code coverage.
  However, this is only a suggestion, as these systems often analyze the passage through a given line rather than complicated processing paths.

## Resilience

  Last but not least on my list are reliability and resistance to code changes.
  Of course, as much as possible.

  **Flaky tests are often problematic**; sometimes they pass and sometimes they fail.
  Very often, this is due to the execution of functionality in an asynchronous way.

  **Another problem can be creating interdependent tests.**
  This is terribly problematic when one test relies on a state set by another test.
  Randomly seeding and running the tests in a different order is becoming less of a problem, but it still occurs.

  **Relying on global variables can also be included here.**
  If one test modifies variables and fails to restore them after execution, all the others may fail.
