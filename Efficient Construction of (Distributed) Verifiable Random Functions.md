# Introduction
* Combining PRFs and digital signatures
* PRF could be made by: one way functions (but inefficient); smaller PRFs to "extend" to bigger ones; based on decisional Diffie-Hellman (DDH) assumption
* Application: lottery where participant sends input x and organizer evaluates the VRF
* Itself is an unforgeable signature scheme
* DVRF (distributed VRF): what if the organizer (in the above lottery example) is compromised?; motivates the construction of DVRF where we have some threshold number of servers cooperate to return the value
* Goal of DVRF: non-interactive; round-efficient
* sf-DDH (sum free DDH) instead of DDH
* Naor-Reingold (NR) construction of (first?) DPRF: ![1](/images/dodisdvrf_1.png)
* Based on above: ![2](/images/dodisdvrf_2.png)
* C is an injective encoding whose output is L-bits (notably different from l-bits)
* Main result: first non-interactive DVRF (albeit multi-round)

# Notes
* VRF's definition includes pseudorandomness (i.e. no value of the function can be distinguished from a random string): ![3](/images/dodisdvrf_3.png)
* VUF (verifiable unpredictable function): pseudorandomness is replaced by unpredictability: ![4](/images/dodisdvrf_4.png)
* DDH (decisional) and its variants have the potential of yielding a (verifiable) pseudorandom function whereas CDH (computational) and its variants have the potential of yielding a (verifiable) unpredictable function
* sf-DDH (main idea: L-dimensional version of DDH with an additional constraint; generalized DDH, i.e. multi-dimensional DDH, could be false but sf-DDH still true, hence VRF possible): ![5](/images/dodisdvrf_5.png)
* Examples of the encoding function broached in the paper: C(x) = (x^3 || x); C(x) = (x^3 || x || 1 || x || 1) -- the second one is used for the VRF construction
* ![6](/images/dodisdvrf_6.png)
* For the distributed part of DVRF: use Shamir's secret sharing 