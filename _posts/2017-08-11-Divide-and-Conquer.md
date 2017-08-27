---
layout: post
title: 분할 정복 (Divide and conquer)
comments: true
category: problem solving
tag: [divide and conquer]
---

* list element with functor item
{:toc}
# (1) 개념



## 분할 정복이란?

분할 정복은 문제를 재귀적으로 둘 이상의 보다 작은 부분 문제로 나눈 뒤, 부분 문제들의 답을 결합하여 문제를 해결하는 방식을 말한다. 분할 정복을 사용하는 알고리즘들은 보통 다음 세 가지 단계를 갖는다.

1. 분할 (divide): 문제를 더 작은 크기의 부분 문제로 나눈다.
2. 정복 (conquer): 부분 문제들은 재귀적으로 해결된다. 만약 부분 문제의 크기가 더 이상 나눌 수 없을 정도로 작거나 바로 해결 가능하다면 바로 해결한다.
3. 병합 (merge): 부분 문제들의 답을 원래 문제의 답으로 병합한다.

분할 정복은 큰 문제를 한 번에 해결하기 어려울 때, 문제 풀이를 단순화하기 위해 많이 사용된다. 더욱이 큰 문제를 비슷한 크기의 부분 문제로 자연스럽게 나눈 뒤, 부분 문제들의 답을 효율적으로 원래 문제의 답으로 결합할 수 있다면 문제를 빠르게 해결할 수 있다.



## 대표적인 분할 정복 사용 예

분할 정복 전략을 취하는 많은 자료구조 및 알고리즘들이 존재한다. 비교적 유명한 것들은 다음과 같다.

### 자료 구조

#### 이진 탐색 트리 (Binary Search Tree)

#### 힙 (Heap)

#### 구간 트리 (Segment Tree)
#### 펜윅 트리 (Fenwick Tree = Binary Indexed Tree)

### 알고리즘

#### 이분 탐색 (Binary Search)
#### 합병 정렬 (Merge Sort)
#### 퀵 정렬 (Quick Sort)
#### 빠른 거듭 제곱 (Fast Pow)
#### 스트라센의 행렬 곱셈 (Strassen's Matrix Multiplication)
#### 가장 가까운 두 점 구하기 (Closest Pair of Points)
#### 카라츠바의 빠른 곱셈 알고리즘 (Karatsuba algorithm for Fast Multiplication)
#### 역전의 수 세기 (Count Inversions)
#### 볼록 껍질 찾기 (Find Convex Hull)

<br>

# (2) 문제

#### BOJ 1992 쿼드 트리 ([문제 보기](https://www.acmicpc.net/problem/1992))

주어진 공간이 모두 0이면 "0"을 찍고, 모두 1이면 "1"을 찍는다. 섞여 있으면 왼쪽 위, 왼쪽 아래, 오른쪽 위, 오른쪽 아래 부분으로 4분할해서 재귀적으로 해결한다. 

#### BOJ 11729 Z ([문제 보기](https://www.acmicpc.net/problem/11729))

숫자는 차례대로 왼쪽 위, 오른쪽 위, 왼쪽 아래, 오른쪽 아래순서이기 때문에 재귀적으로 4등분하다가 (r, c) 1칸에 도달하면 해당 칸에 수를 알 수 있다.

#### BOJ 6549 히스토그램에서 가장 큰 직사각형 ([문제 보기](https://www.acmicpc.net/problem/6549))

총 구간의 가운데를 기준으로 왼쪽, 오른쪽 구간을 나눌 수 있다. 답은 왼쪽 구간에 있거나 오른쪽 구간에 있거나 가운데를 포함하는 형태로 있을 수 있다. 왼쪽, 오른쪽 구간의 답은 1칸이 아닌 이상 재귀적으로 해결하면 된다. 가운데를 포함하는 최대 직사각형은 탐욕적으로 항상 왼쪽 직사각형, 오른쪽 직사각형 중 높이가 더 큰쪽으로 이어서 만들어가면 구할 수 있다.

구간 트리나 스택($$O(N)$$을 이용해서 풀 수도 있다.

#### BOJ 10827 a^b ([문제 보기](https://www.acmicpc.net/problem/10827))

정확한 실수 계산을 위해 큰 정수 배열을 만들고 큰 정수 배열에 대한 거듭 제곱 연산을 구현하면 된다. 거듭 제곱은 다음과 같은 공식으로 계산량을 줄일 수 있다.

$$ a^b =\begin{cases}a^{b/2} \times a^{b/2} & \text{ if } b: even \\ a^{b-1} \times a & \text{ if } b: odd \end{cases} $$ 

#### BOJ 1517 버블 소트 ([문제 보기](https://www.acmicpc.net/problem/1517))

역전의 수를 세는 문제다. 역전의 수는 합병 정렬(merge sort)을 하는 과정(병합 과정에서)에서 찾을 수 있다. 또는 펜윅 트리를 이용해도 풀 수 있다. (구간 트리는 메모리 초과)



# (3) 참고 자료

- 프로그래밍 대회에서 배우는 알고리즘 문제 해결 전략, 구종만
- Competive Programming 3, Steven Halim
- http://www.geeksforgeeks.org/divide-and-conquer/