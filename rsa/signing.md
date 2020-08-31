---
description: 'haha, rsactftool cannot brrr now can it'
---

# Signing

## dp and dq?

$$d_p$$ and $$d_q$$ are often notation for a different way of decrypting RSA messages.

$$
d_p = d \mod (p-1)
$$

and

$$
d_q = d \mod (q-1)
$$

meaning that the message can be calculated by doing

$$
m_1 = c^{d_p} \mod p
\newline m_2 = c^{d_q} \mod q
\newline h = q_{inv}(m_1 - m_2) \mod p
\newline m = m_2 + hq \mod n
$$

There is also a way to recover $$m$$ just given one of these. 

Since:

$$
e * d_p \equiv 1 \mod p
$$

If we take any integer $$x$$, and multiply it by $$e * d_p$$, we get

$$
x  * e * d_p = x \mod p
$$

We can then subtract $$x$$ from both sides to get

$$
(x * e * d_p) - x = 0 \mod p
$$

meaning that $$(x * e * d_p) - x $$ is a multiple of $$p$$, and therefore meaning we can take the GCD of our modulus$$n $$ and $$(x * e * d_p) - x $$ to get our prime $$p$$.

Implementation in Python

```python
from Crypto.Util.number import long_to_bytes
from math import gcd

n = [value]
e = [value]
c = [value]
dp = [value]

p = gcd((2*e*dp)-2,n)
q = n//p
tot = (p-1) * (q-1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```



## Null byte padding

Sometimes, people will use null bytes to pad their message before encrypting it. This can be broken if the exponent is small enough.

```python

```



