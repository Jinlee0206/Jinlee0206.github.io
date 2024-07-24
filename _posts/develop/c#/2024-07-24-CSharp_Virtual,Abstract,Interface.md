---
layout: post
title:  "[C#] 다형성_Virtual, Abstract, Interface"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp unity

toc: true

date:   2024-07-24
last_modified_at: 2024-07-24
comments : true
---
> <span style="font-size: 80%"> **출처** </span>
>> <span style="font-size: 80%"> [건앤로즈's Blog_생각대로 살지 않으면 사는대로 생각한다.](https://hongjinhyeon.tistory.com/m/93)</span>   
</span> 


<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

<span style ="font-size: 120%"> 객체 지향언어에서 **상속**을 얘기할 때, 사용되는 한정자 </span>

# Virtual 
- 가상 키워드
- 메서드, 속성, 인덱서 또는 이벤트 선언을 한정하는데 사용된다
- 파생 클래스에서 필요에 따라서 **재정의**할 수 있지만 필수는 아님
- virtual 한정정자를 사용한 클래스는 **완벽한 기능**을 제공할 수 있다

```cpp
public class Animal
{
  public virtual void Speak()
  {
    Console.WriteLine("Nothing!");
  }
}

public class Dog : Animal
{
  public override void Speak()
  {
    Console.WriteLine("멍멍!");
  }
}

Dog dog = new Dog();
dog.Speak(); // 멍멍
```

# Abstract
- 추상 키워드
- **불완전**하며, 파생클래스에서 구현해야 하는 클래스 및 클래스 멤버를 만들 수 있다
- 추상 클래스의 사용목적은 여러 개의 파생 클래스에서 공유할 기본 클래스의 **공통적인 정의를 제공** 하는 것
- 추상 클래스는 인스턴스화 할 수 없다

```cpp
public abstract class Animal
{
  public abstract void Speak();
}

public class Dog : Animal
{
  public override void Speak()
  {
    Console.WriteLine("멍멍!");
  }
}

Dog dog = new Dog();
dog.Speak(); // 멍멍!
Animal animal = new Animal(); // Error!, 추상 클래스는 인스턴스화 할 수 없다
```

# Interface
- 인터페이스는 abstract와 비슷하지만 멤버변수(필드)를 사용할 수 없다
- 대신 **프로퍼티**는 사용가능
- 인터페이스는 보통 **여러 클래스에 공통적인 기능을 추가**하기 위해 사용

```cpp
public interface Animal
{
  void Speak();
  string Name
  {
    get;
    set;
  }
}

class Dog : Animal
{
  private string name;
  public void Speak()
  {
    Console.WriteLine(name + "-> 멍멍!");
  }
  public string Name
  {
    get
    {
      return name;
    }
    set
    {
      name = value;
    }
  }
}

Dog dog = new Dog();
dog.Name = "흰둥이";
dog.Speak(); // 흰둥이->멍멍!
```

# 정리

|    |    |
| -- | -- |
|**virtual**| 하나의 기능을하는 완전한 클래스 <br> 파생클래스에 상속해서 추가적인 기능 추가 및 virtual 한정자가 달린 것을 재정의해서 사용 가능 |
|**abstract**| 여러 개의 파생클래스에서 공유할 기본 클래스의 공통적인 정의만 하고, 파생 클래스에서 abstract 한정자가 달린 것을 모두 재정의 해야 함. <br> 추상 클래스는 인스턴스화 할 수 없다 |
|**interface**| 추상 키워드와 비슷하지만 *멤버 변수를 사용할 수 없다* <br> 보통 abstract는 개념적으로 계층적인 구조에서 사용되며, interface는 서로 다른 계층이나 타입이라도 기능 추가를 위해 사용한다