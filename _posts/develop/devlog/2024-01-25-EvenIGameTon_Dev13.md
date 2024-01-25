---
layout: post
title:  "[이븐아이 게임톤] 개발 - 서브 캐릭터, UI"
excerpt : "개발"
categories: develop
tags: devlog

toc: true

date:   2024-01-25
last_modified_at: 2024-01-25
comments : true
---

* this unordered seed list will be replaced by the toc
{:toc}  


## 서브 캐릭터 기획

<p align="center">
<img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/90b07f9e-6497-4bfa-889f-079274e57f14" width = "380" height = "250">
</p>

- 서브 햄찌에 해당하는 기획에 따른 작업 진행
- 서브 햄찌는 일반적인 타워 디펜스 게임의 설치형 타워를 의미하고, 인게임 내에서 획득하는 해바라기 씨(인게임 재화)로 설치가 가능하다
  
## 서브 햄찌
- SpawnPoint 2개의 지점 RuleTile로 지정 (Tag "Tile"을 가짐)
- 마우스 RayCast가 Tag와 일치할 때, TowerUI(타워 설치용 UI)를 불러온다
- PopUpWindow 스크립트를 부착하여 PopUpManager의 구조를 사용하여 TowerUI 프리팹을 불러오고, 각각 버튼을 눌렀을 때, 설치형 햄찌를 Instantiate 한다
- 프리팹 Tower01 (for Test) 게임 씬 하이어라키에 생성됨 : 설치형 햄찌 인스턴스

## TowerSpawner
- 타워 스폰 함수 가짐

> 이는 추후에 ObjectPool을 이용한 구조로 변경 할 수도 있다 (참고용)

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class TowerSpawner : MonoBehaviour
{
    [SerializeField]
    private GameObject towerPrefab;

    public void SpawnTower(Transform tileTransform)
    {
        Tile tile = tileTransform.GetComponent<Tile>();
        if (tile.IsBuildTower == true) return;           // 현재 타워 건설되어 있으면 타워건설 X

        tile.IsBuildTower = true;                        // 타워 건설되어 있음으로 설정

        Instantiate(towerPrefab, tileTransform.position, Quaternion.identity);  // 테스트 임시
        // GameManager.Inst.pool.Get(2);                 // 풀매니저 활용
                                                         // 포지션, 위치 매개변수로 넘겨줄 필요 있음
    }
}
```

## ObjectDetector
- 서브햄찌와 설치가 가능한 지점을 확인하기 위한 디텍터 스크립트

```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class ObjectDetector : MonoBehaviour
{
    [SerializeField] TowerSpawner towerSpawner;
    Camera mainCamera;
    Ray ray;
    RaycastHit hit;
    private Transform hitTransform;

    private void Awake()
    {
        mainCamera = Camera.main;
    }

    void Update()
    {
        if(Input.GetMouseButtonDown(0))
        {
            // 카메라 위치에서 화면의 마우스 위치를 관통하는 광선 생성
            ray = mainCamera.ScreenPointToRay(Input.mousePosition);

            // 2D 모니터를 통해 3D 월드의 오브젝트를 마우스로 선택하는 방법
            // 광선에 부딪히는 오브젝트를 검출해서 hit에 저장
            if(Physics.Raycast(ray, out hit, Mathf.Infinity)) 
            {
                if (hit.transform.CompareTag("Tile"))
                {
                    hitTransform = hit.transform;
                    PopUpManager.Inst.CreatePopup(PopUpManager.Inst.PopUpNames.strTowerUI);
                    //towerSpawner.SpawnTower(hit.transform);
                }
            }
        }
    }

    public Transform GetHitTransform()
    {
        return hitTransform;
    }
}
```

## Tile
- 같은 타일에 재생성을 막는 스크립트
```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Tile : MonoBehaviour
{
    // 타일에 타워가 건설되어 있는지 검사하는 변수
    public bool IsBuildTower { get; set; }

    private void Awake()
    {
        IsBuildTower = false;
    }
}
```

## TowerUI
- 타워 설치 가능한 지점을 눌렀을 때, 생성되는 타워 설치 UI


<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/6c87cab6-dc9f-440a-9854-3f2b0a106060" width = "130" height = "210">
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/ee3c8fee-3c85-4159-97a9-3b38860eb6fd" width = "130" height = "210">
</p> 


```cs
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;

public class TowerUI : MonoBehaviour
{
    public Button buttonArrow;
    public Button buttonBomb;
    public Button buttonBlack;
    public Button buttonTank;
    public Button buttonHeal;

    TowerSpawner spawner;
    ObjectDetector objectDetector;
    PopUpWindow popUpWindow;

    private void Awake()
    {
        objectDetector = FindObjectOfType<ObjectDetector>();
        spawner = FindObjectOfType<TowerSpawner>();
        popUpWindow = GetComponent<PopUpWindow>();
    }

    private void Start()
    {
        Init();
    }

    void Init()
    {
        if (objectDetector == null || spawner == null)
        {
            Debug.LogError("ObjectDetector or TowerSpawner not assigned!");
            return;
        }

        if (buttonArrow != null)
        {
            buttonArrow.onClick.AddListener(() =>
            {
                Transform hitTransform = objectDetector.GetHitTransform();
                if (hitTransform != null)
                {
                    spawner.SpawnTower(hitTransform);
                    StartCoroutine(ClosePopUpAfterDelay());
                }
            });
        }
        // 나머지도 적용 최적화 필요
    }

    IEnumerator ClosePopUpAfterDelay()
    {
        yield return new WaitForSeconds(0.3f); // 원하는 대기 시간 설정
        popUpWindow.OnClose();
    }
}
```

## UI
- ScoreUI
  - Bg 교체
<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/2b26493a-0510-48aa-b051-09fb7010ebcb" width = "130" height = "210">
</p> 

  - CardUI
    - 스크롤 이미지 수정완료
    - CardUI 전체 크기 1.3배 확대
    - 배경 이미지 삭제 (전체 패널이 투명하게 보임)
<p align="center"> 
  <img src = "https://github.com/Jinlee0206/EvenIGamethon/assets/105345909/3e0cda15-1853-40d5-bbcb-9755d38da6a7" width = "130" height = "210">
</p> 