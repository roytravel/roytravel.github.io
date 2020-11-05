---
date: 2020-11-06
layout: post
title: Selection Sort
subtitle: What is selction sort and how to implement?
description:
image: /assets/img/Selection_Sort.png
optimized_image:
  
category: algorithm
tags:
  - Algorithm
  - Sorting
  - Selection Sort
  
author: Roytravel
paginate: true
use_math: true
---

이전 포스팅 : <a href="https://roytravel.github.io/bubble-sort/">Bubble Sort</a>

# 목차
* 선택 정렬(Selection Sort)이란?
* 선택 정렬 구현 코드
* 선택 정렬의 시간복잡도

## 선택 정렬(Selection Sort)이란?
선택정렬이란 만약 $\\{7, 5, 3, 1\\}$이라는 배열이 있을 경우 가장 작은 원소부터 선택하여 정렬하는 방법이다. 예를 들어 다음과 같이 동작한다.

1) $\\{7, 5, 3, 1\\}$에서 가장 작은 원소를 찾는다.
가장 작은 원소를 찾기 위해서는 반드시 비교가 필요하다. 기준이 될 초기값을 랜덤으로 설정하는 것이 아니라 배열의 가장 처음에 있는 원소인 $7$을 기준으로 삼는다.

2) $7$과 다른 원소들을 비교하면서 $7$보다 작은 원소를 반복적으로 찾으면서 가장 작은 원소가 어떤 것인지 "업데이트"한다. 예를 들어 $7$을 기준으로 $5$와 비교하여 $5$가 더 작다면 $5$로 업데이트하고 $5$는 다시 $3$과 비교하여 $3$으로 업데이트하고 $3$은 다시 $1$과 비교하여 업데이트를 하는 방식이다.

3) 가장 작은 원소인 1과 교환하여 $\\{1, 5, 3, 7\\}$가 된다.

위의 1) 2) 3)의 과정이 하나의 loop가 되며, 비교의 기준이 되었던 첫 번째 자리에 있던 원소 $7$이 가장 마지막에 정렬되었기에 다음으로 기준을 두 번째 자리에 있는 원소인 5를 기준으로 다시 loop를 돌리면 위와 같은 과정을 거쳐 $\\{1, 3, 5, 7\\}$이 되며 이후 세 번째 요소인 5가 다시 기준이 되어 비교 연산을 진행하고 알고리즘은 종료된다.

이를 코드로 구현하면 다음과 같다.

## 선택 정렬 구현 코드
```c
#include <stdio.h>

void SelectionSort(int array[], int n)
{
	int i = 0;
	int j = 0;
	int minIdx = 0;
	int temp;
	
	for (i=0; i<n-1; i++)
	{
		minIdx = i;
		for (j = i+1; j<n; j++)
		{
			if (array[j] < array[minIdx])
				minIdx = j;
				
			temp = array[minIdx];			
			array[minIdx] = array[i];
			array[i] = temp;		
		}
	}
}

int main(void)
{	
	
	int array[] = {10, 8, 6, 4, 2, 0};
	
	SelectionSort(array, sizeof(array)/sizeof(int));
	
	for (int k=0; k<sizeof(array)/sizeof(int); k++)
		printf("%d ", array[k]);
	
	return 0;
}
```

$i$는 비교의 기준이 되는 변수이다. 기준 요소가 하나 먼저 존재하기 때문에 $n-1$개의 비교를 하게된다. $j$의 경우 배열에 있는 원소들을 의미하며 변수 $j$가 $i+1$로 시작하는 이유는 비교의 기준이 되는 변수 $i$가 자기 자신과 비교를 하지 않고 자신 다음에 있는 원소와 비교하기 때문이다. 

이후 기준이 되는 원소와 기준 원소의 다음에 존재하는 원소와 비교하여 만약 기준이 되는 원소가 더 크다면 비교를 했던 원소의 인덱스를 $minIdx$라는 변수에 할당하고 이후 배열 내의 교환과정을 거친다.

## 선택 정렬의 시간복잡도
선택 정렬에서의 발생하는 연산의 경우 
- 비교 연산
두 개의 for문을 실행하는데 첫 번째인 외부 for문의 경우 $(n-1)$번 실행되며, 최소값을 찾기 위한 for문인 내부 for의 경우 $n-1, n-2, \dots, 2, 1$번 실행된다.

- 교환 횟수
외부 for문의 실행 횟수와 동일한데 이는 즉, 상수 시간이 걸리는 것을 의미한다. 한 번 교환을 위해 3번의 이동이 필요하므로 $3(n-1)$번이다.

- $T(n) = (n-1) + (n-2) + \dots + 2 + 1 = \frac{n(n-1)}{2} = O(n^2)$

선택 정렬의 경우 구현이 간단하나 시간복잡도가 최상, 평균, 최악의 경우 $O(n^2)$로 동일하여 비효율적인 방법에 해당한다.


### Reference
[1] <a href="https://fullyunderstood.com/pseudocodes/selection-sort/">Selection Sort</a><br>
[2] <a href="https://gmlwjd9405.github.io/2018/05/06/algorithm-selection-sort.html">[알고리즘] 선택 정렬(selection sort)이란</a><br>