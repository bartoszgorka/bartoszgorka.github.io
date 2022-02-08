---
title: "Check your password security with Have I Been Pwned?"
image: /assets/images/posts/check_your_password_security_with_have_i_been_pwned.png
excerpt: >-
  Users often do not see the dangers of a weak password.
  Often, they reuse the same passwords on different sites to make their daily lives easier.
  Security is one of the critical aspects of any project.
  With Have I Been Pwned? it is possible to check passwords based on various leaks.
date: 2022-02-09 07:00:00 +00:00
last_modified_at: 2022-02-09 07:00:00 +00:00
tags:
  - security
  - explanation
  - management
  - project
  - learning
---

  There are several ways to authenticate users to information systems.
  One of them is to use a combination of an email address and a login password.
  **Like any method, this one has some problems and weaknesses.**

  Password security covers a broad set of practices, and not all of them are appropriate or possible for all.
  **Users often do not see the dangers of a weak password.**

## Password security

  **What does it mean that the password is secure?**
  What should we protect ourselves from?
  Maybe all these topics are just conspiracy theories of people who earn money on security and need to promote services?

  In fact, we protect a lot.
  For example, a **profile on social media often allows you to log in to many other websites**.
  Also, like a specific person (you), it is possible to send unsafe links to all your friends.
  They may be more likely to follow the link than if they received it from a completely unknown person.

  **One way to discover a user's password is to use the dictionary method.**
  The attacker has a set of passwords that check to see if they are appropriate one by one.
  Such collections are available for purchase in certain places, or the attacker can build them based on public data leaks.

  **It is a big responsibility of the login system to limit the possibility of checking many passwords in a short time (rate-limiting).**
  The blocking of logging in after an incorrect password may also be recommended after a specified number of attempts.
  Unblocking login requires, for example, unblocking the account by opening a message sent to the email address.
  Without it, even entering the correct password will not allow you to log into the system.

  Before this method of attack, good protection will be to use different case characters and special characters.
  It will also **be worth avoiding passwords on known password lists (from various leaks)**.
  But how to obtain such lists?

## Have I Been Pwned?

  Over time, the industry realized that complex password composing rules (requiring a minimum number of special characters) did only a slight improvement.
  **Users often reuse the same passwords on different sites to make their daily life easier.**

  Have I Been Pwned? (I will use the HIBP abbreviation) created by Troy Hunt is an excellent tool for verifying already known passwords.
  The site itself does not publish this data in plain text, but password hashes are available.
  It does not have to do this because it is assumed that this data is already public (part of numerous data leaks).
  Attackers do not have access to text form, so they cannot use this information in attacks on unaware users.

  The scale of password checks, thanks to the API provided by HIBP, is huge.
  It is **even 1_260_000_000 checks per month**.
  Many sites check their customers' passwords to increase the level of security.
  Check out [Troy's post](https://www.troyhunt.com/open-source-pwned-passwords-with-fbi-feed-and-225m-new-nca-passwords-is-now-live/) for these stats, which also summarizes cooperation with government agencies.

  You can use the option of checking your email address for leaks.
  The same is possible for usernames and phone numbers.
  Many famous and highly recommended password managers use the options offered by HIBP to check the security of passwords.

### Is this system worth trusting?

  The FBI and the British National Crime Agency joined the project.
  These are massive datasets about leaks, often inaccessible to a regular company.

  I have one more critical piece of information for you.
  You don't have to trust Troy.
  **If you are using the API, you are not sending the entire password, only a fragment of its hash value.**
  The page will return a list of hashes that match yours, so you can safely check the match locally.
  Technically, this is accomplished by using [k-Anonymity](https://blog.cloudflare.com/validating-leaked-passwords-with-k-anonymity/).
  Instead of transmitting the full, unsalted password, only a fragment is sent that is not enough to indicate which password you are using.

  Additionally, **it is possible to download the entire database.**
  It will enable completely local searches of data and eliminate network communication.
  It may be necessary for specific applications (for example, isolated banking systems).

### k-Anonymity

  Let's start with understanding the **idea of k-Anonymity** to prepare an integration code with [API offered by HIBP](https://haveibeenpwned.com/API/v3#SearchingPwnedPasswordsByRange).
  Let's assume that I use the password `bartoszgorka`.

  ```elixir
  iex(1)> password = "bartoszgorka"
  "bartoszgorka"

  iex(2)> :crypto.hash(:sha, password) |> Base.encode16()
  "E0FCB1C9A40818E6155C1F09BBC1D0F211E07A88"
  ```

  The SHA-1 of my password is `E0FCB1C9A40818E6155C1F09BBC1D0F211E07A88`.
  By shortening them to a certain number of characters (5 by default), I will get the prefix `E0FCB`.
  This hash prefix is then used to query the remote database for all hashes starting with that prefix.
  Then the entire list of abbreviations starting with that prefix is downloaded.
  Each downloaded hash is then compared to see if any matches the locally generated hash.
  If so, you know your password has been leaked.
  It turns out that no one has used such a password.

  ```
  GET https://api.pwnedpasswords.com/range/{first 5 hash chars}

  D13C64E48114D50CA758DA9AE8FC43CAE72:26
  D7E2912C380A7C38542F32F55EADB4E72E2:23
  D87393003CABB670BA801E0521242DA9062:39
  D9D34198F11C2A380EBCC64F89CF1DA59F8:24
  DDF126ACC8AEAC0DE713933DB336C5C68D6:21
  E04AE518A90601A50D714F819FC1329E0EF:324
  E51DAA39622D62A0709CF2B6BEE6093CB3F:36
  EB2B684CFFD312FEA0FD1993C0358B109F9:19
  F1FAAEDB4ECB49C1CC19775E02E8951206A:1
  F2CFE61BC914F1A7CA272623F43BBF94D4D:20
  F375AB4982FFA1073F260B77ACFB6D9868F:9
  FA194BD24E6D9283D01158C42F43E400CEF:31

  ... more data ...
  ```

  Everything can be conveniently implemented using the HTTP protocol.
  It is also possible to introduce caching for performance purposes.
  Examples of Bash implementations are presented in the [gist](https://gist.github.com/IcyApril/56c3fdacb3a640f37c245e5813b98b99) form.

  **The algorithm used for the hash is a one-way transformation, so it is challenging to determine from the result what data was used.**
  HIBP returns additional information in its data: the number of times the data breaches contain the password.
  You can use it, for example, to systematically review which of your customers' passwords are very vulnerable to attacks.

## Summary

  **Security is one of the critical aspects of any project.**
  Sometimes the user is unaware of all the features offered by good-quality and trusted sites like Have I Been Pwned?.
  It will not be a challenge and long work for the programmer to prepare a system that, instead of checking the password length (number of characters) and the use of special characters, also ask the external HIBP system to make sure that the password was not found in explicit data leaks.

  **Let's take care of the safety of our users as best we can.**
  It can also be a strong advantage for our system in the eyes of more advanced users who care about their safety.
