---
layout: post
title:  "[이븐아이 게임톤] 개발 - 기본 전투UI"
excerpt : "게임 구조"
categories: develop
tags: devlog

toc: true

date:   2024-01-10
last_modified_at: 2024-01-10
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 전투 기획 초안
  <details>
  <summary> 접기/펼치기 </summary>  
    
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/ae6f4c1b-b2de-4e65-a4fb-89fe67223a1a" width = "420" height = "930">
 </details>

## 해상도 조절  
 - 9 * 16 모바일 비율 임시 설정 (택)
 - 9 * 19 플립 같은 종횡비가 큰 비율

## UI 작업
 - Title UI
   - 임시 타이틀로 간단하게 배경과 이미지, 타이틀, 그리고 시작 문구로 간단 제장  
   <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/a1f63735-e03f-4134-b89e-c71f951d7c5e" width = "180" height = "320">


 - Stage UI
   - ScoreUI
     - 획득 점수 / 플레이 속도 조절 버튼 (임시)
       - 점수는 Int 형으로 Text 받는 식으로 GameManager 구축 후 추후 연동
       - 플레이 속도 조절은 버튼으로 구현할 예정 => 버튼을 누를 때마다 이미지 변경되게 구현 예정 
     - Wave  
       - 현재 웨이브와 해당 스테이지 총 웨이브 표기
     - Timer
       - 해당 스테이지 시작부터 타이머 쭉 흘러가게 설정
     - Exp Bar
       - 슬라이더로 제작
       - 왼쪽에서 오른쪽으로 채워지는 형식으로 구현
       - 100% 채워질 시 카드 오픈 UI 뜨게 구현 예정
       - 현재 레벨을 알 수 있게 레벨 표시도 진행하게 기획 수정 요청 필요
   - DefenseUI
     - SpawnPoint
       - 중앙에 위치한 스폰 포인트는 기본 햄찌가 소환될 곳 (default)
       - 양 옆에 서브 햄찌가 소환될 공간을 미리 만들어두고 클릭해서 건설할 수 있는 타워 디펜스 형식의 구현 예정  
       
   - AttackUI
     - 해금되는 기본 공격 스킬 탭
     - 쿨타임이 돌면 자동으로 공격이 실행되고 쿨타임 아이콘 UI 보여질 예정
     <p align="center"> <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/87342756-c30a-4d58-b753-39cf4a6e4f3e" width = "40" height = "40">

   - 최종 결과   
    <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/6caae26b-e6ab-4e0d-a905-7df65e6b70b9" width = "180" height = "320">




