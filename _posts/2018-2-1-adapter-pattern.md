---
layout: post
title: java adapter pattern 실습해보기
---

지난번에 [디자인 패턴을 포스팅했을때](https://minwook-shin.github.io/design-pattern/) singleton과 strategy pattern만 배워서 오늘부터 다른 패턴도 알아보려고 합니다.

adapter pattern은 작성한 클래스를 사용자에 맞춰서 인터페이스를 제작하는 방식으로, 호환성이 없는 인터페이스 때문에 동작할 수 없는 클래스들이 함께 작동하도록 해줍니다.

간단히 말해보면, 호환되지 않는 스펙을 가진 타 인터페이스를 기존 클래스에 맞춰서 사용할 수 있도록 제작한다는 것입니다.

## 사용 예시

A와 B의 인터페이스가 있고, A의 인터페이스를 가진 aa가 B의 인터페이스를 가진 bb가 있습니다. 이때 bb를 aa에 맞춰서 다시 제작할 일이 있을때 adapter pattern으로 작성하면 됩니다.

## 코드

* 기존

```java
public class Calc {
	public double div(double n,double i){
		return n/i;
	}
	public double mul(double n,double i){
		return n*i;
	}
}
```

처음 구현된 기능은 나눗셈과 곱셈입니다.

```java
public class Main {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Calc test = new Calc();	
		System.out.println(test.div(6, 2));
		System.out.println(test.mul(6, 2));
	}
}
```

그리고 메인에서 이를 객체로 실행하고 출력합니다.

이 상태로 실행하면 나눗셈과 곱셈밖에 기능이 없습니다.

* 추가된 인터페이스 

```java
public interface Adapter {
	public double div(double d, double i);
	public double mul(double d, double i);
	public double add(double d, double i);
}
```

추가적인 요구 사항은 덧셈도 추가되어 있습니다.

이로 인해 기존 클래스에서 이 인터페이스를 달아 놓을 경우, 미처 구현되지 못한 add() 라는 메소드로 인해 오류가 납니다.

그래서 adapter pattern이 필요합니다.

기존 기능의 클래스는 그대로 두면서, 새로운 기능이 추가된 adapter를 구현합니다.

```java
public class Adapter_im implements Adapter {
	Calc adapter = new Calc();
	

	@Override
	public double div(double d, double i) {
		return adapter.div(d, i);
	}

	@Override
	public double mul(double d, double i) {
		return adapter.mul(d, i);
	}

	@Override
	public double add(double d, double i) {
		return d+i;
	}

}
```

오버라이딩으로 이미 존재하는 기능 부분은 Calc 객체에서 가져오고, 기존에 없는 덧셈 부분만 직접 구현해줍니다.

```java
public class Main {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		Adapter_im test = new Adapter_im();
		
		System.out.println(test.div(6, 2));
		System.out.println(test.mul(6, 2));
		
		System.out.println(test.add(6, 2));
	}
}
```

이미 이전에 준비를 다해놨기 때문에, 메인에서는 그냥 객체를 생성해주고 실행만 하면 됩니다.

지금은 간단한 나눗셈, 곱셈으로만 실습한거지만 대량의 코드가 이미 존재해있는 라이브러리를 가져올때는 위와 같이 활용할 수 있다고 합니다.