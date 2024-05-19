---
layout: post
title:  "[Unreal] 언리얼 기초 C++_02"
excerpt : ""
categories: develop
tags: cpp unreal
toc: true
comments : true
use_math : true

date: 2024-05-19
last_modified_at: 2024-05-19
---
> <span style="font-size: 80%">
본 문서는 어소트락 언리얼엔진 게임프로그래머 양성과정의 강의를 토대로 필기한 내용입니다 </span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 배열
- 하나의 이름으로 변수 여러개를 선언할 수 있게 해주는 자료구조

> `변수타입 배열명[배열 크기]`

- 배열의 이름은 해당 배열의 메모리 시작점의 주소이다
- 배열의 요소에 접근하기 위해서는 인덱스를 사용한다
  - 인덱스의 범위 : 0 ~ (배열 크기 - 1)
  - `배열명[인덱스] = 102;`
- 배열의 초기화

```cpp
#include <iostream>
#define TEXT(text) L##text // 유니코드 문자열을 만들어 줄 수 있는 전처리기
using namespace std;

int main()
{
	// 한글 언어설정 
	setlocale(LC_ALL, "");
	setlocale(LC_ALL, "korean");
	_wsetlocale(LC_ALL, L"korean");

	int array[100];
	cout << array << endl; // 00000038928FF660, 배열의 메모리 시작점의 주소 값
	printf("%d\n", array[0]); // 초기화 하지 않은 배열에는 쓰레기 값이 들어있다

	int array1 = {1, 2, 3, 4}; // 0번부터 3번 인덱스까지 값을 1, 2, 3, 4로 초기화하고 나머지는 0이 자동으로 초기화 됨

	for(int i = 0; i < 100; i++)
	{
		array[i] = i + 1;
	}

	char text[64] = "문자열입니다";
	wchar_t text1[64] = L"유니코드 문자열입니다"; // 문자열의 끝은 null 문자이기 때문에 초기화가 중요
	wchar_t text2[64] = TEXT("유니코드 문자열 테스트");

	printf(text);
	wprintf(L"wchar_t : %s\n", text1);
	wcout << text2 << endl;

	int array2[10][5] = {};
	array2[3][2] = 100;

	return 0;
}
```

## 구조체
- 다양한 타입의 변수를 모아서 사용자 정의 변수 타입을 만들어주는 기능

```cpp
struct 구조체명
{
	원하는 변수 선언
}
```

```cpp
#include <iostream>
#define TEXT(text) L##text // 유니코드 문자열을 만들어 줄 수 있는 전처리기
using namespace std;

namespace EJob
{
	enum Type : short {
		Knight,
		Archer,
		Magician
	};
}

struct FPlayer
{
	wchar_t Name[64];
	EJob::Type Job; // 구조체 멤버 맞춤 기본값이 4바이트
	// char myChar; // 남은공간 2바이트중에 1바이트를 활용할 수 있다
	int Attack;
	int Defense;
	int HP;
	int HPMax;
	int MP;
	int MPMax;
};

namespace EItemType
{
	enum Type
	{
		Weapon,
		Armor,
		Helmet,
		Glove
	};
}

struct FItem
{
	wchar_t Name[64];
	EItemType::Type Type;
	int Option;
	int Price;
	int Sell;
};

int main()
{
	_wsetlocale(LC_ALL, L"korean");

	FPlayer player = {};
	FPlayer playerArray[10];

	printf("%lld\n", sizeof(player)); // 구조체 멤버 맞춤, 서버/클라이언트 맞춰줄 필요가 있다

	player.Job = EJob::Knight;
	player.Attack = 30;

	printf("player atk : %d\n", player.Attack); // player atk : 30

	playerArray[2].Attack = 50;

	printf("playerArray[2] atk : %d\n", playerArray[2].Attack); // playerArray[2] atk : 50

	FItem inventorySlot[20] = {};
	inventorySlot[0].Type = EItemType::Weapon; //inventorySlot[0].Name = TEXT("목검"); // 선언 및 초기화를 하지않을 때 문자열을 할당할 수 없다
	wcscpy_s(inventorySlot[0].Name, TEXT("목검")); // 문자열 복사 함수

	wprintf(L"이름 : %s\n", inventorySlot[0].Name); // 이름 : 목검

	return 0;
}
```

## 포인터
- 메모리 주소를 저장하는 변수 타입
> `변수타입 *포인터명;`

