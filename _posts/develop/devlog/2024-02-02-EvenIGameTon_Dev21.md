---
layout: post
title:  "[이븐아이 게임톤] 개발 - 버그수정"
excerpt : "개발"
categories: develop
tags: devlog, unity

toc: true

date:   2024-02-02
last_modified_at: 2024-02-02
comments : true
---
* this unordered seed list will be replaced by the toc
{:toc}

## UI
- 튜토리얼UI 베이스 제작

## 버그 수정
- ~~UIManager GameManager에 바인딩~~
- 일시정지 버튼 누른 후 게임 재개 하였을 때, 이전 게임 속도가 유지되지 않는 버그 수정
- 재도전 시 스태미너 굴리기 창 뜨게 변경
- 승리 시 인게임 타이머 멈추기
- 승리 시 Yes 버튼 눌렀을 때, 스태미너 굴리기 창 뜨게 변경

## 알려진 버그
- 일시정지 버튼으로 재시작하거나, 게임 승리후 재시작을 하고 난 후, 일시정지 버튼이나, 게임 배속 버튼이 먹통이 되는 현상
- 2배 버튼 적용 이후 게임을 재시작하거나 클리어하였을 때, 재시작하면 Speed버튼은 1배속이고 게임속도는 1.5배속인 채로 진행됨