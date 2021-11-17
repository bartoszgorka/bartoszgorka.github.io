---
title: "Code review is not for catching bugs"
image: /assets/images/posts/code-review-is-not-for-catching-bugs.png
excerpt: >-
  Code review can be an opportunity to sharing knowledge.
  Finding errors is a side effect of the entire code review process.
  Thanks to code review, you can promote one standard style of changes to the entire team.
  Don’t be afraid to ask if you don’t understand something.
date: 2021-09-08 07:00:00 +01:00
last_modified_at: 2021-09-08 07:00:00 +01:00
tags:
  - learning
  - project
  - teamwork
---

  Code review is one of the steps in delivering software changes.
  For most projects, it is a required step to make changes to the product at all.
  Sometimes, however, there is a lack of a clear view of the code review process.

  Each project and team may have different rules for the code review process.
  **However, the most important and universal elements of the process remain the same.**

## Code review is not a bug-finding process

  **It may seem strange, but the process is not about errors at all.**
  By errors, I mean that something is not possible to act as it is.
  **The responsible developer is expected to have checked the proposed changes before even opening the proposal.**
  In juniors, code review can eliminate mistakes, but it is often an excellent opportunity to learn.

  **Finding errors is a side effect of the entire code review process.**
  The most commonly reported errors are visible immediately.
  They can result from not testing changes, sitting too long on a given topic, and being blind to mistakes.
  Sometimes a moment of detachment from the task is enough, and we can see it ourselves.

## Code review is an opportunity to learn

  **Code review can be an opportunity to sharing knowledge** with everyone involved in the process.
  This is a great opportunity to clarify certain assumptions about the system, expectations, and aspirations, often not written directly in the code.

  It is worth using knowledge spillover to team members to **avoid the bus factor problem**.
  It can also be a way of synchronizing between teams.
  By being aware of what is changing in the system, it may be easier to eliminate bugs in future changes.
  We at least partially know the functionality.

## Code review is a way to standardize a project

  Another aspect is standardizing the style used in the codebase.
  **Thanks to code review, you can promote one standard style of changes to the entire team.**
  This could be, for example, using the `is_` prefixes for functions that have logical values of `true | false` as output.

  An additional effect may be the **better readability of the prepared code**.
  Other team members must approve our proposal.
  It is possible that thanks to this, we will get better readability of the code.
  Instead of using vague names that are not very descriptive, we may use more precise, **domain-specific terms** that all reviewers will understand.

  The fact that someone will read our code is also **motivated to use good practices and design patterns**.
  We can avoid unreadable and very difficult to correct code.

## Code review is not terrible

  Many people are afraid to check someone else's code because they don't want to judge others.
  **Remember that code review is not an evaluation or criticism of other people.**
  In this process, **we focus on the code, not the people**.

  Don't be selfish.
  Programming is teamwork.
  **Your knowledge and experience can be valuable for the team and the project.**
  Don't be afraid to share it.
  Don't be afraid to participate in the code review process and check someone else's code.
  It can also be an opportunity for you to learn something new.

## Take care of good quality in your proposal

  **From the reviewer's perspective, good code is well-explained code.**
  When submitting changes to be checked, take care of all necessary references to tasks and additional requirements.
  Also, remember to include a good description and screenshots if they can help in testing.

  If you have a set of rules on what to do (TODO list), try to follow them.
  **Minimize the number of unknowns, ambiguities, and magic to make the entire checking process quick and relatively painless.**
  Close the approved change comments.
  Remember to explain why you are not entering some comments.
  You can indicate that they are as a separate proposal or maybe not related to the topic.
  Also, try to clear up the doubts of your reviewers.
  For them, you become a source of truth on a given topic.

## Be a good man

  If you check someone else's code, be nice to them.
  Use neutral language and phrases "we" instead of "me/you" as this is your joint project.

  **Don't be afraid to ask if you don't understand something.**
  The code may be unclear, or the requirements are not entirely clear.
  Your question may allow you to spot contradictions between the requirements.
  **If something is really unclear, try discussing it directly with the author instead of creating a discussion under the changes.**
  A face-to-face conversation may take a quarter of an hour, but it will save you two days of hitting the ball and clarifying doubts.

  **Try to focus on the really important things.**
  Remember to explain in the comment why you are proposing something.
  This **"why" should be based on facts**, not your opinion on the subject.
  Give thanks and praise if something is well prepared.

  Remember the most important thing!
  > **Treat others the way you want to be treated.**
