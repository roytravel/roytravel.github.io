---
date: 2020-10-15
layout: post
title: Norm, Loss, Regularization
subtitle: How to mitigate overfitting? - L1 & L2
description:
image: /assets/img/logo_normalization.jpg
optimized_image:
  
category: ai
tags:
  - L1 Norm
  - L2 Norm
  - L1 Regularization
  - L2 Regularization
  - Ridge
  - Lasso
  
author: Roytravel
paginate: true
use_math: true
---

이전 포스팅 : <a href="https://roytravel.github.io/convolution-neural-network/">합성곱 신경망 (Convolution Neural Network, CNN)</a>

# 목차
* Norm
* P-Norm
* L1-Norm
* L2-Norm
* $L_\infty$-Norm
* 여러 Norm의 직관적 차이
* L1 Loss
* L2 Loss
* L1 Loss와 L2 Loss의 차이
* Regularization
* L1 Regularization
* L2 Regularization

ML/DL을 공부하다보면 L1과 L2 개념을 접하게 된다. 하지만 L1과 L2와 관련해서 사용하는 용어들이 L1 Norm, L1 Loss, L1 Regularization 등으로 사용되기에 다소 혼동이 있기도하여 개념을 구분하여 바로 잡고자 하는 차원에서 정리해보았다.

## Norm
Norm은 선형대수학에서 나오는 개념으로 벡터의 길이 또는 크기를 측정하는 방법 또는 함수이다. 보통 $\|\|p\|\|$로 표기하며, Normalization과 다른 용어이다. 대표적으로 사용되는 Norm의 경우 $L_1$-Norm, $L_2$-Norm, $L_\infty$-Norm이 있으며, 이를 일반화한 것을 P-Norm이라 부른다.

### 1. P-Norm
P-Norm은 Norm을 구하는 함수를 일반화한 것으로 벡터의 길이 또는 크기를 측정하는 함수이다. Norm이 측정한 벡터의 크기는 원점에서 벡터 좌표까지의 거리 혹은 크기라고 하며 아래와 같은 수식으로 표현한다.

$L_p = (\sum_i^n \|x_i\|^p)^\frac{1}{p}$

$p$는 Norm의 차수를 의미하며 $n$은 해당 벡터의 요소의 수를 의미한다. $p$ = 1이면 L1 Norm이고, $p$ = 2이면 L2 Norm이다.

### 2. L1-Norm
L1-Norm은 위의 P-Norm의 식에 1을 대입하여 얻을 수 있는 것으로 Manhattan Norm이라 불리기도 한다. 벡터의 요소에 대한 절대값의 합을 의미하며, 벡터의 요소의 값 변화를 정확히 파악할 수 있다는 점을 특징으로 가진다.

$L_1 = (\sum_i^n \|x_i\|)$

예를 들어 $x = [3\ -4\ 5]$와 같은 벡터 $x$가 존재할 때, L1-Norm의 경우 벡터의 요소의 절대값의 합이기 때문에 12가 된다. L1-Norm을 시각화를 하면 다음과 같은 플롯을 확인할 수 있다.

![VuePress 이미지](/assets/img/l1_norm.png)*L1-Norm*


### 3. L2-Norm
L2-Norm은 위의 P-Norm의 식에 "2"를 대입하면 L2-Norm이 되는데 그 식은 다음과 같다.

$L_2 = \sqrt{\sum_i^n \|x_i\|^2}$

L2-Norm은 n차원 좌표 평면에서의 벡터의 크기를 계산하기 때문에 유클리드 노름(Euclidean norm)이라 불리기도 한다. 절대값의 제곱의 합의 제곱근으로 구해지는 것으로, 피타고라스 정리는 2차원 좌표 평면상의 최단 거리를 계산하는 L2-Norm이다.

예를 들어 $x = [3\ -4\ 5]$와 같은 벡터 $x$가 존재할 때, L2-Norm의 경우 벡터의 요소의 절대값의 합에 제곱근이므로 $\sqrt{50}$이 된다. L2-Norm을 시각화하면 다음과 같은 플롯을 확인할 수 있다.

<!--![VuePress 이미지](/assets/img/l2_norm.png)*L2-Norm*-->
![VuePress 이미지](/assets/img/squared_l2_norm.png)*L2-Norm*


### 4. $L_\infty$-Norm
Maximum-Norm이라 부르기도 하며, p의 차수를 무한대로 보냈을 때의 Norm을 의미하는 것으로, 단순히 벡터 성분의 최대값을 구하는것이 특징이다.

$L_\infty = max(\|x_1\|, \|x_2\|, \|x_3\|, \dots, \|x_n\|)$

예를 들어 $x = [3\ -4\ -5]$와 같은 벡터 $x$가 존재할 때, $L_\infty = 5$이다.

### 여러 Norm의 직관척 차이
![alt text](/assets/img/norm_representation.png)

위 그림을 확인하면 두 점(벡터)을 잇는 여러 선이 존재하는 것을 알 수 있다. 이는 벡터 사이의 거리를 측정하는 서로 다른 Norm을 표기한 것이다. 위의 그림에서 초록선이 우리가 흔히 아는 Euclidean distance, 즉 L2 Norm이다. 위의 그림에서의 빨간선, 파란선, 노란선 모두 다른 경로를 움직이지만 사실은 모두 같은 N1 Norm의 변형에 해당한다.

