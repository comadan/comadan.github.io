---
layout: post
title:  "N-Gram Language Models"
date:   2017-11-18 02:00:00 -0800
categories: ['Natural Language Processing']
published: true
---

The purpose of a language model is to predict the probability of a sequence of words (tokens). The simplest language models are n-gram language models.

A unigram language model simply multiplies the probability of each word

$$P(w_1, w_2, ..., w_n) = \prod_{i=1}^nP(w_i)$$

A bigram language model simply adds a conditional probability.

$$P(w_1, w_2, ..., w_n) = \prod_{i=2}^nP(w_i|w_{(i-1)})$$

Note that there is an assumption that there is a start token `<start>` which denotes the start of the sentence.

Thus phrase: `The quick brown fox ...` becomes `<start> The quick brown fox ...`. This allows the first conditional bi-gram probability to indicate the probability of the first word (token) occuring first in a sentence. Similarly, adding an `<end>` token allows for the conditional probability that the final word is the last word.



One could imagine a model that detects incorrect spelling by simply doing a character n-gram.