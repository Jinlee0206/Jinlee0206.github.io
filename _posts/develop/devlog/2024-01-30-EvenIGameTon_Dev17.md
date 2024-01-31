---
layout: post
title:  "[이븐아이 게임톤] 개발 - 레터박스, ShopUI"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-30
last_modified_at: 2024-01-30
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 레터 박스
  - 게임화면에 해상도를 Main Camera 기준 1080 * 1920으로 고정
    - 카메라 depth -2로 지정하여 UI 보다 아래 쪽에 그려지게 고정
    - 이에 따라 grid background와 tilemap 각각 -3, -2로 변경
  - UI 해상도 Camera 기준 1080 * 1920 으로 고정
    - Render mode : Screen Space - Camera로 설정
    - depth -1로 지정
  - 위 설정만 진행하였을 시, 에디터 상에서는 적용되나 안드로이드 빌드 후, 레터박스 적용이 안되는 현상 발견
  - [Build Settings] - [Player Settings] - 안드로이드 탭의 Resolution Scaling 의 Resolution Scaling Mode를 LetterBoxed로 설정 이후 문제 해결 (올바른 해결책인지는 모름...)

<p align="center">
<img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/07ac9b63-407d-43c4-96ad-ae7b34bb03ee" width = "130" height = "250">
</p>

## 레터박스 코드

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

// 레터박스 설정을 위한 셋팅
public class CameraResolution : MonoBehaviour
{
    private void Awake()
    {
        Camera camera = GetComponent<Camera>();
        Rect rect = camera.rect;
        float scaleHeight = ((float)Screen.width / Screen.height) / ((float) 1080 / 1920);
        float scaleWidth = 1f / scaleHeight;
        if(scaleHeight<1)
        {
            rect.height = scaleHeight;
            rect.y = (1f - scaleHeight) / 2f;
        }
        else
        {
            rect.width = scaleWidth;
            rect.x = (1f - scaleWidth) / 2f;
        }
        camera.rect = rect;
    }

    private void OnPreCull() => GL.Clear(true, true, Color.black);
}
```

```cs
using UnityEngine;

// UI 레터박스 설정을 위한 셋팅
public class CameraResolutionForUI : MonoBehaviour
{
    private void Awake()
    {
        AdjustCanvasForLetterbox();
    }

    private void AdjustCanvasForLetterbox()
    {
        Camera camera = GetComponent<Camera>();
        Rect rect = camera.rect;
        float scaleHeight = ((float)Screen.width / Screen.height) / ((float)1080 / 1920);
        float scaleWidth = 1f / scaleHeight;

        if (scaleHeight < 1)
        {
            rect.height = scaleHeight;
            rect.y = (1f - scaleHeight) / 2f;
        }
        else
        {
            rect.width = scaleWidth;
            rect.x = (1f - scaleWidth) / 2f;
        }

        camera.rect = rect;
    }
}
```

## UI
  - Speed Toggle 버튼 이미지
    - 이미지 아래 자식 이미지 생성해서 IsGameSpeedIncreased 변수의 불린 값에 의해 자식 오브젝트 SetActive false/true 하는 형식으로 구현

<p align="center">
<img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/c340ba13-93b1-4888-b0af-0967b476deec" width = "240" height = "60">
</p>
  
   - ShopUI
     - 제작 완료
     - 스크롤 뷰 UI 사용

<p align="center">
<img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/75301e04-a2c0-4293-adef-2574af72a31e" width = "240" height = "280">
</p>


## 버그 수정
- 2배속으로 플레이하다가 스킬카드 선택하고 다시 배속돌아옴(유지안됨) => 해결 완 속도 유지
- 스킬이미지 안에 Lv 텍스트 흰색으로 변경됨 