- 모든 변수 타입은 포인터 타입 변수를 선언할 수 있다
- 다른 변수의 메모리 주소를 가지고 있으며 해당 변수의 주소에 접근하여 값을 변경하거나 얻어와서 사용할 수 있다
- 포인터는 다른 변수를 참조하는 경우가 아니라면 어떤 일도 할 수 없다
- 포인터는 메모리 주소를 저장하기 때문에 모든 타입의 포인터 변수의 크기는 32bit 에서는 4바이트, 64bit에서는 8바이트를 차지하게 된다
- 역참조
  - 포인터 변수가 다른 변수의 메모리 주소를 가지고 있을 경우 해당 주소에 접근하는 것
  - 포인터 변수 앞에 역참조연산자(*)를 붙여서 역참조
  - 간접 멤버연산자 (->) : 구조체 포인터를 이용하여 멤버에 접근할 때 사용하는 연산자

```cpp
struct FMonster
{
	wchar_t name[64];
	int attack;
	int defense;
};

int main()
{
	// 포인터 선언 및 초기화
	int num = 10;
	float num2 = 3.14f;

	int* ptr = &num; // 주소 지정 연산자 (&)
	printf("%p : %d\n", ptr, *ptr);
	// ptr = (int*)&num2; // 강제 형변환

	float* ptr2 = &num2; // 각 자료형에 맞는 포인터 타입을 생성하는 것이 좋다
	*ptr = 500; // 역참조연산자(*)를 통해 주소값에 접근 및 할당
	printf("Number = %d\n", num);
	printf("*Pointer = %d\n", *ptr);

	// 배열의 시작 주소를 포인터 변수에 저장하고 배열 인덱스의 주소값에 접근
	int arr[100] = {};
	int* ptrArr = arr; // ptrArr 변수에는 배열의 시작 주소를 대입되었다
					   // 배열의 시작주소를 제외하고는 배열 인덱스의 주소값에 접근할 수 있는 방법은 없다
					   // ptrArr 변수를 활용해서 배열 인덱스의 주소값에 접근 가능
	arr[3] = 500;
	ptrArr[3] = 1200;
	printf("Array[3] = %d\n", arr[3]);

	// 구조체와 포인터
	FMonster monster;
	monster.attack = 20;

	FMonster* monsterPtr = nullptr; // 포인터 초기화
	monsterPtr = &monster;
	(*monsterPtr).attack = 500; // 역참조연산자(*)의 우선순위는 멤버 참조연산자(.) 보다 낮다
	monsterPtr->attack = 200; // 간접멤버연산자(->) : 구조체 포인터를 이용하여 멤버에 접근할 때 사용하는 연산자
	printf("Monster Atk : %d\n", monster.attack);

	// 상수 포인터와 포인터 상수
	const int numConst = 100;
	const int* constPtr = &num; // 상수 포인터, 값이 변경되면 안되는 상수에 지정
	int num3 = 200;
	constPtr = &num3; // 상수 포인터는 포인터 주소는 변경 가능하다
	// *constPtr = 1000; // Error // 상수 포인터는 포인터가 가리키는 주소값은 변경 불가 (역참조 불가)

	int* const ptrConst = &num; // 포인터 상수, 참조하는 대상을 변경할 수 없다. 선언 및 초기화시에 사용한 메모리 주소만을 사용
	// ptrConst = &num3; // Error
	*ptrConst = 500; // 역참조는 가능

	const int* const constPtrConst = &num; // 상수 포인터 상수
	// constPtrConst = &num3; // Error
	// *constPtrConst = 500; // Error

	return 0;
}
```

### 다중 포인터
- 다중포인터도 결국은 포인터 변수이다
- 주소를 저장하는 대상의 차이가 있을 뿐 메모리 주소를 저장하는 것은 같다
- 이중 포인터는 포인터 변수의 메모리 주소를 저장하기 위한 변수
- 삼중 포인터는 이중포인터 변수의 메모리 주소를 저장하기 위한 변수

<p align = "center">
	<img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/188e6a75-3cda-42db-a66f-233320875344" width = 480>
</p>

