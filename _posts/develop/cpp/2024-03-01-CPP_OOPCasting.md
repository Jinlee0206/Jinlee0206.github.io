---
layout: post
title:  "[C++] UpCasting & DownCasting"
excerpt : "개발, 면접"
categories: develop
tags: devlog cpp

toc: true

date:   2024-03-01
last_modified_at: 2024-03-01
comments : true
---

> <span style="font-size: 80%"> **출처** </span>   
> <span style="font-size: 80%"> [ㅎㅇgdprgmer 블로그 링크](https://rehtorb-algorithm.tistory.com/8) </span>  

<!--more-->

* this unordered seed list will be replaced by the toc
{:toc}

## 포인터

주소를 저장하는(가리키는) 변수

<p align="center"> 
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/999aaa80-43b0-43b2-9bb2-43e449ee466e" width = "400">
</p>

**변수의 생성주기** 때문에 사용

- Stack
  - 변수(지역변수, 함수의 복귀주소 (RET), 매개변수)는 기본적으로 Stack에 할당됨
  - 생성된 코드 블록 내에서만 유효

```cpp
int* GetAddress() 
{ 
    int num = 10;       // 지역변수 선언 
    int* pNum = &num;   // 지역변수 선언 및 초기화 
    
    return pNum;        // pNUm이 가지는 지역변수 num의 주소를 반환 
} 

int main(void) 
{ 
    int* pNum = GetAddress();         // 반환된 주소로 초기화 
    std::cout << *pNum << std::endl;  // 제어권을 잃은 상태로서, 반환된 주소에 해당하는 메모리에 남은 값을 출력 (언제 쓰레기 값이 될 지 모름)
    return 0; 
}
```

- Heap
  - 런타임 중에 동적으로 할당한 변수들이 저장되는 공간
  - 프로그램과 생명주기를 함께 함
  - 프로그래머가 직접 new와 delete 연산자로 메모리 할당과 해제를 관여할 수 있다

```cpp
int* GetAddress() 
{ 
    int* pNum = new int(5); // int 타입의 5를 가지고 있는 메모리를 pNum에 동적할당 
    
    return pNum;            // pNum이 가리키고 있는 주소 반환 
 } 
 
int main(void) 
{ 
    int* pData = GetAddress();           //pNum이 가지는 주소로 pData 초기화 
    std::cout << *pData << std::endl;    //pData가 가리키는 주소에 해당하는 값 출력 (5)
    
    int* pNumAddr = pData;               //pNumAddr도 pData와 같은 주소를 가리킴(참조카운팅++) 
    
    delete pData;                        //pData가 가리키는 주소에 해당하는 메모리 해제 
    std::cout << *pNumAddr << std::endl; //쓰레기 값 출력 
    return 0; 
}
```

> nullptr 체크

```cpp
int* GetAddress() 
{ 
    int* pNum = nullptr; return pNum; 
} 

int main(void) 
{ 
    int* pNum = GetAddress(); 
    if (pNum) 
    	std::cout << *pNum << std::endl; else std::cout << "nullptr" << endl; 
        
    return 0; 
}
```


**상속관계의 객체들은 서로를 가로지르는 타입 캐스팅이 가능하다**

## 업캐스팅
- 기본 클래스 포인터로 파생클래스 객체를 가리키는 것
- 그냥 파생클래스 객체를 선언해서 가져다 쓰면 되지 않는가...?
- **다형성을 이용해서 코드 재사용성을 높인다**

```cpp
// 예제 코드
class CBase 
{
    public: virtual void Show() 
    { 
    	std::cout << "CBase" << std::endl; 
    } 
}; 

class CDerived1 : public CBase 
{ 
    public: virtual void Show() 
    { 
    	std::cout << "CDerived1" << std::endl; 
    } //overriding 
}; 
    

class CDerived2 : public CBase
{ 
    public: virtual void Show() 
    {
    	std::cout << "CDerived2" << std::endl; 
    } //overriding 
};
```

![Casting_res](https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/c28935e7-1a95-4f5e-a3a4-c6f8f642c3e4)

위 결과를 내기 위한 3가지 Solution

```cpp
// 1) 각 파생클래스 객체를 선언하여 사용
main int main(void) 
{
    CBase base; 
    CDerived1 derived1; 
    CDerived2 derived2; 
    
    base.Show(); 
    derived1.Show();
    derived2.Show(); 
    
    return 0; 
}
```

```cpp
// 2) 포인터, 동적바인딩을 활용
main int main(void) 
{ 
    CBase* pInterface = nullptr; 
    CBase* pBase = new CBase; 
    CDerived1* pDerived1 = new CDerived1; 
    CDerived2* pDerived2 = new CDerived2; 
    
    pInterface = pBase; 
    pInterface->Show(); 
    
    pInterface = pDerived1; 
    pInterface->Show(); 
    
    pInterface = pDerived2; 
    pInterface->Show(); 
    
    delete pBase; 
    delete pDerived1; 
    delete pDerived2; 
    return 0; 
}
```

```cpp
// 3) 업캐스팅
main int main(void) 
{ 
    std::string classNames[3] = {"CBase", "CDerived1", "CDerived2" }; 
    std::string className = ""; 
    std::map<std::string, CBase*> mapData; 
    
    CBase* pInterface = nullptr; 
    CBase* pBase = new CBase; 
    CDerived1* pDerived1 = new CDerived1; 
    CDerived2* pDerived2 = new CDerived2; 
    
    mapData.insert(std::make_pair(classNames[0], pBase));
    mapData.insert(std::make_pair(classNames[1], pDerived1));
    mapData.insert(std::make_pair(classNames[2], pDerived2)); 
    
    for (int idx = 0; idx < 3; idx++) 
    { 
		className = classNames[idx]; 
        pInterface = mapData.find(className)->second; //업 캐스팅 pInterface->Show(); 
    } 
    
    mapData.clear(); 
    
    delete pBase; 
    delete pDerived1; 
    delete pDerived2; 
    
    return 0;
}
```

<p align="center"> 
  <img src = "https://github.com/Jinlee0206/Jinlee0206.github.io/assets/105345909/be340875-5a82-43c5-90c2-c3539e0028e5" width = "533">
</p>

- 객체를 동적할당을 통해 힙에 생성하고, 주소를 알고 있으면 됨.
- 단 한번의 객체 생성으로 map에 해당 클래스의 이름과 함께 객체의 주소를 저장 (초기화)
- 이 map을 어디서든 가져다 쓸 수 있게 만든다면 (싱글톤 & 팩토리 패턴) 기본 클래스 타입의 포인터 변수만 선언하고 그 포인터 변수로 업캐스팅하게 되면서 for문 속 단 세줄로 효율적으로 구현 가능

## 다운캐스팅
- **동일한 타입의 포인터가 동일한 타입을 가리키는 것**
- 업캐스팅을 하게 되면 기본 클래스에 정의된 멤버만 호출할 수 있기 때문에 파생클래스 고유의 기능을 사용할 수 없다

> 기본클래스 포인터->파생클래스 객체 ==> 파생클래스 포인터->파생클래스 객체

- 이 때 파생클래스는 서로 **동일한 타입**이여야 하며, **명시적 형변환**을 해야한다

```cpp
class CBase 
{
    public: virtual void Show() 
    { 
		std::cout << "CBase" << std::endl; 
    } 
    
    void A() 
    { 
    	std::cout << "A의 고유 기능" << std::endl; 
    }
}; 


class CDerived1 : public CBase 
{ 
	public: virtual void Show() 
    { 
    	std::cout << "CDerived1" << std::endl; 
    } 
    
    void B() 
    { 
    	std::cout << "B의 고유 기능" << std::endl; 
    } 
}; 


class CDerived2 : public CBase 
{ 
	public: virtual void Show() 
    { 
    	std::cout << "CDerived2" << std::endl; 
    } 
    
    void C() 
    { 
    	std::cout << "C의 고유 기능" << std::endl; 
    } 
};
int main(void) 
{
    std::string classNames[3] = { "CBase", "CDerived1", "CDerived2" }; 
    std::string className = ""; 
    std::map<std::string, CBase*> mapData; 
    
    CBase* pInterface = nullptr; 
    CBase* pBase = new CBase; 
    CDerived1* pDerived1 = new CDerived1; 
    CDerived2* pDerived2 = new CDerived2; 
    
    mapData.insert(std::make_pair(classNames[0], pBase)); 
    mapData.insert(std::make_pair(classNames[1], pDerived1)); 
    mapData.insert(std::make_pair(classNames[2], pDerived2)); 
    
    for (int idx = 0; idx < 3; idx++) 
    { 
    	className = classNames[idx]; 
        pInterface = mapData.find(className)->second; //업 캐스팅 
        
        switch (idx) 
        { 
        case 0: 
            pBase = (CBase*)pInterface;         //다운 캐스팅(pInterface를 CBase로 타입 캐스팅) 
            pBase->A(); 
            break; 
        case 1: 
            pDerived1 = (CDerived1*)pInterface; //다운 캐스팅 
            pDerived1->B(); 
            break; 
        case 2: 
            pDerived2 = (CDerived2*)pInterface; //다운 캐스팅 
            pDerived2->C(); 
            break; 
        default: 
            break; 
        } 
    } 
    
    mapData.clear(); 
    
    delete pBase; 
    delete pDerived1; 
    delete pDerived2; 
    
    return 0; 
}
```

- 위 다운 캐스팅에서 쓰인 형변환은 C타입 캐스팅
  - 어떠한 제약 조건도 없이 프로그래머가 지정한 타입으로 강제 캐스팅 -> 실수가 있으면 바로 에러
- 모던 C++에서는 **RTTI(Run Time Type Information)**와 함께 안전하고 강력하고 다양한 방식의 타입 캐스팅을 지원