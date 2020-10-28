---
description: xd nonce haha funny
---

# Nonces (in ECDSA)

## No, not the child predator

In ECDSA, there is a randomly generated value each time, which we refer to as a `nonce`. As said before, this value needs to be:

- not leaked
- not reused
- the same bit length or very close to the same bit length as the prime $$p$$

Well, what happens if any of these things happen to the nonce?

We are going to use the NIST 192 bit curve, with parameters:

```
p = 6277101735386680763835789423207666416083908700390324961279
a = -3
b = 2455155546008943817740293915197451784769108058161191238065
G.x = 602046282375688656758213480587526111916698976636884684818
G.y = 174050332293622031404857552280219410364023488927386650641
```

We then generate our public key, by picking a random integer as our key.

```python
import random

p = 6277101735386680763835789423207666416083908700390324961279
a = -3
b = 2455155546008943817740293915197451784769108058161191238065
E = EllipticCurve(GF(p), [a, b])
Gx = 602046282375688656758213480587526111916698976636884684818
Gy = 174050332293622031404857552280219410364023488927386650641
G = E(Gx,Gy)

d = random.randint(1,G.order())
print(G*d)
```

```
(2361382083859056380587047599129413763762232025488345548260 : 4888304951668242463546470986388036917603657536259664410787 : 1)
```

So, now we are ready to break ECDSA.

## Leaking the nonce

If the nonce is leaked, we are able to work out the private key.

Given:
```
p = prime
G = generator
d = private key
k = nonce
e = hash of message
```

$$
r = G * k\\
s = k^{-1}(e+rd) \mod p
$$

If we know the nonce $$k$$, we can multiply the second equation (the equation for $$s$$) to get:

$$
s * k = k*k^{-1}(e+rd) \mod p
$$

and since $$k * k^{-1} \equiv 1$$, we then know $$(e+rd)$$. We can then just subtract $$e$$ from this, and then divide by $$r$$ (by modular inverting and multiplying, of course).

This then allows us to recover $$d$$, and then allow us to sign our own messages.

## Reusing a nonce

Nonce reuse can also lead to the private key being recovered, and is also easily spotted, due to two signatures having the same $$r$$ value.

Take the equations:

$$
r = k * G\\
s_1 = k^{-1}(e_1+rd)\\
s_2 = k^{-1}(e_2+rd)
$$

Then, if we take $$s_2$$ away from $$s_1$$, we get

$$
s_1 - s_2 = k^{-1}(e_1+rd) - k^{-1}(e_2+rd)\\
s_1 - s_2 = k^{-1}((e_1+rd)-(e_2+rd))\\
s_1 - s_2 = k^{-1}(e_1+rd-e_2-rd)\\
s_1 - s_2 = k^{-1}(e_1-e_2)\\
k(s_1 - s_2) = k*k^{-1}(e_1-e_2)\\
k(s_1 - s_2) = (e_1-e_2)\\
$$

Since we know all the values except $$k$$, it is now trivial to work it out using modular maths.

$$
k = (e_1-e_2)/(s_1-s_2)
$$

From here, we used the "Leaked Nonce" method to recover the private key like before.
