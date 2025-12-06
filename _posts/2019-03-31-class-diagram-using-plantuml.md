---
layout: post
title: PlantUML로 Class Diagram 그리기
---

오늘은 PlantUML로 클래스 다이어그램을 그려 합니다.

## 개요

PlantUML은 일반적인 텍스트로 uml 다이어그램을 그릴 수 있는 오픈소스 소프트웨어이며, 오늘 포스트에서는 클래스 다이어그램을 다뤄보려 합니다.

## 설치

기본적으로 자바가 설치되어 있다는 가정하에 설치합니다.

```
sudo apt install plantuml
```

우분투와 같은 경우는 PlantUML은 apt로 설치하며,

```
brew install plantuml
```

mac에서는 PlantUML을 brew로 설치할 수 있습니다.

## VS code

```
ext install plantuml
```

위 명령으로 plantuml 확장을 설치하거나 에디터에서 직접 설치할 수도 있습니다.

자동으로 파일에서 변경점을 찾아 미리보기를 업데이트하거나, 줌 또는 스크롤할 수 있는 확장입니다.

## hello, world!

```java
@startuml helloworld
left to right direction

class Controller {
    + Controller()
}
note right of Controller : hello,world!

@enduml
```

startuml로 시작해서 enduml에서 끝나며, class에 메소드와 속성을 이루게 할 수 있습니다.

wsd 확장자로 파일을 저장하면 됩니다.

## render

PlantUML: Preview Current Diagram

비주얼스튜디오 코드에서 PlantUML 확장이 설치되어 있다면, 보기 탭에서 명령 팔레트를 수행하여

위 명령을 입력합니다.

에디터 우측에서 uml 클래스 다이어그램이 그려집니다.

## export

```
plantuml file.wsd
```

터미널에서 위와 같이 입력하면 이미지로 생성됩니다.

```
PlantUML: Export Current File Diagrams
```

또는 비주얼스튜디오 코드에서 PlantUML 확장이 설치되어 있다면, 보기 탭에서 명령 팔레트를 수행하여

위 명령을 입력합니다.

출력되는 포맷과 경로를 정하면 파일이 생성됩니다.

## 관계

```java
@startuml rel
Class1 <|-- Class2 : extension
Class3 *-- Class4 : composition
Class5 o-- Class6 : aggregation
@enduml
```

확장, 합성, 집합 관계를 정의할 수 있습니다.

## 메소드

```java
@startuml method

Object <|-- ArrayList

Object : equals()
ArrayList : Object[] elementData
ArrayList : size()

Object2 <|-- ArrayList2

class Object2 {
    equals()
}

class ArrayList2 {
    Object[] elementData
    size ()
}

note top of Object : In java.

@enduml
```

필드와 메소드를 선언하려면 클래스 이름과 메소드를 차례로 작성합니다.

또는 대괄호 {} 사이에 모든 메소드과 속성을 묶을 수 있습니다.

## 접근자

```java
@startuml visible

class Dummy {
    -field1
    #field2
    +field3

    -method1()
    #method2()
    +method3()
}
@enduml
```

순서대로 private, protect, public입니다.

## 패키지

```
@startuml package

package "Collections" {
  Object <|-- ArrayList
}

package net.sourceforge.plantuml {
  Object <|-- Demo1
  Demo1 *- Demo2
}

@enduml
```

패키지 별로 나눌 수 있습니다.

## 위키

더 자세한 내용은 [plantUML 위키](http://wiki.plantuml.net/ko/site/class-diagram)에서 보실 수 있습니다.
