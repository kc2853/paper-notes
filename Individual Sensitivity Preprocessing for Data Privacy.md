# Individual Sensitivity Preprocessing for Data Privacy (2019, Cummings and Durfee)

## Introduction
* Traditional differential privacy (DP) is too focused on worst-case
* What if average-case (e.g. for the median query) is much better than worst-case?
* There have been multiple approaches in the past: smooth sensitivity, Sample-and-Aggregate, Propose-Test-Release (PTR), Lipschitz extensions
* This work helps via preprocessing (meaning one can combine this method with other methods like smooth sensitivity)
* New notions of individual sensitivity (e.g. more specific than local sensitivity, let alone global sensitivity) and personalized differential privacy

## Sensitivity Preprocessing Function
* The main idea is to create an approximate (preprocessing) function of the original query f via query g such that g starts from the null set and starts expanding -> maintains the appropriate individual sensitivity bounds over iterations (until domain gets extended to the original dataset D, i.e. the original domain of f) -> we need to keep g as close as possible to f per iteration as well
* Construction takes exponential time for all general functions f but can be tailored for specific functions like median and mean to achieve O(n) and O(n^2), respectively
* Approximate optimality: g achieves good (i.e. approximately optimal) accuracy results; there is no strictly superior function to g; Pareto optimality
* Hardness: it's NP-hard to compute any other function that performs as well as g
* Hence, the reasonable conclusion is that g is the best (in terms of preprocessing) we can do

## Notes
* Some details around accuracy results not too clear (e.g. solid examples would have helped)
* So g is a preprocessing function that "reduces" the global sensitivity (or otherwise?) of f while approximating it -> can apply (for instance) the traditional Laplace mechanism after computing g?
* So g only satisfies personalized differential privacy as opposed to traditional differential privacy?
* Solid connections and examples among the 5 notions -- which DP, closed form of g in certain statistical contexts, sensitivity, (\epsilon, \delta), accuracy -- would be immensely helpful