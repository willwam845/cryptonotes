---
description: your first step to becoming a pure non rsactftool user!
---

# Background + Basics

## Background

When using RSA, you need to use modular exponentiation, like with a lot of cryptography. This means that you multiply a number by itself a number of times, and then take it modulo another number. Modulo is simply the act of repeatedly subtracting a number from another until the number you are subtracting from is less than the subtracted number. 

For example: $$3^{5} \mod 17 = 243 \mod 17 \equiv 5$$.

The bottom number \(in this case the 3\) is referred to as the base, the top number \(the 5\) is referred to as the exponent \(often referred to as e\), and the number on the right \(the 17\) is the modulus \(often referred to as n\).

In the case of RSA, the modulus is made up of 2 primes, which are often called p and q.

## Basics

RSA is a public-key cryptosystem, so there is a public key and a private key. Our public key is made up of our modulus \(which is 2 primes multiplied by each other\), and our exponent.

To encrypt a message,  a sender converts their message to an integer \(ensuring that it is less than n\), then they raise it to the power of e, and finally they take it mod n.

In other words: 

$$
c \equiv m^{e} \mod n
$$

To decrypt the message, the knowledge of the two primes that make up n need to be known, in order to work out the Euler totient of n, which is calculated \(in the case of there being two unique primes\) by:

$$
\phi(n) = (p-1) * (q-1)
$$

We then take the modular multiplicative inverse of e mod the totient we calculated.

The modular multiplicative inverse is, in the finite field of integers from 1 to p, for every element g in that field, there exists a unique integer d, where:

$$
g * d \equiv 1 \mod p
$$

We can work this out very easily in python with something like:

```python
d = pow(g,-1,p)
```

Finally, we take our encrypted message \(c or ct\), and take this to the power of our calculated d modulo n. In other words:

$$
pt = c^{d} \mod n
$$

Remember when I said there was a public key **and** and private key? Well, our private key is made up of n and d. There are ways to factor n given d, the easiest being letting other people's packages do all the work for you:

```python
from Crypto.PublicKey import RSA
k = RSA.construct((n, e, d))
print(k.p, k.q)
```

So why is RSA reasonably secure? Well, this is because, if implemented properly, the quickest way to decrypt a message without knowledge of the private key or the primes is to factor n into its prime factors, which you can then calculate d from. However, factorising numbers takes a lot of time, way more than compared to the reverse, which is simply multiplying the two primes together to get the modulus in the first place.

Of course, if RSA is implemented incorrectly, there are attacks you can do on it.

## Summary

* RSA has a public key and a private key
* The modulus is \(usually\) made up of 2 large prime numbers and is referred to as n, the exponent is referred to as e.
* When encrypting, a message is taken to the power of e and then taken mod n. `pow(m,e,n)`
* When decrypting, Euler's totient is taken, and then the multiplicative inverse is taken of $$e\mod\phi(n) $$ which results in the private exponent d. The encrypted message is taken to the power of d, and then taken mod n. `pow(c,d,n)`

