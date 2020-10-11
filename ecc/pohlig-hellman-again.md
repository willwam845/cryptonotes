---
description: wow again what a shocker
---

# Pohlig-Hellman for ECC

## How?

For ECC, it is important that the generator of the curve, G, has a prime order.

But what happens if it isn't?

## Example

Let's take the curve (in short Weisertrass form $$y^2 = x^3 + ax + b$$ over GF(p)) with parameters

p = 310717010502520989590157367261876774703
a = 2
b = 3

We will get our generator point and print the order of it, and also try to factor the order:

```python
p = 244050855603586871551581993535730222549
a = 2
b = 3

E = EllipticCurve(GF(p), [a,b])
G = E.gens()[0]
print(G.order())
print(factor(G.order()))
```

Output:

```
244050855603586871532678563418191251452
2^2 * 3 * 3853 * 4943 * 87559 * 310867 * 343411 * 407401 * 280412753
```

Notice how the order is comprised of lots of smaller factors? This means that we only need to solve the ECDLP in the finite fields of all those factors. We can use the Pohlig-Hellman algorithm for this, and luckily Sage has this built in.

```python
p = 244050855603586871551581993535730222549
a = 2
b = 3

E = EllipticCurve(GF(p), [a,b])
G = E.gens()[0]
A = G * 96712717352630003311564495261808282857
a = G.discrete_log(A)
print(f'Privkey found: {a}')
```

Output

```
Privkey found: 96712717352630003311564495261808282857
```

And we can see that our private key has been recovered.

## Pollards Rho

If a maximum value is known for the private key, and the order is not prime, it may be possible to recover the private key.
