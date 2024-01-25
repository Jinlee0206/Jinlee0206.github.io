---
layout: post
title:  "[이븐아이 게임톤] 개발 - 승리, 패배"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-20
last_modified_at: 2024-01-25
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 승리 로직 구현
- VictoryUI
   - No : GoToHome Button
   - Yes : GoToNextStage 함수 구현 : 테스트 진행은 못해봄 (버그 있을 수 있다)

## 패배 로직 구현
- GameOverUI
   - No : GoToHome Button
   - Yes : Retry Button

## Spawner 
- Start함수 변경 : Battle_Proto 에서도 Run 되게 코드 수정

## 카드 뽑기 로직
- AchiveManager와 PlayerPrefs를 활용하여 각각의 Card를 SetActive 하는 방법으로 구현 시도
- 실패 : Card가 모두 Active 되어 있는 상태에서 카드 뽑기가 안되게 끔 하는 방법으로 접근 필요 (Boolean...?)
