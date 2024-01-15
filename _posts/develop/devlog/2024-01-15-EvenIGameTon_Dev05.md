---
layout: post
title:  "[이븐아이 게임톤] 개발 - 업그레이드"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-15
last_modified_at: 2024-01-15
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## Card.cs 추가 작업
- Card UI 리팩토링
  - Card 스크립트에서 Player 스크립트 바로 참조해서, PlayerData의 배열에 접근하여 데미지를 증가 시키는 방식 - 구현 완료
    - 추가적으로 기본 공격으로 나가지 않는 2,3,4,5,6,7,8에 해당하는 스킬들을 해금하는 방법에 대해 수민님과 논의가 필요함
    - 데미지 증가, 관통, 쿨타임 감소는 적용이 간단 하므로 즉시 적용 가능.
    - 폭발 적용, 폭발 데미지 증가, 공격 범위 증가, 스킬 지속시간에 대한 구현 방법에 대한 논의 필요
  - 현재 구현 된 방식으로는 한 종류의 카드가 무한히 뜨게 할 수는 없으니 상한선 기획 필요

- 창 컨트롤 완료
  - 레벨업 시 Card UI 뜨게 설정

- 시간 컨트롤 완료
  - GameManager에 Stop(), Resume() 함수 만들어서 게임 시간에 접근할 수 있게 함수 추가

- 랜덤 아이템 디자인
  - 미리 만들어 둔 스크립터블 오브젝트를 랜덤하게 뜨게 하는 로직 설계 필요
  - 해금이 우선이 되어야 하기 때문에 레벨은 살리는 방향으로 설계 할 예정

<p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/2dcddee9-2418-4ad1-b8a8-67dfa25db668" width = "350" height = "200">   

## StageClear UI
- StageClearUI 제작 완료
- Reward, Stage Clear 랭크, 애니메이션 제작 필요
<p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/f83016d8-0e6c-4831-a50d-8ba60cf75857" width = "150" height = "180">

## GameOverUI
- GameOverUI 제작 완료

## UI Font 변경
- 현재 UI 폰트 한글 지원 안됨 : 변경 예정

## 특이사항
- 파라미터 에러 발생 _unity_self
- 간헐적으로 발생하는 것으로 확인되고, 유니티 버그 인 것 같기도... 유니티 재시작 시 안나는 것 확인했는 데, 지속적으로 나면 Issue Tracking 필요

<p align ="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/60afc6fe-d8f7-4384-9dcb-6140e1860570" width = "350" height = "170">