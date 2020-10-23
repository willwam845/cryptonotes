---
description: what even are they tbh
---

# Coppersmith Attacks

## Basic Attacks

### Hastad's Broadcast Attack

Hastad's Broadcast Attack, named after Johan Hastad, is an attack used when you have the same message encrypted with a number of different public keys and a low public exponent. It is most commonly used with an e of 3 and 3 different public keys.

The main idea behind it \(using the example of $$e = 3$$ and three messages\), is:

Let the encrypted messages be $$m_1, m_2 $$ and $$m_3$$ , and let the public keys be $$n_1,n_2, n_3$$. Let the common message be $$M$$.

We know that $$m_i = M^{3} \mod n_i $$, and we have three of these. We can use the Chinese Remainder Theorem to work out $$M^{3} \mod n_1*n_2*n_3$$

We can then simply take the integer root of $$M^{3} $$ in order to get $$M$$.

Implementation in Sage:

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import iroot

n1 = [value]
n2 = [value]
n3 = [value]
c1 = [value]
c2 = [value]
c3 = [value]
e = 3

ns = [n1,n2,n3]
cs = [c1,c2,c3]
mcubed = CRT(cs, ns)
m = iroot(mcubed, 3)
print(long_to_bytes(m))
```

### Franklin Reiter Related Message Attack

Franklin Reiter's related message attack allows us to work out, given two messages encrypted with the same public key, and are linearly padded through a polynomial, usually of the form in CTF's as $$ax + b$$ , although it works with any polynomial.

If we take the message as $$M$$ , the modulus as $$n$$ , $$e = 3$$ , our padding variables $$a_1,a_2,b_1, b_2$$ , and our ciphertexts $$c_1 , c_2$$, then:

$$
c_i = (a_i * x + b_i)^{3} \mod n
$$

which we can rearrange to form:

$$
p_i(x) \equiv  (a_ix+b)^{3} - c_i
$$

Since we have two of these, and they both share a root of $$x$$, therefore they share a factor of $$x - M$$, and so we can compute the GCD of them to recover $$M$$

Implementation in Sage:

```python
from Crypto.Util.number import long_to_bytes

e = 3
n = [value]
c1, c2 = [values]
a1, a2 = [values]
b1, b2 = [values]

def gcd(a,b):
    while b:
        a, b = b, a % b
    return a.monic()

P.<x> = PolynomialRing(Zmod(n))
p1 = (a1 * x + b1) ^ e - c1
p2 = (a2 * x + b2) ^ e - c2
result = -gcd(p1, p2).coefficients()[0]
print(long_to_bytes(result))
```

## Variations

### Linear Padding

We can also use Hastads if the messages are only padded with a linear polynomial, as proven by Coppersmith.

Let's take an example where they are padded with $$m = aM + b$$, and $$a$$ and $$b$$ are both given to us. $$e = 3 $$ and we have 5 messages.

We start by taking the Chinese Remainder Theorem to compute a list of $$T_i$$ where:

$$
T_i \equiv 1 \mod n_i
$$

and



$$
T_i \equiv 0 \mod{n_{j \neq i}}
$$

We then, for each $$T_i$$ , take the polynomial:

$$
g(x) = T_i * ((a_i * x + b_i)^{5} - c
$$

We then sum these polynomials, and note that for all $$n_i$$ ,  $$\sum g(x) \equiv 0 \mod n_i$$

Finally, we just use Coppersmith's method to get $$M$$.

Implementation in Sage:

```python
from Crypto.Util.number

ns = [n1,n2,n3,n4,n5]
cs = [c1,c2,c3,c4,c5]
bs = [b1,b2,b3,b4,b5]
ass = [a1,a2,a3,a4,a5]
P.<x> = PolynomialRing(Zmod(prod(ns)))

ts = [crt([int(i == j) for j in range(5)], ns) for i in range(5)]
gs = [(ts[i] * ((ass[i] * x + bs[i])**5 - cs[i])) for i in range(5)]
g = sum(gs)
g = g.monic()
roots = g.small_roots()
print([long_to_bytes(root) for root in roots])
```

### Short Pad Attack

i really dont know how to explain it but basically you use coppersmith method to get difference between two plaintexts and then do franklin reiter i think

Implementation in Sage:

```python
e = 3
n = [value]
c1 = [value]
c2 = [value]
PRxy.<x,y> = PolynomialRing(Zmod(n))
PRx.<xn> = PolynomialRing(Zmod(n))
PRZZ.<xz,yz> = PolynomialRing(Zmod(n))

g1 = x^e - c1
g2 = (x + y)^e - c2
q1 = g1.change_ring(PRZZ)
q2 = g2.change_ring(PRZZ)

h = q2.resultant(q1)
h = h.univariate_polynomial()
h = h.change_ring(PRx).subs(y=xn)
h = h.monic()

kbits = [value]
roots = h.small_roots(X=2 ** kbits, beta=0.3)
print(roots)

diff = roots[0]
if diff > 2**32:
    diff = -diff
    C1, C2 = C2, C1

def gcd(a,b):
    while b:
        a, b = b, a % b
    return a.monic()

P.<x> = PolynomialRing(Zmod(n))
p1 = x ^ e - c1
p2 = (x + diff) ^ e - c2
result = -gcd(p1, p2).coefficients()[0]
print(long_to_bytes(result))
```
