---
description: i have ran out of jokes
---

# Modular Square Roots

Say we have some prime $$p$$. In this example, I am going to use 37 as our prime.

Now, to calculate the square of a number $$\mod p$$, this is fairly simple, we just square the number, and then modulo it by $$p$$, i.e. subtract $$p$$ from the square until it becomes less than $$p$$. As an example:

$$

\begin{align*}
7^{2} = 49 \\
7^{2} = 49 - 37 \\
7^{2} = 12 \mod 37
\end{align*}
$$

Two things to note:
- If we were to square the negative of our number, we would get the same result. So $$-7 \mod = p - 7 = 30$$, and therefore $$30^{2} = 7^{2}$$
- Not every number has a square root.

If a number has a square root, this means it is a quadratic residue, and if not, it is called a quadratic non-residue.

An interesting property about quadratic and non-quadratic residues is that they follow the following rules:

```
quadratic residue * quadratic residue = quadratic residue
quadratic residue * quadratic non-residue = quadratic non-residue
quadratic non-residue * quadratic non-residue = quadratic residue
```

## How do we take one?

To take the modular square root, we have to look at the prime $$p$$. If $$p \equiv 3 \mod 4$$, we can take

$$
a^{(p+1)/4} \mod p
$$

however if $$p \equiv 1 \mod 4$$, then we have to use the Tonelli-Shanks method.

```python
def find_N_K(num):
  n=0
  while not(num&1):
    n+=1
    num>>=1
  return n,num  

def find_quad_nonresidue(p):
  for i in range(2,p):
    if pow(i,((p-1)//2),p)==p-1: # -1
      return i


def find_I(r,p):
  i=0
  while 1:
    power=pow(2,i)
    if pow(r,power,p)==1:
      break
    i+=1  
  return i    

def tonneli_shanks(a,p):
  n,k=find_N_K(p-1)
  q=find_quad_nonresidue(p)
  t=pow(a,((k+1)//2),p)
  r=pow(a,k,p)
  while 1:
    i=find_I(r,p)
    if i==0:
      return t%p , (-t)%p
    b=pow(2,n-i-1)  
    u=pow(q,k*b,p)
    t*=u
    r*=pow(u,2)
```

Alternatively, we can just use sage's built in method. (praise sage)
```python
for c in Mod(a, p).nth_root(1<<2, all=True):
  print(c)
```

which gets both mod roots.

## The Legendre Symbol

The legendre symbol can be used to tell whether a number $$a$$ in the field $$F_p$ is a quadratic residue. The legendre symbol can be calculated as follows:

$$
 a^{(p-1)/2} \mod p
$$

If the result is anything other than 1 or 0, this means that it is not a quadratic residue. If it is 1, that means that it is a quadratic residue.

The legendre symbol is often used in cryptosystems such as the Goldwasser-Micali cryptosystem.
