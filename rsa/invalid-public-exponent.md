---
description: and to think i was okay at crypto
---

# Invalid Public Exponent

When we use RSA encryption, we need to ensure that $$gcd(e, \phi(n)) = 1$$, otherwise the private exponent $$d$$ will not exist, since:

$$
ed \equiv 1 \mod \phi(n)\\
gcd(e, \phi(n)) \neq 1\\
$$

and therefore a modular inverse will not exist.

## Can we still decrypt the message?

The answer is "sort of". We are able to get $$e$$ number of candidates for $$pt$$, and therefore if we know anything about the plaintext (for example, flag format if it is a CTF challenge), then we can know which of the $$e$$ candidates is the message.

There is a nice stackoverflow post that explains this topic: https://crypto.stackexchange.com/questions/81949/how-to-compute-m-value-from-rsa-if-phin-is-not-relative-prime-with-the-e
