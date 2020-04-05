# SFLOSCAN: A biologically-inspired data mining framework for community identification in dynamic social networks (2011, Bellaachia and Bari)

## Introduction
* Clustering based on bird flocking model (first of its kind allegedly)
* Model examines social interactions (e.g. phone calls, physical meetings, group travels, etc.) over time, not just one snapshot
* Time is considered discrete
* 3 properties: 1. separation (steering to avoid colliding with nearby birds); 2. alignment (head is aligned towards a certain direction); and 3. cohesion (within a group)

## Model
* Given: n number of individuals and incidents of interactions among them over t amount of time
* Can construct interaction matrices M_t that accumulate numbers of interactions until time t among all individuals
* ![M](/images/sfloscan_M.png)
* ![Vectors](/images/sfloscan_vectors.png)
* Then can perform hierarchical clustering over the entire time period to obtain clusters

## Notes
* Some definitions (e.g. fuzzy members, necessity of observations O_t that contain groupings) not too clear
* Exact mechanism of hierarchical clustering not too clear
* Simple but powerful idea