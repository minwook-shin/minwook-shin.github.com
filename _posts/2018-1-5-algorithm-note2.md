---
layout: post
title: 알고리즘 공부-2 (string class)
---

오늘도 전에 올린 알고리즘 공부 1편과 마찬가지로, C++ 자료구조를 배우기위해서 먼저 string class에 대한 내용을 공부해보았습니다.

아래는 오늘 공부한 내용을 요약한 노트입니다.

저번과 마찬가지로 저 혼자 공부한 내용을 끄적인거라, 잘 정리가 안된 포스팅입니다. 양해부탁드립니다.

## c++ 기존 스트링 문제점

null-terminated char array (char*) : 연속되서 저장되는 char이며, 마지막은 null값입니다.

```c++
char *pstr = new char[10];
strcpy(pstr,"string");
```

위와 같이 사용자가 직접 할당해야합니다.

```c++
char *newp = new char[10];
strcpy(newp, pstr);
```

또한 복사가 불편합니다.

```c++
delete[] pstr;
```

메모리 해제도 직접해야합니다.


## string class

1. 메모리할당에도 신경쓰지않음

1. 잘못된 스트링 사용 안전하게 처리

1. 편리한 함수

1. 직관적인 연산자 제공

```
string str;
str = "string";

str+= "new string";

str.SetAt(100,'A'); // 에러 처리
```

## 디자인

```c++
class String
{
protected:
    char *pBuffer;
    void init();
    void clear();
    void copy(const char *pstr);
public:
    //members
} 
```

외부에 노출되지 않을 변수와 함수를 선언합니다.

```c++
void string::init()
{
    pbuffer = 0;
}
```

char 타입의 변수를 초기화합니다.

```c++
void string::clear()
{
    if(pbuffer)
    {
        delete[] pbuffer;
    }
    init();
}
```

만약 pbuffer가 있다면 메모리에서 해제해주고 초기화로 이동합니다.

```c++
void string::copy(const char *pstr)
{
    if(pstr != 0)
    {
        int len = strlen(pstr);
        pbuffer = new char [len +1];
        if(pbuffer)
        {
            strcpy(pbuffer,pstr);
        }
    }
}
```

만약 인자로 받은 값이 있다면, 인자 값의 길이를 먼저 구해주고 동적할당으로 새로운 char 배열을 만들어 복사합니다.

## storage class

* 자동 변수 : 정의시 생성,블럭 끝날때 제거/스택에서 생성

* 정적 변수 : 프로그램 시작시 생성,프러그렘 끝날때 제거/ 테이터 세그먼트에서 할당

* 동적 변수 : new로 생성,delete로 제거/힙 메모리에서 생성

## 예시

```c++
int global; //static

int main()
{
    static staticbar = 10; // static
```

위 예시는 정적 변수입니다.

```c++

    char *pdanamic = new char[10] //dynamic
    delete[] pdynamic;
```

위 예시는 동적 변수입니다.

```

    int local1 =  0; // automatic
    if(local1==0){
        int local2; // automatic
        local2 = 0;
    }
}
```

위 예시는 자동 변수입니다.

## 생성자

객체가 새로 만들때, 자동 호출됩니다.
또한 객체의 초기화를 담당하고 있으며, 클래스의 이름과 동일해야합니다.

특징으로는 오버로드가 가능하며, 리턴값이 없습니다.

## 기본 생성자 

인자가 없는 생성자입니다.

### 상속과 생성자의 관계

상위 클래스의 생성자를 호출하게 됩니다.

## 소멸자

객체가 제거될때 자동으로 호출되며, 객체가 잡고 있는 자원을 해제합니다.

클래스 이름 앞에 ~를 붙이며 인자와 리턴값업다는 특징을 가집니다.


## 상속과 소멸자의 관계

상속이 사용되는 클래스는 virtual로 이루어져야 합니다.
- 상위 클래스 포인터로 대입된 하위 클래스인 경우, virtual이 아니면 상위 클래스의 소멸자만 호출됩니다.

## 복제 생성자

같은 타입의 객체로부터 새로운 객체를 생성합니다.

x::x(const x&) 형태를 가집니다.

1. 다른 객체로부터 만들어지는 경우

1. 객체를 함수의 인자로 넘길 경우

1. 함수에서 객체를 리턴할 경우


## 연산자 오버로딩

사용자 정의 타입에서 연산자를 새로 정의 가능합니다.

함수 오버로딩과 유사힙니다.