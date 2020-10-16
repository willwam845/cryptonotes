---
description: wow theres like so many of these
---

# Another value?

## What to do with it?

So, you have an RSA challenge, and it's giving you another random value, which is probably going to help you break RSA.

Here's just a dump of some possible values and how you can use them to break RSA.

### yeah just

If you have the difference between the two primes used, you can recover them.

Let $$n$$ be the modulus, and $$o$$ be the difference between them.

We say that the larger prime is $$p$$, and the smaller prime is $$q$$. Then we have

$$
p = q + o
$$

and since $$p * q = n$$, we can substitute in to get

$$
q(q+o) = n
q^2 + oq = n
q^2 + oq - n = 0
$$

We then solve this quadratic equation in order to get $$q$$.

Python implementation:

```python
from gmpy2 import iroot

n =
k =

r = k**2 - (4 * 1 * -n)
r = int(iroot(r,2)[0])
p = (r - k)//2
q = (r + k)//2
print(f'Factors: {p}, {q}')
```

### what

If we know the sum of the two primes, we can also break RSA. We could use a method similar to above, but we can also approach it another way.

Consider a modulus $$n$$, with factors $$p$$ and $$q$$. Our totient is then $$(p*1) * (q-1)$$, which expands to

$$
pq - p - q - 1
$$

We know that $$pq = n$$, so to get the totient, we just subtract the sum plus 1 from n. From here, we have everything we need to get the private key $$d$$

```python
n =
k =
e =
c =

tot = n - (k + 1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
```
