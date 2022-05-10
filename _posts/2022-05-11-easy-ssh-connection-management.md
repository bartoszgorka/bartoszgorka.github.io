---
title: "Easy SSH connection management with configuration file"
image: /assets/images/posts/easy-ssh-connection-management.png
excerpt: >-
  When connecting to the server with SSH, you may need to use a certificate other than the default one.
  Additionally, often the username is also more complicated.
  How do you remember it all?
  Special configuration file that will help us solve the problem.
date: 2022-05-11 07:00:00 +00:00
last_modified_at: 2022-05-11 07:00:00 +00:00
tags:
  - code
  - project
  - teamwork
  - automation
  - explanation
  - tips
  - DevOps
---

  **Working with multiple servers and connecting to everyone via SSH can be painful.**
  If you have one server, you are unlikely to experience a problem.
  However, the more dynamically created servers you have to verify, the more bothersome the problem.

  You may need to use a certificate other than the default one when connecting to the server.
  Additionally, often the username is also more complicated.
  **How to control all this when you get thousands of disturbing messages that something happened to the selected server?**

### Security

  I know that **direct login to the servers is not recommended**.
  Instead, it would be best to **use Bastion** or some other way to connect to the server.
  A good example is the communication method offered by Google Cloud Platform to connect to the Kubernetes Engine.

  However, **this post is for you** if you still need to use the traditional SSH connection at work or for your own projects after hours.

### Connecting via SSH with SSH config file

  **SSH configuration files are located in the** `~/.ssh` directory by default.
  You can put a **special configuration file to help us solve the problem** indicated earlier.
  You can use your favorite text editor to create it.
  It should be created as `~/.ssh/config`.

  Its content may be empty or will be similar to the following settings:

  ```ssh
  Host <your server alias>
    Hostname <URL or IP>
    User <username>
    IdentityFile <private key location>
  ```

  For example, one of my servers has the following configuration (the URL has been changed):

  ```ssh
  Host data-processing
    Hostname data-processing.example.com
    User bartoszgorka
    IdentityFile ~/.ssh/data-processing/id_rsa
  ```

  It is worth noting that `User` and `IdentityFile` are optional.
  **You can specify other parameters in your settings according to the list of [supported options](https://www.ssh.com/academy/ssh/config)**.

### More information

  **The configuration allows you to use multiple wildcards when creating entries.**
  It can be, for example, `*` (substitute for one or more characters) or `?` (substitute for exactly one character).
  It is most often used with server names to, for example, **create a** `server-dev-*` **group in which all servers will use the same** `IdentityFile`.

  The previously mentioned **[SSH Academy](https://www.ssh.com/academy/ssh/config)** website is an **excellent learning resource**.
  It contains a lot of helpful information on how it is all technically implemented.
  You can also see the explanation of cryptographic private and public keys.

  **Configuration files, can be used to increase your productivity.**
  Thanks to them, you will not have to remember all server-dependent settings or use additional notes.
