---
layout: post
title:  "[Unreal] 언리얼 기초 C++_03"
excerpt : ""
categories: develop
tags: cpp unreal
toc: true
comments : true
use_math : true

date: 2024-05-25
last_modified_at: 2024-05-27
---
> <span style="font-size: 80%">
본 문서는 어소트락 언리얼엔진 게임프로그래머 양성과정의 강의를 토대로 필기한 내용입니다 </span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 객체
- 모든 사물을 의미
- 프로그래밍에서는 일반적으로 클래스를 이용해서 생성한 변수를 객체라고 한다
- 객체지향 프로그래밍
  - 다양한 객체를 조립하여 하나의 완성된 프로그램을 만들어 가는 것

## 클래스
- 객체지향 프로그래밍을 지원하기 위해 제공되는 문법
- 다양한 타입의 변수들을 클래스 안에 선언하고 사용할 수 있고 원하는 함수를 클래스 내에 구현하여 사용할 수 있다

```cpp
class 클래스명
{
	멤버변수
	멤버함수
	접근지정자
}

클래스명 변수명;
```

### 접근지정자
  - public : 클래스 내부(클래스 멤버함수 안)와 외부(클래스 멤버함수를 제외한 모든 곳) 모두 접근 가능
  - private : 클래스 내부에서는 접근이 가능하지만 외부에서는 접근이 불가능
  - protected : 클래스 내부와 자식 클래스 내부에서는 접근이 가능하지만 외부에서는 접근이 불가능
  - 구조체와 클래스의 차이
    - 구조체는 default가 public
	- 클래스는 default가 private

  - 도트연산자와 화살표연산자
    - 도트연산자(.), 멤버 접근 연산자 : 클래스의 멤버를 직접적으로 접근
    - 화살표 연산자(->) 간접 참조 연산자 : 포인터가 가리키는 주소의 데이터를 접근 
    - `(*포인터이름).멤버변수이름 = 값` = `포인터이름->멤버변수이름 = 값`


```cpp
#include <bits/stdc++.h>
using namespace std;

class CMonster
{
// 접근지정자 없으면 default는 private
private:
	// 멤버 변수
	char mName[32] = {}; // 선언 및 초기화
	int mAttack = 10;

public:
	// 멤버 함수
	void SetAttack(int Attack)
	{
		mAttack = Attack;
	}

	void Output()
	{
		printf("Name : %s\n", mName);
		cout << "Attack : " << mAttack << "\n";
	}

	void Output1()
	{

	}

private:
	void Output2()
	{
		mAttack = 500; // 클래스 내부에서는 private이라도 사용 가능
	}
};

int main()
{
	CMonster monster1; // 클래스 선언
	CMonster* monster2 = new CMonster; // 클래스 동적할당

	//monster1.mAttack = 100; // (Error) 변수 mAttack 접근 지정자 private, 클래스의 외부이므로 접근이 안됨 
	monster1.SetAttack(100); // 클래스 멤버 함수 호출
	monster2->SetAttack(200); 
	
	monster1.Output();
	monster2->Output();
	monster3.Output();


	delete monster2;
	return 0;
}
```


- 생성자와 소멸자
  - 생성자 : 클래스를 이용하여 객체를 생성할 때 자동으로 호출해주는 함수
  - 소멸자 : 클래스가 삭제될 때 자동으로 호출해주는 함수
  - 특징
	- 생성자와 소멸자의 이름은 클래스 이름과 같다
	- 생성자와 소멸자의 반환타입은 없다
	- 클래스에 생성자와 소멸자가 없을 경우 자동으로 디폴트 생성자와 소멸자가 생성되어 사용된다
    - 멤버 이니셜라이저 목록을 활용해서 멤버변수를 초기화 할 수 있다
    - 생성자 오버로딩이 가능
	  - 컴파일러는 매개변수와 인수가 일치하는 것을 자동으로 호출
   
```cpp
- 생성자
클래스명()
{

}

- 소멸자
~클래스명()
{

}
```

