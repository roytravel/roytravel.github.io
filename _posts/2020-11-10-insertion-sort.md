---
date: 2020-11-10
layout: post
title: Insertion Sort
subtitle: What is Insertion Sort and how to implement?
description:
image: /assets/img/Insertion_Sort.png
optimized_image:
  
category: algorithm
tags:
  - Algorithm
  - Sorting
  - Sorting Algorithm
  - Insertion Sort
  
author: Roytravel
paginate: true
use_math: true
---

이전 포스팅 : <a href="https://roytravel.github.io/selection-sort/">Selection Sort</a>

# 목차
* 삽입 정렬(Insertion Sort)이란?
* 삽입 정렬 구현 코드
* 삽입 정렬 시간복잡도

## 삽입 정렬(Insertion Sort)이란?

삽입정렬이란 카드를 정렬하는 방법과 유사한것으로 두 번째 원소부터 시작하여 그 앞쪽의 원소와 비교하여 삽입할 위치를 지정후 원소를 뒤로 옮기고 지정한 자리에 원소를 삽입하여 정렬하는 알고리즘이다.

두 번째 원소는 첫 번째 원소와만 비교하고, 세 번째 원소는 두 번째 원소와 첫 번째 원소, 네 번째 원소는 세 번째 원소, 두 번째 원소, 세 번째 원소와 비교하여 원소가 삽입될 위치를 찾는다. 예를 들면 다음과 같다.

$\\{8, 6, 2, 7, 4\\}$라는 배열이 있을 경우

**1)** 두 번째 원소 6은 첫 번째 원소 8과 비교하여 삽입할 위치를 설정<br>
--> $\\{6, 8, 2, 7, 4\\}$


**2)** 세 번째 원소 2는 두 번째 원소 8과 비교하여 삽입할 위치를 설정<br>
--> $\\{6, 2, 8, 7, 4\\}$

두 번째 원소가된 2는 다시 첫 번째 원소 6과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 8, 7, 4\\}$


**3)** 네 번째 원소 7은 세 번째 원소 8과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 7, 8, 4\\}$

세 번째 원소가된 7은 다시 두 번째 원소 6과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 7, 8, 4\\}$

두 번째 원소인 6은 다시 첫 번째 원소인 2와 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 7, 8, 4\\}$

**4)** 다섯번째 원소인 4는 네 번째 원소인 8과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 7, 4, 8\\}$

네 번째 원소가된 4는 다시 세 번째 원소 7과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 6, 4, 7, 8\\}$

세 번째 원소가된 4는 다시 두 번째 원소 6과 비교하여 삽입할 위치 설정<br>
--> $\\{2, 4, 6, 7, 8\\}$

두 번째 원소가된 4는 다시 첫 번째 원소 2와 비교하여 삽입할 위치 설정<br>
--> $\\{2, 4, 6, 7, 8\\}$



이를 코드로 구현하면 다음과 같다.

## 삽입 정렬 구현 코드
```c
#include <stdio.h>

void insertion_sort(int array[], int n)
{
	int i = 0;
	int j = 0;
	int temp = 0;
	
	for (i=0; i<n-1; i++)
	{
		for (j=i; j>=0; j--)
		{
			if (array[j] > array[j+1])
			{
				temp = array[j];
				array[j] = array[j+1];
				array[j+1] = temp;
			}
		}
	}
	
}


int main(void)
{
		
	int array[] = {8, 6, 2, 7, 4};
		
	insertion_sort(array, sizeof(array)/sizeof(int));
	
	for (int k=0; k<sizeof(array)/sizeof(int); k++)
	{
		printf("%d ", array[k]);
	}
	
	return 0;
}
```

$i$는 외부 루프로서, 배열에 있는 몇 개의 원소가 정렬을 위해 참여할지에 대한 변수이다. 두 번째 원소부터 시작하기 때문에 외부 루프의 반복 횟수는 "배열의 크기 - 1"이 된다. $j$의 경우 배열에 있는 원소 간의 비교를 위해 사용하는 변수이며, 직접 손코딩을 진행해보면 $j=i$로 시작하여 $j>=0$까지 진행되며 $j$의 크기를 계속 -1 해준다는 사실을 도출할 수 있다(알고리즘 도출은 개인마다 다를 수 있습니다). $temp$의 경우 정렬을 위해 배열의 원소를 임시로 담아두는 변수이다.



## 삽입 정렬의 시간복잡도

최선 : $n$ <br>
평균 : $n^2$ <br>
최악 : $n^2$ <br>

삽입 정렬의 경우 구현이 간단하나 시간 복잡도가 평균과 최악이 $n^2$으로 비효율적인 방법에 속한다.

### Reference
[1] <a href="https://fullyunderstood.com/pseudocodes/insertion-sort/">Insertion Sort</a><br>
[2] <a href="https://gmlwjd9405.github.io/2018/05/06/algorithm-insertion-sort.html">[알고리즘] 삽입 정렬(insertion sort)이란</a><br>