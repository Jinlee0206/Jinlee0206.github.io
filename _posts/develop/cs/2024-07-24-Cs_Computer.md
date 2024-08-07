---
layout: post
title:  "[CS] 컴퓨터란?"
excerpt : ""
categories: develop
tags: cs
toc: true
comments : true
use_math : true

date: 2024-07-24
last_modified_at: 2024-07-24
---
> <span style="font-size: 80%"> 출처 : [모두의 코드](https://modoocode.com/315) </span>

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

# 컴퓨터 언어
## C언어
- 1972년에 출시된 언어
- UNIX 운영체제를 작성하기 위해 만들어짐
- *시스템 프로그래밍*이 주 목적
- **배울 내용이 적다**는 것이 가장 큰 장점

# 컴퓨터란 무엇인가?
- 일련의 연산을 수행하는 **계산기** => **명령어를 읽어들여서 주어진 명령어에 따라 연산**을 수행

## 누가 명령어를 읽는가?
- 컴퓨터의 모든 연산은 **중앙 처리 장치(CPU, Central Processing Unit)**에서 진행
- 컴퓨터 그래픽 관련 연산이 늘어남에 따라 그래픽 관련 연산들만 전문으로 처리하는 **GPU(Graphic Processing Unit)** 또한 개발됨

> 물론 GPU 의 경우 그래픽에 관련된 명령어들은 CPU 와 비교할 수 없을 정도로 빠른 속도로 처리하게 됨. 즉 쉽게 비유하자면 CPU 는 똑똑한 애들 한 두명이 일하는 것이고, GPU 의 경우 조금 덜 똑똑한 애들 수백 수천명이 모여서 일을 하는 것과 같다

## 어디서 명령어를 읽는가?

CPU가 명령어를 실행하기 위해서 두가지 조건이 필요

1. 실행할 명령어를 읽는다
2. 연산된 결과를 어디엔가 저장

CPU는 연산에 특화되어 있는 장치이기 때문에 데이터를 저장해 놓을 공간이 매우 부족함

### 레지스터
> *CPU가 연산을 수행하기 위해서 데이터를 저장하는 공간*

- x86-64 CPU의 경우 일반적인 연산을 수행할 수 있는 레지스터는 총 16개 뿐
- 각 레지스터에 64bit의 데이터를 담을 수 있다
- CPU 최대 데이터의 크기는 128Byte
- 레지스터는 CPU 안에 있는 메모리이기 때문에 CPU 안에서 연산을 수행 시에 매우 빠르게 접근할 수 있다. 그러나 기술의 한계상 레지스터의 개수를 늘리는 것은 어렵기 때문에 CPU 밖에 있는 저장 공간이 필요할 수 밖에 없다

### RAM
> *CPU 옆에 붙어서 저장 공간 역할을 하는 장치*

- 임의 접근 메모리(Random Access Memory)
  - 
- CPU는 램에 실행할 명령어들을 저장해놓고 있다가, 연산을 수행할 때, 램에서 읽어들이게 됨. 필요한 데이터는 랢에서 꺼내 쓰거나 저장해놓음
- **휘발성 메모리** : 전기가 있을 경우에만 유지되는 메모리, 전원이 공급되지 않는다면 메모리에 저장되어 있는 데이터는 모두 날아가게 됨
- 컴퓨터가 꺼져도 데이터를 유지할 수 있는 저장 장치가 필요
- CPU에서 램에 있는 데이터를 가져오는 데, 100 나노초가 걸림 

### ROM
> *전원 공급이 없어도 데이터를 안정적이게 보관 가능한 장치*

- Read-Only Memory
- 데이터 읽기 속도 : SSD는 50~150 마이크로초, HDD는 1 ~ 10 밀리초

### 프로그램 실행 순서
<p align="center">
  <img src = "https://github.com/user-attachments/assets/199883f4-ba92-478a-bc10-a5288be16e89" width = 420>
</p>

1. 하드 디스크에서 저장되어 있는 프로그램의 위치를 찾아서 램에 복사해 놓는다
2. CPU는 램에서 명령어를 읽어들여서 실행을 한다

### 캐시
> *직접적인 연산을 수행할 수는 없지만 빠르게 데이터를 레지스터에 불러올 수 있는 저장 공간*

- CPU 연산 속도 기준 RAM에 접근하는 속도도 그리 빠른 편이 아님
- Cache 메모리라는 것을 만들고, 임시 저장 공간처럼 사용
- 데이터를 읽어 들이는 데 기다리는 시간을 최소화 할 수 있다
- 구성
  - 계층별로 L1, L2, L3 캐시로 이루어져 있음
  - L1 캐시는 크기가 제일 작지만 (보통 256KB) 레지스터와 가장 인접한 캐시, 실제 접근 시간은 0.9 나노초
  - L2 캐시 접근시간 2.8 나노초
  - L3 캐시 접근시간 28 나노초, 크기가 큼 (~ 16MB)
  - **지금 가장 필요한 데이터**의 경우 L1 캐시에 들어가게 되고, 그 중요도에 따라 필요성이 낮으면 낮을 수록 L2, L3 캐시에 배치 됨
- 동작 원리
  - CPU는 자기가 조만간 사용할 것만 같은 데이터들을 미리 캐시에 불러 옴
  - 램에 데이터를 저장할 때 바로 램에 쓰는 것이 아니라 캐시에만 잠깐 써놨다가 나중에 (여유가 좀 생기면) 램에 적는 방식을 사용
  - CPU가 미래에 실행할 명령어들을 미리 볼 수 있는 것이 아니기 때문에 CPU는 여러 가지 *예측 알고리즘*을 사용해서 캐시의 적중률을 높이려고 함
    - 일반적으로 램의 전체 데이터를 중구 난방으로 사용하는 것 보다는 특정 부분만 반복적으로 접근하는 경우 캐시 적중률이 높아짐
    - **Cache Miss** : CPU가 요청한 데이터가 캐시에 없는 경우. 램에서 필요한 데이터를 불러 오느라 상당히 시간이 지체됨. 빠르게 동작하는 프로그램을 설계하기 위해서는 캐시 미스 확률을 낮게 하는 것이 중요

### 데이터 로딩 시간표

|  |실제접근 시간|현실 시간으로 환산했을 때|
| -- | -- | -- |
|CPU 사이클| 0.4나노초 | 1초 |
|L1 캐시 접근|0.9 나노초|2초 |
|L2 캐시 접근|2.8 나노초|7초 |
|L3 캐시 접근|28 나노초|1분 |
|RAM 접근|~100 나노초|4분 |
|NVMe SSD 접근|~25 마이크로초|17시간 |
|일반 SSD 접근|50 ~ 150 마이크로초|1.5일 ~ 4일|
|일반 하드디스크 접근|1 ~ 10 밀리초|1 ~ 9달 |
|서울에서 샌프란시스코 패킷 전송 시간|180ms| 14년 |

## 명령어는 어떻게 작성하는가?
CPU가 램에서 데이터를 읽어들이기 위해서는 램의 **어디**에서 데이터를 읽어들일지 말해줘야 한다. 램에 있는 모든 데이터는 1바이트 단위로 0번을 시작으로 고유의 **주소(Address)**가 부여되어 있다

<p align="center">
  <img src = "https://github.com/user-attachments/assets/dbfa7c4d-a016-407c-80ce-1fd6579412e0" width = 420>
</p>

- CPU는 램에게 *어디에서* 데이터를 읽을지 알려준다면 램은 해당 위치에 있는 데이터를 즉각 전달 해 줌
- *어디에다* 데이터를 저장할 지 알려준다면 램은 해당 위치에 있는 데이터를 CPU가 전달한 데이터로 바꿔치기 함
- *얼마 만큼* 읽어야 할지도 명령어 단계에서 지정해줘야 함


- CPU에게 주소값 0x1234에 1바이트 만큼 3이라는 데이터를 저장하는 방법
  - CPU의 레지스터(a라는 레지스터)에 접근하고자 하는 주소값 0x1234를 저장.
  - `a에 저장된 주소값에서부터 1바이트 부분까지` 3을 저장해 라는 명령어를 내림 

<p align="center">
  <img src = "https://github.com/user-attachments/assets/01869d35-5cea-4187-bc8b-f7132f9077cd" width = 420>
</p>

- 어셈블리어(Assembly) : CPU가 직접적으로 해석하는 명령어

```c
mov eax, 4660 // eax 라는 레지스터에 0x1234를 대입하라. 4660은 0x1234 를 십진수로 나타낸 것
mov BYTE PTR [rax], 3 // rax라는 이름의 레지스터에 들어있는 값을 주소값으로 생각해서 해당 위치에 3을 대입해라. 
                      // BYTE PTR 전달한 주소값으로 부터 1바이트 만큼의 데이터를 위가 저장할 값 (3)으로 덮어 씌워라

mov A, B // A에 B를 대입해라
```

> rax와 eax는 같은 레지스터   
> eax의 경우 rax 레지스터의 마지막 32 비트를 의미

**메모리의 주소값에 해당하는 데이터를 접근하기 위해선, 먼저 그 주소값을 레지스터에 집어 넣고 해당 레지스터를 참조해야 한다**   


## CPU가 명령어를 읽어들이는 방법

CPU 는 **주소값**을 통해서 램에 어디에 접근할지 명령하게 된다

### 프로그램이란?
> 실행할 명령어와 데이터들의 집합

- **프로그램을 실행한다** : CPU에 실행할 명령어를 제공하는 것
- 프로그램 동작 과정
  - OS가 CPU에게 램에 위치해 있는 프로그램의 시작점을 알려주게 되고, 그 후로 CPU는 해당 위치부터 명령어를 쭉쭉 읽어나가며 실행하게 됨
  - CPU가 **현재 램의 어디에서 명령어를 읽어야 할지** 계속 알아야 함
    - CPU 안에 **지금 읽어들일 명령어의 위치 (Instruction Pointer)**만을 보관하는 특별한 레지스터 덕분에 가능, 인텔 64비트 CPU의 경우 해당 레지스터의 이름은 RIP
    - CPU는 현재 내가 어떤 프로그램을 실행하고 있는지를 모른다. 그저 현재 자신의 RIP 레지스터가 가리키는 위치에 있는 명령어를 실행하고 그 다음 명령어의 위치로 RIP를 증가시키기만 함

<p align="center">
  <img src = "https://github.com/user-attachments/assets/524b5777-c35c-435f-8c41-18fd223d5b94" width = 420>
</p>

### 프로그램은 하나만 실행하는 것이 아니다

<p align="center">
  <img src = "https://github.com/user-attachments/assets/be095692-794c-4887-98d3-40be08a5d6b9" width = 420>
</p>

- CPU에서 원하는 위치에 데이터를 쓰거나 가져오기 위해서는 메모리의 *주소값*을 전달해야 한다   
- 미리 사용중인 프로그램이 점유하고 있는 공간을 침범하지 않게 하기 위해서는 다른 메모리 주소를 사용하도록 프로그램 명령어 자체를 다시 작성 해야함 -> 불가능
- CPU에서는 메모리를 조금 더 효율적으로 관리하기 위해서 특별한 메커니즘 제공

## 가상메모리 Vs 물리메모리
- 가상메모리(Virtual Memory) : CPU가 참조하는 메모리 주소 값
- 물리 메모리(Physical Memory) : 일련의 변환 과정에 의해 참조하게 될 실제 메모리의 주소값 

- **페이징(Paging)** : CPU가 보는 메모리는 사실 가상 메모리이고, 실제로는 일정한 크기의 조각들로 쪼개어져서 메모리의 각기 다른 영역에 대응되어 있다

<p align="center">
  <img src = "https://github.com/user-attachments/assets/fdd0bbf8-2edb-49fd-a108-01feff77e95a" width = 420>
</p>


- **페이지 테이블(Page Table)** : 어떻게 변환을 수행할 지 기록한 테이블. 각 프로그램마다 하나씩 가지고 있음.
  - 구글 크롬에서의 0x1234와 그림판의 0x1234가 실제로는 다른 물리 메모리 주소를 참조할 수 있음

<p align="center">
  <img src = "https://github.com/user-attachments/assets/87e98cac-cf75-4983-a117-b53a94b6d29e" width = 420>
</p>

- **페이징 덕분에 각 프로그램들은 마치 혼자서 메모리 전 공간을 사용하는 것 마냥 생각할 수 있다**

# 정리
- 모든 연산은 CPU에서 수행됨
  - 정확히 말하자면, CPU의 자그마한 레지스터 상에서 수행됨
  - 64bit CPU의 경우 16개의 레지스터를 가지고, 크기는 8byte
- CPU는 무슨 연산을 할지 알려주는 **명령어**와, 명령어를 실행하기 위해 필요로 하는 데이터를 **메모리(RAM)**에서 읽는다
- *프로그램을 실행한다*
  - 하드 디스크에 잠들어 있는 *명령어*들과 *데이터*를 메모리에 쓰는 것
  - 운영체제가 CPU에 처음으로 실행해야 할 명령어의 **주소값**을 전달함으로써 프로그램이 시작됨
- CPU에는 Cache가 있어서 메모리 접근 횟수를 줄일 수 있다
- 각 프로그램들은 마치 자신이 방대한 메모리 공간 전체를 사용하는 것처럼 생각하며 작동한다(Paging)
- CPU에서 참조하는 주소값은 실제 물리 메모리 주소값이 아니라 가상 메모리 주소값이다
- 가상 메모리 주소값은 각 프로그램의 페이지 테이블을 통해서 실제 메모리 주소값으로 변환된다