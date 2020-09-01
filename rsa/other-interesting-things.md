---
description: >-
  these don't really fall under any attacks but like they exist and i think they
  are nifty
---

# Other Interesting Things

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

A semi-common CTF challenge is that the person will use null bytes to pad their message before encrypting it. This can be broken if the exponent is small enough.

Let's look at a certain example:

```python
from Crypto.Util.number import bytes_to_long
from secret import flag

def pad(msg):
  return msg + (50-len(msg) * b"\x00")

n = [some value]
e = 3

msg = pad(flag)
ct = pow(bytes_to_long(msg),e,n)
print(ct)
```

If we look closely, by adding a null byte to the end of our message, we are simply adding the binary bits of `00000000` to the end, which is the equivalent of multiplying our message by $$2^{8}$$, or 256.

This means that we can express the encrypted message as:
$$
(m * 256^{(100 - len(message))})^{3}
$$

or simply

$$
m^{3} * 256^{(100 - len(message)) * 3}
$$

This means that we can work out $$m^{3}$$ quite easily.

We simply divide (so multiply by the inverse) to get m^{3}.

Then, we can simply attempt to bruteforce $$m$$, since $$m^{3} = kn + c$$, and so we can bruteforce values for k and then take a cube root.

Implementation in python (soon:tm:)
