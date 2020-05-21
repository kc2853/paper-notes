# Mining of Massive Datasets (2019, Leskovec et al.)

## Chapter 2: MapReduce and the New Software Stack
* Relevant examples: matrix-vector multiplication (e.g. for PageRank) involving billions of dimensions, analyzing (social) graphs with billions of nodes and edges
* Cluster computing: racks of compute nodes (intra-connected by Ethernet, inter-connected by switch)
* Two key desired properties: redundant storage of files, computations divided into tasks
* GFS (Google File System), HDFS (Hadoop Distributed File System), Colossus (improvement over GFS)
* DFS desirable and more sensical if files are enormous and rarely updated
* Master node: monitors overall status
* MapReduce: map -> shuffle -> reduce; key-value pair (but key can be replicated)
* Example of counting the number of occurrences of each word given documents
* Reducer: term referring to single key and its associated value list at Reduce stage
* Task <= node
* ![MapReduce](/images/mmds_mapreduce.png)
* Failures: master node fail (entire job must be restarted), Map worker fail (replace then reinform Reduce workers), Reduce worker fail (just replace)
* Matrix-vector multiplication: stored form of (i, j, m_{ij}) -> key-value pair (i, m_{ij} * v_j)
* Relational-algebra operations (recall: attributes < schema < relation): selection, projection, union, intersection, difference, natural join (< equijoin < inner join -- amid inner join, cross join, left outer join, right outer join, full outer join), grouping and aggregation (i.e. grouping by key then performing basic statistics like SUM, COUNT, AVG, MIN, MAX)
* Evolution into acyclic network of functions (generically called workflow system?): ![Acyclic](/images/mmds_acyclic.png)
* UC Berkeley's Spark, Google's TensorFlow, Google's Pregel (graph model of data, has unique ways of dealing with failures)
* Blocking property: tasks only deliver outputs after they complete
* Spark: more efficient in coping with failures, grouping tasks, and scheduling execution of functions; RDD (Resilient Distributed Dataset); notion of transformation vs action; Map (one-to-one input/output), Flatmap (one-to-many input/output), Filter, Reduce (action not transformation), built-in relational-algebra operators (Join, GroupByKey)
* While Spark manages large RDDs in "splits" a la Hadoop's chunks, two differences: lazy evaluation (of RDDs until action), notion of lineage (e.g. recorded is lineage of R_2 saying that it was made by R_1 via operation X while R_1 was made by R_0 via operation Y) such that aids recovery in a more complicated way at the benefit of greater speed when things go right
* TensorFlow: workflow system specifically dedicated to machine learning
* Dominant cost: usually communication cost (such that trade-off with parallelism)

## Chapter 3: Finding Similar Items
* Jaccard similarity: IoU (intersection over union)
* Example usages: plagiarism, mirror pages, articles from the same source, collaborative filtering (e.g. online purchases, movie ratings, etc.)
* One way to filter ads out from an article: define a shingle to be a 3-gram comprising a stop word followed by two words (b/c ads tend not to have stop words)
* One of main ideas: replace large sets (i.e. document-term matrix) with much smaller representations called signatures
* Signature matrix: can lower the number of rows (of the document-term matrix) by arbitrary amounts via method called minhashing
* ![Minhash](/images/mmds_minhash.png)
* Theorem: the probability that the minhash function for a random permutation of rows produces the same value for two sets equals the Jaccard similarity of those sets
* So if we choose n = 100 permutations for minhashing, then our new signature matrix would have 100 rows (rather than 1 million)
* Can view each row as an independent experiment corresponding to each n in that case
* How do we choose 100 permutations? Hash functions (as "permutation" functions a la ROM)!
* ![Minhash signature matrix](/images/mmds_minhashmat.png)
* Instead of looking at all k rows (of the original matrix), one can run the algorithm on m out of k rows to reduce complexity (but then error bound comes about)
* Locality-sensitive hashing (LSH): hash locally (banding technique, can tweak banding parameters to change false positive and false negative rates) -> candidate pairs
* Second and fourth columns would be bucketed together: ![Banding](/images/mmds_banding.png)
* Crux: Come up with document-term matrix (consider n-grams and filtering techniques) -> minhashing -> signature matrix -> LSH and banding technique as per chosen banding parameters -> candidate pairs
* LSH can be applied for other distance metrics (Hamming, cosine, Euclidean) and other situations (entity resolution, fingerprint matching)

## Chapter 6: Frequent Itemsets
* Market-basket model, e.g. {bread, milk, diaper, beer}
* Support (number of baskets for which set of items I is a subset) and support threshold (for defining "frequent")
* Other applications: related concepts, plagiarism, biomarkers
* Confidence (of the association rule I -> j): ratio of support for I + {j} and support for I
* Interest (of I -> j): difference between its confidence and the fraction of baskets that contain item j -- can be negative
* Reasonable example: support threshold of 1% and confidence of 50%
* Use of main memory example: say n items -> need space to store counts of pairs, i.e. about 2n^2 bytes (if integer is 4 bytes) -> if machine has 2 GB of main memory, then we require about n < 33,000
* Triangular-matrix method (one-dimensional triangular array for all pairs) vs triples method (record [i, j, c] for count c for item i and item j but only if c > 0)
* Monotonicity: if I is frequent, then every J \in I should be frequent
* Maximal: if no superset is frequent
* A-Priori Algorithm: ![Apriori](/images/mmds_apriori.png)
* ![Apriori memory](/images/mmds_apmemory.png)
* ![Apriori general](/images/mmds_apgeneral.png)
* PCY: takes advantage of unused memory in the first pass -> create a hash table for bucket counts (not the pairs themselves) -> create a bitmap (1 if pair is frequent and 0 if pair is not frequent) -> during second pass, discard immediately as per created bitmap to save memory
* Further optimizations: Multistage algo, Multihash algo, randomized algos, SON, Toivonen's

## Chapter 11: Dimensionality Reduction
* To be continued