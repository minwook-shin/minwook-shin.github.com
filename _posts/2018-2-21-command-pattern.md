---
layout: post
title: java Command pattern 실습해보기
---

오늘은 자바로 Command 패턴을 실습해보았습니다.

실행 메소드를 객체의 형태로 캡슐화하여 사용자가 보낸 요청을 나중에 이용할 수 있도록 해주는 패턴입니다.

## 코드

실습에는 배열에 기능 객체를 생성하여 넣어주고, for each문으로 실행하게 됩니다.

```java
public interface Command {
	String execute();
}
```

우선 인터페이스로 string 반환 메소드를 추상화합니다.

```java
import java.util.ArrayList;

public class Main {
	public static void main(String[] args) {
		ArrayList<Command> test = new ArrayList<Command>();
		
		test.add(new Command() {
			
			@Override
			public String execute() {
				return "COM 1";
			}
		});
		test.add(new Command() {
			
			@Override
			public String execute() {
				return "COM 2";
			}
		});
		test.add(new Command() {
			
			@Override
			public String execute() {
				return "COM 3";
			}
		});
		
	for (Command command : test) {
		System.out.println(command.execute());
	}
	}
}
```

사용자가 보낸 요청을 나중에도 쓸 수 있게 arraylist를 만들어주고, 이 저장 공간에 add 메소드를 사용하여 객체를 넣습니다.

객체는 새로 생성하면서 가상함수로 오버라이딩하여 채워줍니다.

구현되면서 저장되므로 나중에 꺼내보면 넣은 객체의 순서대로 출력되게 됩니다.