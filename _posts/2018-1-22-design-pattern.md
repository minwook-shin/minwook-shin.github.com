---
layout: post
title: java 디자인 패턴 실습해보기
---

일정한 패턴을 가지고 코딩을 하는 틀을 디자인 패턴이라고 하며 객체지향에서 중요하다고 합니다.

디자인 패턴에는 많은 패턴들이 있습니다.

다만, 지금은 개념만 다지기 위하여 쉬운 디자인 패턴 2개만 실습해보았습니다.

## singleton

클래스 하나에 하나의 객체를 생성하여 접근합니다.

방법은 생성자를 private으로 막은 다음 getter로 null일때만 할당해주면 됩니다.

* 클래스

```java
public class single_class {
	private static single_class instance = new single_class();
```

private한 static 속성의 인스턴스를 만듭니다.

```java
	public int i = 100;
```

정수형 변수를 public으로 선언하고 100을 할당합니다.

```java
	private single_class() {}
	
	public static single_class getinstance() {
		if(instance ==null)
			instance = new single_class();
		return instance;
	}
```

원래 코딩을 했을 때는 생성자를 썻겠지만, 생성자는 private으로 만들어두어 막아둡니다.

그리고, 인스턴스를 getter로 만들어서 인스턴스가 없을 때만 생성해주고 리턴합니다.

```java
	public int getI() {
		return i;
	}

	public void setI(int i) {
		this.i = i;
	}
}
```

* 인스턴스

```java
public class single_instance {
	public single_instance() {
		single_class test = single_class.getinstance();
		System.out.println(test.getI());
		test.setI(500);
		System.out.println(test.getI());
	}
}
```

첫번째 인스턴스의 생성자를 만들어서 위에 만들어둔 정수형 변수인 i를 조작합니다.

```java
public class single_instance2 {
	public single_instance2() {
		single_class test = single_class.getinstance();
		System.out.println(test.getI());
	}
}
```

두번째 인스턴스의 생성자로 정수형 변수인 i를 가져옵니다.

```java
public class main {
	public static void main(String[] args) {
		single_instance firsrt = new single_instance();
		single_instance2 second = new single_instance2();
	}

}
```

출력을 하기위한 main 함수입니다.

결과는 두번째 인스턴스가 불러와도 첫번째 인스턴스가 만든 결과를 가져오게 됩니다.

## strategy pattern

어떤 객체를 만들 때 객체가 가진 메소드들이 다양하게 있다면, 이 메소드들을 추상화하여 언제든지 적용할 수 있게 만드는 것입니다.

교환 가능한 메소드를 캡슐화하고 상속하면서 사용합니다.

아래는 자동차를 예시로 들면서, 연료와 크기를 기준으로 작성하였습니다.
그리고 예시이므로, 한개의 자동차 모델만 가지고 작성했습니다.

```java
public interface Ifuel {
	public void dofuel();
}
```

```java
public interface Isize {
	public void dosize();
}
```

두개의 인터페이스 파일을 만들어서 연료와 크기에 대한 규격을 만듭니다.

```java
public class C_oil implements Ifuel {

	@Override
	public void dofuel() {
		System.out.println("fuel : oil");
	}

}
```

```java
public class C_size implements Isize {

	@Override
	public void dosize() {
		System.out.println("size : mini");
	}

}
```

특정 모델(C)에 대한 연료, 크기를 작성해줍니다.

```java
public class Car {
	Ifuel fuel;
	Isize size;
	
	public Car(Ifuel fuel, Isize size) {
		this.fuel = fuel;
		this.size = size;
	}
	
	public void dofuel() {
		fuel.dofuel();
	}
	public void dosize() {
		size.dosize();
	}
}
```

자동차에 대한 큰 틀을 작성합니다.

```java
public class main {

	public static void main(String[] args) {
		Car minicar = new Car(new C_oil(), new C_size());
		minicar.dofuel();
		minicar.dosize();
	}

}
```

메인 함수에서 Car 클래스로 자동차의 스펙을 인자로 객체를 만들어서 메소드를 실행합니다.

## 마무리

디자인 패턴의 수가 많으므로 다 실습하려면 오래걸리겠지만, 오늘 실습으로 인해 디자인 패턴이 대략적으로 어떤 것인지는 알 수 있었던 시간이었습니다.