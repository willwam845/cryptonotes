---
description: lattices are fun
---

# Public Exponent Choice Attacks

### e = 1

If e = 1, well, considering that $$n > c$$, and the message is encrypted by taking `pow(c,1,n)`, the mod will not be taken at all, meaning ct = pt.

```python
from Crypto.Util.number import long_to_bytes

n = [value]
e = [value]
c = [value]

print(long_to_bytes(c)) # c = pt
```

### High e attacks

There are two main attacks that can be used when e is very large. One is the weiner attack, the other is the boneh-durfee attack.



