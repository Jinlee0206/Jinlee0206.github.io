---
layout: post
title:  "[이븐아이 게임톤] 개발 - 백엔드"
excerpt : "개발"
categories: develop
tags: devlog unity

toc: true

date:   2024-02-01
last_modified_at: 2024-02-14
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}

## 뒤끝
- PC, 콘솔, 모바일 등의 플랫 폼에서 *온라인 게임 개발 및 운영*을 위해 필요한 *서버와 데이터베이스 구축*을 손쉽게 할 수 있는 서버 시스템

- 게임서버
  - 유저 데이터 유지 : 기기 변경 등을 통해 쉽게 유실될 수 있는 유저 데이터를 안전하게 유지
  - 유저 리텐션 : 푸시, 우편, 이벤트, 랭킹, 소셜 기능 활용으로 유저 리텐션 및 플레이 타임 개선 가능
  - 잦은 앱 업데이트 방지 : 주요 밸런싱 데이터를 서버에서 관리하여 앱 업데이트 없이 게임 밸런스 수정
  - 데이터 분석 관리 : 유저 데이터 분석 및 관리, 로그 및 퍼널 분서으로 유저 이탈 구간 확인 및 개선, 주요 지표 관리
  - 게임 캐시 관리 : 게임 캐시 재화 충전 및 캐시 상품 관리와 영수증 검증 가능
  - 보안 강화 : 부정 결제 방지 및 환불 대응을 위한 유저 결제 정보 관리, 주요 확률 및 아이템 획득 정보를 서버에서 관리

- 자체 개발
  - 서버 개발자/기술 인력 필요
  - 유저 증가 시 대규모 인원 처리에 대한 노하우
  - 모든 게임 장르에 적용할 수 있는 백엔드 구축 노하우
  - 서버 개발 필요

- 뒤끝의 장점
  - 서버 개발자 충원이 필수가 아님 (매우 쉬움) -> 개발 기간 단축 (클라이언트에 전념 가능)

- 뒤끝 시스템 종류
  - 뒤끝 베이스
    - 게임 개발에 필수적인 비동기 기능들을 제공
  - 뒤끝 챗
    - 모바일 게임에 특화된 실시간 채팅 서비스를 제공
  - 뒤끝 매치
    - 1:1 부터 팀전까지 국내 최초 실시간 PvP 서비스를 지원
  - 뒤끝펑션
    - 뒤끝 베이스에 보안을 더한 서버 펑션 기능을 제공

## 뒤끝 베이스
- 게임 유저 관리
  - 회원가입/로그인
  - 유저 정보 관리
  - 유저 닉네임 설정
  - 국가 코드 관리
  - 회원 탈퇴
- 게임 운영 관리
  - 공지사항
  - 이벤트
  - 1:1 문의
  - 정책
- 결제 관리
  - 인앱 결제 영수증 검증
  - 게임 캐시 관리
- 랭킹 관리
  - 유저 랭킹
  - 길드 랭킹
  - 랭킹 보상
- 소셜 기능
  - 친구
  - 길드
  - 쪽지
  - 우편
- 푸시 알람
  - 푸시 등록하기
  - 푸시 테스트
- 게임 정보 관리
  - 데이터 저장 : 유저의 아이템이나 현재 장비 정보 등을 저장할 때 사용
  - 트랜잭션 처리
- 차트 관리
  - 게임 내에서 사용되는 데이터를 서버에 테이블 형태로 저장하고, 게임 내에서 불러와 사용하는 기능을 제공
- 확률 관리
  - 랜덤 보상 상자 등에 사용되는 확률을 제공하는 기능
- 쿠폰 관리