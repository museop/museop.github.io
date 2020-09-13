---
layout: post
title: 이진 탐색(Binary Search)
comments: true
category: Computer Algorithm
tag: [search, binary search]
---
검색이란 여러 자료들로 이루어진 특정 자료 구조에서 주어진 키(key)에 해당하는 자료를 찾아내는 것을 말한다. 검색 알고리즘은 정렬 알고리즘과 함께 많은 프로그램, 알고리즘에서 필수 요소로 사용되기 때문에 가장 많이 연구된 알고리즘이다. 프로그래머라면 여러 유명한 검색 알고리즘들에 대해서 잘 숙지하고 활용할 수 있어야 한다.

비교적 유명한 검색 알고리즘으로는 Linear Search, Binary Search, Jump Search, Interpolation Search, Exponential Search, Ternary Search 등이 있는데 이 글에서는 PS에서 반드시 숙지하고 있어야할 검색 알고리즘인 이진 탐색(Binary Search)에 관한 내용을 중점적으로 다루었다.

## 이진 탐색(Binary Search)

이진 검색은 이미 정렬된 자료에서 분할 정복 전략을 이용하여 $$O(log_2n)$$ 탐색 시간을 갖는 매우 빠른 알고리즘이다. 자료의 집합이 특정 키(key)값을 기준으로 (오름차순으로) 정렬되어 있다고 하자.  이진 탐색은 자료의 중간 위치 값과 찾고자 하는 키(key)값을 비교한다. 만약 키 값이 더 작다면 왼쪽 자료 집합에서 같은 방식으로 재귀적으로 답을 찾고, 더 크다면 오른쪽 자료 집합에서 답을 찾아가는 전략을 취한다.

오름차순으로 정렬된 일차원 배열에서 특정 키 값을 찾는 이진 탐색은 아래와 같이 구현할 수 있다.

```c++
int binary_search(int arr[], int n, int key) {
  int l = 0, r = n - 1;
  while (l <= r) {
    int mid = l + (r - l) / 2;
    if (arr[mid] == key) return mid;
    if (key < arr[mid])
      r = mid - 1;
    else
      l = mid + 1;
  }
  return -1; // not found
}
```

이진 탐색은 워낙 많이 사용되는 검색 알고리즘이기 때문에 많은 언어에서 라이브러리로 제공하고 있다. 그 중 C++ STL에서는 다음과 같은 함수들을 제공한다. 간결한 코딩을 위해 익숙하게 사용하는 것이 좋다.

- `std::binary_search`: 정렬된 순차열에서 특정 값이 존재하는지 판단한다.
- `std::lower_bound`: 정렬된 순차열에서 특정 값보다 작지 않은 첫 원소의 위치(iterator)를 반환한다.
- `std::upper_bound`: 정렬된 순차열에서 특정 값보다 큰 첫 원소의 위치(iterator)를 반환한다.
- `std::equal_range`:  정렬된 순차열에서 특정 값이 포함되는 범위 [first, last)를 반환한다(`lower_bound`와 `upper_bound`의 쌍(pair)을 반환한다).




