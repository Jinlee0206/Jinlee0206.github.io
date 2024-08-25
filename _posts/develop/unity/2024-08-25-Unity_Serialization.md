---
layout: post
title:  "[Unity] 직렬화"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp unity

toc: true

date:   2024-08-25
last_modified_at: 2024-08-25
comments : true
---

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# 직렬화 (Serialization)
> 오브젝트나 연결된 오브젝트의 묶음(오브젝트 그래프)을 바이트 스트림으로 변환하는 과정

복잡한 데이터를 일렬로 세우기 때문에 직렬화 라고 한다. 반대는 역직렬화(Deserialization)

## 장점
- 현재 프로그래밍의 상태를 저장하고 필요할 때에 복원 가능. (게임의 저장)
- 현재 객체의 정보를 클립보드에 복사해서 다른 프로그램에 전송 가능.
- 네트워크를 통해 현재 프로그램의 상태를 다른 컴퓨터에 복원 가능. (멀티플레이어 게임)
- 데이터 압축, 암호화를 통해 데이터를 효율적이고 안전하게 보관 가능.

## 직렬화 규칙
- public이거나 `SerializeField` 속성이 있어야 함
- 정적이 아님
- 상수가 아님
- 읽기 전용이 아님
- 직렬화 가능할 필드 타입이 있어야 함
  - 기본 데이터 형식
  - 열거형 타입(32 바이트 이하)
  - 고정 크기 버퍼
  - Unity 빌트인 타입
  - `Serializable` 속성이 있는 커스텀 구조체
  - UnityEngine.Ojbect에서 파생된 오브젝트에 대한 레퍼런스
  - `Serializable` 속성이 있는 커스텀 클래스
  - 위에서 언급한 필드 타입의 배열
  - 위에서 언급한 필드 타입의 List <T>

## Unity Json 직렬화
> Json을 유니티에서 직렬화 할 때는 JsonUtility 클래스를 사용할 수 있다. JsonUtility는 Unity에서 지원하는 Json 데이터 처리 클래스.

- 사용 조건
  - 직렬화하려는 데이터가 *구조화*된 데이터여야 한다. 즉 저장하려는 변수를 클래스나 구조체로 표현해야 함
  - Monobehavior 혹은 ScriptableObject를 상속받은 클래스 이거나, [Serialzable] 속성을 가진 클래스여야 함
  - 단순 변수나 배열은 직렬화하지 못하며, 클래스 혹은 구조체를 사용하여 나타야 함

```csharp
// Json 데이터에 저장하려는 변수를 설명하는 클래스 혹은 구조를 만들기
[Serializable]
public class MyClass
{
    public int level;
    public float timeElapsed;
    public string playerName;
}

// 클래스의 인스턴스를 생성하고 초기화
MyClass myObject = new MyClass();
myObject.level = 1;
myObject.timeElapsed = 47.5f;
myObject.playerName = "Dr Charles Francis";

// JsonUtility.ToJson() 메소드를 사용해 JSON 포멧으로 직렬화
string json = JsonUtility.ToJson(myObject);
// json now contains: '{"level":1,"timeElapsed":47.5,"playerName":"Dr Charles Francis"}'

// 다시 오브젝트로 전환
myObject = JsonUtility.FromJson<MyClass>(json);

```

# 출처
- 사이트 : [Unity Documentation_JSON 직렬화](https://docs.unity3d.com/kr/2021.3/Manual/JSONSerialization.html)