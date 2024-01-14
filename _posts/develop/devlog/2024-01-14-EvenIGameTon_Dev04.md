---
layout: post
title:  "[이븐아이 게임톤] 개발 - 업그레이드"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-14
last_modified_at: 2024-01-14
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## Card, CardData script 작성
- [일반 몬스터] - [메인 캐릭터 스킬]에 해당 하는 부분
- ScriptableObject를 이용해 스킬 종류에 따라 구분하고, 스킬 카드 종류에 따라 PlayerData의 값을 변화 시키는 로직으로 설계
   - PlayerData와 Card 스크립트 간의 연결을 어떻게 해야할 지 고민이 필요...

## UI Font 변경
- ThaleahFat Legacy Font 사용

## 특이사항
- Stash 하는 과정에서 실수로 전부 Discard를 해버리는 실수를 함
- 항목만 Discard를 하던지 Continue로 다 Stash에 올린 후 하나씩 수정해 나가는 작업을 하자