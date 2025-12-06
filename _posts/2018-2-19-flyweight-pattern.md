---
layout: post
title: java Flyweight pattern 실습해보기
---

오늘은 자바로 flyweight 패턴을 실습해보았습니다.

이 패턴은 객체들을 공유하여 메모리 사용량에 초점을 둔 패턴이며,

대체로 공유되어도 되는 객체를 많이 생성할 때, 이 패턴으로 설계하면 유리하다고 합니다.

객체를 공유하는 유사 패턴으로는 싱글톤 패턴도 있습니다.

## 코드

아래 코드는 자바 코드를 처음 실행할 때는 만들어지고 두번 실행하면 실행만 되는 예시로 작성했습니다.

```java
public interface Iexe {

	public void exe();
}
```

먼저 인터페이스를 만들어서 실행 메소드를 추상화해줍니다.

```java
public class Java implements Iexe {

	public Java(){
		System.out.println("Java created");
	}
	
	@Override
	public void exe() {
		System.out.println("execute");
	}

}
```

자바를 예로 들어 인터페이스로 받아들여, 생성자와 오버라이딩된 메소드를 작성합니다.

생성자는 해당 객체가 생성될 때만 실행되고, 오버라이딩한 메소드는 실행할 때 무조건 출력됩니다.

```java
import java.util.HashMap;

public final class Factory {
	
	private HashMap<String, Java>map = new HashMap<String, Java>();
	
	public Java getInstance(String t){
		Java instance = map.get(t);
		if(instance == null && t == "JAVA"){
			instance = new Java(); 	   
			}
		map.put(t, instance);
		return instance;
	}
}
```

팩토리 패턴을 생각하며 공장 메소드를 만듭니다.

해시맵으로 키와 값을 각각 string, java로 설정합니다.
키로 객체를 찾아서 값을 사용하기 위함입니다.

인스턴스 getter로 싱글톤 패턴과 유사하게 키로 찾아서 값이 null, 즉 없다면 객체를 만들어서 넣어주고 반환합니다.

있다면, 그대로 넣고 반환합니다.

```java
public class Main {

	public static void main(String[] args) {

		Factory factory = new Factory();
		
		Java test = factory.getInstance("JAVA");
		test.exe();
		test = factory.getInstance("JAVA");
		test.exe();
	}

}
```

메인 클래스에서는 우선 팩토리 객체를 만들어서 인스턴스를 만듭니다.

인스턴스가 생성될때마다 자바 객체가 나오면서 처음에 생성자로 실행됩니다.

두번째 실행에는 팩토리 클래스에서 if문을 넘어가면서 실행만하게 됩니다.