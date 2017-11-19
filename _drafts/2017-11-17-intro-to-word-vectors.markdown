---
layout: post
title:  "Introduction to Word Vectors"
date:   2017-11-17 16:20:00 -0800
categories: ['Deep Learning', 'Representations', 'Natural Language Processing', 'Word Vectors']
published: true
---

Most incarnations of word vectors are representations of co-occurance relationships. This is based on the intuition that the meaning of words can be found in the other words that they occur with. Distributional similarity.

"You shall know a word by the company it keeps." - J.R. Firth

Instead of keeping the complete coocurrance vector for every word, which would be a very large N by N matrix, where N is the number of words in the language, we store a smaller N by K vector representation of the cooccurances, where K is the dimension of the vector.

**Co-occurance Matrix**

![Co-occurance Matrix](/assets/word-coocurance-matrix.png)

**Word Vector Matrix**

![Word Vector Matrix](/assets/word-vector-matrix.png)

The Word Vector matrix is a compressed representation of the co-occurance matrix.

### Methods for creating word vectors

Three of the more popular methods for estimating word vectors include:

- Singular Value Decomposition (SVD)
- [Word2Vec](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)
- GloVe ([Global Vectors for Word Representation](https://nlp.stanford.edu/projects/glove/))

### Main Differences between SVD and Word2Vec

Singular value decomposition of a word co-occurance matrix creates an orthogonal vector representation of each word. One challenge is that typical SVD estimation procedures can fail when handling an MxM matrix where M is very large.

Word2Vec instead estimates a single hidden layer neural network representaion of words using stochastic gradient descent.

### Differences between Word2Vec and GloVe

Part of what makes Word2Vec efficient is that it uses stochastic gradient descent, but it is not necessary. GloVe takes the global coocurance matrix and similarly uses a single hidden layer network to represent the co-occurance relationship. Instead of predicting co-occurance example-by-example, as word2vec does, GloVe estimates the entire co-occurance relationship directly.

### References

- [Efficient Estimation of Word Representations in Vector Space (Mikolov et al.)](https://arxiv.org/pdf/1301.3781.pdf)
- [Distributed Representations of Words and Phrases and their Compositionality (Mikolov et al.)](http://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)