- 복사 생성자
  - 객체를 생성할 때 다른 객체를 인자로 받아서 인자로 받은 객체의 데이터를 생성하는 객체의 데이터에 복사할 때 사용한다 
  - **Swallow Copy(얕은 복사)** : 데이터를 있는 그대로 복사하는 방식
    - 만약 클래스의 멤버변수가 동적할당된 메모리 주소를 가지고 있는 포인터 변수가 있고 이를 소멸자에서 delete를 통해 제거 해야할 경우 같은 메모리 주소를 참조하게 되어 하나가 제거하면 다른 하나가 제거할 때 문제가 발생할 수 있다
    - 댕글링 포인터 : 이미 지워진 주소를 가리키는 포인터
  - **Deep Copy(깊은 복사)** : 데이터를 그대로 복사하는 데 만약 돌정할당된 메모리 주소가 있을 경우 새로 생성하는 객체도 동적할당 하여 메모리 공간을 만들어내고 그 공간에 있는 값만 복사하는 방식


```cpp
class CMonster
{
public:
	CMonster() : mName({}), mAttack(10) // 멤버 초기화 리스트
	{
		// 변수의 값을 지정하면 변수를 선언한 후에 값을 지정하는 것
		// mName = {}; 선언과 동시에 초기화 하는 코드이므로 에러가 남
		mArray = new int[10];
		printf("CMonster 생성자\n");
	}

	// 생성자 오버로딩
	CMonster(const char* Name) : mAttack(0)
	{
		strcpy_s(mName, Name);
		mArray = new int[10];
	}

	// 복사생성자
	CMonster(const CMonster& ref)
	{
		strcpy_s(mName, ref.mName);
		mAttack = ref.mAttack;
		// 얕은 복사
		// mArray = ref.mArray;

		// 깊은 복사 : 새로운 동적할당 공간을 새로만들고 거기에 데이터 복제
		mArray = new int[10];
		memcpy(mArray, ref.mArray, sizeof(int) * 10);
	}

	~CMonster()
	{
		// 보통 소멸자에서 동적할당 제거
		if (nullptr != mArray) delete[] mArray;
		printf("CMonster 소멸자\n");
	}

// 접근지정자 없으면 default는 private
private:
	// 멤버 변수
	char mName[32] = {}; // 선언 및 초기화
	int mAttack = 10;
	int* mArray = new int[10]; // 동적 할당된 배열

public:
	// 멤버 함수
	void SetAttack(int Attack)
	{
		mAttack = Attack;
	}

	void Output()
	{
		printf("Name : %s\n", mName);
		cout << "Attack : " << mAttack << "\n";
	}
};

int main()
{
	CMonster monster1, monster3("오크"); // 클래스 선언
	CMonster* monster2 = new CMonster(monster3); // 클래스 동적할당

	//monster1.mAttack = 100; // (Error) 변수 mAttack 접근 지정자 private, 클래스의 외부이므로 접근이 안됨 
	monster1.SetAttack(100); // 클래스 멤버 함수 호출
	monster2->SetAttack(200); 
	
	monster1.Output();
	monster2->Output();
	monster3.Output();


	delete monster2;
	return 0;
}
```

## this 포인터
  - 자기 자신을 가리키는 포인터
  - 멤버 함수가 호출된 객체의 주소를 가리키는 숨겨진 포인터
  - 호출된 객체의 주소를 가리키는 상수 포인터
    - 포인터 자체가 다른 것을 가리키게 할 수 없다
  - 멤버 변수의 이름과 멤버 함수의 매개 변수 이름이 같으면 명시적으로 this를 참조해서 구분할 수 있다
  - *this를 반환하는 방식으로 **함수 체이닝** 기법을 활용할 수 있다

```cpp
class CMonster
{
	char mName[32] = {}; // 선언 및 초기화
	int mAttack = 10;
	int* mArray = new int[10]; // 동적 할당된 배열

	void Output(CMonster* const this) // 컴파일러에 의해 이렇게 함수 정의가 변환된다.
	{
		cout << "Addr : " << this << '\n'; // 클래스의 멤버 함수가 실행 중인 개체에 대한 포인터

		// this-> 는 생략 가능
		printf("Name : %s\n", this->mName);
		cout << "Attack : " << mAttack << "\n";
	}
}

int main()
{
	CMonster monster1; // 클래스 선언

	cout << "mon1 : " << &monster1 << '\n'; // 몬스터의 주소값
	monster1.Output();
	return 0;
}
```

