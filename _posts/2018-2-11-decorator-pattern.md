---
layout: post
title: java Decorator pattern 실습해보기
---

오늘은 decorator 패턴에 대하여 직접 실습해보자 합니다.

주어진 상황 및 용도에 따라 어떤 객체에 책임을 덧붙이는 패턴이라고 합니다.

간단히 말해보면 객체에 또 다른 객체를 붙일 수 있습니다.

## 코드

```java
public interface Linux {
	public void getVersion();
}
```

우선 버전을 나타내는 메소드를 인터페이스로 만들어 둡니다.

```java
public class Ubuntu implements Linux {

	@Override
	public void getVersion() {
		System.out.println("Default version");
		
	}

}
```

인터페이스를 상속받아서 추상 메소드를 구현합니다.

이 코드는 기본 버전을 의미합니다.

```java
public abstract class Deco implements Linux {
	Linux linux;
	public Deco(Linux linux) {
		this.linux = linux;
	}
	
}
```

인터페이스를 또 상속받아서 추상 클래스인 데코레이터를 만듭니다.

그리고 생성자를 만들어 상위 클래스로 값을 넘깁니다.

```java
public class NewVersionDeco extends Deco {

	public NewVersionDeco(Linux linux) {
		super(linux);
		// TODO Auto-generated constructor stub
	}

	@Override
	public void getVersion() {
		System.out.println("NEW version");
	}

}
```

데코레이터 추상 클래스를 상속받아서 데코 클래스를 만듭니다.

생성자를 만들어주고, super로 넘깁니다.

오버라이드도 잊지 않습니다. 

```java
public class Main {

	public static void main(String[] args) {
		Linux test1 = new Ubuntu();
		test1.getVersion();
		
		Linux test2 = new NewVersionDeco(new Ubuntu());
		test2.getVersion();
		
		
	}

}
```

메인 클래스에서는 비교를 위해서 (데코레이터를 적용안한) 기본 클래스랑 데코가 적용된 클래스를 각각 선언하고 출력 메소드를 작성합니다.