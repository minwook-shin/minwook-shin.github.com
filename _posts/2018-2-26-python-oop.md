---
layout: post
title: Python oop 알아보기
---

오늘은 파이썬에서의 객체지향 프로그래밍을 알아보고 실습해보았습니다.

프로그램을 여럿이 개발할 경우에 객체로 묶어서 작성할 수 있습니다.

파이썬 역시 객체 지향 프로그램 언어입니다.

클래스에서 인스턴스를 찍어낸다고 볼 수 있습니다.

## 선언

```python
class name(object):
```

class 예약어와 class 이름 그리고 상속받는 객체명순입니다.

## 속성

속성 추가는 ```__init__``` (생성자)로 객체 초기화하면서 넣습니다.

```python
class name(object):
    def __init__(self, name1, name2):
        self.name1 = name1
        self.name2 = name2
```

객체를 생성하면서 생성자 인자에 들어온 값을 속성으로 넣어줍니다.

## self

function(Action) 추가는 기존 함수와 같지만, self를 추가해야만 해당 class의 함수로 인식됩니다.

## object

이름 선언과 함께 초기값을 입력하면 인자로 넘길 수 있습니다.

```
minicar = car("super","oil")
```

객체명과 Class명 그리고 생성자 함수의 인자값 순입니다.

## 상속

부모 클래스로부터 속성과 메소드를 물려받은 자식 클래스를 생성합니다.

```python
class name(object):
    def __init__(self, name1, name2):
        self.name1 = name1
        self.name2 = name2

class ko_name(name):
    pass
```

자식 클래스에서 상속 받으면 부모 클래스에서 속성과 메소드를 가져올 수 있습니다.

## 다형성

같은 이름의 메소드인데 내부 로직을 다르게 작성할 수 있습니다.

dynamic typing 특성으로 인해 파이썬에서는 같이 부모 클래스의 상속에서 주로 발생합니다.

```python
class car(object):
    def __init__(self,engine,fuel):
        self.engine = engine
        self.fuel = fuel

    def run(self):
        print("running car")

class second_car(car):
    def run(self):
        print("running second car")
```

상속받아서 오버라이딩할 수 있습니다.

## 가시성

누구나 객체의 속성을 볼 필요가 없기 때문에 접근 지정자를 설정할 수 있습니다.

파이썬에서는 접근 지정자가 따로 없지만 이름 규칙으로 사용하게 됩니다.

보호하려면 밑줄을 사용하게 됩니다.

```python
class car(object):
    def __init__(self,engine,fuel):
        self.engine = engine
        self.fuel = fuel
        self.__price = "10000"
```

이러한 속성은 공개적이지 않기 때문에 다른 언어에서의 getter와 setter 처럼 이용해야합니다.

다만, 파이썬에서는 ```@property``` 데코레이터와 setter 속성으로 사용하게 됩니다.

```python
    @property
    def price(self):
        return self.__price

    @price.setter
    def price(self,n):
        self.__price = n
```

이런식으로 코딩하면 인자의 존재 여부로 setter와 getter처럼 사용할 수 있습니다.

```python
print(minicar.price)
```

```@property``` 데코레이터를 이용하여 함수를 변수처럼 호출할 수 있습니다.


## 실습 코드

```python
class car(object):
    def __init__(self,engine,fuel):
        self.engine = engine
        self.fuel = fuel
        self.__price = "10000"

    @property
    def price(self):
        return self.__price

    @price.setter
    def price(self,n):
        self.__price = n

    def __str__(self):
        toString = self.engine + " " + self.fuel + " " + "Car"
        return toString
    def run(self):
        print("running car")

class second_car(car):
    def run(self):
        print("running second car")



minicar = car("super","oil")
print(minicar)
print(minicar.price)
minicar2 = second_car("super","oil")
print(minicar2)
minicar2.run()
print(minicar2.price)
```