# Mining of Massive Datasets (2019, Leskovec et al.)

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

## Chapter 4: Mining Data Streams
* To be continued

## Chapter 6: Frequent Itemsets
* To be continued

## Chapter 11: Dimensionality Reduction
* To be continued