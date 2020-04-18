# TextRank: Bringing Order into Texts (2004, Mihalcea and Tarau)

## Introduction
* Graph-based ranking algorithms (e.g. Google's PageRank and Kleinberg's HITS) have been successful
* TextRank is similar to PageRank
* Can be used for either keyword extraction or sentence extraction (i.e. summarizing sentence)

## Model
* Two parameters include: directed/undirected? weighted/unweighted?
* Weighted means edges are weighted
* Formula for unweighted: ![Formula](/images/textrank_3.png)
* Formula for weighted: ![Weighted](/images/textrank_4.png)
* Keyword extraction:![Keyword](/images/textrank_1.png)
* Sentence extraction: ![Sentence](/images/textrank_2.png)
* The idea is that words have edges if they appear near each other (think n-grams) while sentences have edges if their similarity is high (can use the paper's definition of similarity, cosine similarity, longest common subsequence, etc.)
* Simple idea but powerful results