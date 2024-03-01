---
layout: post
title:  "[유니티 클라이언트 개발] C언어 메모리 구조, 스택프레임, 동적 할당"
excerpt : "개발, 면접"
categories: develop
tags: devlog unity 

toc: true

date:   2024-02-29
last_modified_at: 2024-03-01
comments : true
---
> <span style="font-size: 80%">
**출처**
- [Change is the only constant 블로그 링크](https://lecor.tistory.com/64)  
- [cmaven 깃블로그 링크](https://cmaven.github.io/c/C-Memory-Structure-Malloc/)</span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 메모리 구조

<p align="center"> 
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/41e9f3a6-103c-46ae-b958-cb59d0e149a4" width = "400"> 
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/3c222037-24e3-443d-aa01-2e2018002d63" width = "187.5">
</p>

- 메모리 C 운영체제  

프로그램 실행이되면 OS는 이 프로그램 정보를 읽어 메모리에 Load하게 된다. 이렇게 HDD에 로드 된 프로그램 코드는 CPU에 의해 실행되고 메모리를 관리하게 된다

- 프로세스 (Process)

실행중인 프로그램. CPU가 실행 파일에 있는 명령들을 실행하려면 먼저 운영체제가 실행 파일의 명령들을 읽어서 메모리에 재구성

- 세그먼트 (Segment)

여러 정보나 사용자가 입력한 데이터를 기억하는 메모리 공간

1. Code Segment (Text Segment)
  - 작성한 코드가 기계어로 변역되어 저장되는 곳
  - 프로그램이 실행되면, 이 영역에 있는 코드들이 한 줄씩 실행
  - read-only 영역이라 데이터들이 변경될 수 없음
  - rodata(read-only data)인 상수리터럴도 이 영역에 저장

2. Data Segment

전역변수(global), 정적변수(static), 배열(array), 구조체(structure) 등이 저장
이영역은 read-only가 아니므로 프로그램 실행 중에 값이 바뀌는 것이 가능

- Initialized Data Segment
  - 프로그래머가 직접 초기화를 해준 전역변수와 정적변수가 저장되는 곳

- Unintialized Data Segment (BSS segment (Block Started by Symbol))
  - 초기화 되지 않은 전역변수와 정적변수가 저장되는 곳
  - 초기화 되지 않은 변수는 커널이 자동으로 0으로 초기화시킨다.

3. Stack
- 스택 영역은 힙영역과 인접해있고, 메모리 주소가 높은 곳에서 낮아지는 방향으로 자람
- 함수의 호출과 관계되는 지역 변수와 매개 변수가 저장되는 영역
- 컴파일 시 크기가 결정
- 프로그램이 자동으로 사용하는 임시 메모리 영역
- 함수가 호출되면 함수의 **스택 프레임(stack frame)**이 생성되고, 스택 프레임이 스택에 추가 됨
- 함수의 실행이 끝나 함수가 반환 될 때 스택 프레임이 스택에서 제거
- LIFO(Last In First Out)

4. Heap
- 동적할당(malloc, realloc, free)으로 할당한 메모리들이 위치하는 영역
- 스택과 반대로 메모리 주소가 낮은 곳에서 높은 곳으로 증가
- 런타임 시 크기가 결정
- 힙 영역은 라이브러리와 프로세스에서 동적으로 로드 된 모듈들끼리 공유할 수 있는 영역
- 메모리 할당 해제를 위해 free를 꼭 해줘야 함

## 스택프레임

```c
int sum(int a, int b) {return (a+b);}

int main(void)
{
  int c = sum(1,2);
  return c;
}
```

<p align = "center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/52474457-0580-4317-b560-b963c3c2fff9" width = "280">
</p>

> 스택프레임의 구성
> - RET (Return address) // 함수가 종료된 후 반환되어 돌아갈 위치를 담고 있음
> - RBP (Base Pointer Register) // 스택의 가장 낮은 위치를 가리킴 (메모리에서 제일 높은 주소). 여기부터 스택이 쌓이기 시작할 거라고 알려줌
> - RSP (Stack Pointer Register) // 스택에 새로운 값이 들어올 때마다 스택의 top을 추적
> - 로컬변수 저장을 위한 메모리 공간
> - 매개변수 저장을 위한 메모리 공간

다음으로 sum() 함수가 호출되면 sum() 함수의 스택프레임이 스택에 push 됨   
함수가 끝나면 해당 스택프레임은 스택에서 제거되고 RET 정보에 따라 돌아 가야할 위치로 돌아감

<p align = "center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/86d60105-d226-4de3-9ca8-eaeb76f6207c" width = "280">
</p>

## 프로그램 실행에 따른 메모리 변화

<p align = "center">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/7928d38d-8a13-4a93-af4c-af737287710b" width = "533">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/e394c7f0-82f3-485a-b437-bf66fe860db1" width = "533">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/c46ed7fc-1fe8-449d-b0eb-141a40b102f5" width = "533">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/08dd6579-182d-4890-8200-c072f60477be" width = "533">
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/aa8bfcf1-6009-48f8-afb2-3eef886bbedf" width = "533">
</p>

## 메모리 동적 할당

지역변수와 같이 함수가 호출될 때 매번 할당이 이루어지지만, 할당이 되면 전역변수처럼 함수를 빠져나가도 소멸되지 않는 성격의 변수를 선언할 때 유용   

- 해당 변수를 힙 영역에 할당 및 소멸
```c
#include <iostream>
void * malloc(size_t size);     // 힙 영역에 메모리 공간 할당
void free(void * ptr);          // 힙 영역에 할당된 메모리 공간 해제
// 성공 시 할당된 메모리 주소 값, 실패시 NULL 값 반환
```

- 반환형이 void형 포인터인 malloc 함수는 형변환이 필수, 형변환 없을 경우 오류 발생

```c
void * ptr = malloc(sizeof(int));
// *ptr = 20;       // Error! ptr이 void형 포인트 이므로 형변환이 필수

// 선언 방법
int * ptr1 = (int *)malloc(sizeof(int));
double * ptr2 = (double *)malloc(sizeof(double));
int * ptr3 = (int *)malloc(sizeof(int) * 5);
double * ptr4 = (double *)malloc(sizeof(double) * 10);
```

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

char* ReadUserName(void)
{
    //char buf[30];
    char* name = (char*)malloc(sizeof(char) * 30);
    printf("이름?:");
    /* 이 경우, free() invalid pointer 에러가 발생함
       이는, malloc으로 할당받은 주소값을 name에 넣어놓고,
       name의 주소값을 변경 한 후, free를 수행하기 때문에,
       프로그램은 free 수행 시, 반환해야할 메모리를 잘못 참조하게 되기 때문
    fgets(buf, sizeof(buf), stdin);
    strncpy(name, buf, sizeof(buf)-1);
    name[sizeof(name)-1] = 0;
    name = buf;
    */
    fgets(name, sizeof(char) * 30, stdin);
    // gets(name); // gets은 메모리 사용 위험으로 권장하지 않음
    return name;
}

int main()
{
    char* name1;
    char* name2;

    name1 = ReadUserName();
    printf("name1: %s", name1);

    name2 = ReadUserName();
    printf("name2: %s", name2);

    printf("again name1: %s", name1);
    printf("again name2: %s", name2);

    free(name1);
    free(name2);

    return 0;
}
```

<img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/6c9b2585-45d7-4315-88af-4948df74ca9b" width = "210">

- calloc 함수

malloc이 100 바이트를 힙 영역에 할당해주세요 라면, calloc은 4바이트 크기의 블록(elt_size) 25개(elt_count)를 힙 영역에 할당해 주세요. 또한, calloc은 할당된 공간을 모두 0으로 초기화

```c
#include <stdlib.h>
void * calloc(size_t elt_count, size_t elt_size);
// 성공 시 할당된 메모리 주소 값, 실패시 NULL 반환
```

- realloc 함수

ptr이 가리키는 메모리 크기를 size 크기로 조절하는 함수

```c
#incldue <stdlib.h>
void * realloc(void * ptr, size_t size);
// 성공 시 할당된 메모리 주소 값, 실패 시 NULL 반환

 int * arr = (int *)malloc(sizeof(int)*3);
 arr = (int *)realloc(arr, sizeof(int)*5);
```