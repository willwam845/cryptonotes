---
description: this is basically the rsactftool of diffe-hellman
---

# Pohlig-Hellman

The Pohlig-Hellman algorithm is what you can use to solve most D-H problems if it is just standard textbook DH.

Sage code for doing stuff with it
```python
p = [value]
g = [value]
A = [value]
a = discrete_log(p, A, g)
```

This can be used if the prime used, $$p$$, is very smooth, that is, $$p-1$$ has a lot of factors. If $$p-1$$ is smooth, DLP is said to be easy to solve. This can also be used in ECC
