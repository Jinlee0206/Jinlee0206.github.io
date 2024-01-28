---
layout: post
title:  "[이븐아이 게임톤] 개발 - 서브햄찌, 버그수정"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-28
last_modified_at: 2024-01-28
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 서브 햄찌
  - 제거 버튼 생성 및 로직 구현

## 인게임 재화 (해바라기씨)
  - 게임매니저에서 seed 변수를 사용해서 UI 구현

## UI
  - Card Level 텍스트 색깔 흰색으로 변경
  
## 버그 수정
  - 서브햄찌 설치창 열린채로 레벨업 됬을때, 아무 버튼 안눌리는 버그 (수정완)
  - 카드 레벨업 시 열려있는 모든 팝업을 닫고, 카드를 선택하는 행위 말고는 어떠한 행동도 하지 못하도록 코드 변경
  - 업그레이드 UI 팝업창이 켜져 있을때, 서브햄찌 건설 UI를 팝업하면 버튼이 먹통이되는 버그 (수정완)