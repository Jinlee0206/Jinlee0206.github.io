---
layout: post
title:  "[Unreal] 언리얼 기초 C++"
excerpt : ""
categories: develop
tags: cpp unreal
toc: true
comments : true
use_math : true

date: 2024-05-18
last_modified_at: 2024-05-18
---
> <span style="font-size: 80%">
본 문서는 어소트락 언리얼엔진 게임프로그래머 양성과정의 강의를 토대로 필기한 내용입니다 </span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 프로젝트

```cpp
#include <iostream> // 전처리기
using namespace std; // 네임스페이스

// 콘솔프로젝트의 시작점
// 코드블럭 안에 작성한 작업이 진행된다
int main()
{
    printf("출력 테스트\n");          // C언어 타입의 출력
    std::cout << "cout 출력테스트\n"; // :: 범위 지정 연산자
								     // std 안에 있는 cout을 의미

    return 0;
}
```

## 전처리기
- 전처리기에서 지원하는 기능 여기에 포함시켜서 사용할 수 있게 해준다    
- 컴파일 전에 미리 처리

## 네임스페이스
- 이름 충돌을 막기 위함   
- iostream에서 지원하는 기능들은 std라는 namespace 안에 구현된 기능들이 많다

## 메모리
- 원하는 데이터를 저장할 수 있는 공간 -> 메모리
- 메모리에 공간을 만들고 데이터를 저장하는 방식

- 1bit : 0과 1 둘 중 하나를 표현할 수 있다
- 1byte : 8bit. $2^8$ = 256

## 변수 vs 상수
- 변수 : 변경 가능한 수
- 상수 : 변경 불가능한 수

- signed : 부호를 가짐. -, + 둘 중 하나를 가진다
- unsigned : 부호가 없다. +만 가짐


|       |        |                 |                          |
|------ | --------     | ---       |         -------            |
|타입	|	표현 종류	|	크기	|	표현 범위				 |
|bool	|	참 / 거짓	|	1byte	|	false(0) ~ true(1)		 |
|char	|	문자		|	1byte	|	-128 ~ 127				 |
|short	|	정수		|	2byte	|	-32768 ~ 32767			 |	
|int		|	정수		|	4byte	|	-2147483648 ~ 2147483647 |
|__int64	|	정수		|	8byte	|                       	|
|float	|	실수		|	4byte	|                               |
|double	|	실수		|	8byte	|                              |


`변수타입 변수명; // 선언 : 메모리에 변수타입 크기에 맞는 공간을 만든다`   
`변수명 = 100;   // 할당 : 해당 메모리 주소에 값을 할당한다`

- 변수명명법
  - 숫자로 시작할 수 없다
  - 특수문자는 _만 첫글자로 가능
  - 영문만 사용가능

## 형변환
- 해당 줄에서 일시적으로 다른 타입으로 인식하는 것
- 실수를 정수타입의 변수에 대입을 하게 되면 소수점 자리가 탈락하고 정수 부분만 대입됨

## 진법
- 10진수 : 0 ~ 9 사이의 숫자로 구성된 집합
- 2진수 : 0 ~ 1 사이의 숫자로 구성된 집합
- 16진수 : 0 ~ 15 사이의 숫자로 구성된 집합. 10 ~ 15 사이의 숫자는 A ~ F로 표현

## 연산자
- 대입연산자 : =
- 사칙연산자 : +, -, *, /, %
- 관계연산자 : <, >, <=, >=, ==, !=	/ 값 대 값을 연산하여 참,거짓이 결과로 도출
- 논리연산자 : AND(&&), OR(||), NOT(!)	/ 참,거짓 대 참,거짓을 연산하여 참,거짓을 결과로 도출
- 비트단위 논리연산자 : AND(&), OR(|), NOT(~), XOR(^) / XOR 연산 : 같0다1
- 시프트연산자 : <<, >> / 빠른 곱하기,나누기 (2의 n승 단위의 곱셈, 나눗셈)
- 증감연산자 : ++, --
- 복합대입연산자 : +=, -=, *=, /=, %=, &=, |=, ^=

## 조건문

###  if문
- 조건을 체크할 때 사용
- 여러 분기를 만들어서 여러 개의 분기 중 하나를 선택해서 동작 시켜야할 경우 사용 가능
- else, else if문
  - else, else if는 if 없이 혼자 사용 불가.
  - else는 if 하나에 하나의 else만 존재
  - else if는 if 하나에 여러개의 else if가 존재할 수 있다

```cpp
if (조건식) // 조건식이 true일 경우 코드블록 안의 코드를 동작시킨다
{
	동작할 코드
}
else if (조건식2)
{
}
else	// 위의 조건들이 false일 경우 아래 코드 동작
{
}
```

### switch 문

```cpp
int number = 2;
switch (number)
{
	case 1:
		동작할 코드
		break;
	case 2:
		동작할 코드
		break;
	default:
		동작할 코드
		break;
}
```

## 열거형
- 상수에 이름을 부여하는 기능을 지원
- 사용자 정의 타입이 만들어짐
- 종류 : enum, enum class
- 열거형의 선언되는 요소들은 0부터 시작해서 값이 1씩 증가하게 된다
- 열거형의 요소에 값을 직접 대입할 수도 있다
- 열거형 이름 충돌 방지를 위해 네임스페이스로 감싸주는 방식을 사용한다
- 열거형 선언 시 type의 크기 자체를 제한할 수도 있다

```cpp
// 열거형 이름 충돌 방지를 위해 namespace로 감싸준다
namespace EJob
{
	enum Type : short // Enum 타입의 크기를 2바이트로 제한
	{
		Knight = 1,
		Archer,
		Magician
	};
}

enum class EJob1 : unsighed char // Enum Class의 크기를 1바이트로 제한
{
	Knight,
	Archer,
	Magician
};


int main(){
	int job = EJob::Magician;
	EJob1 job1 = EJob1::Knight;

	switch (job)
	{
	case EJob::Knight:
		cout << "Knight" << '\n';
		break;
	case EJob::Archer:
		cout << "Archer" << '\n';
		break;
	case EJob::Magician:
		cout << "Magician" << '\n';
		break;
	}
}
```

## 반복문
- 같은 코드를 여러 번 동작시킬 때 사용한다
- for, while, do while 3가지 문법 지원

### for문

```cpp
for(초기값; 조건식; 증감값)
{
    반복할 코드
}
```
- 초기값 : for문이 시작될 때 1번만 동작한다
- 조건식 : for문이 동작될 때 계속 동작한다
- 증감값 : for문이 동작될 때 계속 동작한다

- 동작 방식  
  - 초기값 => 조건식 => 코드실행 => 증감값 => 조건식 => 코드실행 => 증감값 => 조건식 ...
- 내가 원하는 횟수만큼 반복 시키기에 유리


- 중첩해서 사용 가능
```cpp
for(int i = 0; i < 3; i++)
{
    for(int j= 0; j < 3; j++)
    {
        if(j == 3) break;
        printf("i = %d, j = %d\n", i, j);
    }
}
```

### break, continue문
- 반복문 안에서 break문을 만나면 해당 반복문을 종료시킨다
- 반복문 안에서 continue문을 만나면 그 아래 코드를 진행시키지 않고 반복문 시작점으로 다시 돌아가서 반복문을 동작한다

### while문, do while문
```cpp
while (조건식)
{
    if(조건2) break;
}
```

```cpp
// 처음 한번은 무조건 동작시키고 그 후에 조건식을 체크한다
do
{

} while (조건식);
```