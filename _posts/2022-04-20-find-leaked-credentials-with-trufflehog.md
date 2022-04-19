---
title: "Find leaked credentials with TruffleHog"
image: /assets/images/posts/find-leaked-credentials-with-trufflehog.png
excerpt: >-
  One of the possible vectors of an attack on the system is the disclosure of the used keys, certificates, or other access data.
  I found TruffleHog as a tool to verify your project.
  The tool can scan projects on GitHub or GitLab, including entire organizations!
  You can attach them to your CI/CD pipeline to verify that no confidential information has been published.
date: 2022-04-20 06:00:00 +00:00
last_modified_at: 2022-04-20 06:00:00 +00:00
tags:
  - code
  - project
  - learning
  - automation
  - security
  - DevOps
---

  **Security is undoubtedly one of the hot topics of recent years.**
  More and more importance is attached to security in the projects to minimize the risk of unauthorized interference with the system.

  **One of the possible vectors of an attack on the system is the disclosure of the used keys, certificates, or other access data.**
  This is often an easy problem to ignore as the effects are not immediately apparent.
  In this post, **I'd like to introduce a way to detect potential keys and other sensitive data automatically**.

## TruffleHog 🐷🔑🐷

  As part of testing tools that can be used to check the code, I came across [TruffleHog](https://github.com/trufflesecurity/trufflehog).
  It is an open-source tool offered by Truffle Security.
  **You can attach them to your CI/CD pipeline to verify that no confidential information has been published.**

### Installation

  **The tool itself is straightforward to install.**
  Personally, I recommend using Brew (on your local machine):

  ```bash
  brew tap trufflesecurity/trufflehog
  brew install trufflehog
  ```

  or Docker image (in processing pipelines):

  ```bash
  docker run -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --repo https://github.com/trufflesecurity/test_keys

  # For Apple M1 users:
  docker run --platform linux/arm64 -it -v "$PWD:/pwd" trufflesecurity/trufflehog:latest github --repo https://github.com/trufflesecurity/test_keys
  ```

### Using

  You can see a simple video example below:

  ![GitHub scanning demo](https://storage.googleapis.com/truffle-demos/non-interactive.svg){: .align-center}

  **The scanner offers to check up to [600 credential detectors](https://github.com/trufflesecurity/trufflehog/tree/main/pkg/detectors).**
  Additionally, it is possible to verify whether the access data are active all the time by checking the appropriate APIs.
  The tool **can scan projects on GitHub or GitLab**, including even entire organizations!

  I also checked the solution for my projects.
  There are indeed false alarms.
  But I managed to find and indicate the exact location of all the keys I knew and even those I did not know.

  Introduce this solution to your project to analyze whether confidential information is not available in the project code.
  Best-protected information is undisclosed information.
  Also, **remember to limit the use of a given key only to selected IP addresses** (if possible) **and services** (excellent in Google Cloud) **to limit the leak's impact** on the system.
