---
layout: post
title:  "[C#] Coroutine vs Invoke"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp unity

toc: true

date:   2024-03-08
last_modified_at: 2024-03-08
comments : true
---
> <span style="font-size: 80%"> **출처** </span>
>> <span style="font-size: 80%"> [medium supercent-blog](https://medium.com/supercent-blog/%EC%9C%A0%EB%8B%88%ED%8B%B0-%EA%B8%B0%EB%B3%B8%EA%B8%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4-coroutine-5048334a2e2f)</span>   
>> <span style="font-size: 80%"> [livelyjuseok.log](https://velog.io/@livelyjuseok/%EC%9C%A0%EB%8B%88%ED%8B%B0-%EC%BD%94%EB%A3%A8%ED%8B%B4%EA%B3%BC-Invoke%EC%9D%98-%EC%B0%A8%EC%9D%B4)</span>    
>> <span style="font-size: 80%"> [TODAYCODE](https://coding-of-today.tistory.com/171
)</span> 


<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# 코루틴과 Invoke

## 코루틴

> 대부분의 경우 메서드를 호출하면 실행을 완료한 뒤 호출한 메서드에 제어와 선택적 반환 값을 반환함. 즉 메서드 내에서 발생한 모든 행동은 단일 프레임 업데이트 내에서 발생해야 한다.   
>
> 반면 코루틴을 사용하면 사용하던 작업을 다수의 프레임에 분산할 수 있음. 유니티에서 코루틴은 실행을 일시 정지하고 제어를 유니티에 반환하지만 중단한 부분에서 다음 프레임을 계속할 수 있는 메서드

- 멀티스레드가 아닌 유니티에서 마치 멀티스레드처럼 보여 병렬적인 구조를 만드는 기능
- 시작과 끝이 항상 동일한 프레임에 발생해야하는 일반적인 메서드들과 달리 작업을 여러 프레임에 분산시킬수 있는 특별한 메서드
- 업데이트와는 달리 하나의 메서드 안에서 제어권을 유니티에 반환하고, 다음 프레임이나 특정 프레임부터 다시 진행할 수 있는 기능

- 코루틴의 특징
  - 반드시 IEnumerator를 반환 해야 함
  - yield return을 만나는 순간마다 다음 구문이 실행되는 프레임으로 나뉘게 됨
  - yield break를 만나면 바로 코루틴이 종료
  - 코루틴을 실행하려면 꼭 `MonoBehavior를 상속 받는 객체`가 있어야 함
  - 코루틴을 다루는 메서드들은 모두 MonoBehavior 클래스에 구현되어 있어야 함
  - 코루틴에는 `소유권`이라는 개념이 있는데, 소유권을 가진 객체가 비활성화되거나 파괴되면 해당 객체가 소유한 모든 코루틴이 중단 됨
  - 비활성화된 객체에 코루틴 시작을 요청하면 해당 코루틴은 실행되지 않음
  - **코루틴은 메인 스레드에서 실행 됨. 코루틴은 멀티 스레드가 아님**
  - 매개변수를 전달할 수 있다

```cs
// 코루틴 기본 형태
IEnumerator myCoroutine()
{
  yield return // + 조건
  // 함수내용
}

// 매개변수 전달
StartCoroutine( 메소드이름( 매개변수1, 매개변수2 ) ); 
StartCoroutine( "메소드이름", 매개변수 );
```

- yield return의 종류
  - yield return null : 다음 프레임에 실행 됨
  - yield return new WaitForSeconds(float) : 매개변수로 입력한 숫자에 해당하는 초만큼 기다렸다가 실행 (유니티 상의 시간 기준)
  - yield return new WaitForSecondsRealtime(float) : 매개변수로 입력한 숫자에 해당하는 초만큼 기다렸다가 실행 (현실 시간 기준)
  - yield return new WaitForFixedUpdate / WaitForEndOfFrame 등...
  - yield break : 코루틴의 종료

## Invoke

함수를 대신 실행 시켜 줌. 또한 간단한 방법으로 지연시간 뒤에 함수를 동작할 수 있게 한다

```cs
Invoke( "함수명"(string) , 지연시간(float));
```

Invoke는 Reflection을 통해 값을 가져오는 데 이 방식이 코루틴보다는 느리다. 코루틴이 메서드 자체를 인자로 받아가는 것과는 다르게 메서드의 이름을 받아감.

> Reflection   
프로그램 실행 도중에 객체의 정보 조사, 다른 모듈에 선언된 인스턴스를 생성, 기존 개체에서 형식을 가져오고 해당하는 메서드를 호출, 접근할 수 있는 강력한 기능   

- Invoke 특징
  - Invoke는 GameObject가 비활성화 되더라도 동작을 함
  - InvokeRepeating을 통해 지속 반복 동작을 시킬 수 있다 -> CancelInvoke, 오브젝트를 파괴하여 종료해주어야 한다.

## 코루틴과 Invoke의 차이점
- 코루틴은 GameObject가 활성화 일때만 동작, 인보크는 파괴 전 까지 동작
- 코루틴은 매개변수 전달 가능, Invoke는 불가능
- 코루틴은 TimeScale이 0인 경우에도 동작 시킬 수 있음 -> yield return new WaitForSecondsRealtime()를 사용
 - 리플렉션의 차이로 코루틴의 속도가 조금 더 빠르다.

<!-- 
<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/6b49e2d9-0634-4495-aef0-83c5252d484c" width = "320">
</p> -->


