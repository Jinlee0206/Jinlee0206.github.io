---
layout: post
title:  "[이븐아이 게임톤] 개발 - UI"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-27
last_modified_at: 2024-01-28
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## UI
- 에너지, 게임 재화
  - 설명란 한글 내용추가
  - 숫자 크기 조정 (천의 자리까지는 표현가능)
- 프로필
  - 이미지 크기 조절
  - 캐릭터 이름 집어 넣는 로직 미구현 => 서버 작업 전에 진행 예정
  - 레벨업 버튼
    - 소모 재화 표시 UI 구현완료
    - 레벨업에 실패했을 때 뜨는 하단 문구 구현 완료
    - 전체 레벨로직 현재 미구현 => 서버 작업 전에 진행 예정
- 전투
  - 현재 Chapter 창에서 Stage를 누르고 Chapter-Stage가 보이는 상태에서 Start 버튼을 누르는 방식인데,  그냥 이렇게 쓰는 것은 어떤지 의견 묻고 싶음 (초기 개발에 이 구조로 짜놓음)
  - 스테이지 클리어 진행도 저장 현재 미구현
  - 챕터 이동시 흑백이 현재 구현에러가 나는 상태 (원인이 뭔지 모르겠음) => 가능은 하겠지만 디버깅할 시간이 하루 정도는 필요 (FGT 기간에 진행 할 예정)
  - 스테이지 시작 시 스테이지 시작 UI 팝업창 구현완
  - 스토리 탭 구현 완료 (기능 미구현)
- 카드
  - (추가) 카드 선택창 UI 한글 업데이트

- 미구현
> 도감, 상점