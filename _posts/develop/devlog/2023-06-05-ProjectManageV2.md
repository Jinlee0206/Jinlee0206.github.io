---
layout: post
title:  "[AtentsTeamPortfoilo] 개발 업데이트 - V.2"
excerpt : "TeamPortfoilo 작성을 위한 개발 버전 업데이트와 Asana를 이용한 프로젝트 매니지먼트"
categories: develop
tags: devlog unity

toc: true
toc_sticky : true

date:   2023-05-02
last_modified_at: 2023-06-05
---
---
## ***Phase2*** 04.17(Mon) ~ 05.01(Mon)
---
### V.2.0.2 - JS
> CharacterMovementV2 Script 추가  
- abstract 구조로 변경  

- 플레이어와 에너미 공통적으로 쓰는 메서드들 중 재정의가 필요한 것은 abstract 함수로 정의 후 각각 스크립트에서 재정의 필요  

- 플레이어와 에너미가 공통적으로 쓴느 메서드 중 100% 일치하는 것은 그냥 protected로 선언 후 사용  

> Enemy Script 추가  
- CharacterMovementV2로부터 재정의할 함수 Enemy Script에 구현 필요  
- StateMachine과 스크립트의 분할을 위해 기틀 작업  
- StateMachine, State 구현 예정 (4/19)  
---

### V.2.0.4 - JS
> CharacterMovementV2 Script 수정  
- 플레이어의 Attack 함수 가져와서 Collider를 활용하여 Attack성공여부 확인 할 수있게 수정  
> Abstract State 스크립트 구현   

> StateMachine 및 Enemy Script 구현 예정  
---

### V.2.0.6 - JS  

> CharacterProperty Script 수정  
- Protected 한정자 일부 Public으로 수정  

> State - Idle, Trace Script 구현  

> State - Fly Script 구현  
- Fly Enter 구현, Land 상태 구현 필요
- Fly Animation 

---
### V.2.0.7 - JS  
> State - Trace Script 수정  
- Interface 재정의 누락으로 인한 버그 픽스
---
### V.2.0.11 - JS  
> Enemy State - Battle, Recall 상태 구현  

> Enemy State - Die 상태 구현 예정 (~5/2)  
---