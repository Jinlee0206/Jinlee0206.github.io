---
layout: post
title:  "[C++] 프로그래머스 : N진수 게임"
excerpt : "문자열, 진법변환"
categories: develop
tags: cpp algorithm

toc: true
comments : true

date:   2023-09-10
last_modified_at: 2023-09-10
---
<!-- > <span style="font-size: 80%">
인프런에 있는 큰돌님의 강의 10주완성 C++ 코딩테스트 | 알고리즘 코딩테스트를 듣고 정리한 필기입니다.</span> -->

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 문제

[프로그래머스 : N진수게임(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/17687)

## 알고리즘

  1. 반복문
  2. 진법 변환  

## 풀이

  1. 이진법부터 십육진법까지 진법 변환하는 문제가 나왔을 때 진법 변환을 하는 함수를 만든다.
  2. 문제에서 주어진 대로 구해야 하는 지점 까지를 제한하여 비트 문자열을 구한다
  3. 정답으로 제출 할 순서를 구하기 위해 반복문의 이터레이터를 적당히 변형한다
## 코드  

```cpp
#include <bits/stdc++.h>

using namespace std;
string numbers = "0123456789ABCDEF"; // 16진수 까지의 비트 문자열을 미리 만들어 놓는다

// 진법 변환을 위한 함수 (암기 필)
string Invert(int n, int base)
{
    string r = "";
    while(n > 0)
    {
        r += numbers[n%base];
        n /= base;
    }
    reverse(r.begin(), r.end());
    return r;
}
    
string solution(int n, int t, int m, int p) {
    string answer = "";
    string a = "0"; // 0은 미리 담아 둔다
    
    // 구해야 하는 최대 길이 = t * m, a.size()의 길이를 종료 조건으로 생각한다
    for(int i = 1; a.size() <= t * m; i++)
    {
        a += Invert(i, n);
    }
    
    for(int i = p - 1; i < t * m; i += m) answer += a[i];
    
    return answer;
}

```

## 평가  
* 진법 변환을 자유 자재로 할 수 있도록 코드를 짜는 연습이 필수적으로 이루어져 한다
* for 문의 세가지 조건에 다양한 조건을 주어 활용 하는 방법 -> 유연한 사고의 활용이 필요하다
