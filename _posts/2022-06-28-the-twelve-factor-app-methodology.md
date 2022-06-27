---
title: "The Twelve-Factor App methodology"
image: /assets/images/posts/the-twelve-factor-app-methodology.png
excerpt: >-
  The Twelve-Factor App methodology is a methodology for building software-as-a-service applications.
  It allows you to eliminate the most common problems with our applications.
  Some of them have already become the industry standard, so you should get to know them all.
  The methodology constitutes design guidelines strongly focused on cloud-native, portable, and resilient applications.
date: 2022-06-28 06:00:00 +00:00
last_modified_at: 2022-06-28 06:00:00 +00:00
tags:
  - management
  - project
  - teamwork
  - learning
  - tips
---

  **The Twelve-Factor App methodology is a methodology** for building software-as-a-service applications.
  It allows you to **eliminate the most common problems** with our applications.

  **These best practices were designed to make applications more portable and less sensitive to deployment.**
  It is worth bearing in mind that they were **created around 2011**, proposed by Heroku.
  It is quite a long time ago for the volatility of the IT world.
  Some have become the industry standard, while some are more dubious.

## Assumptions

  The methodology constitutes **design guidelines strongly focused on cloud-native, portable, and resilient applications**.
  Despite the performance over ten years ago, they are still worth getting to know.

  Especially if we want to break them consciously.
  **Some became the standard based on which many useful tools were created.**

  As a great place to learn and understand the rules, I recommend the website [12factor.net](https://12factor.net/).
  It covers each principle with examples.
  In this post, I would like to cover all the rules and enrich them with my thoughts.

## Factors

### One codebase tracked under version control

  **Version control should be an integral part of any project.**
  Thanks to it, we can verify changes and use previous versions in case of errors.

  **What we test on the test servers should be the same as what goes into production later.**
  The credentials data configuration may change, but the code should remain the same.
  If we need to share code between applications, we should take it to a library that we will use within projects.
  Thanks to this, **one code can be deployed in many environments**.

### Explicit and isolated dependencies

  **The application should be self-contained and independent.**
  You cannot expect anything to be installed on the server.
  If something is needed, the application must take care of it to install it or indicate that it needs it (dependency manifest/lock file/Gemfile - depending on the language).

  **Containerization will be of great importance here**, enabling the preparation of the state for the application with the installation of all the necessary add-ons.
  **It simplifies the introduction of new programmers to the team** - all the necessary dependencies should be prepared automatically.
  Additionally, it **eliminates the problem of version differences of all dependencies**.

### The configuration should be stored in the environment

  This point relates to the first point.
  **We can set variables thanks to the environment while the code remains unchanged.**

  Configuration in the environment is universal, not directly related to a given programming language.
  This configuration should be stored within the application code.

  The latter is problematic for me because I like to have some default values if the environment variables are not used.
  However, when variables must be set, it is worth checking the state and raising an exception when there is no data.

### Supporting services

  A supporting service is any service our application uses for its proper operation, e.g., data warehouse, queuing system, or SMTP service.
  **Our services should be easy to replace without interfering with the code.**
  It can be achieved by **communicating through the API issued by the services**.

  The easy replacement may be questioned here.
  Ecto in Elixir is a good example.
  It allows you to change the database from PostgreSQL to MySQL by selecting a different adapter.
  However, suppose we are associated with specific technologies (such as PostGIS). In that case, it is difficult to expect that it will be just as easy to migrate.

### Separation of building stages from running

  **Separation of the stages of building, publishing, and launching.**
  We create an executable package (for example, Docker image), which can then be run together with environment variables to create a release.
  The idea is the ability to **run the same code multiple times without building it from scratch each time**.

  **It works great, especially when we have Docker images.**
  They allow you to use the same built image in many environments and test it thoroughly before it goes to the production server.

### Application as stateless processes

  A fundamental postulate - the **application should not use the file system as permanent, non-volatile**.
  It can only use it as its cache.
  The entire system should be one or more independent, stateless processes.
  **If you need to store something, use supporting services like a database or external file system.**

  This point is even part of the use of the cloud.
  Although we have a server, each version can use completely different instances, so if we want to store something, we have to keep it outside the operating system.

### Export services via port binding

  This point mainly relates to how you communicate with our service.
  It should **expose a specific interface that can be used**, for example, by listening for HTTP requests on port 4000.

  **It allows using a given application as a supporting service for another application.**
  Building the micro-services architecture will probably be straightforward for us to fulfill.

### Concurrency through scaling

  We are **process-oriented**.
  If our application receives increased traffic, we can start other processes.
  However, **instead of scaling up and creating one large instance, it is sometimes better to scale out and run our application on other servers**.

  It is very convenient and often (although you need to verify it) cheaper than one very powerful server.
  **Orchestrators like Kubernetes perform this behavior.**

### Maximize reliability

  **The app should launch and exit quickly.**
  It can be very inexperienced if it takes a few minutes for the entire application to start.
  Thanks to the speed, we can restore the version in case the new one turns out to be faulty.
  Any changes in the configuration or scaling, in this case, will also be faster and easier to prepare.

### Uniformity of environments

  This principle applies primarily to **maintaining similar development, test, and production environments**.
  To be able to work on something or change something, we must be sure that we can verify the changes.
  Differences may result in time impact.
  You have worked on changes in the development environment for a long time, and many changes have been made.
  Finally, the final version goes to production without all the changes step by step.

  **We should accelerate delivery.**
  Instead of months or weeks, changes from the developer should go to production as soon as possible.
  Additionally, we should keep the same toolkit in all environments, preferably with the same version.
  We have Docker images, Homebrew, or asdf for package management, so it shouldn't be a problem.

### Logs as a stream of events

  Logs provide insight into the operation of the application.
  They are the list of events in the system, aggregated and ordered over time.

  **Our application should not care about the way logs are saved and processed.**
  Instead, it **should use the standard stdout/stderr output and report events** happening in the system there.
  Further **data capture and processing should be done with the help of other tools**, for example, thanks to DataDog.

### Application and task management

  **The application itself should not run one-time tasks.**
  They should be externalized.
  It will eliminate the abuse of the application that runs some background tasks.

  In the case of data migration, we may have a slowdown and a release block.
  Still, at the same time, we will not forget to start the necessary data migration.
  In such a case, it is worth running the migration as a separate script throughout the entire process instead of executing it directly from the code of the executed application.
