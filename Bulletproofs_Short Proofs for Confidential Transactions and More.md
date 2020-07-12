# Bulletproofs: Short Proofs for Confidential Transactions and More (2017, Bunz et al.)

## Introduction
* Key differentiators: proof size is logarithmic in the witness size (2 log n + 9 for range proof, 2 log n + 13 for arithmetic circuit); no trusted setup (based on Fiat-Shamir); discrete log assumption
* Main method: inner product argument (such that range or arithmetic circuit relations are represented via inner product) + evaluating a vector polynomial at a random point
* Marginal time to verify 16 range proofs = that to verify 16 ECDSA signatures
* Improves previous inner product argument by Bootle (2016) by a factor of 3
* Applications: confidential transaction, Provisions (proving an exchange's solvency), verifiable shuffle, smart contracts, arithmetic circuit
* 160 GB (for Bitcoin range proofs) -> 17 GB; 18 GB (for exchange solvency) -> 62 MB
* MPC, aggregated proof, and batch verification possible
* Pedersen vector commitment: ![Pedersen](/images/bulletproofs_pedersen.png)

## Inner Product Argument
* Trying to prove (can turn second proof into proving first): ![Inner](/images/bulletproofs_inner.png)
* ![Inner2](/images/bulletproofs_inner2.png)
* ![Recursion](/images/bulletproofs_recursion.png)
* Multi-exponentiation: delays performing exponentiations until the very end -> leads to performance improvement for the verifier (by roughly a factor of 2)
* See detailed protocol in paper

## Range Proof
* Trying to prove: ![Range](/images/bulletproofs_range.png)
* Range proof can be represented by one inner product (second, for random challenges y and z) from many (first) as follows: ![Range2](/images/bulletproofs_range2.png)
* Involves vector polynomials (that embed the above relation) and evaluating them at a random point: ![Range3](/images/bulletproofs_range3.png)
* Verifier checks (can be improved by multi-exponentiation and batch verification): ![Range4](/images/bulletproofs_range4.png)
* Inner product argument (hence the meat of optimization) is used in the last verification steps above (so the prover doesn't send bolded l and bolded r in raw to begin with)
* Proof aggregation: additive(!) term of 2 log m (where m is the number of proofs to be aggregated)
* MPC possible: constant round + linear communication OR logarithmic round + logarithmic communication
* For quantum adversary, can use ElGamal commitment (Pedersen + commitment to the random value used in Pedersen) such that perfectly binding?!
* See detailed protocol in paper

## Arithmetic Circuits
* Trying to prove: ![Circuit](/images/bulletproofs_circuit.png)
* Quite remarkable this can be represented by inner product as well!
* So anything that can be represented by inner product relation can be proved by Bulletproofs?!
* More variables involved than range proof, e.g. t(X) becomes a degree 6 polynomial instead of degree 2
* See detailed protocol in paper