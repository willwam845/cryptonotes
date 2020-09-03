---
description: what the heck is this
---

# Basics

## Basics

Diffe-Hellman is relatively similar in terms of the maths knowledge required, and it also uses modular exponentiation, however the main difference between the two is that while RSA is used for encryption of an actual message, Diffe-Hellman is a way to get a key on both sides, even with everything being made public. This "shared-key" or "shared-secret" as it is sometimes known, can then be used for encryption of a message with AES.

The process of using DHKE works as follows. Say Alice wants to send a message to Bob:

- Alice and Bob both generate large numbers, which they keep secret. Alice's number will be called $$a$$ and Bob's wll be called $$b$$
- They then agree on a "generator" $$g$$ and a prime $$p$$
- Alice works out $$g^{a} \mod p$$. This is called $$A$$. She then sends $$g$$, $$p$$ and $$A$$ over to Bob
- Bob then works out $$g^{b} \mod p$$. This is then called $$B$$. Bob then takes Alice's A, and then works out $$A^{b} \mod p$$. This is the shared secret. He then sends $$B$$ over to Alice
- Alice, after receiving Bob's $$B$$, will work out $$B^{a} \mod p$$. This will be exactly the same as Bob's shared secret.
- Finally, Alice takes her message, and then encrypts it with, say, the sha1 hash of the shared secret using AES. She then sends the encrypted message over to Bob
- Bob can now decrypt the message using the shared secret that they both have.

Even if an attacker can intercept $$g$$, $$p$$, $$A$$ and $$B$$, it is very difficult to work out either of $$a$$ or $$b$$, since this would be solving the Discrete Log Problem, often abbreviated to DLP.

Like RSA, if this is implemented incorrectly, it can lead to the DLP being easily solved however. I'll go over some attacks.