## 상속
- 클래스는 부모와 자식의 관계를 형성할 수 있다
- 자식 클래스는 부모 클래스의 멤버(변수, 함수)를 물려받아 사용할 수 있게 해주는 기능
  - 코드 재사용성 증가
- 생성자 소멸자 호출 순서
  - 순서 : 부모 생성자 - 자식 생성자 - 자식 소멸자 - 부모 소멸자
- 접근 제한자에 따른 분류
  - private 상속 : 내부에서는 부모의 멤버에는 접근 가능 but 외부에서는 접근 불가
  - public 상속 : 일반적인 상속, 내외부에서 모두 접근 가능
- 자식클래스의 이니셜라이져에서 부모클래스의 멤버변수를 초기화 할수 없다
- 상속관계를 형성하고 있는 클래스들은 서로 형변환이 가능

```cpp
부모클래스
{

};

자식클래스 : 접근지정자 부모클래스
{

};
```

### 함수 오버라이딩
- 함수의 재정의 : 부모클래스에 있는 함수를 자식클래스에 재정의
- override 키워드 : 부모에 있는 함수를 자식클래스에서 재정의할 때, 명시적으로 override 키워드를 사용할 수 있다
- final 키워드 : 더 이상의 재정의는 없다

```cpp
class CParent
{
	void Output()
	{
		printf("CParent Output Function\n");
	}
}

class CChild : public CParent
{
	void Output()
	{
		printf("CChild Output Function\n");
	}
}
```

```cpp
// 부모 클래스
class CParent
{
public:
	CParent()
	{
		printf("CParent 생성자\n");
	}
	~CParent()
	{
		printf("CParent 소멸자\n");
	}

public :
	int mA = 0;

	void Output()
	{
		printf("CParent Output Function\n");
	}
};

// 자식클래스 : 부모클래스 상속
class CChild : public CParent
{
public:
	CChild() : 
		// mA(10), // 자식클래스의 이니셜라이저에서 부모클래스의 멤버변수를 초기화할 수 없다
		mB(10)
	{
		printf("CChild 생성자\n");
	}
	~CChild()
	{
		printf("CChild 소멸자\n");
	}

public :
	int mB;

	void Output()
	{
		CParent::Output(); // 클래스 내부에서도 호출 가능

		printf("CChild Output Function\n");
	}

	void Output1()
	{
		printf("CChild Output1 Function\n");
	}	
};

// 자식클래스는 여러 개일 수 있다
class CChild1 : public CParent
{
public:
	CChild1() :
		// mA(10), // 자식클래스의 이니셜라이저에서 부모클래스의 멤버변수를 초기화할 수 없다
		mB(10),
		mC(10),
		mD(10)
	{
		printf("CChild1 생성자\n");

	}
	~CChild1()
	{
		printf("CChild1 소멸자\n");
	}

public:
	int mB;
	int mC;
	int mD;

	void Output1()
	{
		printf("CChild1 Output1 Function\n");
	}
};

int main()
{
	CParent parent;
	CChild child;
	CChild1 child1;

	// 멤버 변수 접근
	//parent.mA = 100;
	child.mA = 100; // 부모의 변수도 사용 가능
	child.mB = 200; 

	// 함수 호출 
	parent.Output(); // 부모에 공통적으로 해줘야하는 메서드 구현
	child.Output(); 
	child.CParent::Output(); // 범위 지정 연산자를 이용해 부모클래스에 만들어진 메서드를 호출할 수도 있다 
	child1.Output();

	//parent.Output1(); // Error!, 부모에는 없는 함수
    child.Output1(); // 자식에 존재할 경우 메서드 호출

	return 0;
}
```

