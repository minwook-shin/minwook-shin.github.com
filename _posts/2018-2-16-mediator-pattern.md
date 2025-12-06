---
layout: post
title: java Mediator pattern 실습해보기
---

오늘은 java로 Mediator pattern을 실습해보려 합니다.

프로그램의 상호작용을 해야 하는 개체들이 서로 복잡하게 얽혀 있을 경우, 중재를 맡는 개체를 두게하는 패턴입니다.

번역하면 중개자 패턴이라고 하며, 중개자와 객체가 서로 통신하면서 상호작용합니다.

```java
public interface Imediator {
	void send(String colleague, String event);
}
```

인터페이스를 정의하여 통신을 보내는 메소드를 추상화합니다.

```java
import java.util.ArrayList;

public class Mediator implements Imediator {
	
	private ArrayList<Colleague> TestArray = new ArrayList<Colleague>();
	
	public Mediator(Colleague e) {
		e.setMediator(this);
        TestArray.add(e);
	}

	@Override
	public void send(String colleague, String event) {
		for (Colleague s : TestArray) {
            if (s.getString() == colleague) {
                    s.receive(colleague, event);
            }
    }
	}

}
```

우선 Arraylist를 만들어서 생성자를 통해 Mediator 객체의 setter에 보내준 다음, Arraylist에 저장합니다.

추상화한 인터페이스를 구체화합니다.
보내줄 때에 for문으로 일치할 때만 받는 것으로 작성합니다.

```java
public abstract class Colleague {
	Imediator mediator;
	
	public void setMediator(Imediator mediator) {
		this.mediator = mediator;
		}
	
	public void send(String name, String event) {
        mediator.send(name, event);
        }
	
	abstract public void send(String event);
	  
    abstract public void receive(String name, String event);
    
    abstract public String getString();
}
```

추상 클래스에서 중개자 객체를 만들어서 setter로 데이터를 받게 합니다.

받는 메소드로 중개자한테 같은 메소드로 넘겨줍니다.

나머지는 각 개체에 대한 추상 메소드를 만들어 줍니다.

```java
public class Com_1 extends Colleague {
	
	private String name = "COM 1";

	@Override
	public void send(String event) {
		mediator.send(name, event);
	}

	@Override
	public void receive(String name, String event) {
		System.out.println("Receive from " + name);
		
	}

	@Override
	public String getString() {
		return name;
	}

}
```

위에 만들어준 추상 클래스를 상속받아서 오버라이드합니다.

각 개체별로 이름을 private로 지정합니다.
그리고 이름은 getter로 접근할 수 있도록 만들어줍니다.

```java
public class Com_2 extends Colleague {
	
	private String name = "COM 2";

	@Override
	public void send(String event) {
		mediator.send(name, event);
	}

	@Override
	public void receive(String name, String event) {
		System.out.println("Receive from " + name);
		
	}

	@Override
	public String getString() {
		return name;
	}

}
```

두번째 개체도 위와 동일하게 작성합니다.

```java
public class Main {

	public static void main(String[] args) {
		Com_1 test1 = new Com_1();
		Com_2 test2 = new Com_2();
		
		Mediator test_M1 = new Mediator(test1);
		Mediator test_M2 = new Mediator(test2);
		
		test_M1.send("COM 1","hello world!");
		test_M2.send("COM 2","hello world!");
		
		test1.send("hello world!");
		test2.send("hello world!");
	}

}
```

메인 클래스에서는 각자의 개체를 객체로 선언해주고, 중개자도 선언합니다.

중개자는 생성자로 각 개체를 넣어줍니다.

마지막으로 개체에서 보내거나 중개자에서 보내면 결과를 확인할 수 있습니다.