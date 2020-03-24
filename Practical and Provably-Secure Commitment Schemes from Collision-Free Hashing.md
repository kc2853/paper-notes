# Practical and Provably-Secure Commitment Schemes from Collision-Free Hashing (1996, Halevi and Micali)

## Introduction
* Need two properties for commitment schemes: hiding (Receiver cannot figure out Sender's message from commitment string) and binding (Sender cannot figure out two different decommitment strings for the same commitment string)
* Previous work by: Blum, Brassard/Crepeau, Goldwasser/Micali/Rivest, Halevi, Pedersen, Chaum, Naor, Naor/Ostrovki/Venkatesan/Yung
* This work uses CRHF combined with universal hash function in an efficient, non-interactive manner
* Complexity theoretically, this work allows 2 things: constant round statistical zero-knowledge arguments and constant-round computational zero-knowledge proofs for NP
* ![Comparison](/images/crhf_commitment.png)

## Protocol
* Let u be a universal hash function from L to n bits (need L >> n) such that u(x) = m
* x is randomly sampled
* m is the message (of length n) to be committed
* h is a CRHF from L to k bits
* Commitment string c = (u, h(x))
* Decommitment string d = x
* If m is of length n, then L should be O(n + k)
* Hence, the improvement is to commit h(m) (which is of length k) so that length of commitment string is O(k) rather than O(n + k)

## Notes
* Binding (easy): reduces to finding collision for CRHF
* Hiding (harder than it seems): proving it involves the notion of statistical distance and nontrivial algebra
* The intuition is that since h by itself may "leak" information about its input (such that the hiding property is not fully satisfied theoretically), we hash a "representation" of m (which is x) from a much larger space (L instead of n)
* Open problems: reducing assumptions (e.g. relaxing constraint to universal one-way functions); adding desirable homomorphism properties somehow