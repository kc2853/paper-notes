# Noninteractive Zero-Knowledge (1991, Blum et al.)

## Notes
* Shared random string = CRS model
* Random beacon: a bit different b/c future randomness is unpredictable
* NIZK similar to AM[2] b/c verifier needs to send a single message (i.e. CRS) first
* In fact, Bounded-NIZK = AM[2] under QRA (quadratic residuosity assumption)
* Bounded-NIZK: use one CRS per statement and proof
* NIZK (without Bounded): can use one CRS for multiple statements; same as adaptively-secure NIZK (or stronger)?
* ![Definition](/images/nizk_def.png)
* Bounded NIZK is closed under parallel composition (which is a true property as long as verifier can't cheat)
* ∃ Bounded NIZK for 3SAT (hence all of NP)
* ∃ NIZK for 3SAT (hence all of NP)
* ![3SAT](/images/nizk_3sat.png)
* Two main open problems that became solved: many provers can share the same CRS; NIZK can exist with just the assumption of OWP (though the prover needs exponential computing power in that case)