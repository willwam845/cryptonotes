---
description: very smart
---

# Smart's Attack on anomalous curves

## yeag

An anomalous curve is a curve where the order is equal to the prime $$p$$. This makes it vulnerable to an attack called Smart's Attack

The main idea is to move the problem from the elliptic curve $$E$$ to the field $$F_p$$.

```python
p = [whatever]
a = [whatever]
b = [whatever]

E = EllipticCurve(GF(p), [a, b])

G = E(whatever)
A = E(whatever)

def a0(P, Q):
    if P[2] == 0 or Q[2] == 0 or P == -Q:
        return 0
    if P == Q:
        a = P.curve().a4()
        return (3*P[0]^2+a)/(2*P[1])
    return (P[1]-Q[1])/(P[0]-Q[0])

def add_augmented(PP, QQ):
    (P, u), (Q, v) = PP, QQ
    return [P+Q, u + v + a0(P,Q)]

def scalar_mult(n, PP):
    t = n.nbits()
    TT = PP.copy()
    for i in range(1,t):
        bit = (n >> (t-i-1)) & 1
        TT = add_augmented(TT, TT)
        if bit == 1:
            TT = add_augmented(TT, PP)
    return TT

def solve_ecdlp(P,Q,p):
    R1, alpha = scalar_mult(p, [P,0])
    R2, beta  = scalar_mult(p, [Q,0])
    return ZZ(beta*alpha^(-1))

a = solve_ecdlp(G,A,p)
print(f'Private key: {a}')
```
