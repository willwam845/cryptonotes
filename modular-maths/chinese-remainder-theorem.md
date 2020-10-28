---
description: everything is made in china
---

# Chinese Remainder Theorem

The Chinese Remainder can be used to solve a set of equations where you are given:

$$
n \mod 2\\
n \mod 3\\
$$

where the moduluses are primes, and $$n$$ is a number.

The Chinese Remainder Theorem is useful in many aspects of cryptography, such as Hastad's Broadcast attack in RSA.

Sage has a handy implementation once again (thanks sage)

```python
residues = [1,3,2,5,3,5]
primes = [2,3,5,7,11,13]
print(CRT(residues,primes))
```
