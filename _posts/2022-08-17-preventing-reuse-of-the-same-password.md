---
title: "Preventing reuse of the same password"
image: /assets/images/posts/preventing-reuse-of-the-same-password.png
excerpt: >-
  Safe passwords are a very important aspect of application security.
  How can you check if the password has not been used before?
  Compare passwords in Elixir thanks to Bcrypt.
date: 2022-08-17 06:00:00 +00:00
last_modified_at: 2022-08-17 06:00:00 +00:00
tags:
  - Elixir
  - code
  - security
  - project
  - explanation
  - tips
---

  Safe passwords are a very important aspect of application security.
  **Forcing users to change their passwords often results in using the same passwords repeatedly.**

  In this post, I will not discuss issues related to enforcing password changes.
  Instead, we will **focus on the way you can check if the newly entered password is not the same as the one already stored in the database**.
  At the outset, I would like to reassure all concerned.
  The passwords in the database should and must be stored in a hashed form!

### Comeonin and Bcrypt

  **In Elixir, perhaps the most popular password management library is Comeonin.**
  It allows us to use many hashing functions such as Pbkdf2, Argon2, and Bcrypt.
  Argon2 is assumed to be the strongest variant.
  **In this post, I will focus on probably the most popular one: Bcrypt.**

  We want to compare the new password entered by the user with the one he has already set.
  **Of course, we store the previous password in a hashed form.**
  For example, storing the last five passwords may be a good practice.

  **Let's start by understanding how a hashed password is structured.**

  ```elixir
  iex(1)> Bcrypt.hash_pwd_salt("password")
  "$2b$12$wGy5rRdpE/fpInBo41a4iuSohGb0Yv5tAiK2h/.VCgFX5N/GRviuC"

  iex(2)> Bcrypt.hash_pwd_salt("password")
  "$2b$12$PMpwStb1MzNx4/o8V3n4.eIjB3hX5J67JeScriNKH52a3PTW3WybS"
  ```

  As you can see, thanks to `hash_pwd_salt/1`, we get a **different result each time**.
  It is due to the use of **randomly generated salt**.
  However, **we can see some patterns**:

  ```
  |Algorithm Identifier | Cost |           Salt         |              Hash
        (Bcrypt)
          $2b             $12$   wGy5rRdpE/fpInBo41a4iu   SohGb0Yv5tAiK2h/.VCgFX5N/GRviuC
  ```

  The hash starts with the string `$2b` that identifies Bcrypt.
  I am using the default settings, so the computational cost is 12.
  Then we have **22 characters for the salt** itself and **31 to store the password hash**.

  **After all, we do not know the previous password's explicit (plain) form.**
  How can password reuse be checked based on the build?

### Compare passwords

  It is best to read the Elixir code to answer the above question.
  We will use `Bcrypt.Base.hash_password(password, salt)` to check the password.
  **We can compare whether we get the same hashed password by indicating the salt from the previous password.**

  ```elixir
  defmodule MyApp.PasswordChecker do

    # Length:
    # Algorithm Identifier  => 3
    # Cost                  => 4
    # Salt                  => 22
    # In total              =  29 first characters
    @password_range 0..28

    @doc """
    Verify - the password is the same as the given hashed password.
    Used Bcrypt internally.

    ## Example
        iex> Bcrypt.hash_pwd_salt("password")
        "$2b$12$wGy5rRdpE/fpInBo41a4iuSohGb0Yv5tAiK2h/.VCgFX5N/GRviuC"

        iex> old_hashed_password = Bcrypt.hash_pwd_salt("the-old-one")
        "$2b$12$LfVwaQwfIVSCfTPPjeuJpen4KS/RqnACq3GlChjuq7YB9aTUmwz8C"

        iex> already_used?("the-old-one", old_hashed_password)
        true

        iex> already_used?("password", old_hashed_password)
        false
    """
    @spec already_used?(binary(), binary()) :: boolean()
    def already_used?(new_password_text, old_hashed_password) do
      salt_with_bcrypt_settings = String.slice(old_hashed_password, @password_range)

      Bcrypt.Base.hash_password(new_password_text, salt_with_bcrypt_settings) == old_hashed_password
    end
  end
  ```
