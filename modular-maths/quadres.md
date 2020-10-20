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
