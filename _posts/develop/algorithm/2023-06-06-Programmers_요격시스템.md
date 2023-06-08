---
layout: post
title:  "[C++] 프로그래머스 : 요격 시스템"
excerpt : "문자열, 함수"
categories: develop
tags: cpp algorithm

toc: true
toc_sticky : true

date:   2023-06-06
last_modified_at: 2023-06-08
---
<!-- > <span style="font-size: 80%">
인프런에 있는 큰돌님의 강의 10주완성 C++ 코딩테스트 | 알고리즘 코딩테스트를 듣고 정리한 필기입니다.</span> -->

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 문제

[프로그래머스 : 요격 시스템(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/181188)

## 알고리즘

  1. 구현
  2. 라인스위핑
  3. 정렬
  

## 풀이

  1. 이차원 벡터를 cmp 함수를 이용해서 두번째 요소를 기준으로 정렬한다
  2. 라인 스위핑을 이용해서 iterator를 옮겨가며 문제 기준에 맞는지 확인한다
  3. 미사일 개수만큼 반복한다

## 코드  

```cpp
#include <bits/stdc++.h>

using namespace std;

bool cmp (vector<int> &a, vector<int> &b)
{
    if(a[1] == b[1]) return a[0] < b[0];
    else return a[1] < b[1];
}

int solution(vector<vector<int>> targets) {
    int answer = 1;
   
    sort(targets.begin(), targets.end(), cmp);
    
    int from = targets[0][0];
    int to = targets[0][1];
    
    for(int i = 1; i< targets.size(); i++)
    {
        if(targets[i][0] < to) continue;
        from = targets[i][0];
        to = targets[i][1];
        answer++;
    }
    
    return answer;
}
```

## 평가  
* 이차원 벡터의 커스텀 정렬에 대해 이해하고, 라인스위핑에 풀이법에 대해 이해한다

__참고 : [풀이-GitHub링크](https://github.com/Jinlee0206/BOJ/blob/main/%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%A8%B8%EC%8A%A4/unrated/181188.%E2%80%85%EC%9A%94%EA%B2%A9%E2%80%85%EC%8B%9C%EC%8A%A4%ED%85%9C/%EC%9A%94%EA%B2%A9%E2%80%85%EC%8B%9C%EC%8A%A4%ED%85%9C.cpp)__