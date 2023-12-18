---
layout: post
title:  "[C++] 프로그래머스 : 더 맵게"
excerpt : "우선순위 큐"
categories: develop
tags: cpp algorithm
toc: true
comments : true

date: 2023-11-23
last_modified_at: 2023-11-23
---

* this unordered seed list will be replaced by the toc
{:toc}

## 문제 

[프로그래머스 : 더 맵게(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/42626)

매운 것을 좋아하는 Leo는 모든 음식의 스코빌 지수를 K 이상으로 만들고 싶습니다. 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 Leo는 스코빌 지수가 가장 낮은 두 개의 음식을 아래와 같이 특별한 방법으로 섞어 새로운 음식을 만듭니다.

섞은 음식의 스코빌 지수 = 가장 맵지 않은 음식의 스코빌 지수 + (두 번째로 맵지 않은 음식의 스코빌 지수 * 2)
Leo는 모든 음식의 스코빌 지수가 K 이상이 될 때까지 반복하여 섞습니다.
Leo가 가진 음식의 스코빌 지수를 담은 배열 scoville과 원하는 스코빌 지수 K가 주어질 때, 모든 음식의 스코빌 지수를 K 이상으로 만들기 위해 섞어야 하는 최소 횟수를 return 하도록 solution 함수를 작성해주세요.

제한 사항  
scoville의 길이는 2 이상 1,000,000 이하입니다.
K는 0 이상 1,000,000,000 이하입니다.
scoville의 원소는 각각 0 이상 1,000,000 이하입니다.
모든 음식의 스코빌 지수를 K 이상으로 만들 수 없는 경우에는 -1을 return 합니다.

## 알고리즘
  1. 구현
  2. 우선순위 큐

## 풀이
  1. 오름 차순으로 이루어진 우선순위 큐를 만들고 scoville 값을 우선순위 큐에 넣는다
  2. size가 2이상일 때 pq.top() 값(가장 작은 값)을 K와 비교하여 조건에 맞는지 확인하고 아니라면 섞인 값을 다시 큐에 집어 넣는다
  
## 코드  
```cpp
// 우선순위 큐
#include <bits/stdc++.h>

using namespace std;

int solution(vector<int> scoville, int K) {
    int answer = 0;
    
    priority_queue<int, vector<int>, greater<int>> pq;
    
    for(int i = 0; i< scoville.size(); i++) pq.push(scoville[i]);
    
    while(pq.size() > 1 && pq.top() < K)
    {
        int a, b;
        a = pq.top();
        pq.pop();
        b = pq.top();
        pq.pop();
        pq.push(a + b * 2);
        answer++;
    }
    if(pq.size() && pq.top() < K) return -1;
    return answer;
}
```

## 평가  
* 우선순위 큐를 활용해서 min heap을 구현하고 대소비교를 효율적으로 진행할 수 있다
* pq.top() 등 queue의 요소를 참조할 때, queue가 empty가 아닌지 먼저 체크하는 것을 습관하 하자
* min heap을 만들기 위한 선언 형태를 기억하자