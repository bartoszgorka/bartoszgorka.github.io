---
title: "GitHub Copilot - a way to automatically generated code"
image: /assets/images/posts/github-copilot-automatically-generated-code.png
excerpt: >-
  GitHub Copilot tries to understand the programmer's intentions and generate the code closest to the expectations.
  Aside from generating code, understanding the programmer's intentions can be a much bigger problem.
  The use of the code created by this tool remains controversial.
date: 2021-07-28 07:00:00 +01:00
last_modified_at: 2021-07-28 07:00:00 +01:00
tags:
  - project
  - automation
  - management
---

  Recently [GitHub Copilot](https://copilot.github.com/) has been launched to help programmers write the code.
  The project is currently in the technical preview phase.
  A waiting list was applied.
  You can sign up when you want to support the project and test the offered solution.

  **The project is designed to help programmers to create code faster and with less effort.**
  For this purpose, artificial intelligence was used.
  **The model was learned from millions of publicly available repositories on GitHub.**

  Technically, the solution is based on GPT-3 (Generative Pre-trained Transformer 3).
  It is an autoregressive language model that uses **deep learning to create text similar to human language**.
  It was created by [OpenAI](https://openai.com/), an organization covering a wide range of topics related to artificial intelligence and machine learning.

## The power of GitHub

  The GitHub solution itself is still under development.
  Often, the suggestions are not perfect.
  However, if we know what we want to achieve, maybe Copilot will help us in it.
  **GitHub Copilot tries to understand the programmer's intentions and generate the code closest to the expectations.**

  Aside from generating code, **understanding the programmer's intentions can be a much bigger problem**.
  More precisely, the user must describe his request as precisely as possible to make it understandable for the machine.
  It is often much tricky because we do not know what and how we want to get.

  How is it possible that no one else has such solutions?
  Well, Microsoft, the owner of the GitHub platform, supported[^partnership] OpenAI and also obtaining the exclusive[^exclusive] right to use this technology for code generation.

  [^partnership]: [OpenAI forms exclusive computing partnership with Microsoft to build new Azure AI supercomputing technologies](https://news.microsoft.com/2019/07/22/openai-forms-exclusive-computing-partnership-with-microsoft-to-build-new-azure-ai-supercomputing-technologies/)

  [^exclusive]: [Microsoft teams up with OpenAI to exclusively license GPT-3 language model](https://blogs.microsoft.com/blog/2020/09/22/microsoft-teams-up-with-openai-to-exclusively-license-gpt-3-language-model/)

## Copyright problem

  **The use of the code created by this tool remains controversial.**
  It is based mainly on open source solutions, the code of which (not all solutions) will be partially used in other projects without the authors' knowledge.

  It can be indicated that open source solutions are prepared to be publicly shared and duplicated.
  However, **most of them are based on the [GNU General Public License](https://en.wikipedia.org/wiki/GNU_General_Public_License).**
  **Copilot can propose copy-paste solutions straight from repositories.**
  Often even with original comments.
  Ultimately, **the developer is responsible for the legal aspects of the software** they provide.
  We can even **accidentally copy the GPL-licensed code directly into our project**.

  The issue of a **Non-disclosure agreement** can also be a significant threat.
  It often assumes that we cannot provide any information about the code and solutions used in the project.
  However, **Copilot transmits the data to its servers when proposing the code**.
  In case of acceptance or rejection of the suggestion, the relevant information is passed to the algorithm.
  Indeed, the entire code is not available, but the imports and functions are sent [^telemetry].

  [^telemetry]: [About GitHub Copilot telemetry](https://docs.github.com/en/github/copilot/about-github-copilot-telemetry)

## Summary

  One may wonder about the power of this solution.
  It cannot be denied that more and more powerful tools are developed every year.
  They are often tools that make our lives easier and automate specific activities.

  **Generating text close to natural language, text that makes sense, is a non-trivial task.**
  It is no wonder that it requires huge financial outlays, and companies that get involved will want to use the technology commercially.

  **The solution itself seems to be an attempt to circumvent the search for answers (popular googling).**
  Instead of searching through the results, we can stay focused on the IDE.
  This will be a step to bring GitHub (and possibly StackOverflow) to our editors.

  However, after the initial interest in the project, **most programmers will not use the solution in their daily work**.
  Legal issues aside, the biggest problem is getting the code that meets our expectations.
  **Instead of preparing something ourselves in a few minutes, we will be spending much more time rejecting Copilot's suggestions.**
