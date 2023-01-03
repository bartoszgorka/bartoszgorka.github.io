---
title: "TIL: Using phash2 for data partitioning"
image: /assets/images/posts/using-phash2-data-partitioning.png
excerpt: >-
  In programming, we often need fairly distribution of data.
  Using phash2 we can get the same hash for the same Erlang term regardless of machine architecture and ERTS version.
  This fast function should always be used for hashing any data and limiting the result to a range of integers.
date: 2023-01-04 06:00:00 +00:00
last_modified_at: 2023-01-04 06:00:00 +00:00
tags:
  - Elixir
  - code
  - TIL
  - learning
  - tips
---

**In programming, we often need fairly distribution of data.**
For example, this can be used when assigning work to one of several running processes and partitioning.

Recently, while realizing such a problem, I decided to approach the subject more "mathematically".
Using [erlang:phash2/2](https://www.erlang.org/doc/man/erlang.html#phash2-2) **you can get the same hash for the same Erlang term regardless of machine architecture and ERTS version**.
It is possible to indicate the maximum range in the form `0..Range-1`.
The function should always be used for hashing terms; **thanks to its speed, it is also great for binaries**.
We can hash any data and limit it to a range of integers.

If you expect to use **atoms as input, it makes sense to use a preprocessing step** with `:erlang.term_to_binary/1` so that the results obtained are more spread out in the given range.
Example from [GitHub discussions](https://github.com/bitwalker/libring/issues/12#issuecomment-373416948):

```elixir
iex(1)> :erlang.phash2(:foo)
27999
iex(2)> :erlang.phash2(:erlang.term_to_binary(:foo))
86811783

iex(3)> :erlang.phash2(:erlang.term_to_binary(:a))      
64216263
iex(4)> :erlang.phash2(:erlang.term_to_binary(:aaaaaa))
86861957

iex(5)> :erlang.phash2(:erlang.term_to_binary(:b))     
14428281
iex(6)> :erlang.phash2(:erlang.term_to_binary(:b), trunc(:math.pow(2,32)-1))
3235653753

iex(7)> :erlang.phash2(:foo, trunc(:math.pow(2,32)-1))                      
27999
iex(8)> :erlang.phash2(:erlang.term_to_binary(:foo), trunc(:math.pow(2,32)-1))
355247239
```
