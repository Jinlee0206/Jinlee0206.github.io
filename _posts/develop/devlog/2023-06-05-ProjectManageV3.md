---
layout: post
title:  "[AtentsTeamPortfoilo] 개발 업데이트 - V.3"
excerpt : "TeamPortfoilo 작성을 위한 개발 버전 업데이트와 Asana를 이용한 프로젝트 매니지먼트"
categories: develop
tags: devlog unity

toc: true
toc_sticky : true

date:   2023-05-15
last_modified_at: 2023-06-05
---

---
## ***Phase3*** 05.01(Mon) ~ 05.14(Sun)
---
---
### V.3.0.00 - JS / Midterm discussion
* Enemy -> Monster 이름 변경  

* Script 정리  
  - 쓸모없는 스크립트 정리
  - 프로퍼티 공통적으로 쓰는 값들 정리하고 안쓰는 것들은 각자 스크립트에서 오버라이딩

* SingleTon 구현할 내용 정리  
  - GameManager에 들어갈 내용 전체적으로 간추리기

* Skill Manager 구현 방법 토의  

* UI 구현 개념 정리  

---

### V.3.0.03 - JS  
* Enemy State Machine 완료 

* Boss  
  - Enemy Inherit 구조 사용 가능하게 변경  
  - FlyState 구현 완료  

* DragonAI Scene에서 Boss-Player 피격 타격 구현   

---

### V.3.0.07 - JS
* CharacterMovement_V2.cs  
  - Awake 함수 추가
  - Attack 함수 Player 안쓰면 몬스터로 내리는 거 고려 필요 - with SM 

* AttackPhase.cs  
  - Attack 함수 두 AttackPoint Binding으로 공격 위치 해결  
  - Pattern delay 주기 작업 실패...  
  - AttackPattern 에 ObjectPooling 추가 하는 Idea 구상 완료  

* Monster.cs  
  - Fly 시 Recall 상태로 돌아가지 않도록 Monster에 canFly 변수 추가 및 LostTarget() 업데이트  

---
