---
layout: post
title:  "[C++] 프로그래머스 : 단어변환"
excerpt : ""
categories: develop
tags: cpp algorithm
toc: true
comments : true

date: 2024-08-15
last_modified_at: 2024-08-15
---
> <span style="font-size: 80%">
인프런에 있는 큰돌님의 강의 10주완성 C++ 코딩테스트 | 알고리즘 코딩테스트를 듣고 정리한 필기입니다.</span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 문제 

[프로그래머스 : 단어변환(링크)](https://school.programmers.co.kr/learn/courses/30/lessons/43163)

두 개의 단어 begin, target과 단어의 집합 words가 있습니다. 아래와 같은 규칙을 이용하여 begin에서 target으로 변환하는 가장 짧은 변환 과정을 찾으려고 합니다.

1. 한 번에 한 개의 알파벳만 바꿀 수 있습니다.
2. words에 있는 단어로만 변환할 수 있습니다.

예를 들어 begin이 "hit", target가 "cog", words가 ["hot","dot","dog","lot","log","cog"]라면 "hit" -> "hot" -> "dot" -> "dog" -> "cog"와 같이 4단계를 거쳐 변환할 수 있습니다.

두 개의 단어 begin, target과 단어의 집합 words가 매개변수로 주어질 때, 최소 몇 단계의 과정을 거쳐 begin을 target으로 변환할 수 있는지 return 하도록 solution 함수를 작성해주세요.

## 제한사항
- 각 단어는 알파벳 소문자로만 이루어져 있습니다.
- 각 단어의 길이는 3 이상 10 이하이며 모든 단어의 길이는 같습니다.
- words에는 3개 이상 50개 이하의 단어가 있으며 중복되는 단어는 없습니다.
- begin과 target은 같지 않습니다.
- 변환할 수 없는 경우에는 0를 return 합니다.

## 입출력 예

|**begin**|	**target**|**words**|	**return**|
| -- | -- | --| -- |
|"hit"|	"cog"|	["hot", "dot", "dog", "lot", "log", "cog"]|	4|
|"hit"|	"cog"|	["hot", "dot", "dog", "lot", "log"]|	0|

## 알고리즘
1. DFS
2. 백트래킹

## 풀이
- 하나의 줄기를 탐색해가며 변환 단계의 최소 값을 구해 나가면 된다
  - **하나의 줄기 탐색** => **DFS**
  - 한번 사용한 단어 다시 사용하지 않는다 (최소값 구하기)
  - 탐색함수를 재귀적으로 호출
    - 종료 조건 세우기
  - `target` 문자열과 동일하게 변환이 완료 되어 재귀 호출이 종료된 경우 다른 최소 경로의 변환 과정이 있을 수 있기 때문에 *해당 탐색 과정에서 다시 가장 가까운 갈림길로 돌아와서(Back-Tracking) 다른 방향으로 다시 탐색 진행 한다*
- 한 글자만 다른 단어인지 판별하는 함수 작성

### 백트래킹 수도 코드
```cpp
for(int i = 0; i< words.size(); i++)
{
    // 1. 방문 처리
    visited[i] = true;
    // 2. 재귀 호출, 단계 증가 
    dfs(... , step + 1);
    // 3. 백트래킹(방문 처리 해제) 후 다음 분기점부터 다시 탐색
    visited[i] = false;
}
```

### DFS의 종료조건과 매개변수
- 종료조건(기저사례)
  - 탐색 중인 단어와 `target`단어가 같아졌을 때

```cpp
if(current == target) return;
```

- 재귀함수의 매개변수
  - 현재 탐색 중인 단어가 매개변수로 주어져야지 종료조건으로써 참조할 수 있다
  - 단어 변환이 몇 번 일어 났는지 체크할 변수 (최소값 비교를 위해)

```cpp
void dfs(string& current, string& target, vector<string>& words, int step);
```

### 효율적인 코드
현재 갱신하는 중인 최소 값 step이 만약 answer(항상 최소 값으로 갱신) 보다 크다면 해당 노드는 탐색할 필요가 없다

```cpp
answer = min(answer, step);
```

## 코드
```cpp
// 프로그래머스 단어변환 Lv.3
// DFS, 백트래킹
#include <bits/stdc++.h>

using namespace std;

int answer = 50;
bool visited[50];

bool check_diff(string& a, string& b)
{
    int cnt = 0;
    for(int i = 0; i<a.size(); i++)
    {
        if(a[i] != b[i])
        {
            cnt++;
        }
    }
    if(cnt == 1) return true;
    return false;
}

void dfs(string& current, string& target, vector<string>& words, int step)
{
    if(answer <= step) return;
    
    if(current == target)
    {
        answer = min(answer, step);
        return;
    }
    
    for(int i = 0; i< words.size(); i++)
    {
        if(check_diff(current, words[i]) && !visited[i])
        {
            visited[i] = true;
            dfs(words[i], target, words, step + 1);
            visited[i] = false;    
        }
    }
    return;
}

int solution(string begin, string target, vector<string> words) {   
    dfs(begin, target, words, 0);
    
    if(answer == 50) return 0;
    
    return answer;
}
```

## 평가  
- DFS 문제는 종료조건(기저사례)와 DFS의 매개변수 설정이 중요
- DFS, BFS 문제 중 백트래킹이 필요한 문제 중 기본적인 템플릿
- 효율적인 코드를 위해 이미 최소 값이 안되는 탐색 노드는 계산하지 않는 코드를 사용한 것도 잘 지켜보기