---
layout: post
title:  "[Unreal] 언리얼 기초 C++_04"
excerpt : ""
categories: develop
tags: cpp unreal
toc: true
comments : true
use_math : true

date: 2024-05-26
last_modified_at: 2024-05-27
---
> <span style="font-size: 80%">
본 문서는 어소트락 언리얼엔진 게임프로그래머 양성과정의 강의를 토대로 필기한 내용입니다 </span>

>> 참조 : [평생 공부 블로그 : Today I Learned‍ 🌙
](https://ansohxxn.github.io/cpp/chapter8-10/#chapter-8-%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5%EC%9D%98-%EA%B8%B0%EC%B4%88--static-%EC%A0%95%EC%A0%81-%EB%A9%A4%EB%B2%84-%EB%B3%80%EC%88%98)
<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## static 멤버 변수
- 모든 객체가 한 메모리를 **공유**하는 멤버 변수
- DS에 할당
- **static 멤버의 메모리**는 클래스 객체가 생성될 때 생성되지 않고, **프로그램 시작 시에 생성**되기 떄문에 **클래스의 메모리 크기에 영향을 주지 않음**
- **객체를 여러 개 생성하더라도 정적 멤버는 하나만 존재**
- **객체를 생성하지 않아도 사용 가능**
- 클래스 안의 정적 변수
  - 전역 범위에서 `자료형이름 클래스이름::static변수이름 = 초기화할 값` 형식으로 초기화 필요

<p align = "center">
    <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/f14790c0-5913-48f9-9c38-9170846c7bed" width = 320>
</p>


## static 멤버 함수
- 객체와 독립적, 객체 생성과 상관 없음
  - 멤버 변수는 객체가 생성되어야 메모리를 할당 받기 때문에 static 멤버 함수 내에서는 **멤버 변수를 사용할 수 없다**
  - 전역에서 메모리가 할당되는 **static 멤버 변수는 사용 가능**
- CS에 할당
- static 멤버 함수는 객체들의 생성과 무관하며 언제 어디서든 클래스 이름으로도 접근이 가능해야 하기 때문에 **static 멤버 함수 내부에서는 this 포인터를 사용 불가**

```cpp
// static 멤버 변수와 static 멤버 함수
#include <iostream>
using namespace std;

void TestStatic() {
	static int Number = 100; // 정적 멤버 변수 DS에 할당된다
	++Number;
	cout << Number << endl;
}

class CStatic
{
public :
	CStatic()
	{
	}

	~CStatic()
	{
	}
public :
	long long mA;
	static int mB; // 선언만 가능, 전역 변수와는 다름, 클래스에 소속된 정적 멤버 변수

	void Output()
	{
		cout << "A : " << mA << " B : " << mB << endl;
	}

	// 정적 멤버 함수는 CS에 할당된다
	// static 멤버함수는 this가 없다
	// static 멤버 함수 안에서는 static 멤버 변수와 static 멤버 함수만 사용 가능
	static void OutputStatic()
		
	{
		// mA = 100; // 비정적 멤버참조는 특정 객체에 상대적이어야 한다 // (this->) 가 없음
		mB = 200;
		// this->Output(); // 멤버 변수 호출 불가
		cout << mB << endl;
	}
};

int CStatic::mB = 0; // 초기화를 클래스 외부(전역 범위)에서 진행 

int main()
{
	// static 키워드
	TestStatic(); // 101
	TestStatic(); // 102
	TestStatic(); // 103
	
	// static 멤버 변수
	CStatic st1, st2, st3;
	st1.mB = 500;

	cout << sizeof(CStatic) << endl; // long long 타입 mA 8바이트만 CStatic 사이즈에 영향을 끼친다

	st1.mA = 200;
	st2.mA = 300;
	st3.mA = 400;

	st1.mB = 500;
	st2.mB = 600;
	st3.mB = 700;
	CStatic::mB = 800; // 클래스안의 정적 멤버 변수 유일하기 때문에 CStatic:: 접근지정연산자로 접근 가능

	st1.Output(); // A : 200 B : 800
	st2.Output(); // A : 300 B : 800
	st3.Output(); // A : 400 B : 800

	// static 멤버 함수
	st1.OutputStatic();
	st3.Output(); // A : 400 B : 200

	st1.OutputStatic();
	st2.OutputStatic();
	st3.OutputStatic();
	CStatic::OutputStatic();
	
	// 함수 포인터로 함수 실행하기 : static 멤버 함수
	void (*Func)() = CStatic::OutputStatic; 
	Func();

	// 함수 포인터로 함수 실행하기 : 일반 멤버 함수
	// void(*Func1)() = &CStatic::Output; // Error!, 일반 멤버 함수는 안됨
	void (CStatic:: * Func2)() = &CStatic::Output; // 일반 멤버 함수는 주소를 붙여줘야 한다
	// Func2(); // Error! // 객체로 접근해야 한다
	(st1.*Func2)(); // 멤버 함수 포인터는 객체의 내용물을 참조하기 때문에 (객체.*멤버함수포인터)(매개변수)로 사용해야 한다.


	return 0;
}
```

### 멤버 함수 포인터와 static 멤버 함수
- 멤버 함수 포인터는 `&클래스이름::함수이름`으로 접근해야 한다
- 멤버 변수는 각 객체들마다 따로 메모리를 가져 주소가 다르지만
- 멤버 함수는 객체마다 함수 메모리를 따로 갖는 방식이 아니기 때문
  - 멤버 함수는 어딘가 한군데에 저장되어 있고 각 객체마다 그 공간에 동일하게 접근하여 각자의 다른 데이터로 사용하는 방식이다.
  - 따라서 일반 함수의 주소와는 다르게 멤버 함수의 주소를 받아와야 함
  - 그럴려면 속해있는 클래스가 어디인지 알려주어야 한다. `&Something::`
- 함수 포인터로 함수 실행하기
  - 일반 멤버 함수
    - 포인터 선언 시 `&클래스이름::함수이름`으로 선언해야 함
    - 함수 포인터로 일반 멤버 함수를 실행할 때, 꼭 `(객체.*멤버함수포인터)(매개변수)`로 실행해야 함
    - 함수 포인터는 객체에 종속되는 일반 멤버 함수를 참조하고 있기 때문에 단독으로 호출할 수 없기 때문
    - 포인터로 객체를 참조하고 있다면 `(객체->*멤버함수포인터)(매개변수)`로 실행
  - static 멤버 함수
    - 객체와 무관하게 연산이 이루어 짐
    - **일반 함수 포인터**로 취급 받는다 

```cpp
// 함수 포인터로 함수 실행하기 : 일반 멤버 함수

// void(*Func1)() = &CStatic::Output; // Error! // 일반 멤버 함수는 안됨
void (CStatic:: * Func2)() = &CStatic::Output; // 일반 멤버 함수는 주소를 붙여줘야 한다

// Func2(); // Error! // 객체로 접근해야 한다
(st1.*Func2)(); // 멤버 함수 포인터는 객체의 내용물을 참조하기 때문에 (객체.*멤버함수포인터)(매개변수)로 사용해야 한다.
```

```cpp
// 함수 포인터로 함수 실행하기 : static 멤버 함수

void (*Func)() = CStatic::OutputStatic; // static 멤버 함수는 주소를 붙여주지 않아도 호출 가능
Func(); // output
```
- std::functional 

```cpp
#include <functional>
using namespace std;
// CStatic 클래스 생략 //
int main()
{
    function<void()> Func3 = CStatic::OutputStatic;
    Func3;
    Func3 = bind(&CStatic::Output, st1); // 함수주소와, 객체를 묶어서 사용도 가능
    Func3();
    return 0;
}
```

## friend
- 클래스에서 어떤 함수나 다른 클래스를 friend, 즉 친구로 지정하면 자신의 private 멤버들에도 자유롭게 접근할 수 있게 허락해 줌
- 클래스를 친구로 삼는 경우
  - `class A { friend class B;}`
  - A 라는 클래스에서 B 라는 클래스를 통째로 친구 삼으면 B 클래스의 모든 멤버 함수에서 A 의 private 멤버들에도 자유롭게 접근할 수 있게 됨
- 다른 클래스의 특정 멤버 함수만 친구로 삼고 싶을 때
  - `friend void B::doSomething(A &a);`

```cpp
class CClass1
{
	friend class CClass2;
private:
	int mA;
};

class CClass2
{
private :
	CClass1 mC;
public :
	CClass2() {
		mC.mA = 100;
	}
	~CClass2() {

	}
};
```

## 싱글톤
- static 멤버를 이용해서 객체 생성을 컨트롤하는 디자인 패턴 중 하나
- 게임 상이나 메모리 상으로 단 하나만 존재하며 언제 어디서든 사용 가능한 오브젝트를 만들 때 사용되는 패턴

```cpp
#include <iostream>
using namespace std;

class CSingleton
{
private:
	CSingleton()
	{
	}

	~CSingleton()
	{
	}

public:
	void Output()
	{
		cout << "CSingleton Output Function" << endl;
	}

private:
	static CSingleton* mInst; // 동적 할당을 통해 할당하면 메모리 제어가 용이

public:
	static CSingleton* GetInst()
	{
		if (nullptr == mInst)
		{
			mInst = new CSingleton;
		}
		return mInst;
	}

	static void DestroyInst()
	{
		if (nullptr != mInst)
		{
			delete mInst;
			mInst = nullptr;
		}
	}
};

CSingleton* CSingleton::mInst = nullptr; // 포인터 변수 선언은 nullptr 할당과 동시에 진행하기

int main()
{
	// CSingleton single1; // private 생성자, 소멸자의 경우 객체 생성이 불가능
	CSingleton::GetInst()->Output();
	CSingleton::GetInst()->Output();
	CSingleton::GetInst()->Output();

	CSingleton::DestroyInst();

	return 0;
}
```