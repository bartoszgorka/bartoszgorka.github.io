---
title: "What is Open Graph protocol and how to use it for your website?"
image: /assets/images/posts/what-is-open-graph-protocol-and-how-to-use-it-for-your-website.png
excerpt: >-
  The presentation of extra details about your website is straightforward.
  All you need to do is use Open Graph protocol.
  Any web page can become a rich object in a social graph.
  You can use it to show a thumbnail, author details, or even a short description.
date: 2022-03-09 07:00:00 +00:00
last_modified_at: 2022-03-09 07:00:00 +00:00
tags:
  - code
  - project
  - learning
  - explanation
  - tips
---

  It's a beautiful morning, and you've come across an interesting article.
  **You would like to share it** on your profile on social media - maybe Facebook, it can be Twitter, or even something completely different.
  So you **copy the link and paste it** into the field for adding a new post.
  **And suddenly, you see an extra image that you didn't add yourself.**

  In this post, I would like to bring you closer to the topic of **[Open Graph protocol](http://opengraphprotocol.org/)**.
  **This is how you can tell other sites which thumbnails (not only this) to use instead of a blank or no image.**
  For example, my previous post might be presented as:
  ![Twitter post](/assets/images/what-is-open-graph-protocol-and-how-to-use-it-for-your-website/twitter_post_opengraph.png){: .align-center .border .max-width-300px}

## Open Graph protocol

  **The protocol is the industry standard for providing information regarding the presentation of pages.**
  Enables any web page to become a rich object in a social graph.

  Two sides can be specified - the entry and the system on which the link to the entry is posted.
  For the entry page, it is necessary to specify the fields in `<head>` inside HTML code so that the `<meta>` fields contain the appropriate keys and values.
  **The platform is responsible for the entire handling of this information to show something more**, such as a thumbnail or description of the entry, instead of the link itself.

  You can see some of the information available from my blog below:

  ```html
  <!-- Required data -->
  <meta property="og:type" content="article">
  <meta property="og:title" content="Ecto Changeset for verifying parameters used in your&nbsp;API">
  <meta property="og:url" content="https://bartoszgorka.com/ecto_changeset_for_verifying_parameters_used_in_your_api">
  <meta property="og:image" content="https://bartoszgorka.com/assets/images/posts/ecto_changeset_for_verifying_parameters_used_in_your_api.png">

  <!-- Extra data -->
  <meta property="og:locale" content="en_US">
  <meta property="og:site_name" content="Bartosz Górka">
  <meta property="og:description" content="You can use Ecto to&nbsp;check any information from the user. Extra, we have another layer of&nbsp;security. Only the supported parameters are passed to&nbsp;the domain layer. Using Ecto to&nbsp;verify parameters from the user is not difficult and can be part of your API verification pipeline.">
  ```

  The first four entries are for the required information indicating: type, title, address, and thumbnail.
  Any further information is optional but is often used by platforms.

### Twitter

  **In the case of Twitter, we are dealing with a combination of Open Graph protocol and their own in which they use the prefix** `twitter:`.
  Using [documentation](https://developer.twitter.com/en/docs/twitter-for-websites/cards/guides/getting-started#twitter-cards-and-open-graph), we can check what tags are supported.
  Their `twitter:` tag has priority over those defined by Open Graph protocols.
  However, the Open Graph property is supported as the falls back mechanism when `twitter:` was not found.

  You can see some examples of tags (only `twitter:`) below:

  ```html
  <meta name="twitter:card" content="summary_large_image">
  <meta name="twitter:site" content="@bartoszgorka96">
  <meta name="twitter:creator" content="@bartoszgorka96">
  <meta property="twitter:title" content="Ecto Changeset for verifying parameters used in your&nbsp;API">
  <meta property="twitter:image" content="https://bartoszgorka.com/assets/images/posts/ecto_changeset_for_verifying_parameters_used_in_your_api.png">
  ```

## Technical solutions

  **To verify the correctness of the presentation** of information about an entry, **you can use**, for example [Facebook Object Debugger](https://developers.facebook.com/tools/debug/) or [Twitter Card validator](https://cards-dev.twitter.com/validator).
  Suppose you decide to enter this information on your website.
  In that case, you should start by checking if there are already libraries/plugins for your programming language that support this.
  For this blog, it's the `gem "jekyll-seo-tag"`.
