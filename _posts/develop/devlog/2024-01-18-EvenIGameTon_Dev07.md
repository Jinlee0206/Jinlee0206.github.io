---
layout: post
title:  "[이븐아이 게임톤] 개발 - 로비_전투 UI"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-18
last_modified_at: 2024-01-18
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## LevelUp.cs 로직 구현
  - 공통 적용 부분 구현 완료
    - Damage, CoolTime 등
    - 해금 시스템 구현 필요...

## Lobby_Battle UI
  - 프로토 타입용 Battle UI 구현 완료
  - Chapter, Stage 별 버튼 스크립트화 완료
  - StageSelect를 기준으로 chapter, stage spwaner에서 사용하고 있기 때문에, 다시 Lobby_Battle 씬이 로드 되어도 이전 정보 저장할 수 있게 코딩 완료
  - <p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/b94acab3-22ae-4f5c-8863-ccb5efe97a5b" width = "130" height = "210">   