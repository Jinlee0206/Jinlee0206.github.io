---
layout: post
title:  "[이븐아이 게임톤] 개발 - 도감, UI"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-02-14
last_modified_at: 2024-02-14
comments : true
---
* this unordered seed list will be replaced by the toc
{:toc}

## 도감
- 도감 몬스터, 스킬 아래 Text 변경 "Sound 준비예정" -> "Sound On 권장"
- 도감 햄찌, 몬스터, 스킬 Sound 적용 완료

## UI
- 밧줄 애니메이션 추가
  - 설정UI, 전투선택UI, 프로필UI, 전투결과화면UI, 게임오버UI
  - DoTween 이용
- 설정UI
  - 제작자화면, 출처화면 버튼만 추가
- 전투 결과 화면 UI
  - 이미지 배치 수정
  - Chapter, Stage 표기 HUD 추가
- UI ver4 일부 작업완료 (사진참고)
  -  미완료 내용
     - 제작자들, 출처 (내용 구현)
     - 프로필
     - 전투 선택창 별

<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/a7158d2a-b0f7-4aaa-801b-a8d9ea6871da" width = "160" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/9d14c800-6003-4ddf-80e1-9c873f9ee357" width = "160" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/2812bf9c-c257-468d-97ff-91045862f26b" width = "160" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/fc91912a-1b06-4546-8559-38613b9e2ab3" width = "160" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/2c11ee32-f954-472a-944c-b85e22470279" width = "160" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/8df1627d-5209-4989-a124-82bea9fa6a71" width = "160" height = "210">
</p> 

## Ladder.cs
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using DG.Tweening;

public class Ladder : MonoBehaviour
{
    [SerializeField] GameObject myPanel;
    [SerializeField] RectTransform panelRect;
    [SerializeField] float topPosY, targetPosY;
    [SerializeField] float tweenDuration;

    void Awake()
    {
        panelRect.anchoredPosition = new Vector2(panelRect.anchoredPosition.x, topPosY);
    }

    //// 팝업
    //void Start()
    //{
    //    Intro();
    //}

    // 인게임UI
    private void OnEnable()
    {
        Intro();
    }

    private void OnDisable()
    {
        
    }

    void Intro()
    {
        panelRect.DOAnchorPosY(targetPosY, tweenDuration).SetUpdate(true);
    }

    public void Outro()
    {
        StartCoroutine(OutroCoroutine());
    }

    public IEnumerator OutroCoroutine()
    {
        Tweener tweener = panelRect.DOAnchorPosY(topPosY, tweenDuration);

        yield return tweener.WaitForCompletion();

        Debug.Log("Animation Complete");

        if (myPanel != null)
        {
            myPanel.SetActive(false);
            panelRect.anchoredPosition = new Vector2(panelRect.anchoredPosition.x, topPosY);
        }
    }
}
```