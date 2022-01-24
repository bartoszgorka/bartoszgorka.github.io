---
title: "Zero downtime deployments"
image: /assets/images/posts/zero-downtime-deployments.png
excerpt: >-
  Not so long ago, to release the application, it was necessary to modify the binary files on the servers.
  Thanks to good DevOps practices, the entire process can be changed.
  It is possible to achieve tens or hundreds of releases each day.
  Everything without the downtime of the application.
date: 2022-01-26 07:00:00 +00:00
last_modified_at: 2022-01-26 07:00:00 +00:00
tags:
  - project
  - release
  - DevOps
  - automation
  - tips
---

  **Not so long ago, to release the application, it was necessary to modify the binary files on the servers.**
  It required pausing the application to be able to make changes.
  It was often done at night, during designated application release windows.
  I hope you don't have to do this anymore.

  **Thanks to good DevOps practices, the entire process can be changed.**
  It is **possible to achieve tens or hundreds of releases** each day.
  Everything **without the downtime** of the application.

  Changes have been outside the production environment for too long in some projects.
  For programmers, it would be best if their changes, after approval by members of the development team and the quality analysis team, were published on the test environment (staging).
  Then, if everything works as intended, it will be immediately transferred to the production environment.

  Frequent releases have many benefits.
  **Allows limiting the number of changes and their potential impact on the entire project.**
  It is much easier to check a new version containing one task than changes consisting of dozens of interacting tasks.
  Small, frequent releases are less risky.
  **Plus, you can deliver business value faster.**
  You can also receive customer feedback immediately instead of waiting to prepare all changes.

## Blue-green deployments

  **The first technique for releasing applications without downtime is to use a second environment.**
  The blue half handles regular production traffic.
  The second part (green) remains dormant until the application release process begins.

  ![Blue-green deployment](/assets/images/zero-downtime-deployments/blue_green_deployment.jpeg)

  New changes go to the green environment.
  **You can switch to the current version when everything is launched correctly** and checked automatic health checks.
  **The load-balancer that directs requests will stop passing them to the blue environment and start to the green.**
  Ultimately, all requests are handled by the green group.

  The previous blue group is becoming obsolete and can be turned off.
  Eventually, the old green becomes the new blue after receiving the latest changes.
  The entire process can be performed again by releasing another version of the system.

  What about bugs?
  If errors occur during the automatic check, it is enough to stop the process.
  The user can still use the version offered by the blue group.

  The cost issue is also not that painful.
  **The second group of servers is only needed when releasing a new version.**
  After all, you can delete the part containing the old version of the application.
  It is especially convenient and easy when using the Cloud.

## Canary deployment

  The second technique is similar to the one previously discussed.
  However, in this case, **we start with a small group of servers** that should be affected by the change.
  The load-balancer can redirect there, for example, **2-5% of the traffic of the entire application**.

  ![Blue-green deployment](/assets/images/zero-downtime-deployments/canary_deployment_start.jpeg)

  **If no bugs are encountered, and everything is fine, you can gradually increase the proportion of the new version.**
  Ultimately, 100% of all requests are handled by the new version.

  ![Blue-green deployment](/assets/images/zero-downtime-deployments/canary_deployment_final.jpeg)

  If something fails, very few users will experience the problem.
  You will be able to reverse these changes by redirecting all traffic back to servers running the older version of the software.

  Depending on the company's policy, sample selection can be made based on different parameters.
  In the case of some companies, it will simply be a fragment of network traffic.
  For others, it may be necessary to redirect only selected users.
  These can be, for example, internal users and employees.

  As a curiosity, I can explain why this method is called so.
  **In the nineteenth and twentieth centuries, canaries were used as an early warning system in mines.**
  Their remarkable sensitivity has been used to detect toxic gases such as carbon monoxide, methane, or carbon dioxide.
  When a canary was behaving atypically or losing consciousness, it was a clear warning signal for the miners.
  They were to leave the dangerous area immediately.

## Summary

  The methods discussed are intended to **reduce the risk of delivering defective software, which may cause significant losses to the company**.
  Usage depends in most cases on the processes and capabilities of the company.

  **Both green-blue deployment and canary deployment should not significantly increase server infrastructure costs when using the Cloud.**
  In the case of your own servers, the first option may be more expensive as half of the servers will not be used.

  I see another significant advantage for the use of canary deployment.
  **It allows you to test changes in the production environment instead of imitations.**
  It can be crucial with performance issues.
  By slowly increasing the workload, you can monitor and report the most important metrics about the impact of the new version on the production environment.
