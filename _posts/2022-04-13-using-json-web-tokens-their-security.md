---
title: "Using JSON Web Tokens and their security"
image: /assets/images/posts/using-json-web-tokens-their-security.png
excerpt: >-
  JSON Web Tokens known as JWT, are a great way to ensure security in communication between system parts.
  Remember not to trust the user and verify the data before use.
  JWT contains a lot of information about the user and his permissions.
  It is also a well-thought-out structure.
  It indicates in an accessible way for whom and by whom the token was prepared.
  Also, check out the topic of key rotation and its benefits.
date: 2022-04-13 06:00:00 +00:00
last_modified_at: 2022-04-13 06:00:00 +00:00
tags:
  - project
  - teamwork
  - security
  - explanation
  - tips
---

  In the case of API development, it is necessary to ensure that the user's identity can be verified.
  There are many ways to indicate who the server is dealing with.
  **In this post, I will focus on the approach using JSON Web Tokens** (JWT, [RFC7519](https://datatracker.ietf.org/doc/html/rfc7519)).

  An additional **benefit of JWT is the ability to verify the data transferred between the servers/parts** of the system.
  It allows us to check who we are talking to and whether we should allow it.
  **This token is URL-safe, and the information contained is encoded as JSON Object.**

### Construction

  **The JWT consists of three parts separated by a dot.**
  It is the **header, payload, and signature**.

  ```js
  eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
  .eyJpc3MiOiJqd3QuaW8iLCJzdWIiOiJodHRwczovL2JhcnRvc3pnb3JrYS5
  jb20iLCJuYW1lIjoiQmFydG9zeiBHw7Nya2EiLCJpYXQiOjE1MTYyMzkwMjJ9
  .UZRT2W_keMXghVftSlECePWMdWgJuSIfnQUtOmdJuCM

  header.payload.signature

  header = {
    "alg": "HS256",
    "typ": "JWT"
  }

  payload = {
    "iss": "jwt.io",
    "sub": "https://bartoszgorka.com",
    "name": "Bartosz Górka",
    "iat": 1516239022
  }
  ```

  The header should specify which algorithm was used.
  **Never use** `none`!
  Although it is one of the permissible (according to [RFC7518](https://datatracker.ietf.org/doc/html/rfc7518#section-3.1)) algorithms.
  In the case of using `none` **no signature is created so that any user can create their own token**!

  It is best to **choose and use only one algorithm**.
  **RS256 or HS256 are recommended**.

  **RS256 is an asymmetric algorithm that uses a private and public key pair.**
  The private key is used to sign tokens, and the public key is used for validation.
  It is assumed that the private key is never shared outside the system.

  **HS256**, on the other hand, is based on a symmetric approach **where both signer and verifier** (it can be the same system) **share a secret**.

  Personally, **I recommend using RS256 due to the advantages of asymmetric encryption**, even at the cost of a bit slower operation.
  A guarded private key that is not shared anywhere may not leak.
  In addition, the parties do not have to trust each other to use this mechanism (impossible in the case of HS256, in which both parties can generate any token).
  **We also have the option of dynamic key rotation** (more in the section below).

### Don't trust the user

  In the case of computer systems, it is advisable to handle the data received from the user with caution.
  **You should follow the rule: do not trust what you get; check and verify before using.**

  Also in the case of JWT.
  **If the token can be decoded, it does not mean that it is correct.**
  You should verify it (check its contents and signature) before using it.

  If you cannot decode the token - reject it.
  If you have a problem with verification - also reject the token.
  **Only when all checks are satisfied allow the token to be used.**

### Check not only the signature

  JWT contains a lot of information about the user and his permissions.
  **It is also a well-thought-out structure.**
  It **indicates in an accessible way for whom and by whom the token was prepared**.
  Use information about the algorithm used (`alg`), who signed the token (`iss`), for whom it was issued (`aud`), and information about the time: expiration time (`exp`), issued at (`iat`) and not before (`nbf`).

  The expiration time should be in the future (the token is valid all the time), while the other two cannot be in the future.
  Remember to allow yourself for small inaccuracies related to unsynchronized clocks, for example, by introducing a tolerance of about 5 seconds.

### Increase your security thanks to the rotation of the keys

  This part may refer to the public key infrastructure.
  Maybe there will be an opportunity to say more about it in future posts.
  In this, however, I will focus on what should be of most interest to you.

  **JWT allows you to increase system security thanks to the rotation of the keys to sign the tokens.**
  You can **tell which key has been used by indicating the key identifier** in the `kid` field in the header.
  If the key is leaked, you must stop using it, and all issued tokens will expire.
  **That is why it is worth rotating the keys and systematically exposing the oldest keys at a specified time.**

  If the token you received uses an unsupported key, reject the token as invalid.
  Of course, here again, you need to support the issue of the influence of time on the subject.
  Ideally, you should have two keys in use.
  The **newer one is used to sign new tokens**, and the **older one, as backward compatibility** for a certain period, remains usable but **only for the verification of tokens**.

### Find out more

  **The topic of security is critical.**
  In this post, I only touched upon the basics of working with JSON Web Tokens (JWT).
  If you want to learn more or are looking for a great tool to verify the content of tokens, use [JWT.io](https://jwt.io/).

  The security of your application is critical!
  **If you have any comments or questions, I am here to help.**
