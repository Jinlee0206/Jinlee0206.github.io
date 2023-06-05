---
layout: post
title:  "[AtentsTeamPortfoilo] 개발 업데이트 - V.1"
excerpt : "TeamPortfoilo 작성을 위한 개발 버전 업데이트 규칙과 Asana를 이용한 프로젝트 매니지먼트"
categories: develop
tags: devlog unity

toc: true
toc_sticky : true

date:   2023-04-08
last_modified_at: 2023-04-08
---

---
### V.1.0.2 - JS
* GameManager SingleTone 구조 구현  
* ShakeCamera SingleTone 스크립트 작성 및 플레이어 담당 수민에게 전달  
* ObjectPooling source Code 구현  

***
### V.1.0.0 - JS  

* __개발 버전 업데이트 규칙__  

|V.1.0.0  | 1             | 0            |  0           | JS      |
|:-------:|:-------------:|:------------:|:------------:|:-------:|
|   의미  | 개발일정 페이즈 | 씬수정       | 스크립트 작업 |작업자    |


* __개발 일정 페이즈__
  

|구간   |    Phase1    |     Phase2   |      Phase3   |     Phase4    |
|:----:|:------------:|:------------:|:-------------:|:-------------:|
|기간   |03.27 ~ 04.17| 04.18 ~ 05.01 | 05.02 ~ 05.15 | 05.16 ~ 05.26 |
|개요   | 개발 초기 시스템 설정 및 기반 확립 | 오브젝트간 상호작용 및 상태 머신 구현 | UI 작업 및 세이브 데이터 구현| 추가적인 시스템 구현 및 개발 최적화| 


* __개발 일정 Asana 프로젝트 매니저로 업데이트__

- 사용법  
  1. 메인 담당자 확인 및 Phase 확인
![ProjectManager01](https://user-images.githubusercontent.com/105345909/230705711-a3b1db32-9918-4815-9d6e-0f82869dbb8b.PNG)
  2. 마감일 탭의 시작일을 작업 시작 일로 설정하고 작업 진행  
      - 작업 내용은 Git README.md 파일에 업데이트
      - 상태 업데이트 
  3. 완료 시 완료 체크 하고 완료 일에 해당 날짜 기입    
    3.1. 미완료 시 예상 완료 시점 기입 및 작업 진행 
  4. 간단한 업무 내용 Asana에도 기입 가능  
<img src = "https://user-images.githubusercontent.com/105345909/230705713-2d2a0ddb-fdeb-438e-8949-3ff0a6e94bfb.PNG" width = "260" height ="300"/>
       - 공동 작업이 필요한 경우 공동 작업자 태그를 걸고 사전에 업무 공유하기
