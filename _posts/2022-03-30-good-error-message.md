---
title: "Good error message"
image: /assets/images/posts/good-error-message.png
excerpt: >-
  Error handling is an integral part of any project.
  However, only a few can do it well.
  What makes an error message good?
  Take care of the language of the message, its length and indicate how to mitigate errors.
  Have automatic bug reporting tools to quickly respond to system problems.
date: 2022-03-27 07:00:00 +00:00
last_modified_at: 2022-03-27 07:00:00 +00:00
tags:
  - management
  - project
  - teamwork
  - learning
  - explanation
  - tips
---

  **Error handling is an integral part of any project.**
  However, only a few can do it well.
  **Have you wondered what a good error message is?**

  As system users, we often come across an error that doesn't explain anything.
  Even as programmers using numerous libraries, we may have this problem.

  I have mentioned this topic several times during the code review.
  So I decided to prepare an article to systematically approach the problem of good error messages.

## Good message from the user's perspective

  **In the post, I would like to focus mainly on the user's errors.**
  We will not show him exact information that could compromise the system's security.
  For example, we will not reveal the paths to a file that should be read but does not exist.

  **We want to show the user a bug that will explain how he can fix the problem himself.**
  I will come back to this later in the post.

  The second type of bug, the one a programmer sees using, for example, libraries, is a bit different.
  In this case, we want to help as much as possible with a quick fix to the unexpected error.
  However, this topic is beyond the scope of this post.

## Components of a good error message

  **What makes an error message good?**
  It is definitely a combination of several elements.

  **What matters is the description of the error itself.**
  It is worth **using a domain language** consistent with the user's expectations.
  For example, if the user sees Teacher in the application, we should also use that.
  When we return the information "User not exists", it may be surprising for the user.

  **It's also worth keeping an eye on the length of the error.**
  It should be as concise as possible while conveying the expected information.

  **The most important aspect, however, is how to mitigate the error.**
  After determining the problem, it is worth allowing the user to clearly define what action should be performed.
  For example, after exceeding the maximum number of allowed login attempts, it is worth telling the user what he can do next.
  Depending on the system, it may be necessary to enter the received SMS code or confirm the e-mail message sent to his e-mail address.

  A good example is also exceeding the number of requests at any given time.
  Instead of information about the exceeding itself, it is worth indicating how many minutes the user should wait before being able to act again.

  **Nothing is more frustrating than leaving the user alone.**
  The information "you did it wrong" is as helpful as no information, and it can only annoy users.

## Documentation reference

  I found it more often in the case of errors for the programmer.
  Still, some applications for "normal" users also supported it.
  **You can refer the user to the FAQ section or general documentation if you have stable documentation.**
  It will allow him to check the limitations of the system and at the same time, eliminate the need to return very long information about the cause of the error and how to eliminate it.

  **Users will be very grateful if you also share unique, standardized error codes.**
  It will allow them to look for additional material and even ask the application's authors directly why they are getting a specific error.

  For you, this is also a great way to track your system.
  If users frequently get specific bugs, it is possible that something in the process is not exactly what users expect.
  In addition, the occurrence of an unknown error will allow you to react quickly in a situation where the system does not work as it should.

## Inform about what the user can do

  Finally, the point I wanted to come back to.
  In a situation where internally in the system you have actions over which the user has no influence, **think about whether to inform about errors**.
  For example, the lack of permission to create a file in the operating system on which your server runs, from the user's perspective, is not serviceable at all.
  Or a similar situation: there is no required relationship between objects in the database (for example, a post with `user_id` as blank).

  There is **nothing the user can do** to fix this error.
  **As a developer, you should have additional alerts that will immediately notify you of this problem category.**
  On the other hand, the user should receive information about a critical error that has been automatically reported for repair.

  Additionally, you can allow users to manually report a problem (for example, by displaying an additional button), where they can provide additional information.
  However, do not rely on information from users.
  Numerous studies show that bugs are not reported very often, so you should have mechanisms on your side to catch them.
