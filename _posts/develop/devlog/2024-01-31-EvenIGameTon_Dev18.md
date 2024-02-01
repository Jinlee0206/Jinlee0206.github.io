---
layout: post
title:  "[이븐아이 게임톤] 개발 - 사운드"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-31
last_modified_at: 2024-02-01
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 사운드
- 사운드 클립 설정
  - Force To Mono : 모노 채널 타입으로 변경
  - SFX
    - Decompressed On Load : 로드 시 압축 해제
    - 압축품질 40으로 셋팅
  - BGM
    Compressed In Memory : 실행 시 압축 해제
  - Compression Format : Vorbis
  - <AudioSource\>.dopplerLevel , <AudioSource\>.reverbZoneMix 각각 0으로 초기화
  - 오프닝(배경음) Load In Backgroudn Check : 비동기 방식으로 사운드 설정
  
- BGM 사운드 적용
  - Opening, Shop, Battle01

- SFX 사운드 적용
  - UI PopUpWindows