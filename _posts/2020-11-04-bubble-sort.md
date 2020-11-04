---
date: 2020-11-04
layout: post
title: Bubble Sort
subtitle: What is bubble sort and how to implement?
description:
image: /assets/img/Bubble_Sort.png
optimized_image:
  
category: algorithm
tags:
  - Algorithm
  - Sorting
  - Bubble Sort
  
author: Roytravel
paginate: true
use_math: true
---

이전 포스팅 : <a href="https://roytravel.github.io/norm-loss-regularization/">Norm, Loss, Regularization</a>

# 목차
* 버블 정렬(Bubble Sort)이란?
* 버블 정렬 구현 코드

## 버블 정렬(Bubble Sort)이란?
버블 정렬 알고리즘의 경우 정렬 알고리즘 중 가장 간단한 알고리즘 중의 하나로 인접한 배열의 원소와 비교하여 정렬하는 알고리즘이다.

만약 배열이 {5, 4, 3, 2, 1}와 같이 최악의 상황과 같이 주어져 있을 경우 다음과 같이 동작한다.

1) {5, 4, 3, 2, 1}<br>
if 5 > 4 --> {4, 5, 3, 2, 1}<br>
if 5 > 3 --> {4, 3, 5, 2, 1}<br>
if 5 > 2 --> {4, 3, 2, 5, 1}<br>
if 5 > 1 --> {4, 3, 2, 1, 5}<br>

2) {4, 3, 2, 1, 5}<br>
if 4 > 3 --> {3, 4, 2, 1, 5}<br>
if 4 > 2 --> {3, 2, 4, 1, 5}<br>
if 4 > 1 --> {3, 2, 1, 4, 5}<br>
if 4 > 5 --> {3, 2, 1, 4, 5}<br>

3) {3, 2, 1, 4, 5}<br>
if 3 > 2 --> {2, 3, 1, 4, 5}<br>
if 3 > 1 --> {2, 1, 3, 4, 5}<br>
if 3 > 4 --> {2, 1, 3, 4, 5}<br>
if 4 > 5 --> {2, 1, 3, 4, 5}<br>

4) {2, 1, 3, 4, 5}<br>
if 2 > 1 --> {1, 2, 3, 4, 5}<br>
if 2 > 3 --> {1, 2, 3, 4, 5}<br>
if 3 > 4 --> {1, 2, 3, 4, 5}<br>
if 4 > 5 --> {1, 2, 3, 4, 5}<br>

위의 과정을 통해 알 수 있듯 버블 정렬의 경우 모든 요소에 대해 전체 배열을 반복해야 하므로 버블 정렬 알고리즘의 시간 복잡도의 경우 최상, 평균, 최악 모두 일정하게 $O(n^2)$이다.

여기에서 구현에 필요한 변수를 추출할 수 있다. 1) 2) 3) 4)의 경우 배열의 각 원소마다 비교를 진행하는 것으로 변수 $i$로 설정할 수 있다.
다음으로 배열의 인접한 원소와 크기를 비교하는 변수를 $j$로 설정이 가능하며, 배열의 위치를 옮기기 위해 필요한 임시 변수를 $temp$로 설정할 수 있다.

## 버블 정렬 구현 코드
```c
#include <stdio.h>

void BubbleSort(int array[], int n)
{
	int i = 0; 
	int j = 0; 
	int temp = 0;
	
	for (i=0; i<n-1; i++)
	{
		for (j=0; j<n-1-i; j++)
		{
			if (array[j]>array[j+1])
			{
				temp = array[j+1];
				array[j+1] = array[j];
				array[j] = temp;
			}
		}
	}
}

int main(void)
{
	int array[] = {2, 9, 6, 5, 4, 1, 8, 3, 7};

	BubbleSort(array, sizeof(array)/sizeof(int));
	
	for (int i=0; i<sizeof(array)/sizeof(int); i++)
	{
		printf("%d ", array[i]);
	}
	
	return 0;
}
```

### Reference
[1] <a href="https://fullyunderstood.com/pseudocodes/bubble-sort/">Bubble Sort</a><br>
