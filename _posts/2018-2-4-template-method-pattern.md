---
layout: post
title: java Template Method Pattern 실습해보기
---

지난 시간에 이어서 이번에는 Template Method Pattern을 실습하며 배워보려합니다.

알고리즘의 구조를 메소드에 정의하고 하위 클래스에서 알고리즘의 구조의 변경없이 정의하는 패턴입니다.

strategy pattern과 유사하지만, Template Method Pattern은 추상 클래스에 동일한 부분을 미리 구현해두어 조금 더 정적인 부분이 있습니다.

## 특징

구현하는 알고리즘에 일정한 프로세스가 있고, 변경 가능성이 있다는 특징이 있습니다.

## 방식

1. 알고리즘을 여러 단계로 나눕니다.

1. 나눈 단계를 메소드로 선언합니다.

1. 알고리즘을 수행할 템플릿 메소드를 만듭니다.

1. 하위클래스에서 나누어진 메소드를 구현합니다.

## 코드 

예제 코드로 자동차의 엔진을 시작하면서 연료의 종료, 소모량 등을 언급하고 차를 움직이게 하는 것을 작성해보았습니다.

우선 엔진을 시작하고 차를 움직이는 작업은 어느 자동차나 똑같다는 것을 인지합니다.

```java
public abstract class Car {

	public void engine() {
		System.out.println("preparing...");
		fuel();
		consum();
		System.out.println("running car!");
	}
	
	protected abstract void fuel();
	protected abstract void consum();
}
```

그리고 최상단의 추상 클래스를 작성할때 위에 인지한 공통점을 엔진 메소드에 넣고, 그 외 차이점은 추상 메소드로 작성합니다.

```java
public class mini_car extends Car {

	@Override
	protected void fuel() {
		System.out.println("oil");
	}

	@Override
	protected void consum() {
		System.out.println("low");
	}

}
``` 

```java
public class super_car extends Car {

	@Override
	protected void fuel() {
		System.out.println("oil");
	}

	@Override
	protected void consum() {
		System.out.println("high");
	}

}
```

추상 메소드를 포함된 클래스를 상속받아서 차이점을 채우기만 하면 됩니다.

```java
public class Main {

	public static void main(String[] args) {
		// TODO Auto-generated method stub

		mini_car car1 = new mini_car();
		super_car car2 = new super_car();
		
		car1.engine();
		
		car2.engine();
	}

}
```

메인에서는 각자의 객체로 구동합니다.