```cpp
int main()
{
	int num = 10;
	int* ptr = nullptr;
	int** pptr = nullptr;
	ptr = &num;
	pptr = &ptr;
	printf("number = %d\n", num);
	printf("number address = %p\n", &num);
	printf("pointer = %p\n", ptr);
	printf("pointer address = %p\n", &ptr);
	printf("*pointer = %d\n", *ptr);
	printf("double pointer = %p\n", pptr);
	printf("double pointer address = %p\n", &pptr);
	printf("**double pointer = %d\n", **pptr);

	return 0;	
}
/*
number = 10
number address = 000000CE29CFFC84
pointer = 000000CE29CFFC84
pointer address = 000000CE29CFFCA8
*pointer = 10
double pointer = 000000CE29CFFCA8
double pointer address = 000000CE29CFFCC8
**double pointer = 10
*/
```

## 함수
- 특정 동작을 하는 코드를 미리 작성하고 필요할 때 해당 코드를 동작시킬 수 있게 해주는 기능
- 전달인자(argument)
  - 함수를 호출할 때, 필요로 하는 데이터나 변수가 있을 경우 전달인 자를 이용해서 데이터 변수를 함수로 넘겨준다
- 매개변수(parameter)
  - 함수 영역 안에서 쓰이는 변수
  - 함수를 호출할 때 주어지는 전달인자의 값을 대체하는 용도로 쓰임

```cpp
반환타입 함수명(매개변수)
{
	동작 코드
}
```

```cpp
int Add(int a, int b)
{
	return a + b;
}

void Output()
{
	cout << "Output Function" << endl;
}

int main()
{
	cout << Add(1, 2) << "\n";

	return 0;
}
```

### void 포인터
- void는 타입이 없다는 의미이기 때문에 일반 변수를 선언하여 사용하는 것이 불가능
- void의 포인터 타입 변수는 선언하여 사용이 가능하다
- void 포인터 타입의 변수는 어떠한 메모리 주소든 모두 저장이 가능하지만 역참조가 불가능
- 역참조가 필요하다면 원하는 타입으로 형변환을 해준 후에 역참조를 진행한다
- 단, 현변환을 할 때 int변수의 메모리 주소를 담고 있다면 int 포인터 타입으로 형변환을 하여 접근해야 한다

```cpp
int main()
{
	void* voidPtr = nullptr;
	int num = 500;
	voidPtr = &num;
	int* intPtr = (int*)voidPtr; // 다시 담고 있는 변수의 타입으로 형변환을 해야한다
	cout << *intPtr << '\n'; // intPtr 주소값을 출력 : 500

	float num2 = 3.14f;
	voidPtr = &num2;
	intPtr = (int*)voidPtr; // float 타입을 담고있는 voidPtr을 int형으로 형변환 하는 경우 
	cout << *intPtr << '\n'; // float 타입을 int 타입으로 강제 형변환 할 때 값 손실이 일어날 수 있다
	return 0;
}
```

### 함수 오버로딩
- 같은 이름의 함수를 다양한 형태로 만들어줄 수 있는 기능
- 함수 이름이 같고 매개변수의 타입 혹은 개수가 다르다면 오버로딩이 성립된다
- 반환타입은 오버로딩과 관련이 없다
- 디폴트 매개변수
  - 매개변수에 기본 값을 미리 설정해두는 기능
  - 이 함수를 호출하며 매개변수에 전달인수를 전달할 경우 기본 값은 무시되며 전달된 값을 사용하게 된다
  - 이 함수를 호출하며 인자에 값을 전달하지 않을 경우 인자의 값은 기본값으로 설정이 되어 사용하게 된다  
  - 함수의 디폴트 매개변수는 **가장 오른쪽 매개변수부터 왼쪽으로** 차례대로 설정해야한다
  - 함수 오버로딩 사용 시 **디폴트 매개변수의 개수가 동일한 문제**가 일어날 수 있으니 주의해서 사용

```cpp
int Add(int a, int b)
{
	return a + b;
}

int Add(float a, float b)
{
	return (int)(a + b);
}

int Add(int a, int b, int c)
{
	return a + b + c;
}

// 디폴트 매개변수
void OutputNumber(int n = 1111) // 인자를 0개 혹은 1개를 전달 받을 수 있다
{
	cout<< n << '\n';
}

// 함수의 디폴트 매개변수는 가장 오른쪽 매개변수부터 왼쪽으로 차례대로 설정해야한다
void OutputNumber(int n1, int n2 = 100) // 인자를 1개 혹은 2개를 전달 받을 수 있다
{
	printf("Number1 = %d, Number2 = %d\n", n1, n2);
}

int main()
{
	int num = Add(10, 20);
	num = Add(3.14f, 2);
	num = Add(1, 2, 3);

	OutputNumber(); // 1111
	OutputNumber(101010); // Error, 어떤 함수를 호출할 지 컴파일러가 판단하지 못함

	OutputNumber(500, 200);
	OutputNumber(500); // Error 
	return 0;
}
```

