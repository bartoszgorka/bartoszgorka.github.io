---
title: "Refactoring is the key to success"
image: /assets/images/posts/refactoring-key-to-success.png
excerpt: >-
  Refactoring should be an integral part of a developer's day.
  Systematic refactoring allows for reducing the process of self-degradation of the project.
  This change makes our code easier to understand and cheaper to maintain.
date: 2022-11-23 06:00:00 +00:00
last_modified_at: 2022-11-23 06:00:00 +00:00
tags:
  - management
  - project
  - teamwork
  - tips
  - business
---

  I have repeatedly raised topics related to [technical debt]({% post_url 2021-06-30-business-doesnt-understand-technical-debt %}) and project management on my blog.
  Often, during the discussions, you can hear the mysterious term [refactoring]({% post_url 2021-07-14-empirical-approach-to-refactoring %}).
  Your team members may indicate this as their job for the next few days.

  **What is refactoring?**
  How can it be used to modify the current project?

### Refactoring

  The concept of **refactoring refers** (Martin Fowler inspired me) **to changing the internal structure of the software**.
  This change **makes our code easier to understand and cheaper** to maintain.
  **The introduced change must not affect the observable behavior of the software.**

  This last part is extremely important.
  Refactoring is a change of existing code to bring it to the latest standards and improve its operation.
  **However, this is a functionality that has already been introduced!**

### Purpose of refactoring

  For our efforts to make sense, **we should start by indicating the purpose of refactoring**.
  We should not focus on changes just for the sake of introducing them.
  It should not be a way to prove to others that we are masters of architecture and have built all functionality behind a wall of redundant abstraction and design patterns.

  **The main factor determining to refactor should be the economic aspect.**
  It should be the maintainability of our code.
  **Clean code becomes easier to maintain**, and the costs of introducing new functionalities are much lower than before refactoring.

### Advantages of refactoring

  It is possible that the manager is not willing to approve the idea of refactoring.
  Of course, in some respects, this is perfectly understandable.
  However, when you are absolutely sure that the proposed changes will be good for the project, **you can present the following benefits of the process**:
  - systematic refactoring allows for **reducing the process of self-degradation** of the project resulting from the constant introduction of new functionalities
  - **improving the readability of the code** may translate into easier onboarding of a new developer who, instead of spending a week analyzing the code, will be able to deliver value sooner
  - the process makes it **easier to find gaps and errors** in the system, which can translate into savings for the company (limiting additional requests and minimizing resource consumption)
  - the refactored code also **supports quick changes**

### When to refactor code?

  The answer is simple: always!
  **Refactoring should be an integral part of a developer's day.**
  If you notice a fragment of the system that significantly differs in quality from the assumptions, you can mark it so that you or someone else can look at it.

  Instead of patching the functionality again, it may be better to analyze it from scratch and think about changes.
  It is possible that entering another code workaround will take more than reworking the functionality to fit new expectations.

  **Try to avoid messing up the entire system.**
  As part of the sprints, try to add one or more tasks related to improving the quality of the software.
  At first, the profit will be relatively small.
  However, in the longer term, a lot can be achieved.

  As a suggestion that a given fragment is suitable for refactoring, it is often possible to come across code repetitions, a long list of parameters, or an overly complex single function that most likely has more than one responsibility.
  It is worth not overdoing it.
  If refactoring takes longer than long-term code maintenance, we likely won't get the expected return on our time investment.
