---
layout: post
title: java Chain of responsibility pattern 실습해보기
---

오늘은 java로 Chain of responsibility pattern, 즉 책인 연쇄 패턴에 대하여 실습해보려고 합니다.

객체간의 결합을 느슨하게 해주기 위한 방식으로서, 여러 객체가 요청을 처리하도록 맡겨두는 패턴입니다.

try catch문이 이 패턴과 유사한 패턴이라고 합니다.

## 코드

이번 코드는 계산기의 덧셈만 예시로 들면서 작성해보았습니다.

```java
public abstract class Calc {
	
	private Calc calc;
	
	abstract protected boolean operator(Request request);

	public boolean process(Request request) {
		if (operator(request)) {
			return true;
		} else {
			if (calc != null)
				return calc.operator(request);
		}
		return false;
	}
}
```

접근 지정자가 private인 Calc 클래스 타입의 변수를 만들고, operator 추상 메소드도 만듭니다.

process 메소드로 calc 변수에 값이 존재한다면 추상화한 operator 메소드에 인자를 넣어서 반환합니다.

```java
public class Request {
	
	private int i,j;
	
	private String string;

	public Request(int i, int j, String string) {
		super();
		this.i = i;
		this.j = j;
		this.string = string;
	}

	public int getI() {
		return i;
	}

	public void setI(int i) {
		this.i = i;
	}

	public int getJ() {
		return j;
	}

	public void setJ(int j) {
		this.j = j;
	}

	public String getString() {
		return string;
	}

	public void setString(String string) {
		this.string = string;
	}
	
	

}
```

숫자를 저장할 정수형 변수 두개와 덧셈을 체크할 문자열 변수를 private으로 선언합니다.
그리고 생성자를 만들어줍니다.

getter와 setter를 만들어서 private 변수에 접근할 수 있는 메소드도 만들어줍니다.

```java
public class PlusCalc extends Calc {

	@Override
	protected boolean operator(Request request) {
		if(request.getString().equals("+")){
			int a = request.getI();
			int b = request.getJ();
			int result = a + b; 
			System.out.println(result);
		}
		return false;
	}

}
```

덧셈을 맡은 클래스에서는 String 변수에 접근한 getter 메소드에서 문자열을 비교해서 덧셈을 수행합니다.

```java
public class Main {

	public static void main(String[] args) {
		Calc plus = new PlusCalc();

		Request request1 = new Request(1,2,"+");
		Request request2 = new Request(10,2,"-");

		plus.process(request1);
		plus.process(request2);
		}
}
```

메인 클래스에서는 덧셈 객체를 만들어서 요청을 받습니다.

하지만, 아직 뺄셈은 만들지 않았으므로 두번째 요청은 받아들여지지 않습니다.