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

Take the set of parameters

p = 292065371966288667389477330182200680503
a = 2
b = 3

First, we get our generator point and check its order, also checking if it factorizes:
```python
p = 292065371966288667389477330182200680503
a = 2
b = 3

E = EllipticCurve(GF(p), [a,b])
G = E.gens()[0]
print(G)
print(G.order())
print(factor(G.order()))
```

Output:
```
(204366579416802079649956538515698682695 : 110086697047375768357527710448004845440 : 1)
292065371966288667386303556576295133830
2 * 5 * 7 * 11 * 1367687 * 15437518133 * 17964914065182788849
```

Notice how there are a couple of small factors (which should be easy to solve ECDLP in) and a couple of large factors (where it is infeasible).

If we have a maximum value of the private key, we can simply use Pohlig-Hellman to compute ECDLP in some of the subgroups, and then use the Chinese Remainder Theorem to get a value for the private key (or get the private key mod some value, and if the value it is being modulo'ed by is small enough, you can keep adding the modulus to the value until you either hit the max value, or find the actual private key).


```python
p = 292065371966288667389477330182200680503
a = 2
b = 3

E = EllipticCurve(GF(p), [a,b])
G = E.gens()[0]

A = G * 12345678997654
order = G.order()
max_val = 1000000000000000
subresults = []
factors = []
modulus = 1
for prime, exponent in factor(order):
    if modulus >= max_val: break
    _factor = prime ** exponent
    factors.append(_factor)
    G2 = G*(order//_factor)
    A2 = A*(order//_factor)
    subresults.append(discrete_log_lambda(A2, G2, (0,_factor), '+'))
    modulus *= _factor

n = crt(subresults,factors)
assert(n * G == A)
print("found private key:", n)
```

Output
```
found private key: 12345678997654
```

And we can see that we have recovered the private key.
