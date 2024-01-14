---
layout: post
title:  "[이븐아이 게임톤] UI 개발 - 전투, 일시정지"
excerpt : "UI 개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-11
last_modified_at: 2024-01-14
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  

## 전투 기획 초안
  <details>
  <summary> 접기/펼치기 </summary>  
    
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/ae6f4c1b-b2de-4e65-a4fb-89fe67223a1a" width = "420" height = "930">
 </details>

## 싱글톤 제네릭 업데이트
 - 재생산성을 높이기 위해 제네릭 타입으로 싱글톤 스크립트 작성완료, 게임 매니저에 적용

 ```CSharp
 using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Singleton<T> : MonoBehaviour where T : Component
{
    static T _inst = null;

    // 제네릭 기반으로 Inst 프로퍼티를 정의
    public static T Inst
    {
        get
        {
            if(_inst == null)
            {
                _inst = FindObjectOfType<T>();
                if(_inst == null)
                {
                    GameObject obj = new GameObject();
                    obj.name = typeof(T).ToString();
                    _inst = obj.AddComponent<T>();
                }
            }
            return _inst;
        }
    }

    protected void Initialize()
    {
        if(_inst == null)
        {
            _inst = this as T;
            DontDestroyOnLoad(gameObject);
        }
        else
        {
            Destroy(this);
        }
    }
}

 ```

## UI 작업
 - Stage UI
   - CardUI
     - 특정 조건(레벨업)이 되었을 때, CardUI가 오픈되고 게임은 일시 정지된다.
     - 세가지 선택지 중에 하나를 선택하였을 때, 해당 카드의 효과를 스테이지 상에 즉각 업데이트하며(미구현) 게임 플레이가 재개된다.  
     <p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/e275ccf9-b6df-40de-bd7a-17619c7fda21">  

   - GameSpeedUI
     - 속도 조절 UI  
     <p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/dae52a1c-2b97-4edb-b00b-900cbd48559a">  

   - PauseUI  
     - Home  
     <p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/5932d5ed-2ba6-4a67-bef5-3ee50ce4816b">   
  
      - Resume  
     <p align="center"> <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/4cbe18b7-650c-47f2-814a-dcb03694f070">  
      - Retry  
        - 게임씬을 로드하는 과정에서 Spawner의 참조 중 하나가 풀려 에러가 나는 것으로 보인다 에러 수정 필요



