---
layout: post
title:  "[AtentsTeamPortfoilo] 개발 업데이트 - V.4"
excerpt : "TeamPortfoilo 작성을 위한 개발 버전 업데이트와 Asana를 이용한 프로젝트 매니지먼트"
categories: develop
tags: devlog unity

toc: true
toc_sticky : true

date:   2023-05-31
last_modified_at: 2023-06-05
---

---
## ***Phase4*** 05.15(Mon) ~ 05.30(Tue)
---
### V.4.0.02 - JS
* Scene 정리  
  - Village 씬 - 수민
  - Forest 씬 - 교원
  - Boss 씬 - 진수

* Warp Point 지정  
  - 씬 전환 담당 => 진수 (~5/19)  
  - 씬 Script 추가 (Intro Scene.cs, Scene Loader.cs)

* Boss Script
  - Boss Attack Pattern Coroutine 반복 호출 에러 수정
  - Boss Attack 각 패턴 별 Attack Delay 수정 가능하게 변경

---
### V.4.0.05 - JS
* Boss Scene  
  - Game Manager 추가
  - UI Manager 추가 (Event Handler)
  - Skill Manager 추가 - Skill 사용 Test
  - Boss Map 
    * Plane Collider 추가
    * CineMahcine - 추가 예정

---
### V.4.0.06 - JS
* Boss Scene  
  - Boss OnCreate Skill - DevilEye 추가
* Effect
  - URP Setting adopted

---

## 1st Camera Test Meeting (05-21, 22)
* UI Bug Corrected  

* 공격력, 방어력 계산식 / 세부 능력치 설계  

* 재화 관련 구조 설계 및 코드 작성 방향 토의  

* 시네머신 연출 관련 토의  

---

### V.4.0.08 - JS
* Boss Scene  
  - Boss CutScene 시네머신으로 제작 완료    
      * Boss CutScene01 Encounter 테스트 완료  
         + CutScene01 에서 GameScene으로 바로 넘어가는 방식 // 동기 방식 씬 전환
      * Boss CutScene02 Ending
         + Additive 방식으로 씬 다중 Play 방법으로 씬 전환  
         + CutScene02 재생 시 Transform 넘겨 주는 방법 고려필요
         + CutScene02 재생 이후 본 GameScene으로 넘어왔을 때, 이전 상태 유지하는 방법 고려 필요

* Boss Skill & Effect
  - DevilEye
    * 등장 시 사용되고 공격 기능은 없으므로 Object Pool List로 프리팹 위치 변경 
  - SpitFire  
    * 수민이와 협업 중, 충돌 처리 관련 수정 작업중
  - Bress
    * 추가적인 Boss Skill 제작 구상 중...

* 1st Camera Test  
  - 1차 보스 씬 촬영 영상 Upload
    * https://youtu.be/V-PLCbNNiCU

---

## Scene Merge & Map Discussion
* Boss Scene
   - 마법진 입체감 향상을 위해 이중 Image로 변경하고 Animation 처리를 통해 생동감을 주는 작업 진행 예정 (SM)  
   - 마법진 블룸처리 (URP와 충돌 문제 해결가능 한지 확인해보고 진행)  
   - 맵 톤 설정  
     * 씬 전환 시 맵 전체적인 분위기 연결을 위해 Light 색 조정 (Tone down)  
       
* Forest Scene  
  - 등장 연출 토의  
    * Warp Cut Scene 제작 예정 (JS)
    * 중간 보스 조우 Cut Scene 제작 예정 (JS)  
      + 걸어가다가 서부영화처럼 발부터 올라가면서 캐릭터 전체를 보여주고 다리 너머 기사들이 대화하다가 캐릭터를 발견한 후 캐릭터에게 달려오는 듯하게 연출

* Village Scene  
  - 등장 연출 토의
    * Dummy NPC 생성하고, Nav Mesh Agent 이용해서 정해진 경로를 움직이다가 Player를 만날 시 반대로 도망가는 연출 삽입
    * Village 소환 CutScene 필요시 제작

* Additive Scene
  - Intro, Death, Loading Scene 제작 예정
  - Loading Scene의 경우 각 Map의 스틸 컷을 활용하여 이미지 사용 할 예정 (MO)

* UI  
  - 미니맵 (GY)

---

### V.4.0.09 - JS
* Cut Scene  
  - CutScene Script revised  
* Loading Scene
  - UI 생성 및 Sprite 분류 
* Forest Scene  
  - CutScene01 - Intro Scene
  - CutScene02 - Play-In CutScene
* Build Index 정리  

* 2차 Cam Test 완료  
  - Build 후 라이트 Auto Generate만 해결하면 문제 없을 것으로 보임

---

### V.4.0.11 - JS
* 영상 촬영
  - 각 Game Scene에서 촬영 후 자막 작업 하는 방식으로 진행
  - Build, Light 등 후처리 관련 부분은 추가 작업 진행하는 것으로 합의

  - 최종본 : <iframe width="942" height="530" src="https://www.youtube.com/embed/ygd3OhDwnwI" title="Unity 3D Action RPG - Grim Reaper (Final)" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

---