---
description: factordb go brrrr
---

# Factoring attacks on RSA

This is really just a dump of some factoring methods, not really any mathsy attacks being used.

### Low N

If the modulus N is very low \(128 bits or below takes a couple of seconds\), then the modulus can be easily factorised to get primes p and q, and then get d

```python
from Crypto.Util.number import long_to_bytes
from factordb.factordb import FactorDB as fdb
from math import prod

n = [value]
e = [value]
c = [value]

f = fdb(N)
f.connect()
f = f.get_factor_list()
tot = prod(p-1 for p in f.get_factor_list())
d =pow(e, -1, tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

Alternatively, if the factors aren't on FactorDB and the modulus is relatively small, Sage has a built in factor\(\) function.

```python
from Crypto.Util.number import long_to_bytes

n = [value]
e = [value]
c = [value]

f = str(factor(n)).split(" * ")
f = [int(x) for x in f]
tot = prod(p-1 for p in f)
d = pow(e, -1, tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

### Single prime

Sometimes, people may only use one prime to make up the modulus. This means that the totient becomes $$\phi(n) = (n - 1)$$. The rest is trivial.

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import is_prime

n = [value]
e = [value]
c = [value]

assert n.is_prime
tot = n - 1
d = pow(e, -1, tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

### Square N \(or really any N where $$n = x^{y} $$ \)

The totient becomes $$\phi(n) = p * (p - 1) $$ . Again, rest is trivial after that.

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import iroot

n = [value]
e = [value]
c = [value]

p = iroot(n, 2) # in the case that n = p ^ 2
tot = p * (p-1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

### Multiple primes

When N is made up of more than 2 primes, the totient is calculated differently again. Often when there are multiple primes, the factors are very small, so it can be factored with Sage or FactorDB.

```python
from Crypto.Util.number import long_to_bytes

n = [value]
e = [value]
c = [value]

f = str(factor(n)).split(" * ") # exact same script as before
f = [int(x) for x in f]
tot = prod(p-1 for p in f)
d = pow(e, -1, tot)
pt = pow(c,d,n)
print(long_to_bytes(pt)) 
```

### Root attack

When N is many many times greater than c, and the public exponent is quite small, sometimes a root attack can be used, as when you do `pow(c,e,n)`

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import iroot

n = [value]
e = [value]
c = [value]

pt = int(iroot(c,e))
print(long_to_bytes(pt))
```

### Fermat Factoring

If the two primes are close together, then you can use Fermat's method of infinite descent. \(You would probably just have to try this and see if it worked.\)

```python
from Crypto.Util.number import long_to_bytes
from gmpy2 import isqrt, square

n = [value]
e = [value]
c = [value]

def fermatfactor(n):
    assert n % 2 != 0
    a = isqrt(n)
    b2 = square(a) - n
    while not is_square(b2):
        a += 1
        b2 = square(a) - n
    p = a + isqrt(b2)
    q = a - isqrt(b2)
    return int(p), int(q)
    
p,q = fermatfactor(n)
tot = (p-1) * (q-1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

### Mersenne Primes

Named after French mathematician Marin Mersenne, these are special primes which are all of the form $$2^{x} - 1$$. Since they are so rare, it is very easy to just go through them and see if one is a factor.

```python
from Crypto.Util.number import long_to_bytes

n = [value]
e = [value]
c = [value]

i_s = [2,3,5,7,13,17,19,31,61,89,107,127,521,607,1279,2203,2281,3217,4253,4423]

for i in i_s:
  p = (2**i) - 1
  if n % p == 0:
    break
q = n // p
tot = (p-1) * (q-1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

### ROCA

It kinda just.. exists... 

[https://keychest.net/roca](https://keychest.net/roca) to check if vulnerable, and

[https://github.com/brunoproduit/roca](https://github.com/brunoproduit/roca) for implementation \(too long for here\)

### GCD attack with multiple keys

If you happen to have a lot of keys, there's a chance that a pair of them will share a prime. This can be worked out quickly with Euler's method.

```python
from Crypto.Util.number import long_to_bytes
from math import gcd

n1 = [value]
n2 = [value] # pretend just 2 keys for now
e = [value]
c = [value]
p = gcd(n1,n2)
q = n // p
tot = (p-1) * (q-1)
d = pow(e,-1,tot)
pt = pow(c,d,n)
print(long_to_bytes(pt))
```