### L1 Loss
이러한 Norm을 기준으로 만들어진 L1 Loss의 수식도 크게 다르지 않다. 두 개의 벡터가 들어가던 자리에 실제값 $y_i$와 예측값 $\hat{y_i}$가 들어간다. 실제값과 예측값 사이의 오차의 절대값을 구하고 그 오차의 합을 L1 Loss라고 한다.

$L = \sum_{i=1}^n \|y_i - \hat{y_i} \|$

L1 Loss는 오차의 제곱의 합의 제곱근으로 정의되며 이를 Least Absolute Deviations(**LAD**)라고 부른다.

### L2 Loss

$L = \sum_{i=1}^n (y_i - \hat{y_i})^2$

L2 Loss는 오차의 제곱의 합으로 정의되며 이를 Least Squares Error(**LSE**)라고 부른다.

### L1 Loss와 L2 Loss의 차이
L2 Loss는 직관적으로 확인했을 때 오차의 제곱을 더하기에 Outlier에 더 큰 영향을 받는다. 따라서 outlier가 적당히 무시되길 원할 경우 비교적 outlier의 영향을 적게 받는 L1 Loss를 사용해야 하며, 반대로 outlier를 주목할 필요 있을 경우 L2 Loss를 사용해야 한다.

## Regularization

Regularization은 trained weight에 noise term을 추가하여 모델에 제약(penalty)을 주어 overfitting을 방지하는 방법으로 , 쉽게 말해 perfect fit을 포기함으로써 potential fit을 증가시키고자 하는 것이고, 다시 말해 training accuracy를 낮춤으로써 testing accuracy를 높이는 것이다.
![alt text](/assets/img/complexity_comprison.png)

위의 그래프 중 오른쪽 모델의 경우 모든 training data에 대해 완벽하게 학습되어 있다. 이러한 모델의 경우 조금만 다른 값이 생기면 예측하기 어려워지기에 일반적인 모델로 적용하기에 어려움이 있다. 반면에 왼쪽 모델의 경우 perfect fit을 일부 포기하였으나 대신 potential fit이 높기 때문에 조금 더 적합한 모델이라 할 수 있는데 이러한 높은 Complexity를 피하기 위해 사용되는 방법이 Regularization이다.

Regularization은 위의 기존 L1, L2 Loss에 Regularization noise term을 붙인 것에 불과하다. 아래의 두 Regularization 식에서 noise term에 절대값을 취하면 L1 Regularization이고, 제곱합을 취하게 되면 L2 Regularization으로 나뉜다. $\lambda$는 Regularization에 얼마나 비중을 줄 것이냐를 정하는 계수로, 0에 가까울수록 Regularization의 효과는 낮아진다. 적절한 $\lambda$ 값은 k-fold cross validation과 같은 방법으로 찾을 수 있다. 

### L1 Regularization

$cost(W,b) = \frac{1}{n}\sum_i^nL(y_i,\hat{y_i}) + \lambda \frac{1}{2}\|w\|$

특히 Sprase feature에 의존하는 모델에 L1 Regularization을 사용할 경우, 불필요한 feature에 대응하는 weight를 0으로 만들기에 feature selection이 가능하고 이러한 특징 때문에 L1은 convex optimization에 유용하게 사용된다. 하지만 L1 Reguliarzation의 경우 미분 불가능한 점이 있기 때문에 Gradient-based learning에는 주의가 필요하다.

### L2 Regularization

$cost(W,b) = \frac{1}{n}\sum_i^nL(y_i,\hat{y_i}) + \lambda \frac{1}{2}\|w\|^2$

반면에 L2 Regularization은 선형 모델의 일반화 능력을 개선시키는 것으로 L2 Regularization을 사용하는 선형회귀 모델을 Ridge Model이라 한다.


### Reference
[1] <a href="https://unsplash.com/@clintadair?utm_source=medium&utm_medium=referral">logo</a><br>
[2] <a href="http://taewan.kim/post/norm/">딥러닝을 위한 Norm, 노름</a><br>
[3] <a href="https://towardsdatascience.com/calculating-vector-p-norms-linear-algebra-for-data-science-iv-400511cffcf0">Calculating Vector P-Norms — Linear Algebra for Data Science -IV</a><br>
[4] <a href="https://elementary-physics.tistory.com/31">(선형대수학) 4.2 Norm</a><br>
[5] <a href="https://m.blog.naver.com/jamiet1/221229735965">[For 빅데이터 수학#3] 선형대수 (1) - 용어정리, Norm, 고유값, 고유벡터 등</a><br>
[6] <a href="https://light-tree.tistory.com/125">딥러닝 용어 정리, L1 Regularization, L2 Regularization 의 이해, 용도와 차이 설명</a><br>
[7] <a href="https://m.blog.naver.com/sky3000v/221536661344">L1, L2 규제(또는 정규화, Regularization)</a><br>
[8] <a href="https://hichoe95.tistory.com/55">Regularization과 Normalization</a><br>
[9] <a href="https://www.globalsoftwaresupport.com/regularization-machine-learning/">Regularization in Machine Learning</a><br>
[10] <a href="https://m.blog.naver.com/dbstmdgks93/221478457085">[기계학습 용어] Norm/Normalization/Standardization/Regularization</a><br>
[11] <a href="https://junklee.tistory.com/29">L1, L2 Norm, Loss, Regularization?</a><br>
[12] <a href="https://karmainearth.tistory.com/162">L1-norm and L2-Norm</a><br>
