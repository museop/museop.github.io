---
layout: post
title: 완전 탐색 (Complete Search)
comments: true
category: problem solving
tag: [complete search]
---
* list element with functor item
{:toc}
## (1) 완전 탐색 (Complete Search)

### 1.1 완전 탐색이란?

완전 탐색은 답이 될 수도 있는 모든 경우를 만들어보면서 답을 찾는 방법을 말한다. 모든 경우를 다 고려해본 후 결론(답)에 이르기 때문에 속도가 매우 느리다. 하지만 완전 탐색은 답의 구조나 패턴을 파악하는데 도움을 주어 더 좋은 알고리즘을 설계하는데 도움을 줄 수도 있고, 다른 알고리즘이 작은 입력에서 정확하게 동작하는지 검증하는 용도로도 사용할 수 있다.

완전 탐색은 보통 반복문 또는 재귀 함수로 구현한다. 일반적으로 반복문으로 작성하는 것이 속도면에서 유리하겠지만, 다양한 형태의 문제를 보다 간결하게 구현하는데에는 재귀 함수가 낫다.

### 1.2 예제

#### BOJ 2309 일곱 난쟁이 ([문제 보기](https://www.acmicpc.net/problem/2309))

가능한 모든 조합 $$\binom n k = \binom 9 7$$을 모두 만들어보면 된다.

#### BOJ 1182 부분 집합의 합 ([문제 보기](https://www.acmicpc.net/problem/1182))

집합의 각 원소를 고르거나 고르지 않는 모든 경우 ( $$2^N$$)를 모두 만들어보면 된다.

#### BOJ 1759 암호 만들기 ([문제 보기](https://www.acmicpc.net/problem/1759))

모든 문자들을 사전순으로 정렬한 후  $$\binom C L$$의 경우를 모두 만들어보고, 최소 한 개 모음과 최소 두 개의 자음으로 구성되어 있는지 확인하면 된다.

#### BOJ 10974 모든 순열 ([문제 보기](https://www.acmicpc.net/problem/10974))

c++ STL std::next_permutation 함수를 사용하면 편한 문제이지만, 직접 재귀 함수로 모든 순열을 만들어보는 것도 좋다.

#### BOJ 10971 외판원 순회2 ([문제 보기](https://www.acmicpc.net/problem/10971))

모든 순열(도시들의 나열)을 만들어보면 되는 문제다.

#### BOJ 10448 유레카 이론 ([문제 보기](https://www.acmicpc.net/problem/10448))

세 개의 삼각수의 합이 될 수 있는 모든 경우를 미리 생성하여 그 정보를 저장해두면 된다. 

#### BOJ 1018 체스판 다시 칠하기 ([문제 보기](https://www.acmicpc.net/problem/1018))

보드의 각 위치에서 흰색으로 시작하는 경우와 검정색으로 시작하는 경우를 모두 따져보면 되는 문제이다.
