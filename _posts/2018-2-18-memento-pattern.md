---
layout: post
title: java Memento pattern 실습해보기
---

오늘은 Memento 패턴을 자바로 실습해보겠습니다.

Memento 패턴은 객체의 상태를 저장하여 때에 따라서 복원할 수 있는 패턴입니다.

## 코드

```java
public class Memento {
	private String state;

	public String getState() {
		return state;
	}
	
	public void setState(String state) {
		this.state = state;
	}
	
	public Memento(String state) {
		setState(state);
	}
}
```

우선 객체의 특정 상태를 저장하고 반환할 클래스를 만듭니다.

이 코드에서는 문자열로 저장하기에 아무나 접근하지 못하는 private 변수로 선언합니다.

그리고 private에 접근할 공개적인 getter와 setter 메소드를 만듭니다.

생성자를 만들어서 setter에 들어가도록 해줍니다.

```java
public class Originator {
	private Stack<Memento> list = new Stack<>();
	private String state;
	
	public Memento setState(String state) {
		this.state = state;
		return list.push(new Memento(state));
	}
	
	public Memento getList() {
		return list.pop();
	}
	
	public String getState() {
		return state;
	}
	
	public void restore() {
		Memento val = getList();
		this.state = val.getState();
	}

}
```

특정 객체의 상태를 관리해주는 클래스입니다.

우선 메멘토 클래스처럼 상태를 나타내는 문자열 변수는 똑같이 해주며, 상태를 저장할 스택도 선언합니다.

그리고 private이므로 변수에 대한 setter와 getter를 만들어줍니다.
메멘토 클래스와 차이점이 있다면, 스택에 같이 넣어준다는 점입니다.

스택 역시 private이르모 getter를 만들어서 pop해줍니다.

복원 메소드는 준비해둔 스택 getter로 값을 불러와 메멘토 클래스에서 값을 가져와 상태 변수에 넣어줍니다.

```java
public class Main {

	public static void main(String[] args) {
		Originator originator = new Originator();
		
		originator.setState("state 1");
		originator.setState("state 2");
		originator.setState("state 3");

		originator.restore();
		System.out.println(originator.getState());
		originator.restore();
		System.out.println(originator.getState());
		originator.restore();
		System.out.println(originator.getState());
	}

}
```

메인 클래스에서는 위의 객체 상태 관리 클래스로부터 객체를 만들어줍니다.

그리고 상태들을 한개씩 저장해봅니다.

복원하면 역순으로 출력됩니다. 