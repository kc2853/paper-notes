# Can Two Walk Together: Privacy Enhancing Methods and Preventing Tracking of Users (2020, Naor and Vexler)

## Introduction
* Situation 1: Alice uses LDP (local differential privacy) with a fresh set of randomness drawn every time -> problem b/c then one can just take the "average" and achieve an estimate of Alice's value
* Situation 2: Alice fixes the randomness for her LDP but reports the value multiple times from her work IP and home IP -> problem b/c then one can "link" Alice's work IP with home IP and thus gain more information than theoretically desirable
* Question: now how do we improve upon Situation 2?

## Stateful Mechanisms and Tracking
* Idea (simplified): have another set of randomness (i.e. notion called state) that fluctuates around the fixed LDP from Situation 2
* ![State](/images/untrackable_state.png)
* Framework for trackability (a la linkability): can define the trackable parameter as the log of the maximum ratio between the probability that a set of reports originated from a single user and the probability that the same set of reports originated from two users
* ![Definition of untrackable](/images/untrackable_definition.png)
* To be continued