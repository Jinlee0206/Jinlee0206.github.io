---
layout: post
title:  "[이븐아이 게임톤] 개발 - 해금"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-21
last_modified_at: 2024-01-21
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## Battle_Proto_Test
 - 배틀씬을 테스트하기 위한 씬 복제

## CardData / 스크립터블 오브젝트
 - 카드 잠금 여부 확인하는 불 값 추가
 - 폭발 스킬 해금여부 확인하는 불 값 추가
 - 투과 스킬 해금여부 확인하는 불 값 추가
 - 플레이 모드에서 OnEnable시 해당 불 값 초기화

## LevelUp
 - 카드 3장 뽑고 중복체크, 유효한 카드 체크한 후 조건에 맞지 않으면 다시 뽑는 로직 설계 및 추가
 - 더블 업 카드 관련해서 시간 복잡도 증가 때문에 우선 미적용 (해결 필요)

## Card
 - 각 카드에 대한 로직 작성 (더블 업 포함)

## Wall
 - 게임 끝났을 시 게임 시간 멈추는 로직 구현

## UI
 - Card Description overflow에서 Wrap, Truncate 적용

## 1차 빌드
 - 1차 Build Test 진행
 - 해금이 완료 된 1.5차 빌드 테스트 명일 예정