---
layout: post
title: java Visitor pattern 실습해보기
---

오늘은 Visitor pattern에 대하여 실습해보려 합니다.

Visitor pattern은 알고리즘을 객체 구조에서 분리시키는 디자인 패턴입니다.

이렇게 분리를 하게되면, 새로운 기능을 위해서 프로그램의 구조를 수정하지 않고서도 기존의 객체 구조에 기능을 추가할 수 있게 됩니다.

## 코드

```java
interface CarAccept {
	void accept(Visitor visit);
}
```

우선 방문자를 수락한다는 메소드를 가진 인터페이스를 만듭니다.

```java
interface Visitor {
	void visit(Car car);
	void visit(Body car);
}
```

방문자 인터페이스를 만들어서 각각 메소드에 인자를 건내줘야하므로 다른 인자 타입을 갖습니다.

```java
public class Car implements CarAccept{
	
	CarAccept[] list;
	
	public Car() {
		this.list = new CarAccept[] {
	            new Body(), new Engine()
	        };

	}

	@Override
	public void accept(Visitor visit) {
		for (CarAccept v : list) {
            v.accept(visit);;
        }
        visit.visit(this);
	}

}
```

객체들을 담을 리스트를 만들어준 뒤, 생성자를 통해 리스트에 인터페이스 타입의 객체를 넣어줍니다.

그리고 수락하는 메소드를 구현해줍니다.

```java
public class Body implements CarAccept {

	@Override
	public void accept(Visitor visit) {
		visit.visit(this);

	}

}
```

본체라는 클래스를 만들면서 호출 받으면 역 호출하면서 자기 자신을 방문 메소드로 건내줍니다.


```java
public class Implements_visit implements Visitor {

	@Override
	public void visit(Car car) {
		System.out.println("visit car");
		
	}

	@Override
	public void visit(Body car) {
		System.out.println("visit body");
		
	}
}
```

여태 방문 메소드를 추상으로만 뒀으니, 이제 구현을 해야합니다.

이 구현 클래스가 알고리즘을 객체로부터 분리시키는 역할을 합니다.

```java
public class Main {

	public static void main(String[] args) {
		Car test = new Car();
		
		test.accept(new Implements_visit());
	}

}
```

테스트해주는 메인 클래스에서 코드를 작성합니다.

객체를 만들면서 구현된 방문 메소드를 수락합니다.