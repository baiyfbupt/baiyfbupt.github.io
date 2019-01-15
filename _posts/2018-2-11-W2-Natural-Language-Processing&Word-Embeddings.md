---
layout:     post
title:      "Sequence Models 第二周笔记"
subtitle:   "Natural Language Processing&Word Embeddings"
date:       2018-2-11 18:00:00
author:     "baiyf"
header-img: "img/post/coursera-bg.jpg"
header-mask: 0.3
catalog:    true
tags:
    - 公开课
---

# Natural Language Processing&Word Embeddings

## Introduction to Word Embeddings

### Properities of word embeddings

word embeddings可以解决类似*man to woman is like king to __*的问题,因为词义相近的词在向量上也比较接近

![word_vector](/img/post/word_vector.png)

- Cosine similarity

$$sim(u,v) = \frac{u^Tv}{||u||_2||v||_2}$$

### Embedding matrix

$$E*O_j=e_j$$ ，E矩阵是Word2vec和GloVe学习的目标

![embedding_matrix](/img/post/embedding_matrix.jpg)

## Learning Word Embeddings:Word2vec&GloVe

### Word2Vec

* Skip-gram Model

learn Content c(“orange”) to Target t("juice")

![skip_gram](/img/post/skip_gram.jpg)

* Negative Sampling

定义一个模型，学习词与词之间的相关性

### GloVe(global vector for word representation)

![GloVe](/img/post/GloVe.jpg)

## Applications using Word Embeddings

* Simple sentiment classification model

> 一种简单的实现情感分类的模型是：依次去除每一个word的embeddings，然后将所有embeddings求均值并送进softmax层进行多分类

> 这种方法的一个缺陷是不能考虑到句子中各个词的出现顺序

![simple](/img/post/simple.jpg)

* RNN for sentiment classification

> 构造RNN序列模型完成分类任务

![RNN SENTI](/img/post/RNN_SENTI.png)



