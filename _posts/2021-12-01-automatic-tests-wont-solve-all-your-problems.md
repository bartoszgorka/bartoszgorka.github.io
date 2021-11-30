---
title: "Automatic tests won't solve all your problems"
image: /assets/images/posts/automatic-tests-wont-solve-all-your-problems.png
excerpt: >-
  Automated testing can burden Quality Assurance teams from the monotonous manual review of every change.
  They can also be a great acceleration for programmers.
  But they won't make your organization or product great overnight.
  It would help if you had people's support, a clear plan, and an understanding of expectations.
date: 2021-12-01 07:00:00 +00:00
last_modified_at: 2021-12-01 07:00:00 +00:00
tags:
    - management
    - project
    - teamwork
    - team
    - learning
---

  The topic of testing in software development is very critical.
  It is generally accepted that if tests are available, it is better than not having them.
  I also sincerely support the creation of tests, especially automatic ones.
  **Automated testing can burden Quality Assurance teams from the monotonous manual review of every change.**

  **Automatic tests can also be a great acceleration for the programmer** - the tests will immediately indicate if something was accidentally violated.
  Tests can also be an excellent way to introduce the project and provide documentation describing the system's allowed and forbidden actions.

  However, automatic tests are not always as valuable as I indicated.
  There are projects, or rather situations, where it is not easy to have a valuable set of tests.

## Force testing without proper explanation

  If a company wants to reduce the number of errors in its products, it may force it to provide software with a full suite of tests.
  It is generally a good approach but can be dangerous if your team has been forced to create tests.
  **Perhaps a slightly biased example, but forcing 100% code coverage is not a very good approach.**
  The value can be presented to the management team as a KPI (Key Performance Indicator), but it adds nothing.

  **It can be much more valuable to try to motivate the team.**
  No matter your good intentions, people will not follow them unless they fully understand how and why it is behind your thoughts.

  Automated testing is not a universal cure for all project problems.
  They won't make your organization or product great overnight.

  Sometimes I am afraid that when planning tasks, you do not think about the task as changes to the code and, at the same time, tests that should be implemented.
  Sometimes I hear that a given functionality is ready, but all you need to do is prepare the tests.
  The preparation of the tests itself is estimated for the same amount of time as the preparation of changes to the implementation.
  Overall good, let's prepare the tests, but it might be worth considering this phase when planning.

## No long term plan and vision

  **The team likes to know where it is going or what goals we want to achieve.**
  If there is no one common vision of what and how to test, it is possible that sooner or later, nothing will be checked.

  **The beginning of each project and new functionality is an explosion of energy, great motivation, excitement, and enthusiasm.**
  It is the moment when we decide that it will not be another fragment of the system that we fear to touch.
  We bravely start creating tests and documenting everything.
  **However, after a week, the enthusiasm fades away.**
  **The tests and explanations appear less and less often, less precise, and helpful.**

  It is worth setting long-term goals from the beginning to be able to follow the progress of work.
  It is also good to think about what to do after delivering a given functionality.

  **Often the test suite appears early on and remains untouched throughout the rest of the project life.**
  I am not saying that you should prescribe all the tests monthly.
  Instead, I suggest you **establish a clear roadmap for updating both your production code and tests**.
  It will allow you to do systematic maintenance in your project and may also improve unclear code snippets.

  **A roadmap and clearly defined milestones can work great.**
  These can include reducing regression after implementing changes or achieving specified code coverage in modules with high business value.

## Lack of involvement of people

  Ideally, everyone is involved in the project and has a similar understanding of it.
  **Nobody expects the business to write functional tests.**
  However, it would be great if all team members could get involved in the tests, cover possible use cases.
  **It is also necessary to interact with the business so that the tests can verify the business assumptions.**

  The task of team leaders, managers, CTOs, etc., is to help the team deliver the best results.
  Tests can be one of the expected values.
  **Without the support of their superiors, the team may have the impression of hitting a wall or completely ignoring their efforts.**
  It is not good for the motivation and rational use of the resources at the company's disposal.

  Team and organizational leaders usually work under enormous pressure to achieve the best possible results.
  In this constant rush, the temptation to give up tests that do not translate directly into the fulfillment of requirements can be very high.

  A good way is to negotiate a time to start small and demonstrate some value.
  There is no guarantee that this will work for all scenarios.
  Don't force others to follow you.
  Instead, **try to show them what they can gain if they decide to help you**.
