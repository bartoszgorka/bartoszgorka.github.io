---
title: "Response status and the security of your API"
image: /assets/images/posts/response-status-and-the-security-of-your-api.png
excerpt: >-
  Have you thought about the response statuses of your endpoints?
  Some of them can be used for scanning your architecture.
  Some attacks on IT systems target the weakest areas, which may be addresses used for internal purposes.
date: 2021-08-25 07:00:00 +01:00
last_modified_at: 2021-08-25 07:00:00 +01:00
tags:
  - project
  - security
---

  In recent days I came across an interesting [discussion](https://twitter.com/gkotfis/status/1425442945679888389) on Twitter regarding the application's security.
  It concerned the response status in case the resource was not found.
  According to the documentation, most of the projects use [HTTP status 404](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404) - the resource was not found.

## Security

  However, is this the correct behavior?
  Especially from the point of view of data security.
  Some attacks on IT systems target the weakest areas, which may be addresses used for internal purposes.

  Checking addresses and differences in responses between the [401 Unauthorized](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/401) and [404 Resource not found](https://developer.mozilla.org/en-US/docs/Web/HTTP/Status/404) statuses may be the first step in an attack, i.e., a scan of our architecture.

  I agree that publicly available addresses are often displayed along with documentation (e.g., Swagger).
  However, most often, they are more rigorously checked and protected than "private" addresses.
  Such "secret" routes can be treated less rigorously because "nobody knows them".
  Some of them may also reveal too much data about users, which attackers can use.

## Impact

  The vulnerability itself is a relatively low-security risk, as information about the paths to the actual attack must still be used.
  However, this does not change the fact that maybe instead of the default 404 in the case of a lack of resources, you can use the 401 status and hide information about this fact.

  Special thanks to [Grzegorz Kotfis](https://twitter.com/gkotfis) for bringing up the topic and the opportunity to reflect on the security issue of the developed programming solutions.
