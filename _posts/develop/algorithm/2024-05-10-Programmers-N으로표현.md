---
layout: post
title:  "[C++] 프로그래머스 : N으로 표현"
excerpt : ""
categories: develop
tags: cpp algorithm
toc: true
comments : true

date: 2024-05-10
last_modified_at: 2024-05-10
---
> <span style="font-size: 80%">
인프런에 있는 큰돌님의 강의 10주완성 C++ 코딩테스트 | 알고리즘 코딩테스트를 듣고 정리한 필기입니다.</span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 문제 

[프로그래머스 : N으로 표현(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/42895)

아래와 같이 5와 사칙연산만으로 12를 표현할 수 있습니다.

- 12 = 5 + 5 + (5 / 5) + (5 / 5)
- 12 = 55 / 5 + 5 / 5
- 12 = (55 + 5) / 5

5를 사용한 횟수는 각각 6,5,4 입니다. 그리고 이중 가장 작은 경우는 4입니다.


## 입력

|   |   |   |
| - | - | - |
|N	|number|	return|
|5	|12|	4|
|2|	11|	3|

## 출력

이처럼 숫자 N과 number가 주어질 때, N과 사칙연산만 사용해서 표현 할 수 있는 방법 중 N 사용횟수의 최솟값을 return 하도록 solution 함수를 작성하세요.

## 알고리즘
1. DP

## 풀이
- N이 한 개인 경우 : N
- N이 두 개인 경우 : NN / N+N, N-N, N*N, N/N
- N이 세 개인 경우 : NNN / (NN+N, NN-N, NN*N, NN/N), ((N+N)+N, (N+N)-N, (N+N)*N, (N+N)/N), ((N-N)+N, (N-N)-N, (N-N)*N, (N-N)/N), ((N*N)+N), (N*N)-N, (N*N)*N, (N*N)/N), ((N/N)+N, (N/N)-N, (N/N)*N, (N/N)/N)
  - NNN과 s[1]과 s[0]의 사칙연산, s[0]과 s[1]의 사칙연산의 합
  - j 부터 i-j-1까지의 사칙연산의 합

## 코드
```cpp
// 프로그래머스 : N으로 표현
#include <bits/stdc++.h>
using namespace std;

int solution(int N, int number) {
    int answer = -1; // 최소값이 8보다 크면 -1 리턴
    unordered_set<int> s[8];
    
    int sum = 0;
    // 각 항목에 N, NN, NNN 넣어줌
    for(int i = 0; i < 8; i++) 
    {
        sum = sum * 10 + N;
        s[i].insert(sum);
    }

    for(int i = 1; i< 8; i++)
    {
        for(int j= 0; j<i; j++)
        {
            for(int a : s[j])
            {
                for(int b : s[i-j-1])
                {
                    s[i].insert(a+b);
                    s[i].insert(a-b);
                    s[i].insert(a*b);
                    if(b != 0) s[i].insert(a/b);
                }
            }
        }
    }
    
    for(int i = 0; i< 8; i++)
    {
        if(s[i].find(number) != s[i].end())
        {
            answer = i + 1;
            break;
        }
    }
    
    return answer;
}
```

## 평가  
- Bottom-up DP의 경우 점화식을 만드는 것에 초점을 둔다