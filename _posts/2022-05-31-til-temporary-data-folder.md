---
title: "TIL: Temporary data folder"
image: /assets/images/posts/til-temporary-data-folder.png
excerpt: >-
  The temporary directory for data storage is often used.
  However, /tmp dir is not always a good solution.
  Depending on the configuration, it can be represented differently.
  Check how you can fix it with simple modification.
date: 2022-05-31 06:00:00 +00:00
last_modified_at: 2022-05-31 06:00:00 +00:00
tags:
  - Elixir
  - code
  - TIL
  - explanation
  - tips
---

  **The temporary directory for data storage is often used.**
  It can be especially needed when the system prepares a file for sending, downloading, and processing data.
  The most common use of `/tmp` as the temporary directory is; **however this can be dangerous!**

  Simply using the code below may not always work.
  **You will feel it, mainly when you use releases or different operating systems.**

  ```elixir
  temp_file = Path.join("/tmp", file_name_variable)
  File.write!(temp_file, content)
  ```

  **Depending on the configuration**, `/tmp` **may be represented differently**.
  But that's not a problem for the Elixir language!
  Just make a small modification to fix this problem:

  ```elixir
  temp_dir = System.tmp_dir!()

  temp_file = Path.join(temp_dir, file_name_variable)
  File.write!(temp_file, content)
  ```

  **[System.tmp_dir!/0](https://hexdocs.pm/elixir/System.html#tmp_dir!/0) may return different results, depending on environment variables and operating system.**
  See the documentation for more details.
