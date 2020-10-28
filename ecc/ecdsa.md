---
description: oh boy i sure love nonces
---

# ECDSA

## Signing?

To sign a message using elliptic curves, we can use the Elliptic Curve Digital Signature Algorithm.

The algorithm works like this:

We first, generate a random nonce $$k$$. It is very important that this nonce is:

- not leaked
- not reused
- the same bit length or very close to the same bit length as the prime $$p$$

Firstly, we compute $$r$$.

$$
r = k * G
$$

where G is the generator point of the curve

Then, using this nonce, we work out the second part of the signature, $$s$$

We use $$m$$ as the message (usually hash of the message) being signed, $$d$$ as the private key, and $$n$$ as the order of $$g$$

We work out $$s$$ by

$$
s = k^{-1} * (m + rd) \mod n
$$

The signature is then `(r,s)`, and is given.

## To verify?
