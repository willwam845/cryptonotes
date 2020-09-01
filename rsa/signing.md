---
description: 'haha, rsactftool cannot brrr now can it'
---

# Signing

## How?

If we want to verify the integrity of a message, that is, make sure the person actually wrote it, we can use signing to verify that they actually wrote it.

Say Alice wants to send a message to Bob. Bob wants Alice to sign her message so that he can make sure she wrote it. The process would look something like this:

- Alice encrypts her message with Bob's **public** key.
- She then takes the hash of her message (using an agreed on hashing algorithm), and encrypts it with her own **private key**.
- She then sends the message along with the signature to Bob.
- Bob then decrypts Alice's message with his **private** key.
- Bob then encrypts the signature with Alice's **public** key.
- He then hashes the decrypted message and compares it to the "encrypted" signature. If they match, it means that the signature is valid.
