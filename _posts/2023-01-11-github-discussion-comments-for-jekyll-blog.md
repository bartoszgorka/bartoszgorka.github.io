---
title: "Use GitHub discussion to have comments on your Jekyll blog"
image: /assets/images/posts/github-discussion-comments-for-jekyll-blog.png
excerpt: >-
  I introduced the possibility of commenting on posts directly on the page.
  With giscus, we can use Discussions on GitHub to store comments.
  In this simple way, you can add comments to your Jekyll blog!
date: 2023-01-11 06:00:00 +00:00
last_modified_at: 2023-01-11 06:00:00 +00:00
tags:
  - code
  - blog
  - learning
  - tips
  - explanation
---

Recently, **I've been considering introducing the possibility of commenting on posts directly on the page**.
Some of the readers do it via Twitter or by contacting me via mail.
However, it is not as convenient as discussing a given topic publicly.

My blog is fully static.
**GitHub Pages is responsible for hosting, which allows me to automatically build and release new changes** as soon as a new post appears in the repository.
This has great advantages.
It eliminates the need to build and transfer files to the server manually.

### Available solutions

When analyzing the available solutions, **I found Disqus**, for example.
It allows you to join the blog network that uses a given solution.
Unfortunately, **recent changes, especially related to the issue of user tracking, have led me to resign from this solution**.
Another downside is the **large size of the loaded data**, which **slows down page loading**.

**I wanted to rely entirely on the possibilities offered by GitHub.**
One of them was the ability to use [utterances](https://github.com/utterance/utterances).
It is an open-source solution that does not use tracking.
In addition, there are no ads in it, and the comments are stored directly in GitHub issues.

It would seem that we have the perfect solution.
However, it did not give me peace of mind **why Issues, when Discussions on GitHub, have been available already**.
Ultimately, the choice fell on [giscus](https://github.com/giscus/giscus).

### Giscus for your Jekyll blog

**The introduction of this comment system is straightforward.**
For simplicity of management, I used `_config.yml` to store configuration variables:

```yml
comments:
  giscus:
    enabled: true
    repo: "bartoszgorka/bartoszgorka.github.io"
    repo_id: "MDEwOlJlcG9zaXRvcnkyOTk2NTk5MzI="
    category: "Announcements"
    category_id: "DIC_kwDOEdxynM4CThkc"
    mapping: "pathname"
    reactions_enabled: "1"
    theme: "light"
```

Technically, **giscus uses the GitHub Discussions Search API to find the corresponding discussion related to a given post** based on the selected mapping (`URL, pathname, <title>`, etc.).
**If not found, giscus bot will fully automatically create a new discussion when someone leaves a comment or reaction.**

Please refer to [giscus.app](https://giscus.app/) for an explanation of all parameters.
There will also be generated `repo_id` and `category_id`, needed for the correct operation of the scripts.

The script itself can be represented as follows:

```html
\{\{ if site.comments.giscus.enabled \}\}
<script src="https://giscus.app/client.js"
        data-repo="{{site.comments.giscus.repo}}"
        data-repo-id="{{site.comments.giscus.repo_id}}"
        data-category="{{site.comments.giscus.category}}"
        data-category-id="{{site.comments.giscus.category_id}}"
        data-mapping="{{site.comments.giscus.mapping}}"
        data-strict="0"
        data-reactions-enabled="{{site.comments.giscus.reactions_enabled}}"
        data-emit-metadata="0"
        data-input-position="bottom"
        data-theme="{{site.comments.giscus.theme}}"
        data-lang="en"
        data-loading="lazy"
        crossorigin="anonymous"
        async>
</script>
<noscript>Please enable JavaScript to view the comments.</noscript>
\{\{ end \}\}
```

**In this simple way, you can add comments to your Jekyll blog!**
