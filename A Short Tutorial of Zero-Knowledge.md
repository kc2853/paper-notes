# A Short Tutorial of Zero-Knowledge (2010, Goldreich)

## Introduction
* Completeness (truth shall be accepted), soundness (non-truth shall be accepted with negligible probability), zero-knowledge (can show via simulator technique that verifier ends up with no additional knowledge)
* Proof of knowledge: replace above soundness with knowledge soundness (defined using a knowledge extractor)
* Argument: assume prover P is computationally bounded (generally, proof assumes computationally unbounded P and computationally bounded V)

## Parameters to ZK
* Consideration of auxiliary inputs (can assume this although the original definition by GMR did not incorporate auxiliary inputs)
* Black-box (simulator uses V's code as a black-box) vs non-black-box simulation (seminal paper by Barak)
* Honest vs potentially malicious verifiers (there is a theorem saying every Arthur-Merlin protocol that is HVZK is also ZK)
* PZK (perfect) ⊂ SZK (statistical) ⊂ CZK (computational)
* Note that CZK = IP = PSPACE

## Compositions
* Sequential: ZK is preserved
* Parallel: generally not preserved; every NP-set has a constant-round parallel ZK proof
* ![Parallel](/images/zktutorial_parallel.png)
* Concurrent: 1. purely asynchronous model (every NP-set has a concurrent ZK proof with sub-logarithmic number of rounds under standard intractability assumptions); 2. asynchronous model with timing (each party runs protocols employing time-driven operations by holding a local clock whose rates are bounded by an a priori known constant)
* Currently unknown if every NP-set has a constant-round concurrent ZK proof?
* Barak has partial results for every NP-set (under standard intractability assumptions): 1. ZK argument (not proof though) with respect to non-uniform adversaries; 2. constant-round(!); 3. bounded (fixed polynomial number prior to the protocol however?) concurrent ZK; 4. Arthur-Merlin (public coin); 5. simulator runs in strict ppt (as opposed to expected)

## Other Notes (including those from elsewhere) 
* NP ⊂ CZK; NP ⊂ SZKarg (every NP-set has a SZK argument); unlikely that NP ⊂ SZK (for proofs as well)
* BPP ⊂ SZK ⊂ AM (Arthur-Merlin) ∩ coAM
* ![AM](/images/zktutorial_AM.png)
* AM := AM[2] = AM[k]; AM[poly(n)] = IP
* NIZK: basic (prove single assertion), unbounded (many assertions), adaptive (assertions are selected adaptively based on the common reference string and previous proofs), non-malleable (interesting mix of ZK and soundness conditions)
* Knowledge complexity: ZK = knowledge complexity zero; study of characterizing languages according to knowledge complexity of their interactive proof systems
* Resettable ZK (rZK, verifier can reset prover's random tape) vs resettably-sound ZK (rsZK, prover can reset verifier's random tape)
* Every NP-set has an argument (not proof though) system that is both rZK and rsZK
* MIP (multi-prover IP): e.g. two isolated slots of ATM
* Model involving strict computational soundness (timed-ZK): prover is computationally even more bounded
* Some factors to keep in mind: which assumptions are being made (standard number theory assumptions vs OWF); constant-round or how many rounds (round complexity); AM or not
* GNI (graph non-isomorphism): SZK(4) -- SZK with 4 rounds
* GI (graph isomorphism): SZK(5)
* NP ⊂ CZKarg(4) (assuming OWP) -- CZK argument with 4 rounds
* NP ⊂ CZK(5) (assuming 2-round perfectly hiding commitment scheme) -- CZK (proof) with 5 rounds
* NP ⊂ CZKPoK(5) (assuming 2-round perfectly hiding commitment scheme) -- CZK proof of knowledge with 5 rounds
* CZK(2) ⊂ BPP = bbCZK(3) -- CZK with 3 rounds using black-box simulation
* bbCZK(4) ⊂ coMA ∩ AM