### 참조형 변수와 포인터
- 참조형 변수
  - 다른 변수를 참조할 때 사용
  - 레퍼런스는 처음 레퍼런스 변수가 선언될 때 반드시 참조할 대상을 지정해야 한다
  - `변수타입& ref`
  - 일단 초기화되면 값을 변경할 수 없다
  - 매개변수에 ref 변수를 사용해서 인자로 넘겨주며 참조 대상을 지정하는 식으로 많이 사용한다
  - const 키워드와 함께 함수 안에서 값이 변경되지 않게 사용하는 것이 안전하다
  - 인수의 값을 수정하려는 경우나 인수의 비싼 복사본을 만들지 않으려는 경우 함수 매개변수로 자주 사용
- 포인터
  - 다른 변수의 주소를 가지고 있는 변수

```cpp
int value = 5; 
int* const ptr = &value;
int& ref = value;
// *ptr과 ref는 동일하게 평가된다
*ptr = 5;
ref = 5;
```

```cpp
// Call by Value
void ChangeNumber(int n)
{
	n = 555;
}

// Call by Address
void ChangeNumber(int* n)
{
	*n = 555;
}

// Call by Reference
void ChangeNumberRef(int& n)
{
	n = 555;
}

int main()
{
	int num = 60;

	// 값으로 전달	
	cout << "num" << num << '\n';
	ChangeNumber(num);
	cout << "changed number : " << num << '\n';

	// 주소로 전달 (포인터)
	n = 60;
	cout << "num" << num << '\n';
	ChangeNumber(&num);
	cout << "changed number : " << num << '\n';

	// 참조로 전달
	n = 60;
	cout << "num" << num << '\n';
	ChangeNumberRef(num);
	cout << "changed number : " << num << '\n';

	return 0;
}
```

```cpp
// 인수의 비싼 복사본을 만들지 않으려는 경우 함수 매개변수로 사용
struct FTestStruct
{
	wchar_t test1[64];
	wchar_t test2[64];
};

void OutputTestStruct(const FTestStruct& str) // 구조체를 인자로 넘겨줄 때 ref로 지정
{
	//str.test1[2] = 1020; // 참조하는 대상의 값을 바꿀 수 없다
}
```

### 함수 포인터
- 함수의 이름은 해당 함수의 주소가 된다
- 함수의 타입을 결정하는 요소는 함수의 반환타입과 함수 매개변수에 의해 결정이 된다
- 형식 : `반환타입 *포인터변수이름(인자타입);`

```cpp
void Output()
{
	cout << "Output Function" << '\n';
}

void Output1()
{
	cout << "Output1 Function" << '\n';
}

int main()
{
	void (*Func)(); // 함수 포인터 선언, 함수의 주소를 저장하기 위한 포인터 변수
	Func = Output;
	Func(); // Output Function
	Func = Output1;
	Func(); // Output1 Function
	return 0; 
}
```

## 동적할당
- new 키워드
  - 메모리를 동적으로 할당하고 해당 메모리 주소를 반환해주는 기능
  - 형식 : `new 할당할타입;`
  - int의 크기인 4바이트 만큼 메모리에 공간을 만들고 만들어 준 메모리의 주소를 반환해준다
- delete 키워드
  - 동적할당한 메모리는 사용이 끝나면 반드시 제거를 해주어야 한다
  - 형식 : `delete 할당된메모리주소`
- 동적배열 할당
  - `new 타입[개수]`를 이용해서 배열을 할당할 수 있다

```cpp
// 동적할당
int* dynamicInt = new int;
// 동적할당 제거
delete dynamicInt;

// 동적배열 할당
int* dynamicIntArr = new int[100];
// 동적배열 제거
delete[] dynamicIntArr;

// 정적배열
int staticIntArr[100] = {};
int* staticIntArr = staticIntArr;
```

```cpp
// 이중포인터의 사용 예시
void TestAllocate(int** ptr)
{
	*ptr = new int;
}

int main()
{
	int* intAlloc = nullptr;
	TestAllocate(&intAlloc);
	delete intAlloc;
	return 0;
}
// intAlloc의 값으로 new int로 동적할당한 메모리 주소를 할당
```