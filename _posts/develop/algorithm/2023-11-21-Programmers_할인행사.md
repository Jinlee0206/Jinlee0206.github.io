---
layout: post
title:  "[C++] 프로그래머스 : 할인 행사"
excerpt : "맵"
categories: develop
tags: cpp algorithm
toc: true

date: 2023-11-21
last_modified_at: 2023-11-21
---

* this unordered seed list will be replaced by the toc
{:toc}

## 문제 

[프로그래머스 : 할인 행사(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/131127)

XYZ 마트는 일정한 금액을 지불하면 10일 동안 회원 자격을 부여합니다. XYZ 마트에서는 회원을 대상으로 매일 한 가지 제품을 할인하는 행사를 합니다. 할인하는 제품은 하루에 하나씩만 구매할 수 있습니다. 알뜰한 정현이는 자신이 원하는 제품과 수량이 할인하는 날짜와 10일 연속으로 일치할 경우에 맞춰서 회원가입을 하려 합니다.

예를 들어, 정현이가 원하는 제품이 바나나 3개, 사과 2개, 쌀 2개, 돼지고기 2개, 냄비 1개이며, XYZ 마트에서 15일간 회원을 대상으로 할인하는 제품이 날짜 순서대로 치킨, 사과, 사과, 바나나, 쌀, 사과, 돼지고기, 바나나, 돼지고기, 쌀, 냄비, 바나나, 사과, 바나나인 경우에 대해 알아봅시다. 첫째 날부터 열흘 간에는 냄비가 할인하지 않기 때문에 첫째 날에는 회원가입을 하지 않습니다. 둘째 날부터 열흘 간에는 바나나를 원하는 만큼 할인구매할 수 없기 때문에 둘째 날에도 회원가입을 하지 않습니다. 셋째 날, 넷째 날, 다섯째 날부터 각각 열흘은 원하는 제품과 수량이 일치하기 때문에 셋 중 하루에 회원가입을 하려 합니다.

정현이가 원하는 제품을 나타내는 문자열 배열 want와 정현이가 원하는 제품의 수량을 나타내는 정수 배열 number, XYZ 마트에서 할인하는 제품을 나타내는 문자열 배열 discount가 주어졌을 때, 회원등록시 정현이가 원하는 제품을 모두 할인 받을 수 있는 회원등록 날짜의 총 일수를 return 하는 solution 함수를 완성하시오. 가능한 날이 없으면 0을 return 합니다.

## 알고리즘
  1. 맵

## 풀이
  1. want에 있는 것들을 number에 맞춰 map에 미리 담는다
  2. discount를 하루하루 10일 단위로 잘라 map에 넣어 갱신 한 후 check 함수를 통해 wantMap과 m을 비교한다  
      * 사려는게 할인 품목에 없거나 수량이 안맞는다면 false를 리턴한 후 true 라면 answer++ 한다
	  * map 자체를 operator == 를 활용해 비교할 수도 있다 
  
## 코드  
```cpp
#include <bits/stdc++.h>

using namespace std;

map<string,int> wantMap;    

bool check(map<string,int> m)
{
    for(auto it : wantMap)
    {
        // 사려는게 할인 품목에 없다면
        if(m.find(it.first) == m.end())
        {
            return false;
        }
        // 사려는 수량만큼 할인 품목에 없다면 (10일 연속 일치해야 한다)
        else if(m[it.first] != it.second)
        {
            return false;
        }
    }
    return true;
}

int solution(vector<string> want, vector<int> number, vector<string> discount) {
    int answer = 0;
    
    for(int i = 0; i< want.size(); i++)
    {
        wantMap[want[i]] = number[i];
    }
    
    for(int i = 0; i < discount.size() - 10 + 1; i++)
    {
        map<string, int> m;
        for(int j = i; j < i + 10; j++)
        {
            m[discount[j]]++;
        }
        answer += check(m);
        m.clear();
    }
   
    return answer;
}
```

## 코드 2
```cpp
#include <bits/stdc++.h>

using namespace std;

map<string,int> wantMap;    

int solution(vector<string> want, vector<int> number, vector<string> discount) {
    int answer = 0;
    
    for(int i = 0; i< want.size(); i++)
    {
        wantMap[want[i]] = number[i];
    }
    
    for(int i = 0; i < discount.size() - 10 + 1; i++)
    {
        map<string, int> m;
        for(int j = i; j < i + 10; j++)
        {
            m[discount[j]]++;
        }
        if(m == wantMap) answer++; // 맵 자체를 operator == 을 이용해 비교할 수도 있다
    }
   
    return answer;
}
```

## 평가  
* 두 개의 map<string, int> 를 이용해 비교 한다는 아이디어를 떠올리는 데 까지는 성공하였으나 코드를 써내려가는 것에 실패한 문제
* map 자체를 operator '==' 로 비교도 가능하기 때문에 코드를 최적화 할 수 있다
* 문제를 꼼꼼히 읽는 연습을 좀더 할 필요가 있다 (10일 연속 일치해야 한다 == map 자체가 동일해야한다)