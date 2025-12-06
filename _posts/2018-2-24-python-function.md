---
layout: post
title: Python function 알아보기
---

오늘은 파이썬에서의 함수에 대하여 종합적으로 배워보려 합니다.

우선 함수는 여러 사람들이 모여 개발할 때, 필요한 부분을 나누어 코딩을 하는 상황에서 유용합니다.

기능별로 묶을 수 있습니다.

## 함수

어떤 일을 수행하는 코드의 묶음입니다.

```python
def func(a,b):
    return a+b
```

위와 같이 a와 b를 더하는 함수를 작성했습니다.

이로 인해 반복적인 수행을 한번만 작성하고 메인에서 재활용합니다.

캡슐화되어 타인의 코드를 사용하기도 용이합니다.

## 선언 문법

```python
def 이름 (인자):
    수행
    return 반환
```

## 코드 수행 순서

메인 코드부터 진행되며 함수가 불러와지면, 함수에 포함된 수행문을 수행합니다.

## 용어 정리

* parameter : 함수의 입력 값

* argument : 실제 파라미터에 대입된 값

## 타 용어와 차이점

반환 타입이 명시되어 있지않아서 단순하게 반환이 없다면 암묵적으로 None 객체를 반환하며 수행문을 수행합니다.

## 함수 호출 형식

* call by value : 함수에 인자를 넘길 때 값만 넘깁니다.

* call by reference : 함수에 인자를 넘길 때 주소값을 넘깁니다.

파이썬에서는 call by assignment 형식으로 호출하며 객체의 주소가 함수로 전달하기 때문에 전달된 객체를 참고할 때만 호출자에게 영향을 줍니다.

또한 immutable object와 mutable object로 나누어져 있어서 각각 int, float, str, tuple과 set, list, dict가 해당됩니다.

참고로 immutable 객체는 처음에는 call by reference로 받지만 값이 변경되면 call by value로 동작하고,
mutable 객체는 call by reference로 동작합니다.

```python
def func(temp):
    temp.append(1)
test = []
func(test)
```

위 코드에서는 인자의 객체 주소값을 참고하기 때문에 메인 코드도 변경됩니다.

```python
def func(temp):
    temp = [10]
test = []
func(test)
```

위 코드에서는 인자에서 새로운 객체를 생성했기 때문에 메인 코드를 간섭하지 않습니다.

## 변수의 범위

변수가 사용되는 범위를 말하며, 지역변수와 전역변수가 있습니다.

지역변수는 함수내에서만 사용되며, 전역변수는 프로그램 코드 전체에 영향을 끼칩니다.

만약 전역변수와 이름이 같은 지역변수가 생긴다면 그 함수에서의 지역변수가 더 생깁니다.

함수 내부에서 전역변수로 선언하고 싶을 때는 global를 사용합니다.

```python
def func():
    global val

print(val)
```

## 재귀함수

자기 자신을 호출하는 함수입니다.

종료 조건이 만족될 때까지 함수를 무한히 호출합니다.

```python
def factorial(n):
    if n is 1:
        return 1
    else:
        retun n + factorial(n-1)
```

## passing argument

함수에 입력되는 인자는 다양한 형태를 가집니다.

* keyword argument : 함수에 사용된 파라미터의 변수명을 사용하여 인자를 넘깁니다.

```python
def print_name(name, age):
    print(name + age)

print_name(age = 20,name = "name")
```

* default argument : 파라미터의 기본값을 사용합니다.

```python
def print_name(name, age = 20):
    print(name + age)

print_name(name = "name")
```

* variable-lenth argument

가변인자를 사용하여 개수가 정해지지 않는 변수를 함수의 파라미터로 사용할 수 있습니다.

*기호를 사용하여 함수의 파라미터를 표현합니다.

단, 한개만 설정할 수 있으며 마지막 파라미터에서만 사용할 수 있습니다. 

```python
def print_name(name, age, *arg:
    print(name + age + arg)

print_name(name = "name",age = 20, "hello,", "world")
```

키워드 가변 인자라고해서 파라미터의 이름을 지정하지 않고 입력할 수도 있습니다.

> (update : call by assignment 관련내용 추가 및 일부 정정)