---
layout: post
title:  "[기술 면접] 02.C# & Unity"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp unity

toc: true

date:   2024-03-04
last_modified_at: 2024-03-04
comments : true
---
> <span style="font-size: 80%">
> Romanticism-GameDeveloper 님의 게임 개발자 면접 정리본을 참고로 만든 자료입니다. </span>

> <span style="font-size: 80%"> [GameDeveloper_Interview_GitHub 링크](https://github.com/Romanticism-GameDeveloper/GameDeveloper-Client-Interview?tab=readme-ov-file)</span>   

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# C# & Unity

## Unity 생명주기

<p align="center"> 
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/109ea0a7-6dba-4e4b-b100-2115b446e994" width = "400">
</p>

- Awake vs Start
  - Awake 
    - 스크립트와 연결된 GameObject가 인스턴스화 되거나 스크립트가 처음 로드될 때 불림
    - 해당 오브젝트가 Enable 상태가 아니라고 해도 위 조건에 따라 로드되면 호출됨
    - 다른 오브젝트에 대한 참조를 생성할 때 주로 사용하게 됨
      - 단 Awake 호출은 무작위
      - 무작위성으로 인해 다른 스크립트의 참조를 통해 접근을 하면 `NullReferenceException`이 발생하게 됨
  - Start
    - 해당 스크립트 컴포넌트가 활성화 되는 순간 불리게 됨
    - 호출 시기는 Awake 보다는 느리게 첫 Update보다는 빠르게 불린다
    - Start에서는 참조를 통해 접근하는 작업이 가능함

  - OnEnable vs Start
    - 둘 다 `컴포넌트가 활성화 될 때` 불린다는 공통점이 있어 묶이게 되지만 Start는 한번, OnEnable은 활성화 될 때마다 불리게 된다는 차이점이 있음 
    - 초기화 작업에 OnEnable을 활용하면 안됨
    - OnEnable은 주로 오브젝트 풀링에 사용하게 되는 함수라고 볼 수 있다

- Update vs FixedUpdate vs LateUpdate
  - Update : 프레임 단위로 호출됨
  - LateUpdate : Update 호출 뒤 불리게 됨
  - FixedUpdate : 고정단위로 불리게 되는 함수
    - FixedUpdate의 `고정단위`를 아는 것이 중요
    - 이 고정 단위는 물리 엔진에 의해 결정이 되므로 컴퓨터 성능에 따라 프레임이 다르게 나와 호출 간격이 일정하지 않은 Update와는 달리 일정하게 불리게 됨
    - 이러한 이유로 인해서 Rigidbody를 조작할 때는 FixedUpdate를 사용하게 됨

## 박싱과 언박싱
- 값 타입
  - C#에서 구조체, 열거형 등은 값타입
  - `System.ValueType`로부터 항상 상속
  - 스레드 스택에 할당됨
- 참조 타입
  - C#에서는 모든 클래스는 참조 타입이 됨
  - `System.Object`로부터 상속
  - 힙에 저장이 되며 GC가 관리
    - 이 힙 메모리의 주소를 가리키는 값은 스택에 저장 됨

- 박싱
  - 값 타입을 참조 타입으로 변경
- 언박싱
  - 참조 타입을 값 타입으로 변경

박싱, 언박싱 과정을 통해 힙에 가비지가 쌓여 GC에 무리를 줄 수 있음 (기본 작업보다는 비용이 큼)

가급적 제네릭을 활용하는 방식으로 타입 변경을 진행하는 것을 추천!

## 직렬화와 역직렬화
- 직렬화
  - 특정 객체를 바이트 단위로 변경한 뒤 디스크에 저장하거나 네트워크로 보낼 수 있게 만들어 주는 것
- 역직렬화
  - 직렬화된 바이트 배열을 원래 객체로 변경하는 과정

- 직렬화를 하는 이유
  - 현재 사용하고 있는 데이터에 대해서 `영속성`을 부여하기 위함
  - 영속성은 프로그램을 종료하더라도 사라지지 않는 특성을 의미
  - 프로그램 종료 후에도 객체에 관한 정보를 남겨두고 싶을 때 직렬화를 사용
  - 주로 플레이어의 데이터처리 등을 의미

- 유니티에서 직렬화가 되는 것
  - public이거나 [SerializeField] 속성이 있어야 함
  - static, const, readonly가 아니여야 함
  - 직렬화할 수 있는 필드 타입이 있어야 함
    - [Serializable] 속성이 있는 비추상, 비일반 커스텀 클래스
    - [Serializable] 속성이 있는 커스텀 구조체
    - `UnityEngine.Object`에서 파생된 오브젝트에 대한 레퍼런스
    - int, double, bool 같은 기본 데이터 형식
    - 열거형 타입
    - Vector2, Vector3, Color 등과 같은 특정 Unity 내장 타입

## const와 readonly
- const
  - 컴파일 타임 상수 (컴파일 시 변수가 값으로 대체)
  - 스택에 위치하게 된다
  - 선언과 동시에 값을 할당
  - 내장 자료형에만 사용 가능
    - 사용자 정의 클래스로는 불가능
- readonly
  - 런타임 상수 (런타임 시 상수에 대한 참조)
  - 힙에 위치하게 된다
  - 생성자에서 초기화 가능 (그 외 변경 불가능)
  - 어떤 타입과도 사용 가능

- const 보다는 readonly가 좋다
  - 둘 중 가장 큰 차이는 readonly는 상수에 대한 참조 코드를 생성한다. 때문에 const의 값을 변경하게 된다면 이를 사용하는 곳은 전부 재컴파일을 해야함
  - readonly의 경우 일부만 리빌드 해도 이를 사용하는 다른 코드들은 참조를 가지고 있으므로 리빌드 없이 올바르게 사용 가능
  - const는 스택에 있어 속도가 빠름
  - readonly를 사용하면 좋은 케이스
    - 특성의 매개변수
    - switch/case 문의 레이블
    - enum 정의

## string
- C#의 string은 immutable(불변) 속성을 가짐
- 멀티스레드 환경을 고려해 여러 스레드들이 엑세스할 때 이들에 대한 동기화 처리를 하는 것 보다 변경이 안되게 **읽기전용**으로 만드는게 값이 더 싸다고 생각
- string에 대한 조작을 하게 되면 이전의 객체에서 복사 후 연산을 한 뒤 이를 대입해주므로 이전의 객체는 가비지가 되어 이후 GC의 처리를 받게 됩니다
- 수정이 많이 일어나는 문자열은 `StringBuilder` 등의 클래스를 사용하는 게 좋음


- StringBuilder
  - 기본적으로 16문자를 담을 수 있는 자리를 잡음
  - 할당된 크기 내에서는 어떠한 수정을 해도 가비지가 생성되지 않음
  - 미리 할당한 버퍼가 다 찬상태에서 append 하게 되면 **새 버퍼를 할당한 뒤 버퍼간 링크를 구성**

## Garbage Collection

C++과의 대표적인 차이가 GC의 유무. C# 즉 .NET의 GC `Mark and Sweep` 알고리즘을 사용하고 있습니다.

- 전역 변수, 현재 함수의 로컬 변수 등을 Root 잡게 됨
- 