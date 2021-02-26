---
title: "TIL: Weak optimization of System.get_env/0 in Elixir"
excerpt: "
  Based on Erlang VM, Elixir is often limited by a virtual machine's capabilities or the language itself.
  One such limitation manifests itself in weak optimization of System.get_env/0 function.
"
date: 2021-02-16 07:00:00 +01:00
last_modified_at: 2021-02-16 07:00:00 +01:00
tags:
  - Elixir
  - code
  - TIL
---

  **Each day brings new opportunities and challenges.**
  In the series "Today I Learned" (TIL), I wanted to cover topics that may not be widely known.
  Let me know in the comments what you think about such a series.
  Here is the first such topic.

  **Based on Erlang VM, Elixir is often limited by a virtual machine's capabilities or the language itself.**
  One such limitation manifests itself in weak optimization of `System.get_env/0`[^1].

  [^1]: [System.get_env/0](https://github.com/elixir-lang/elixir/blob/v1.11/lib/elixir/lib/system.ex#L447) in Elixir v11.0

  ```elixir
  @spec get_env() :: %{optional(String.t()) => String.t()}
  def get_env do
    Enum.into(:os.getenv(), %{}, fn var ->
      var = IO.chardata_to_string(var)
      [k, v] = String.split(var, "=", parts: 2)
      {k, v}
    end)
  end
  ```

  Breaking the string into `[key, value]` is necessary because from `:os.getenv()`[^2] we get concatenated information instead of divided.
  See that `$=` in line 3.


  [^2]: [Erlang :os.getenv()](https://github.com/erlang/otp/blob/OTP-23.0/lib/kernel/src/os.erl#L117) in OTP 23.0

  ```erlang
  -spec getenv() -> [env_var_name_value()].
  getenv() ->
      [lists:flatten([Key, $=, Value]) || {Key, Value} <- os:list_env_vars() ].
  ```

## Impact

  **I have never used this feature.**
  Instead, I used `System.get_env/1`[^3], which is much better prepared and does not need to operate on the entire environment variables list.

  [^3]: [System.get_env/1](https://github.com/elixir-lang/elixir/blob/v1.11/lib/elixir/lib/system.ex#L477) in Elixir v11.0

  **The impact of this bug is relatively low.**
  If we have to use these variables in runtime, a better solution seems to be `System.get_env/1` or the use of the cache mechanism.

### Source

  This article was based on information from Trevor Brown and his blog.
  You can find the source article in which the author explains the background of the changes here: [Performance of Elixir's System.get_env/0 Function](http://stratus3d.com/blog/2020/09/30/performance-of-elixirs-system-get-env-function/).
