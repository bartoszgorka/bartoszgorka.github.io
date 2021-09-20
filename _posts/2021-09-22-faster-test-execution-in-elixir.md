---
title: "Faster test execution in Elixir"
excerpt: >-
  Try to use `setup_all` to prepare the data once and re-use it in tests.
  Use tags to have a better context and be able to exclude some tests.
  Prepare a processing pipeline to check the quick tests first, and when they do not fail, take care of the more time-demanding tests.
date: 2021-09-22 07:00:00 +01:00
last_modified_at: 2021-09-22 07:00:00 +01:00
tags:
  - Elixir
  - code
  - project
  - teamwork
---

  Software testing is probably part of every project, regardless of its size.
  When the project is small, the execution of all tests takes only a moment.
  However, **as the project size and the number of tests increases, the time required to complete all tests can increase significantly**.

  A lot depends on the characteristics of the tests.
  In the case of **unit tests, their execution should be relatively quick.**
  **The more complex and multi-layered / functional tests, the slower the execution becomes.**

  Testing is essential to make sure nothing is damaged.
  You can test everything manually, but you'd rather not check each path manually.
  Especially if they are repeatable tests.
  In this case, it is much better to use automatic tests.

## The setup[^setup] vs setup_all[^setup_all] functions

  [^setup]: [ExUnit.Callbacks - setup](https://hexdocs.pm/ex_unit/ExUnit.Callbacks.html#setup/1)
  [^setup_all]: [ExUnit.Callbacks - setup_all](https://hexdocs.pm/ex_unit/ExUnit.Callbacks.html#setup_all/1)

  Let's start with the `setup` and `setup_all` callbacks.
  Their task is simple - prepare the system state for test execution.
  In the case of `setup_all`, we get one call per module.
  It takes place before running the tests.
  The `setup` is run before each test.

  **For many tests, preparing the data once instead of preparing the data repeatedly, in the same way, can speed up the process.**
  It is possible, especially if we do not modify the data, affecting other tested modules.
  Whenever you can, **instead of** `setup`**, use the** `setup_all` **version to prepare the data once and then use it.**

  It is worth noting that it is possible to use more than one callback in a given module.
  They will be invoked one by one, as they appear in the module.

  **Remember about the separation of processes in which these callbacks are performed.**
  The `setup` is run in the same process as the test itself.
  In the case of `setup_all`, this is a separate, independent process per module.

## async: true[^async]

  [^async]: [ExUnit.Case - async: true](https://hexdocs.pm/ex_unit/ExUnit.Case.html)

  **The second way to speed up the execution of your tests is to make them more concurrent.**
  It allows you to run concurrent tests with other tests outside the given module.
  **Tests in the same module never run concurrently.**

## partitions[^partitions]

  [^partitions]: [Operating system process partitioning](https://hexdocs.pm/mix/master/Mix.Tasks.Test.html#module-operating-system-process-partitioning)

  We can execute the tests asynchronously, but this is not always enough acceleration.
  It may even be impossible in the case of tests based on global resources or making changes to the database.

  Most of the projects have continuous integration processes prepared that allow you to test every change in the code.
  **We can use the** `--partitions` **flag to define how many test partitions that particular instance is running.**
  It can also be helpful if you want to **distribute testing across multiple machines**.
  Suppose `mix test --partitions 3` is specified.
  In that case, the tests will be arranged and **allocated round-robin** to one of the three groups, which are then executed independently.

  **It is a huge acceleration for testing.**
  If something takes several minutes locally, it can be performed in just a few minutes with several groups of tests.

## tags[^tags] and filters[^filters]

  [^tags]: [Tags](https://hexdocs.pm/ex_unit/ExUnit.Case.html#module-tags)
  [^filters]: [Filters](https://hexdocs.pm/mix/master/Mix.Tasks.Test.html#module-filters)

  Tags allow you to enter context for test execution.
  You can use `@tag key: value` to match the nearest test or `@moduletag` or `@describetag` inside each context, respectively.
  This way, you can indicate that a given test takes a long time, concerns integration with an external API, or you want to ignore it (for example, due to race conditions).

  **Filtering allows you to select or exclude specific tests from running.**
  It may be necessary to, for example, run integration tests with external systems only in a previously prepared environment instead of each time on the developer's local machine.

  **We can use the** `--include` **option to select a group or the** `--exclude` **option to exclude such a tag from execution.**
  It is also possible to use `--only` to select only one tag group.

## Summary

  **By using the options provided by ExUnit, you can significantly speed up your tests.**
  The best results can be achieved by using all the indicated methods at the same time.
  For tests that can share data, `setup_all` will be an excellent solution for preparing data for tests.

  **Combining** `--partitions` **and tags (**`--include` / `--exclude`**) will allow you to test solutions even faster.**
  **In the beginning, performing quick tests** that will verify the overall correctness of the system operation.
  **If they are successful, more time-consuming tests can be performed.**
  You can discover these by calling `mix test --slowest num`, which will give you a list of `num` tests that took the longest to execute.

  **Successfully accelerate your tests and make your project work more enjoyable!**
