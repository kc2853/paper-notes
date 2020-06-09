# Voting with unconditional privacy: CFSY for booth voting (2009, Graaf)

## Introduction
* Unconditional privacy is important b/c imagine a future dictator can come after you if the cryptography is safe now but becomes broken in (say) 5 years
* Can't have unconditional privacy and correctness at the same time

## Protocol
* Pedersen commitment (where r is the randomness while x is a vector of binary values): ![CFSY booth voting_Pedersen](/images/CFSY%20booth%20voting_Pedersen.png)
* Note that Pedersen commitments are homomorphic
* Voter -> Voting Machine (VM) -> Tallying Authority (TA)
* ![CFSY booth voting_tally](/images/CFSY%20booth%20voting_tally.png)
* Individual Voter Verifiability (compare with published h(i)) and Universal Verifiability (check if overall was calculated correctly)
* Assume VM and TA have a private channel

## Considerations
* Ways to mitigate the private channel assumption (between VM and TA) and trusting TA assumption: both located on the same server, OTP, homomorphic encryption (details not clear), Bos' Dining Cryptographer's net, verifiable secret sharing among multiple TAs
* Mitigating trust on VM: voter issues a random challenge to VM (a la Adida and Neff)
* Other works: Moran and Naor, SplitBallot, MarkPledge, Scratch & Vote
* This work emphasizes that simplicity is of utmost importance

## Questions/Comments
* Recall that a commitment scheme must have the two properties: hiding and binding
* Unconditional privacy ~= perfect hiding
* Computational correctness ~= computational binding
* The biggest problem: seems bad that Tallying Authority gets to know each ballot exactly