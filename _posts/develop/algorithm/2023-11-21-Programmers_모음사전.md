---
layout: post
title:  "[C++] 프로그래머스 : 모음사전"
excerpt : "구현, 정수론, DFS"
categories: develop
tags: cpp algorithm
toc: true

date: 2023-11-21
last_modified_at: 2023-11-21
---

* this unordered seed list will be replaced by the toc
{:toc}

## 문제 

[프로그래머스 : 모음사전(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/84512)

사전에 알파벳 모음 'A', 'E', 'I', 'O', 'U'만을 사용하여 만들 수 있는, 길이 5 이하의 모든 단어가 수록되어 있습니다. 사전에서 첫 번째 단어는 "A"이고, 그다음은 "AA"이며, 마지막 단어는 "UUUUU"입니다.

단어 하나 word가 매개변수로 주어질 때, 이 단어가 사전에서 몇 번째 단어인지 return 하도록 solution 함수를 완성해주세요.

제한사항
word의 길이는 1 이상 5 이하입니다.
word는 알파벳 대문자 'A', 'E', 'I', 'O', 'U'로만 이루어져 있습니다.

## 알고리즘
  1. DFS
  2. 백트래킹
  3. 정수론

## 풀이
  1. 'A', 'E', 'I', 'O', 'U' 5글자, 전체 길이 5 이하
     - 시간복잡도 상 완전탐색 가능
  2. DFS
     - 기저사례 : 단어를 찾으면 answer에 cnt 할당
     - 1부터 5까지 5글자를 모두 돌며 dfs 진행
     - "A", "AA", "AAA", "AAAA", "AAAAA", "AAAAE", ... ,
     - size가 5이상이 되면 백트래킹
  3. 
  
## 코드  
```cpp
// 백트래킹, DFS
#include <bits/stdc++.h>

using namespace std;

int cnt = 0, answer = 0;
string aeiou = "AEIOU";

void dfs(string word, string str)
{
    // 기저 사례
    if(str == word) // 단어를 찾으면 answer에 cnt 할당
    {
        answer = cnt;
        return;
    }
    
    for(int i = 0; i< 5; i++)
    {
        if(str.size() < 5)
        {
            cnt++; // 알파벳을 하나 씩 붙이는 과정에서 cnt 증가
            dfs(word, str + aeiou[i]); // word의 길이는 1이상 5이하
        }
    }
}

int solution(string word) {
    dfs(word, "");
    return answer;
}
```

## 코드 2
```cpp
// 정수론
#include <bits/stdc++.h>

using namespace std;

int solution(string word) {
    int answer = 0;
    vector<int> v
    {
        781, 156, 31, 6, 1
    };

    map<char, int> m
    {
        make_pair('A', 1),
        make_pair('E', 2),
        make_pair('I', 3),
        make_pair('O', 4),
        make_pair('U', 5),
    };

    for(int i = 0; i < word.size(); i++)
    {
        answer += (m[word[i]] - 1) * v[i] + 1;
    }

    return answer;
}
```

## 평가  
* DFS, 백트래킹을 이용해 5글자수를 다 채울 때까지 