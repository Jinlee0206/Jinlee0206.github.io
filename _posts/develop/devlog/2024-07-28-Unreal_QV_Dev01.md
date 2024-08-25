---
layout: post
title:  "[Unreal_쿼터뷰프로젝트] 포트폴리오"
excerpt : ""
categories: develop
tags: cpp unreal
toc: true
comments : true
use_math : true

date: 2024-07-28
last_modified_at: 2024-08-25
---
> <span style="font-size: 80%">
본 문서는 어소트락 언리얼엔진 게임프로그래머 양성과정의 강의를 토대로 필기한 내용입니다 </span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}



## V.1.0.0 - 24.08.25

### Today I Do
- Default AI 블랙보드, 비헤이비어트리 세팅
<p align = "center">
    <img src = "https://github.com/user-attachments/assets/9b7073f1-43ae-4b5e-9f0f-9b0fe436c266" width = 420 alt = "240825_BT">
</p>

- BTTask_TraceTarget 구현
  - TraceTraget 노드와 Patrol 노드일 때 Task 처리 하는 로직 구현
  - Patrol Point와 Spawn Point는 추가 구현 필요
- BTTask_Attack 구현
  - [TraceTarget] - [Attack] - [AttackEnd] - [RotationAtTarget] 의 로직
  - 타겟과의 거리 게산, 타겟과의 회전 계산, 액터의 공격 반경, z 높이체크 (스폰 시 다양한 크기의 몬스터들 일괄 적용을 위함) 등의 함수는 재사용 가능성을 위해 `GameInfo.cpp`에 구현

### Plan
- BTTask_Attack 마무리
- 포트폴리오 기획서 08.31까지 마무리!