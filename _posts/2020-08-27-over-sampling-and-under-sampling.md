---
date: 2020-08-27
layout: post
title: Oversampling and Undersampling
subtitle: What is oversampling and undersampling and why use it?
image: /assets/img/machine-learning-2.jpg
 
category: ai
tags:
  - machine-learning
  - oversampling
  - undersampling
author: Roytravel
paginate: true
use_math: true
---

## Purpose
Oversampling and undersampling is used to solve problems that when label value is unbalanced distributed on machine learning.
representatively, oversampling is more used than undersampling because it is more advantageous on prediction performance. <br>

## Feature
The following describes the sampling method.

![alt text](/assets/img/oversampling-and-undersampling.png)

Undersampling is a method for reducing data. for example, it is like, if normal data count is 10,000 and abnormal data count is 100, reducing normal data 10,000 to normal data 100.<br>

Oversampling is a method for multiplying data to enough learning. But, if we were using the method to increase the same data, it could cause overfitting. So we have to find the scientific way, there is a SMOTE(Synthetic Minority Over-sampling Technique) change the feature of original data a little bit.

## SMOTE
SMOTE is a method that creates new data that changed a little bit comparison to original data by using KNN(K-Nearest-Neighbor).


![alt text](/assets/img/smote.png)

### Reference

[1] <a href="http://www.yes24.com/Product/Goods/69752484">book</a><br>
[2] <a href="https://joonable.tistory.com/27">blog<a>