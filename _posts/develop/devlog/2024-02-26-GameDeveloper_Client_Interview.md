---
layout: post
title:  "[유니티 클라이언트 개발] 면접 준비"
excerpt : "개발, 면접"
categories: develop
tags: devlog unity

toc: true

date:   2024-02-26
last_modified_at: 2024-02-26
comments : true
---
> <span style="font-size: 80%">
Romanticism-GameDeveloper 님의 게임 개발자 면접 정리본과 평생 공부 블로그의 자료를 참고로 만든 정리 자료입니다.

- [GameDeveloper_Interview_GitHub 링크](https://github.com/Romanticism-GameDeveloper/GameDeveloper-Client-Interview?tab=readme-ov-file)  

- [평생 공부 블로그 링크](https://ansohxxn.github.io)</span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# C++

## struct와 class
- 공통점
  - 사용자 정의 데이터를 정의할 때 사용
  - 데이터 멤버와 함수를 가질 수 있음
- 차이점
  - struct는 단순 데이터의 집합체로 사용됨 / 기본 접근지정자는 public / 스택 할당 / 값 형식
  - class는 객체 지향 프로그래밍에서 객체를 정의하기 위해 사용 / 기본 접근 지정자는 private / 힙 할당 / 참조 형식 
  - 초기화 방식도 아래 방식처럼 차이가 있음

```cpp
struct SomeStruct
{
    int var;
    int varPrivate;
};

class SomeClass
{
    int var;
    int varPrivate;
};

int main(){
    SomeStruct someS = {1, 2};
    SomeClass  someC = {1, 2};  // ERROR
}
```

## 포인터와 정적 배열

```cpp
int array[5] = {0, 1, 2, 3, 4}; 
int * ptr = array;
```

- 포인터와 배열 이름은 일맥상통 하는 면이 있다
  - 배열의 이름은 **포인터 상수**나 마찬가지 (변하지 않는 주소 값)
  - 배열의 이름은 배열의 첫번째 원소의 주소
    - array와 &array[0]은 같다
  - 배열의 첫번째 원소를 간접 참조
    - *array와 array[0]은 같다
  - ptr 포인터 변수가 array 배열의 주소를 담게 되었으니 이제 ptr로 array 배열에 간접참조 가능

- 배열의 이름과 포인터의 차이

```cpp
int array[5] = {0, 1, 2, 3, 4};
int * ptr = array;

sizeof(array) // 20, 배열의 총 메모리 크기 리턴
sizeof(ptr)   // 4, 포인터의 크기만 리턴
```

- 함수 파라미터로 배열을 넘길 때  

```cpp
void printArray(int * array)
{
    cout << sizeof(array) << endl;  
    // 지역변수 array에 배열의 이름이 들어오든 포인터가 들어오든 지역변수 array는 포인터이므로 언제나 4가 출력될 것. 
}

int main()
{
    int array[5] = {0, 1, 2, 3, 4}; 
    int * ptr = array;
    
    printArray(array);  // 배열의 주소가 printArray 함수 내부의 array 지역변수에 대입된다. 
    printArray(ptr); // 포인터 ptr 값이 printArray 함수의 지역변수 array에 대입된다.
}
```

- **함수의 매개변수로 배열을 전달할 때는 포인터로서 전달된다.** (주소만 복사되서 넘어감)
  - 매개변수를 포인터로 선언해서 배열의 이름만 복사하여 넘겨주면 포인터로 배열 원소에 접근할 수 있으므로 효율적이다.

- 구조체 혹은 클래스의 멤버로 속한 배열을 매개변수로 넘길 때
  - 포인터로 전환되지 않고 배열 그 자체가 그대로 넘겨짐


## malloc/free vs new/delete
- malloc/free는 함수, new/delete 연산자인게 가장 큰 차이점
- malloc/free
  - #include "stdlib.h" 헤더에 포함
  - malloc는 void함수이기 때문에 반환받은 걸 해당 타입의 포인터로 형변환하는 과정이 필요
  - 생성자를 호출할 수단이 없기에 초기 값을 줄 수 없다는 단점이 있다
  - free도 소멸자를 부르는게 아닌 함수 호출
  - malloc으로 할당한 경우에는 *realloc*을 활용해 메모리 크기를 조정할 수 있다.
- new/delete
  - new/delete는 기본적으로 C++ 내 있는 키워드로 따로 include 할 필요 없음
  - new를 했을 시에는 반환값이 해당 타입의 포인터로 자동 반환이 됨
  - 생성자를 호출 해 초기값을 줄 수 있음
  - delete는 소멸자를 호출해 줌
  - new로 할당한 경우 직접적으로 메모리 크기를 조정할 수 없음 => 재할당이 많다면 malloc이 더 나은 방법이 될 수 있다.

## Overloading / Override
- 오버로딩 : 같은 이름의 함수에 인자를 다르게 해 다른 함수가 실행되게 하는 것
- 오버라이드 : 상속 관계에서 부모 클래스의 가상 함수를 자식 클래스에서 재정의 하는 작업

- 오버로딩 특징
  - 함수명이 같은 경우
  - 리턴 타입이 같은 경우
  - 인자의 종류, 갯수가 달라 구분이 되는 경우
    - 인자의 이름만 다르다면 오버로딩 불가능 (같은 함수로 처리해 컴파일 불가능)
  - 인자의 const성으로 구분은 *포인터 종류*만 가능

```cpp
void Func(int& a)
{
  cout << "normal reference a";
}

void Func(const int& a)
{
  cout << "const reference a";
}

void Func(int* a)
{
  cout << "normal pointer a";
}

void Func(int a)
{
  cout << "just a";
}

// void Func(const int a)
// {
//   cout << "const a";
// }  // !Error
```

위에서 일반 변수로 들어온 a의 경우 const 유무에 상관없이 복사를 하기 때문에 const로 받는지 아닌지를 판단해야할 이유가 없음. 함수 외부의 변수가 영향을 받을 일이 없기 때문.   

단 포인터나 참조의 경우 이게 const인지 아닌지에 따라서 함수 외부 값이 변경될 우려가 있기에 const 유무에 따른 오버로딩을 지원하게 되는 것.

- 오버라이드 특징
  - 함수에 없던 const성을 파생 클래스에서 재정의할 수 없다

```cpp
class Base
{
  public:
    virtual void Print(int a) {cout << a;}
};

class Derived : public Base
{
  public:
    // Error!
    virtual void Print(const int a) const override {cout << a;}
};
```

- 파생클래스에서 인자에 const를 붙일 수 있다

```cpp
class Base
{
  public:
    virtual void Print(int a) {cout << "Base\n"}
};
class Derived : public Base
{
  virtual void Print(cont int a) override {cout <<"Derived\n"}
};
```

- 함수 인자의 기본 값은 파생클래스에서 재정의할 수 없다

```cpp
#include <iostream>
using namespace std;

class Base
{
  public:
    virtual void Print(int a = 1) {cout << a;}
}
class Derived : public Base
{
  public:
    virtual void Print(const int a = 3) override {cout << a;}
}

int main()
{
  Base* b1 = new Base();
  Base* b2 = new Derived();

  b1->Print(); // 1
  b2->Print(); // 1
}
```