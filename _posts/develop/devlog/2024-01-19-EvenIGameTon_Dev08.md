---
layout: post
title:  "[이븐아이 게임톤] 개발 - 팝업, 씬"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-19
last_modified_at: 2024-01-19
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## Image Upload
  - 시드 (해바라기 씨)
  - 로고, 배경이미지
  - 타이머 (인게임 타이머 이미지) 

## Lobby
 - 로비 씬 UI 작업 완료 
<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/114fa4ac-c6cc-415b-a588-f2e8f5ec941c" width = "150" height = "250">
</p> 
   
 - 배경은 Grid로 작업할 예정

## PopUpManager

- PopUpManager 시스템 구축
  - PopUpManger.cs : 스태틱 프로퍼티로 프리팹으로 생성된 PopUp들을 [Canvas] - [PopUps] 아래 생성하게 하고 차례로 Close 할 수 있게 구현
  - PopUpWindow.cs : Stack 기반으로 PopUpWindow가 열림
  - PopUpNames.cs : PopUp 이름을 관리하는 MonoBehaviour를 상속 받지않는 단일 클래스. PopUpManager에서 생성 및 초기화를 함
  - PopUpHandler.cs : 각 버튼에 대한 동작함수(PopUp)를 정리

  > PopUpManager를 활용하여 직관적이고 가독성 높은 코드를 작성함.  

  > 구현 중에 유지보수하기 쉬운 코드로의 리팩터링을 해나가며 작업한 것이 큰 도움이 됨.  

- Settings, ExplainStamina, ExplainCorn 작업 완료

<p align = "center">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/0b02e72c-d263-40c1-8def-10e0434c1031" align = "center" width = "150" height = "250">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/a73f2a6c-9b98-4c5b-9233-d13f50a96ded" align = "center" width = "150" height = "250"></img>
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/9b9a437c-6a8d-4d45-8328-cdf15533eb56" align = "center" width = "150" height = "250"></img>
</p>

## DragPlayer
- IPointerClickHandler 인터페이스 활용
  - 클릭시 Profile이 PopUp 되게 구현
  - 추가적으로 IDragHandler, IEndDragHandler를 활용한 Dragable Object도 활용 가능
  - 개발PM님과 미팅 완료하였고 추후 애니메이션 추가되면 활용 가능성 있음

## Scene
- SceneManager 시스템 구축
  - SceneLoader를 싱글톤으로 구현
  - Loading 바 차는 것을 시각적으로 보여주기 위해 SmoothDamp 이용해서 Debug용 딜레이 추가  
   
<p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/30c72f99-b5c0-4a18-ba49-1358908e604f" width = "150" height = "250">
</p>

### 해야할 일

## AchieveManager
  - 해금 조건 달성 매니저 초기 세팅 완료
  - 이차원 배열안에 해금 카드들의 GameObject를 다 담아 놓고, 해금 카드를 선택했을 시 SetActive하는 방식으로 구현 예정 
  - 1차 프로토 타입 Build Test를 위해 UI 작업을 우선 진행하였고, 21일 이후에 다시 Card 뽑기 로직 작업 진행 예정