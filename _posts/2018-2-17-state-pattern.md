---
layout: post
title: java State pattern 실습해보기
---

오늘은 자바로 state 패턴을 실습해보았습니다.

기능을 메소드로 묶어든 뒤, 객체의 속성에 따라 객체의 메소드를 변화줄 수 있습니다.

if문이 계속 바꿔야되는 상황에서 이 패턴이 쓰인다고 합니다.

## 코드

on과 off로 상태를 변화하면서 기능도 변화시킵니다.

```java
public class state {
	
	IState State;
	
	IState test_state1 = new IState() {
		
		@Override
		public void on() {
			System.out.println("on");
			State = test_state2;
		}
		
		@Override
		public void off() {
			System.out.println("none");
		}
	};
	
	IState test_state2 = new IState() {
		
		@Override
		public void on() {
			System.out.println("none");
		}
		
		@Override
		public void off() {
			System.out.println("off");
			State = test_state1;
		}
	};
	
	public state() {
		State = test_state1;
	}
	
	public void on() {
		State.on();
	}
	
	public void off() {
		State.off();
	}
	
}

interface IState {
	
	void on();
	void off();
	
}
```

test_state1와 test_state2의 상태를 정의하면서 기능 메소드는 인터페이스로 정의합니다.

정의한 상태 객체에서 인터페이스를 오버라이드라여 구체화합니다.

상태에 변화를 줄 기능 구현으로 다른 상태로 넘어갈 코드를 작성합니다.

```java
public class Main {

	public static void main(String[] args) {
		state test = new state();
		
		test.off();
		test.on();
		test.on();
		test.off();
		test.off();

	}

}
```

메인 클래스에서 상태 객체를 만들어서 on, off로 각각의 상태로 넘어갑니다.