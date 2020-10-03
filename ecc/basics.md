---
description: god i hate curves
---

# Basics

## Basics

Elliptic curve cryptography, often abbreviated to ECC, is a type of cryptography involving elliptic curves. There are many types of curves ECC can be done on, but most commonly they are done on curves of the Weisetrass form, and more specifically, you will mostly see them in the Short Weisetrass form, which is

$$
y^{2} = x^{3} + ax + b
$$

which is over a finite field $$GF(p)$$, $$p$$ being a prime.

We also need to make sure, for the parameters $$a$$ and $$b$$:

$$
4a^{3} + 27b^{2} \neq 0
$$

to ensure there are no singularities on the curve. If there are singularities, firstly, it is not an elliptic curve, but also, if someone has simply taken the point addition formulas and used them, this can be exploited.

As with RSA and Diffe-Hellman, there is an underlying trapdoor function which makes ECC hard to break. This is point multiplication. If we take two points on an elliptic curve, we can "add" them together to get a third point.

Point addition on elliptic curves have the following properties:

1. P + 0 = 0 + P = P
2. P + (-P) = 0
3. (P + Q) + R = P + (Q + R)
4. P + Q = Q + P
