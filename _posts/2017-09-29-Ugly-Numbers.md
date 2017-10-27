---
layout: post
title: Ungly Numbers
comments: true
tags: [problem solving, dynamic programming]
---

[문제 출처 및 참고 자료](http://www.geeksforgeeks.org/ugly-numbers/)

## 문제 설명

어떤 수를 소인수분해 했을 때, 소인수가 오직 2와 3, 5뿐인 수를 ugly number라 해보자. 예를 들면 아래와 같은 수들이 ugly numbers인데 크기순으로 $$n$$번째 ugly number를 찾는게 문제다.

`1, 2, 3, 4, 5, 6, 8, 9, 10, 12, 15, 16, ... (주의: 관계상 1도 포함)`

## 풀이

가장 단순한 풀이는 1부터 차례대로 그 수가 ugly number인지 확인하고, 맞으면 횟수를 기록하여 $$n$$번째 ugly number를 찾는 것이다. 당연히 느리다.

ugly numbers에 대해서 좀 더 관찰해보자. $$n$$번째 ugly number는 $$1$$ ~ $$n-1$$번째 ugly number에 2를 곱했거나 3, 5를 곱한 수 중에 하나라는 것을 알 수 있다. 그러면 다음과 같이 풀 수 있다. 시간복잡도는 $$O(n^2)$$이다.

``` c++
unsigned getNthUglyNo(unsigned n) { // O(n^2)
    unsigned ugly[n + 1]; // ugly[i]: i-th ugly number
    ugly[1] = 1;
    for (int i = 2; i <= n; i++) {
        ugly[i] = numeric_limits<unsigned>::max();
        for (int j = 0; j < i; j++) {
            if (ugly[j] * 2 > ugly[i - 1]) {
                ugly[i] = min(ugly[i], ugly[j] * 2);
            }
            if (ugly[j] * 3 > ugly[i - 1]) {
                ugly[i] = min(ugly[i], ugly[j] * 3);
            }
            if (ugly[j] * 5 > ugly[i - 1]) {
                ugly[i] = min(ugly[i], ugly[j] * 5);
            }
        }
    }
    return ugly[n];
}
```

위의 동적계획법 풀이를 좀 더 개선할 수 있을 것 같다. $$i$$번째 ugly number를 구할 때마다 1 ~ $$i-1$$번째 ugly number에 2, 3, 5를 곱해보는데 ugly[i - 1]보다 작거나 같은 값은 다시 계산해볼 필요가 없다. 시간복잡도는 $$ O(n)$$이다.

```c++
unsigned getNthUglyNo2(unsigned n) { // O(n)
    unsigned ugly[n + 1];
    unsigned i2 = 1, i3 = 1, i5 = 1;
    ugly[1] = 1;
    for (int i = 2; i <= n; i++) {
        unsigned t1 = ugly[i2] * 2;
        unsigned t2 = ugly[i3] * 3;
        unsigned t3 = ugly[i5] * 5;
        ugly[i] = min({t1, t2, t3});
        if (ugly[i] == t1)
            i2++;
        if (ugly[i] == t2)
            i3++;
        if (ugly[i] == t3)
            i5++;
    }
    return ugly[n];
}
```