### 업캐스팅과 다운캐스팅
- 업캐스팅 : 자식타입으로 생성된 객체를 부모타입으로 형변환
- 다운캐스팅 : 부모타입으로 생성된 객체를 자식타입으로 형변환 
  - 부모타입에 없는 자식타입의 변수를 참조하려고 시도할 수도 있기 때문에 문제가 생길 수 있다

```cpp
// 업캐스팅
CParent* ConvertParent = (CParent*)&child; 
CParent* ConvertParent1 = (CParent*)&child1;

// 다운캐스팅
CChild1* Child1Addr = (CChild1*)ConvertParent; 
Child1Addr->mC; // 없는 메모리에 접근하려고 시도
				// dynamic_cast를 활용하여 다운캐스팅 안전하게 하기
```

## 가상함수

```cpp
// 업캐스팅을 통해 자식클래스 객체를 부모타입으로 형변환
CParent* ChildArray[2] = {};

ChildArray[0] = new CChild;
ChildArray[1] = new CChild1;

for (int i = 0; i < 2; i++) delete ChildArray[i];

/*
CParent 생성자
CChild 생성자
CParent 생성자
CChild1 생성자
CParent 소멸자 
CParent 소멸자 // 현재 호출된 소멸자의 부모클래스의 소멸자만 호출됨
			  // CChild, CChild1 소멸자 호출되지 않음 (메모리 누수)
*/
```

- 클래스의 일반 멤버함수나 소멸자에 virtual 키워드를 붙여서 가상함수를 만들 수 있다
- 가상함수를 가지고 있는 클래스는 가상 함수 테이블이 만들어진다
  - **__vfptr 포인터(가상함수테이블의 메모리 주소를 담고 있는 포인터)**를 기본으로 가지게 된다 (8바이트가 기본)
- 가상함수테이블은 가상함수의 메모리 주소를 저장하기 위한 배열
- 동작 과정
  - 모든 가상함수를 호출하게 되면 가상함수테이블을 확인한다
  - 가상함수테이블에 저장되어 있는 주소를 찾아서 함수를 호출한다
- **dynamic_cast**
  - 자식클래스의 포인터를 부모 클래스의 포인터로 변경했다가 다시 자식 클래스의 포인터로 변경하는 경우, 동적 형변환 사용

```cpp
class CParent
{
public:
	CParent()
	{
		printf("CParent 생성자\n");
	}

	// 소멸자를 가상함수로 만들기
	virtual ~CParent()
	{
		printf("CParent 소멸자\n");
	}

public :
	virtual void Output()
	{
		printf("CParent Output Function\n");
	}
};

class CChild : public CParent 
{
public:
	CChild()
	{
		printf("CChild 생성자\n");
	}
	~CChild() // 소멸자는 이름이 달라도 재정의 된다(virtual은 부모클래스에만 해주면 됨)
	{
		printf("CChild 소멸자\n");
	}

public :
	virtual void Output() override // 부모에 있는 함수를 자식클래스에서 재정의할 때, 명시적으로 override 키워드를 사용할 수 있다
						// final   // 더 이상의 재정의는 없다
	{
		printf("CParent Output Function\n");
	}
};

class CChild1 : public CParent
{
public:
	CChild1()
	{
		printf("CChild1 생성자\n");
	}

	~CChild1()
	{
		printf("CChild1 소멸자\n");
	}
};

int main()
{
	printf("CParent Size : %lld\n", sizeof(CParent)); // 클래스의 기본 사이즈 1바이트
	printf("CChild Size : %lld\n", sizeof(CChild));
	printf("CChild1 Size : %lld\n", sizeof(CChild1));

	// 업캐스팅을 통해 자식클래스 객체를 부모클래스 객체타입으로 형변환
	CParent* ChildArray[2] = {};

	ChildArray[0] = new CChild;
	ChildArray[1] = new CChild1;

	CChild* ChildAddr = dynamic_cast<CChild*>(ChildArray[0]); // dynamic_cast를 활용한 부모 클래스 객체를 자식클래스 객체 타입으로 다운 캐스팅
	CChild1* ChildAddr1 = dynamic_cast<CChild1*>(ChildArray[0]); // 형제끼리의 형변환 캐스팅에 실패하면 nullptr이 나온다

	CChild1* ChildAddr2 = (CChild1*)ChildArray[0]; // C언어 타입의 다운캐스팅 (잘못된 타입의 형변환이 되도 값이 나옴)


	for (int i = 0; i < 2; i++) delete ChildArray[i];

	return 0;
}
```

