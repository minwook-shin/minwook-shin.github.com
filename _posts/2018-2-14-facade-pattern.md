---
layout: post
title: java Facade pattern 실습해보기
---

오늘은 Facade pattern을 java로 실습해보았습니다.

퍼사드 패턴은 다수의 패키지와 퍼사드, 클라이언트로 구성되며, Facade는 "건물의 정면"을 의미한다고 합니다.

## 코드 

```java
package system;

class COM_1 {
	public void RaidCOM1() {
		System.out.println("booting com1...");
	}

}
```

우선 예시로 1번 컴퓨터라고 언급해두고 메소드를 작성합니다.

```java
package system;

class COM_2 {
	public void RaidCOM2() {
		System.out.println("booting com2...");
	}
}
```

2번째 컴퓨터도 언급하면서 메소드를 작성합니다.

```java
package system;

class COM_3 {
	public void RaidCOM3() {
		System.out.println("booting com3...");
	}
}
```

3번째 컴퓨터도 언급하면서 메소드를 작성합니다.

```java
package system;

public class Facade {
	
	public Facade() {
		System.out.println("calling facade");
	}
	COM_1 com1 = new COM_1();
	COM_2 com2 = new COM_2();
	COM_3 com3 = new COM_3();
		
	public void boot() {
		com1.RaidCOM1();
		com2.RaidCOM2();
		com3.RaidCOM3();
	}

}
```

가장 중요한 퍼사드 클래스에서 각자의 컴퓨터 클래스에 대한 객체를 만들어줍니다.

이로서 퍼사드 패턴의 주요 클래스까지 작성되었습니다.

```java
package facade;

import system.Facade;

public class Main {

	public static void main(String[] args) {
		Facade test = new Facade();
		
		test.boot();
	}

}
```

클라이언트 코드로는 아무리 시스템쪽에 코드가 추가된다고 해도 변화가 거의 없습니다.

단지 퍼사드 객체를 불러와서 주요 메소드만 실행하면 됩니다.