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

In the Skip-gram approach, we take a word and try to predict it's context. In the CBOW model, we take a context and try to predict the inner word. One way to think about the difference is that the skip-gram model tries to predict outer words from the inner word while the CBOW model tries to predict the inner word from the outer words, but in a way this characterization is misleading, as their is an equivalence to the task. We are actually predicting the joint occurance probability. The key differences between the CBOW and Skip-gram models is that the CBOW model uses a sum of the context vectors to dot product with the target vector, whereas the Skip-gram model estimates probabilities pair-wise. The CBOW model trully combines the context to predict the inner word, whereas the Skip-gram's predictions of the joint-occurance of a target word with it's context is an independent prediction for each context word.

### CBOW (Continuous Bag of Words)

Both the CBOW and Skip-gram models estimate an (n x k) matrix of vector representations $$U$$ to represent the inner word and $$V$$ to represent the context words. $$U$$ and $$V$$ are convenient to use because they mirror the common matrix names $$U \Sigma V$$ used in SVD (singular value decomposition).

$$X$$ is an (m x n) one-hot matrix that represents the text of m tokens (words) of a vocabulary of size n. $$x_{(i)}$$ is the one-hot vector for the ith word in the text, such that it's values are 1 at the jth position indicating the word and 0 elsewhere.

First We obtain our vector representations from $$V$$ for each of the context words. The context is of size 2m, m in either direction.

$$v_{(c-m)} = x_{(c-m)}V$$

$$...$$

$$v_{(c-2)} = x_{(c-2)}V$$

$$v_{(c-1)} = x_{(c-1)}V$$

$$v_{(c+1)} = x_{(c+1)}V$$

$$v_{(c+2)} = x_{(c+2)}V$$

$$...$$

$$v_{(c+m)} = x_{(c+m)}V$$

Next the context vectors are averaged

$$\widehat{v} = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i}}{2m}$$

And the dot product is taken with the vectur $$u_{(c)}$$, which represents the inner word.

$$z_{(c)} = \widehat{v} \cdot u_{(c)}$$ 

$$u_{(c)} = x_{(c)}U$$

$$u_{(i)}$$ is the vector for words when they are in the center position, $$v_{(i)}$$ is the vector for words which are in the context. $$U$$ and $$V$$ are matrices of all vectors for all words, where the ith row entry is the vector for word $$w_i$$. It is possible to use the same $$U$$ and $$V$$, $$U=V$$, but not recommended.

or for the entire vector z across all possible inner words:

$$z = U\widehat{v}$$ 

Note that the dot product with the average of the context vectors $$v$$, $$\widehat{v} \cdot u$$ , is equivalent to the average of the dot products: 

$$z = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i} \cdot u}{2m} = \frac{\sum_{i=c-m, ..., c-1, c+1, ... c+m}{v_i}}{2m} \cdot u$$

In order to create a probability, we use the softmax function

$$P(w_{c}|w_{c-m}, ..., w_{c-1}, w_{c+1}, ...w_{c+m}) = \frac{exp(z_c)}{\sum_{i}{exp(z_i)}}$$

Our cost function is the log loss or cross entropy loss function

$$J = - \sum_{i\in{corpus}}{y_i\log(\widehat{y}_i)}$$

The word2vec implementation trains with stochastic gradient descent, going word by word in the corpus, so $$J$$ is simply

$$J = - y_i\log(\widehat{y}_i)$$

of course, $$\widehat{y}_i$$ is simply our estimate of the probability

$$\widehat{y}_i = \widehat{P}(w_{c}|w_{c-m}, ..., w_{c-1}, w_{c+1}, ...w_{c+m}) = \frac{exp(z_c)}{\sum_{i}{exp(z_i)}}$$

because the $$log$$ is the inverse of $$exp$$ and because a $$log$$ of a division is the subtraction of logs, and $$y_i$$ is 1 for the observed values, we have

$$J = - (z - log(\sum_{i}{exp(z_i)})) =  - z + log(\sum_{i \in Vocabulary}{exp(z_i)}))$$

### Efficient Estimation with Negative Sampling

There are two primary approaches to efficiently estimating this problem: Hierarchical Soft-max and Negative Sampling. Here we cover negative sampling.

It is very expensive to estimate the normalization quantity $$\sum_{i}{exp(z_i)}$$. So instead we create a sampled corpus D, sampling words in proportional to their unigram probability $$P(w_i)$$.

However, Mikolov et al. found that this oversampled very frequent words. And so, instead, they raise the unigram probability to the 3/4 power. Sampling is proportional to $$P(w_i)^{\frac{3}{4}}$$

Interestingly Socher also used a similar 3/4 power sampling for their GloVe model.

It is useful to know that

$$ 1 - \sigma(x) = \sigma(-x) $$

With negative sampling, we actually are optimizing a related, but different optimization function. Instead of optimizing 

CBOW:

$$ J = -u_c^T \cdot \widehat{v} + log(\sum_{j=1}^{|V|}exp(u_j^T \cdot \widehat{v})) $$

we optimize the log loss cost for the observed as if we were doing a binary regression, and the log loss cost of the sampled corpus.

SkipGram:

$$ J = -u_{c-m+j}^T \cdot v_c + log(\sum_{k=1}^{|V|}exp(u_k^T \cdot v_c)) $$

where u is the outer vector and v is the inner vector. This is switched in the CBOW model notation above.


### References

- [Efficient Estimation of Word Representations in Vector Space - Mikolov et al.](https://arxiv.org/pdf/1301.3781.pdf)
- [Distributed Representations of Words and Phrases and their Compositionality - Mikolov et al.](http://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)