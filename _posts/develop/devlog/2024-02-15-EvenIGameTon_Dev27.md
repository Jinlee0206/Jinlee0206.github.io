---
layout: post
title:  "[이븐아이 게임톤] 개발 - 시나리오, 레벨디자인, 사운드, 버그 수정"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-02-15
last_modified_at: 2024-02-15
comments : true
---
* this unordered seed list will be replaced by the toc
{:toc}

## 시나리오
- 스토리 컷신 셋업 완료
- 로그아웃 시 오프닝 컷툰 안나오는 버그 수정
- 스토리 컷신 01 내부 업데이트 적용 완료

## 레벨디자인
- 증폭 피해 (전체) 60% -> 30% 실적용 수정 / 카드 문구 수정
- 폭발 피해 (루모스) 30% -> 10% 실적용 수정 / 카드 문구 수정
- 스킬 지속시간 (아구아멘티, 루모스, 액서니아) 각각 1.7s, 1s, 1.3s로 수정 / 카드 문구 수정
- 쿨타임 감소 (전체) 20% -> 15% 실적용 수정 / 카드 문구 수정

## 사운드
- 5.7_Main_Hamster_Voice_01 -> 5.20_Main_Hamster_Voice_01 로 변경하여 삽입
- 5.4_Sub_Hamster_Voice_03 흑찌로, 5.6_Sub_Hamster_Voice_05 힐찌로 변경

## 버그리포트
- 스태미너 관련
  - 로비 화면 상태에서 다른 앱 갔다오면 다시 충전 20분으로 리셋
  - 16분 즈음 (정확한 시간은 없음) 스태미너 충전 시간이 다시 20분으로 리셋되는 현상이 있음
- 게임 TimeScale 관련 이슈 (수정완)
  - 게임 속도 1.5배인 상태에서 로비씬으로 왔을 때, 1.5배 유지 되는 현상 있음
  - 로비씬 Awake 함수에 TimeScale = 1.0f 되게 처리

## 최종발표
- 관련 미팅 진행
  - 15분 최종발표 개발 파트 발표 진행 맡을 예정