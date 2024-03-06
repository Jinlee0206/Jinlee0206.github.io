---
layout: post
title:  "[기술 면접] 02.C# & Unity"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp unity

toc: true

date:   2024-03-04
last_modified_at: 2024-03-06
comments : true
---
> <span style="font-size: 80%"> **출처** </span>
>> <span style="font-size: 80%"> [GameDeveloper_Interview_GitHub 링크](https://github.com/Romanticism-GameDeveloper/GameDeveloper-Client-Interview?tab=readme-ov-file)</span>      
>> <span style="font-size: 80%"> [껍데기방:티스토리](https://husk321.tistory.com/358)</span>    

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

- GC 동작 과정
  - 전역 변수, 현재 함수의 로컬 변수 등을 Root로 잡게 됨
  - 이 Root를 기반으로 점점 참조를 타고 다니면서 방문한 것들을 Mark 해줌
  - 이러한 Mark 작업이 끝나게 된다면 Sweep 단계로 진입
  - Sweep 단계에서는 Mark 되지 않은 것들을 가비지로 판단해 처리하게 됨

> **Root 부터 사용하는 객체들을 타고 가면서 사용하는 객체들을 mark 하고 이후 mark 되지 않은 객체들을 전부 제거!**

- .Net과 Unity의 GC
  - 공통
    - GC의 알고리즘은 `Mark and Sweep`을 기반으로 함
  - .Net
    - 0~2세대까지 총 3개의 세대를 통해서 관리
  - Unity
    - `Boehm-Demers-Weiser` 알고리즘을 통해 GC 작업을 하게됨
    - `Mark and Sweep`인 것은 같으나 세대 구분이 없고 메모리 정렬도 없음
    - 점진적 GC 작업을 활용하거나 오브젝트 풀링 기법들을 활용해서 최대한 최적화를 해줘야 할 필요가 있음

- 상호 참조 해결법
  - C#에서 상호참조 중인 객체 해제에 대해서는 위 `Mark and Sweep` 알고리즘을 설명하면 됨
  - 두 객체가 서로 참조 중이라 하더라도 외부에서 참조가 없어 Mark 되지 않는다면 Sweep 단계에서 해제되게 됨

> 상호 참조 제거의 과정 예시)   
>> 1, 2가 서로를 참조하고 있을 때 1이 더이상 사용되지 않는 상황이 되었다고 가정. 이 경우 GC가 한번 동작하면 2가 1을 참조하고 있으므로 1이 살아있을 수 있게 됨. 물론 2는 직접 사용하고 있으니 당연히 사라지지 않음.      
>> 이후 2도 사용을 하지 않게 되면 mark 단계에서 1, 2는 더이상 mark되지 않고 이후 sweep 과정에서 둘 다 사라지게 됨. 이런 식으로 상호 참조중인 객체들이 delete되지 않는걸 피할 수 있게 됨. 

## delegate & event

> delegate : 대표, 위임하다

- delegate (델리게이트)
  - C#에서 델리게이트는 함수를 타입화한 것.
  - C++에서 함수 포인터와 비슷한 개념
  - 파라미터와 리턴 타입을 통해 정의하게 되며 이후 리턴, 파라미터 타입이 같은 메소드들과 호환되어 이 메소드들에 대한 참조를 가질 수 있게 됨

```cs
public delegate void VoidAndIntEx(int i);

public class ExampleClass
{
  public void DoSomething(VoidAndIntEx exFunc)
  {
    // 인자로 받은 함수를 호출
    exFunc(1);
  }
}
```

C#에서 이런 델리게이트를 활용해 메소드를 담아두는 역할을 하거나 함수 인자로 넘겨 콜백 패턴을 구현하는 등 다양한 곳에 사용하게 됨

- event
  - 델리게이트와 비슷한 역할을 함
  - **이벤트를 호출할 수 있는 건 해당 이벤트를 가진 클래스**만 가능

```cs
class ExampleClass
{
  // Action에 대한 설명은 아래
  public event Action ExampleEvent;

  // ...
  if(ExampleEvent != null)
  {
    ExampleEvent();
  }
}
```

- Action, Func, Predicate
  - Action
    - 함수 파라미터가 T이고 반환값이 void 인 경우
  - Func<T, TResult>
    - 함수 파라미터가 T이고 반환값이 TResult 인 경우
  - Predicate
    - 함수 파라미터가 T이고 반환값이 bool 인 경우

> 자주 사용하게 되는 델리게이트를 템플릿화 한 것들

- null 조건부 연산자 (?.)

델리게이트나 이벤트를 다루다 보면 null인지 체크를 해줘야 한다.   
만일 null인 델리게이트를 호출한다면 `NullReferenceException`이 발생하게 됨

```cs
if(ExampleEvent != null)
{
  ExampleEvent();
}
```

위 코드의 문제점은 2가지 존재 
1. 멀티 스레드에서 호출할 경우의 문제
2. 타자가 많다

``` cs
if(ExampleEvent != null) // 여기서는 문제가 없었는 데
{
  // 여기서 다른 스레드가 구독을 취소해서 null이 됨
  ExampleEvent(); // NullReferenceException!
}
```

이런 복작한 문제는 검출이 어렵기에 아래 '복사후 실행'이란 방법을 통해서 예방할 수 있음

```cs
var CopiedEvent = ExampleEvent;

if(CopiedEvent != null)
{
  // 여기서 ExampleEvent 구독 취소해도 문제 없음
  CopiedEvent();
}
```

다만 이 경우 위에 언급한 '타자가 많다' 문제는 해결할 수 없음. 매 번 복사하는 것도 비효율적   
때문에 `?.`연산자 활용

> `?.` : `?` 왼쪽의 항이 null이 아니라면 `.` 뒷부분을 실행하겠다는 의미

```cs
ExampleEvent?.Invoke();
```

함수의 경우 `Invoke`를 붙여서 호출 가능. 그리고 이 연산자의 경우 **원자적으로(처리 중간에 다른 것들이 끼어들 여지를 주지 않음) 수행이 되는 연산자**라서 이 연산 도중 다른 스레드가 개입할 여지가 없어 멀티 스레드 환경에서도 안전하게 돌아감

## this

- this 키워드
  - **클래스의 현재 인스턴스**를 가리키는 키워드
  - 매개변수 이름과 클래스 필드가 이름이 같다면 this로 구분할 수 있음
  - 클래스 내에서 클래스 필드를 사용할 때는 다 this가 생략된 경우

- 생성자 this()

생성자의 이름은 클래스 이름과 동일해야 하며 void 형식이어야 함

```cs
class MyClass
{
  int a;
  int b;

  public MyClass()
  {
    a = 10;
  }

  public MyClass(int b)
  {
    a = 10;
    this.b = b;
  }
}
```

다만 이 경우 너무 중복되는 코드들이 양산될 수 있어서 `this()` 생성자를 사용   
`this()`는 자기 자신의 생성자를 가리키며 이는 생성자에서만 활용이 가능

```cs
class MyClass
{
  int a;
  int b;
  
  public MyClass()
  {
    a = 10;
  }

  public MyClass(int b) : this()
  {
    this.b = b;
  }
}
```

`this()` 키워드는 생성자를 가리키기에 인자를 줘서 인자를 받는 다른 생성자를 가리킬 수도 있음

- 정적 함수 파라미터의 this
  - 정적 함수 파라미터에 하는 this는 `확장 메서드`를 만드는 데 사용되는 키워드
  - 멤버 함수를 호출하듯 함수 호출 가능

```cs
public static void Shuffle<T>(this IList<T> list);

List<MyClass> exList = new List<MyClass>();

Shuffle(exList);
exList.Shuffle(); // this 덕분에 가능
```

이런식으로 클래스나 인터페이스를 확장할 수 있음.   

다만 기존 클래스의 함수나 동일한 시그니처로 정의하면 호출하지 않게 됨.   
=> 확장 메서드는 컴파일 타임에 바인딩되는 데 컴파일러가 함수 호출을 볼 때 인스턴스 함수를 먼저 보게 되고 그 다음 확장 메서드를 보게 됨.   
따라서 확장 메서드가 우선순위에서 밀려 호출되지 않음

## List와 Dictionary

- 내부 자료구조

| 컨테이너              | 자료구조       |
| --------------------- | -------------- |
| `List<>`              | 배열           |
| `SortedSet<>`         | 레드-블랙 트리 |
| `HashSet<>`           | 해시 테이블    |
| `Dictionary<,>`       | 해시 테이블    |
| `SortedList<,>`       | 배열           |
| `SortedDictionary<,>` | 레드-블랙 트리 |


- List는 C++의 Vector와 유사
  - 메모리에는 배열처럼 올라감
  - 원소 삽입이 있을 때, `List`의 용량을 초과하게 되면 새 공간을 할당해 기존 원소들을 복사해가기에 최대 O(N) 시간 복잡도를 가짐
  - Remove의 경우 용량 변화 없음

- SortedSet은 set, HashSet은 unordered_set
  - 내부적으로 레드-블랙트리를 사용하는 자료구조는 정렬된 완전 이진트리이므로 삽입, 삭제에 있어 O(logN) 시간이 소요되며 해시를 사용하는 자료구조들은 대부분 O(1)이지만 최악의 경우 O(N)이 될 수 있음

- Dictionary는 unordered_map, SortedDictionary는 map
  - 항시 정렬된 상태로 데이터를 저장하는 것 외에는 `Dictionary`사용이 더 좋음

## C# vs C++
- GC
  - 두 언어의 가장 큰 차이
  - C#의 GC는 `Mark and Sweep` 알고리즘에 기반을 두고 있으므로 힙에 할당된 객체에 대한 포인터 추적을 진행하게 됨. 이로 인해 C++의 생성-소멸 주기보다는 오버헤드가 걸릴 수 있음
  - 수 많은 객체들을 생성한 후 나중에 GC로 돌리게 되면 더 많은 시간을 사용하게 되므로 속도가 더 느려짐
  - 최근에는 세대 기반 알고리즘을 통해 '살아있을 가능성이 있는 객체'는 뒤로 물러나게 해서 GC 시간을 줄이려는 방향으로 발전 중

- 가상머신 (VM - Virtual Machine)
  - 초기 구동시에 JIT 컴파일러를 위해 한 번 대기하는 과정이 있음
  - C++로 작성된 프로그램은 대부분 32bit 코드로 컴파일 되며 64bit 운영체제에서도 32bit로 돌아감. JIT 컴파일러를 사용하는 C#의 경우 타겟 플랫폼에 대한 이해도를 가질 수 있어서 64bit에 맞춰 컴파일을 하게 됨. 

> **JIT 컴파일러** : Just-In-Time 컴파일러 바이트 코드를 컴퓨터 프로세스(CPU)로 직접 보낼 수 있는 명령어로 바꾸는 프로그램

- C++의 TMP / C#의 리플렉션
  - TMP
    - 정적인 프로그램
    - 컴파일 시간에 계산을 할 수 있다는 장점이 존재 (런타임에는 불가)
    - TMP로 팩토리얼을 O(1)로 계산할 수 있지만 이는 컴파일 타임, 사용자 입력에 따른 유동성을 고려하지 않은 것
  - 리플렉션
    - 런타임에 리플렉션을 통해 수행. trade-off(안정성-성능)가 존재

> **TMP**(Template Meta Programming) : 컴파일 도중에 실행되는 템플릿 기반의 프로그램을 작성하는 일

> **리플렉션** : 어떤 Type에 대한 정보를 가져오거나 접근하는 등의 작업을 런타임에 동적으로 수행할 수 있도록 해주는 기능. 리플렉션을 사용하면 런타임에서 메서드를 호출하거나 필드의 값을 바꾸는 등의 작업을 할 수 있다.

