---
layout: post
title:  "Polysemy: Words with multiple meanings"
date:   2017-11-19 00:00:00 -0800
categories: ['Deep Learning', 'Representations', 'Natural Language Processing', 'Word Vectors']
published: true
---

A simple way to create a sentence embedding is to average all the word vectors. However a slightly better way is to take a weighted average of the word vectors and then remove the first principal component.