---
layout: post
title:  "Word2Vec: Skip Gram Model and Continuous Bag of Words Model"
date:   2017-11-18 00:00:00 -0800
categories: ['Deep Learning', 'Representations', 'Natural Language Processing', 'Word Vectors']
published: true
---

Their are two main flavors of word2vec, the skip-gram approach and the continuous bag of words (CBOW) approach.

![CBOW and Skip Gram Models](/assets/cbow-skipgram.png)
*Image reproduced from: [Efficient Estimation of Word Representations in Vector Space (Mikolov et al.)](https://arxiv.org/pdf/1301.3781.pdf)*

In the Skip-gram appraoch, we take a word and try to predict it's context. In the CBOW model, we take a context and try to predict the inner word. One way to think about the difference is that the skip-gram model tries to predict outer words from the inner word while the CBOW model tries to predict the inner word from the outer words, but in a way this characterization is misleading, as their is an equivalence to the task. We are actually predicting the joint occurance probability. The key differences between the CBOW and Skip-gram models is that the CBOW model uses a sum of the context vectors to dot product with the target vector, whereas the Skip-gram model estimates probabilities pair-wise. The CBOW model trully combines the context to predict the inner word, whereas the Skip-gram's predictions of the joint-occurance of a target word with it's context is an independent prediction for each context word.

### CBOW (Continuous Bag of Words)

Both the CBOW and Skip-gram models estimate an (n x k) matrix of vector representations $$U$$ to represent the inner word and $$V$$ to represent the context words. $$U$$ and $$V$$ are convenient to use because they mirror the common matrix names $$U \Sigma V$$ used in SVD (singular value decomposition).

$$X$$ is an (m x n) one-hot matrix that represents the text of m tokens (words) of a vocabulary of size n. $$x^(i)$$ is the one-hot vector for the ith word in the text, such that it's values are 1 at the jth position indicating the word and 0 elsewhere.

First We obtain our vector representations from $$V$$ for each of the context words. The context is of size 2m, m in either direction.

$$v_{(c-m)} = x^{(c-m)}V$$

$$...$$

$$v_{(c-2)} = x^{(c-2)}V$$

$$v_{(c-1)} = x^{(c-1)}V$$

$$v_{(c+1)} = x^{(c+1)}V$$

$$v_{(c+2)} = x^{(c+2)}V$$

$$...$$

$$v_{(c+m)} = x^{(c+m)}V$$

Next the context vectors are averaged

$$\widehat{v} = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i}}{2m}$$

And the dot product is taken with the vectur $$u_{(c)}$$, which represents the inner word.

$$z_{(c)} = \widehat{v} \cdot u^{(c)}$$ 

$$u_{(c)} = x^{(c)}U$$

$$u_{(i)}$$ is the vector for words when they are in the center position, $$v_{(i)}$$ is the vector for words which are in the context. $$U$$ and $$V$$ are matrices of all vectors for all words, where the ith row entry is the vector for word $$w_i$$. It is possible to use the same $$U$$ and $$V$$, $$U=V$$, but not recommended.

or for the entire vector z across all possible inner words:

$$z = U\widehat{v}$$ 

Note that the dot product with the average of the context vectors $$v$$, $$\widehat{v} \cdot u$$ , is equivalent to the average of the dot products: 

$$z = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i} \cdot u}{2m} = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i}}{2m} \cdot u$$

In order to create a probability, we use the softmax function

$$P(w_{c}|w_{c-m}, ..., w_{c-1}, w_{c+1}, ...w_{c+m}) = \frac{exp(z_o)}{\sum_{i}{z_i}}$$

### Efficient Estimation

There are two primary approaches to efficiently estimating this problem: Hierarchical Soft-max and Negative Sampling. Here we only cover negative sampling.

### References

- [Efficient Estimation of Word Representations in Vector Space - Mikolov et al.](https://arxiv.org/pdf/1301.3781.pdf)
- [Distributed Representations of Words and Phrases and their Compositionality - Mikolov et al.](http://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)