### 순수가상함수
```cpp
// 기본 형태
virtual void OutputPure() = 0;

// 구현부 작성 가능
virtual void OutputPure1()
{
	CParent::OutputPure1();
	printf("CChild1 OutputPure1 Function\n");
}
```

- 순수가상함수는 자식 클래스에서 무조건 재정의 해야 함
- 순수가상함수를 가지고 있는 클래스를 추상 클래스라고 한다
  - 부모가 추상 클래스일 때, 자식 클래스에서 재정의를 하지 않으면 자식클래스도 추상클래스가 된다
- 추상클래스는 객체 생성이 불가능

## 연산자 오버라이딩
- 연산자를 재정의해서 연산자 기능을 커스터마이즈할 수 있다
- 연산자 오버로딩을 통해 전달인수에 따라 다른 메서드를 호출할 수 있다

```cpp
#include <iostream>
using namespace std;

struct FVector
{
	float x;
	float y;
	float z;

	// 연산자 오버라이딩
	// 클래스나 구조체의 멤버 함수 혹은 operator는 뒤에 const를 붙일 수 있다
	// 멤버함수나 operator 뒤에 const를 붙여주면 해당 멤버함수나 operator에서는 멤버변수의 값을 변경할 수 없다
	FVector operator+ (const FVector& v) const
	{
		// x = 30.0f; (Error)
		FVector resV;
		resV.x = x + v.x;
		resV.y = y + v.y;
		resV.z = z + v.z;
		return resV;
	}

	// 연산자 오버로딩
	// operator+의 매개변수를 다르게 구현할 수 있따
	FVector operator+ (float v) const
	{
		FVector resV;
		resV.x = x + v;
		resV.y = y;
		resV.z = z;
		return resV;
	}

	// 할당 대입 연산자 재정의
	void operator+= (const FVector& v)
	{
		x += v.x;
		y += v.y;
		z += v.z;
	}

	void operator+= (float v)
	{
		x += v;
		y;
		z;
	}

	// new 연산자 재정의
	void* operator new(size_t Size)
	{
		printf("FVector New Operator\n");
		return nullptr;
	}

	// [] 랜덤 접근 연산자 재정의
	float operator[] (int idx) const
	{
		if (idx == 0) return x;
		else if (idx == 1) return y;
		else if (idx == 2) return z;
		else return 0.0f;
	}
};

int main()
{
	new FVector;
	FVector v1, v2, v3;

	v1.x = 10.0f;
	v1.y = 20.0f;
	v1.z = 30.0f;

	v2.x = 40.0f;
	v2.y = 50.0f;
	v2.z = 60.0f;

	const FVector v4 = v2; // 상수 구조체 or 상수 클래스 => 구조체나 클래스의 멤버변수 값을 변경할 수 없다
	v3 = v2;
	//v3 = v1 + v2; // Error! -> 연산자 오버로딩을 활용해 operator를 재정의
	
	// v1의 + 연산자 함수를 호출하겠다
	// 전달인수로 뒤의 v2를 넘겨줌
	v3 = v1 + v2;
	cout << v3.x << ' ' << v3.y << ' ' << v3.z << '\n'; // 50 70 90

	v3 = v1 + 30.0f;
	cout << v3.x << ' ' << v3.y << ' ' << v3.z << '\n'; // 40 20 30

	v3 = v4 + v2; // Error! //  const객체는 일반 멤버함수나 일반 operator를 호출할 수 없다
							// 단 뒤에 const가 붙은 멤버함수나 operator는 호출이 가능하다
	cout << v3.x << ' ' << v3.y << ' ' << v3.z << '\n'; // 80 100 120

	v1 += v2;
	cout << v1.x << ' ' << v1.y << ' ' << v1.z << '\n'; // 50 70 90

	cout << v1[1] << '\n';

	return 0;
}
```
