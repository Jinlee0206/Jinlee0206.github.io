---
layout: post
title:  "[C#] 스레드"
excerpt : "개발, 면접"
categories: develop
tags: devlog csharp

toc: true

date:   2024-03-05
last_modified_at: 2024-08-18
comments : true
---
> <span style="font-size: 80%"> **출처** </span>
>> <span style="font-size: 80%"> [베르의 프로그래밍 노트](https://wergia.tistory.com/187)</span> 

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# 스레드

여러 작업을 동시에 작업하기

 일반적으로 우리가 사용하는 **운영체제(Operation System, OS)**은 **멀티 태스크**를 지원한다. 그 덕분에 우리는 구글에서 자료를 찾으면서, 유튜브에서 강좌를 듣고, 동시에 비주얼 스튜디오에서 작업을 할 수 있으며 그와 동시에 오디오 재생 프로그램을 통해서 음악을 들을 수 있다. 이때 구글과 유튜브에 접속할 수 있게 해주는 브라우저, 코드 작업을 하는 비주얼 스튜디오, 음악을 재생하는 오디오 재생 프로그램이 각각 하나의 **프로세스(Process)**이다.   
 
 또 여기서 이 *프로세스는 하나 이상의 스레드(Thread)로 이루어진다*. **스레드**는 *프로세스를 여러 개의 조각으로 나눈 것*으로, 한 OS에서 여러 프로세스가 작업하는 것처럼, 한 프로세스에서 여러 스레드가 동시에 작업을 처리할 수 있게 해준다. 방금 앞에서 든 예시 중에 오디오 재생 프로그램을 예시로 들자면, 오디오 프로그램은 하나의 프로세스으로, 그 안에서 여러 스레드로 나뉘어서 한 스레드는 음악을 재생하고, 또 다른 스레드는 가사를 보여주면서 음악 재생 시간에 맞춰서 싱크를 맞추는 등의 방식으로 동시에 여러 가지 작업을 동시에 처리하는 것이다.


## 스레드 생성하기

```cs
using System;
using System.Threading;

namespace ThreadTest
{    
  class ThreadTestProgram    
  {
    public static void Main(string[] args)
    {
      Thread thread = new Thread(() => Run(0));
 
      thread.Start();
      Run(1);
    }

    public static void Run(int idx)
    {            
      Console.WriteLine(string.Format("Run {0} Start", idx));
        
      for (int i = 0; i < 100; i++)
      {
        Console.WriteLine(string.Format("Run {0} :: {1}", idx, i));          
      }
      
      Console.WriteLine(string.Format("Run {0} End", idx));        
    }
  }
}
```

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/6b49e2d9-0634-4495-aef0-83c5252d484c" width = "320">
</p>

> 스레드를 사용한 후의 실행결과는 어느 함수가 끝나기 전에 두 함수가 동시에 진행 됨

```cs
Thread thread = new Thread(() => Run(0)); // 스레드 객체를 생성하고 생성자의 매개변수로 스레드로 돌리고자 하는 함수를 넣어 줌
 
thread.Start(); // Start함수 호출
```

## 스레드 양보하기
- 스레드는 몇 번의 연산을 처리하고 잠시 다른 스레드에 처리 시간을 넘겨주고 다시 돌려 받음
- 스레드 프로그래밍에서는 CPU 점유 상태를 다른 스레드에 언제 얼마동안 양보할 지를 알리는 함수가 존재
- Thread.Sleep()
  - 해당 함수를 호출한 스레드가 매개변수의 시간만큼 쉬면서 다른 스레드에 처리 우선권을 양보하게 만듬
  - 매개변수의 시간 단위는 밀리세컨드(Milisecond)로 1000분의 1초에 해당함
  - 즉 위 코드에 적힌 시간으로는 0.001초 동안 다른 스레드에 처리 우선권을 양보한다는 의미

```cs
using System;
using System.Threading;
namespace ThreadTest
{
    class ThreadTestProgram
    {
        public static void Main(string[] args)
        {
            Thread thread0 = new Thread(() => Run(0));
       
            thread0.Start();
            Thread thread1 = new Thread(() => Run(1));
       
            thread1.Start();
        }
        
        public static void Run(int idx)
        {
            Console.WriteLine(string.Format("Run {0} Start", idx));
            
            for (int i = 0; i < 100; i++)
            { 
                Console.WriteLine(string.Format("Run {0} :: {1}", idx, i));
                Thread.Sleep(10);
            }
            
            Console.WriteLine(string.Format("Run {0} End", idx));
        }
    }
}
```

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/4a396cb2-c9b4-4560-aed9-c1fe9c7a5ac0" width = "320">
</p>

> Sleep() 함수를 사용하지 않을 때와는 다르게 허용된 시간에 최대한 몰아서 처리하지 않고 필요한 계산만 처리한 뒤에 바로 다른 스레드에게 처리 우선권을 넘기는 것을 확인할 수 있음

## 스레드 중단하기

```cs
using System;
using System.Threading;
namespace ThreadTest
{    
  class ThreadTestProgram
  {
    public static void Main(string[] args)
    {
      Thread thread0 = new Thread(() => Run(0));
 
      thread0.Start();
      Thread.Sleep(100);
 
      thread0.Abort();            
      Thread thread1 = new Thread(() => Run(1));
 
      thread1.Start();            
      Thread.Sleep(100);
 
      thread1.Join();
    }

    public static void Run(int idx)
    {
      Console.WriteLine(string.Format("Run {0} Start", idx));
    
      for (int i = 0; i < 100; i++)
      {
        Console.WriteLine(string.Format("Run {0} :: {1}", idx, i));
        Thread.Sleep(10);
      }
      Console.WriteLine(string.Format("Run {0} End", idx));
    }
  }
}
```

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/270af5d5-e0f5-452d-ba5c-79e33d569603" width = "180">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/dd1188eb-0acc-4586-88dd-16147dd9c850" width = "202">
</p>

> thread0은 Abort() 시키고 thread1은 Join() 시키는 코드   
> Run(0)는 반복문이 동작하던 도중에 중단되고, Run(1)은 End까지 무사히 호출되고 종료된 것을 확인 가능

## 스레드 동기화

여러 개의 스레드를 두고 작동하는 프로그램의 경우에 여러 스레드가 자원이나 변수를 공유하는 경우가 많음

```cs
// 스레드 동기화 예시 프로그램 - 적용(X)
class ThreadTestProgram
{
    public class Villige
    {
        public int population =1000; 
            
        public void AddVillager()
        {
            population++;
 
           for(int i = 0; i < population; i++)
            {
               for(int j = 0; j < population; j++)
                {
                    // 몇가지 처리를 하는 함수
                }
            }
            // 추가된 주민에게 주민번호 주기
           Console.WriteLine(string.Format("새 주민의 주민번호 :: {0}", population));
        }
    }
 
    public static void Main(string[] args)
    {
        Villige manager = new Villige();
        for(int i = 0; i < 10; i++)
        {
            new Thread(new ThreadStart(manager.AddVillager)).Start();
        }
    }
}
```

작은 마을을 키우는 게임을 만든다고 가정했을 때, 마을에 새로운 마을 주민이 태어나거나 새로 들어오면 인구 수를 늘려주고 몇 가지 처리를 한 뒤에 주민번호를 매겨주는 AddVillager() 함수를 구현했다. 그리고 주민번호는 고유한 번호이기 때문에 각 주민 마다 번호가 중복되어서는 안된다고 가정해보자. 이 때 마을 주민이 동시에 추가될 수도 있기 때문에 스레드 처리를 한다.

그런데 플레이 도중에 마을에 10명의 주민이 동시에 추가되었다고 해보자. 그러면 현재까지 1000명의 주민이 있었으니 그 뒤에 추가되는 주민들의 번호는 1001, 1002, 1003, ..., 1009, 1010이 되기를 기대할 것이다.

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/2464f767-4df3-41c1-8490-2006a7c0ad04" width = "320">
</p>

하지만 실행결과는 새 주민들의 주민번호가 중복되어서 발급되어 버렸다. 이러한 문제를 스레드 세이프 하지 않다(Not thread-safe)라고 하는데 이 문제를 해결하기 위해서 필요한 것이 바로 스레드 동기화이다. 스레드 동기화는 하나의 공용된 자원이나 변수에 여러 개의 스레드가 접근할 때, 스레드들이 순서를 지켜서 사용하고 다른 스레드가 사용 중일 때는 사용하지 못하게 만드는 것이다.

```cs
// 스레드 동기화
class ThreadTestProgram
{
    public class Village
    {
        public int population = 1000;
 
        public object populationLock = new object();
 
        public void AddHuman()
        {
            lock (populationLock)
            {
                population++;
 
                for (int i = 0; i < population; i++)
                {
                    for (int j = 0; j < population; j++)
                    {
                        // 몇가지 처리를 하는 함수
                    }
                }
                // 추가된 주민에게 주민번호 주기
                Console.WriteLine(string.Format("새 주민의 주민번호 :: {0}", population));
            }
        }
    }
 
    public static void Main(string[] args)
    {
        Village manager = new Village();
        for(int i = 0; i < 10; i++)
        {
            new Thread(new ThreadStart(manager.AddHuman)).Start();
        }
    }
}
```

스레드를 동기화하는 방법은 lock을 사용사는 것이다. 스레드 락을 하기 위한 객체를 하나 만들어서 lock()을 해주면 lock() { } 으로 묶어준 블럭이 한 스레드에서 실행되는 동안에는 같은 객체의 lock으로 묶인 스레드는 멈춘 상태로 해당 코드를 진행하지 못하게 된다.

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/0db99850-fb7b-420c-8e62-32120ca5cd50" width = "320">
</p>

> 스레드를 lock() 함수로 동기화하여 실행하면 새로 들어온 주민들의 주민번호가 겹치지 않고 정상적으로 매겨짐

## 데드락

> 공유 자원에 대해서 타 프로세스가 선점한 자원을 필요로 하고 그 선점한 프로세스도 타 프로세스의 선점 자원을 원해 서로 무한정 기다리게 되는 것

스레드 동기화에도 단점은 있는 데 스레드 동기화 되는 부분은 동시에 처리가 안되고 한 스레드씩 작업을 진행하기 때문에 프로그램의 속도가 느려질 수 있다

<p align="center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/eeeb2e15-edd9-49ba-b220-0d251d1fdac2" width = "400">
</p>

그리고 스레드의 동기화 구조가 복잡한 경우라면, 위의 이미지처럼 두 개의 스레드가 두 자원을 사용하려고 할 때, 스레드 1이 자원 1을 사용하며 자원 2가 풀리기를 기다리고 있고 스레드 2가 자원 2를 사용하며 자원 1이 풀리기를 기다려서 두 스레드가 멈춰버리는 데드락(Dead lock, 교착상태)이 발생할 수도 있다.   
 

이렇게 스레드는 동시 처리를 하기에 유용한 방법이지만, 호출 순서를 보장할 수 없고 디버깅이 어려운 구조이기 때문에 잘못 사용할 경우 해결하기 어려운 문제를 발생시키기 쉽다. 그러므로 스레드를 사용할 때는 조심해서 사용해야만 한다.

- 데드락의 발생 조건
  - 공유 자원에 대한 상호 배제를 하고 있을 때
  - 추가적인 자원을 기다리고 있을 때
  - 선취 불가능 (No Preemptive)
    - 다른 프로세스 종료가 안되는 경우
  - 자원 관계에 있어서 사이클이 있을 때
    - 원하는 자원 -> 가지는 프로세스로 그래프를 그릴 때 사이클이 있는 경우

- 데드락의 해결 방법
  - 위 데드락의 발생 조건 중 1번 `상호 배제`를 제외한 것을 하나라도 제거 하면 해결 가능
  - 대부분의 경우 사이클 방지에 초점을 맞춤
  - 프로세스를 전부 종료하거나 데드락이 사라질 때까지 프로세스를 하나씩 중단 시킴
