# On Σ-protocols (2010, Damgard)

## Introduction
* Σ-protocol if the following 3 properties: 1. 3-move form (commit a, challenge e, response z); 2. special soundness (can efficiently compute witness w from two pairs (a, e, z) and (a, e', z') given that e != e'); 3. special honest-verifier ZK (existence of simulator M that inputs statement x and e and outputs a and z such that the probability distribution of accepting conversation (a, e, z) is the same as that of conversations between honest P and honest V on input statement x)
* Exercise 3 shows special honest-verifier ZK (SHVZK) is implied by HVZK?
* Lemma: The properties of Σ-protocol are invariant under parallel composition
* Lemma: If a Σ-protocol for relation R exists, then there exists a Σ-protocol with challenge length t for any t
* Theorem: Σ-protocol => PoK (proof of knowledge)
* OR-proof: proving that a prover knows w such that either (x, w) ∈ R or (y, w) ∈ R without revealing which is the case
* ![OR](/images/sigma_OR.png)
* OR-proof is witness indistinguishable and witness hiding

## Examples
* ![Examples](/images/sigma_examples.png)
* Can prevent man-in-the-middle attack during identification scheme: ![Prevent](/images/sigma_prevent.png)
* ![ZK](/images/sigma_zk.png)
* ![Commitment](/images/sigma_commitment.png)

## Additional Exercises
* SHVZK is when input e to simulator is V's challenge whereas HVZK is when the simulator doesn't take e as input?; HVZK => SHVZK
* A la Pedersen: ![Exercise](/images/sigma_exercise.png)
* There are q different pairs (w1, w2) for above protocol; witness indistinguishable and witness hiding