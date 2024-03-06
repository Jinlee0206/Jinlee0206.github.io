---
layout: post
title:  "[C#] 확장 메서드"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp

toc: true

date:   2024-03-05
last_modified_at: 2024-03-05
comments : true
---
> <span style="font-size: 80%"> **출처** </span>
>> <span style="font-size: 80%"> [개발자 지망생](https://blockdmask.tistory.com/604)</span> 

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# 확장 메서드

## C# 확장 메서드 설명

**클래스 외부**에서 **클래스의 메서드 처럼 사용할 수 있는 새로운 메서드를 만드는 기법** 

- 확장 메서드를 만드는 방법
  - static class
  - static method
  - 첫번째 매개변수 키워드

```cs
// 확장 메서드 형식
namespace 네임스페이스 이름
{
  public static class 클래스 이름
  {
    public static 반환형 메서드이름(this 확장하려 하는 클래스, 매개변수들 , ... , )
    {
      // 코드 구현 부
    }
  }
}
```

```cs
// int 클래스를 확장한 예시
using BlockMask;
namespace BlockMask
{
  public static class IntExtension
  {
    // 확장 메서드
    public static string IntToString(this int num, string extraStr)
    {
      return num.ToString() + " " + extraStr;
    }
  }
}

// 사용부
class MainApp
{
  public static void Main()
  {
    int a = 10;
    int b = 20;

    string resultA = a.IntToString("BlockMask...");
    string resultB = b.IntToString("GitHub Blog...");

    Console.WriteLine(resultA); // 10 BlockMask...
    Console.WriteLine(resultB); // 20 BlockMask...
  }
}
```

- 확장 클래스를 선언할 때 static으로 선언 해주고   
- 확장 메서드를 만들 때도 static으로 선언을 해줌   
- 확장 메서드의 첫 번째 매개변수는 this 키워드를 사용한 후 확장하고자 하는 클래스 타입을 적어 줌

> int 클래스의 다른 메서드들처럼 확장 메서드도 `인스턴스.함수이름()` 형태로 사용 가능

## 확장 메서드 사용 이유
- **C# 혹은 외부 (dll)에 이미 정의되어 있는 클래스들에 새로운 기능이 있는 메서드를 추가해야 할 때**
  - int 클래스에 새로운 메서드를 추가하고 싶다거나, string 클래스에 새로운 메서드를 추가하고 싶을 때

- **메서드를 새롭게 만들려 하는 클래스가 이미 상속등으로 인해 영향을 클래스가 많을 때 확장 메서드로 만든 함수는 자식클래스에서 재정의 할 수 없기 때문**
  - 클래스 내부에 메서드를 하나 추가하게 되면 해당 클래스를 상속 받는 메서드들이 overriding 등의 문제가 생길 수 있음

- **오히려 영향을 주고 싶은 클래스가 많을 때**
  - `System.Collection.Generic.IEnumerable<T>`에 확장 메서드를 만들어버리게 되면 `IEnumerable<T>`을 상속받는 모든 컬렉션에서 해당 확장 메서드를 사용할 수 있게 됨

## C#에서 확장 메서드의 사용처
C#의 LINQ에서 확장 메서드를 사용하고 있음   

기존에 int에는 OrderBy 메서드가 없음 => LINQ 네임스페이스를 임포트 하는 순간 사용 가능

## C# 확장 메서드 예시 코드

1. 기존

```cs
```

2. 사용자 정의 클래스에 확장 메서드 추가하기

```cs
```

3. 확장 메서드와 내부 인스턴스 메서드의 우선순위
```cs
```