---
layout: post
title: 포스팅 테스트
category: General
tag: [test]
---

* list element with functor item
{:toc}

# 마크다운 문법 및 Mathjax 테스트

'#' 을 이용해 h1, h2, h3, .. 등을 표현한다
## (1) 마크다운 문법 테스트
이것은 중간 제목
### 1.1 List 테스트
#### unordered list

- Linux
  - Ubuntu
  - Mint
- Windows
  - Windows 10
  - Windows 7
  - Windows XP
- Mac OS


#### ordered list

1. 문제를 읽는다.
2. 어떻게 풀지 생각한다.
3. 푼다.

### 1.2 코드 하이라이팅 테스트
아래와 같이 **코드**를 작성할 수 있다.
```c++
#include <iostream>
using namespace std;

int main()
{
    cout << "hello, world" << endl;
    return 0;
}
```

### 1.3 인용문

아래와 같이 인용문을 사용할 수 있다.

> *"C is quirky, flawed, and an enormous success.” - Dennis M.Ritchie*

### 1.4 테이블

| 월, 수 | 화, 목 | 금, 토, 일 |
| :--: | :--: | :-----: |
|  게임  |  코딩  |   게임    |
|  코딩  |  게임  |   코딩    |
|  영어  |  영어  |   영어    |


## (2) Mathjax 테스트

인라인은 이렇게 $$O(n^2)$$ 작성하고, 블럭은 아래와 같이 작성하면 된다.

$$ ax^2+bx+c=0 $$
