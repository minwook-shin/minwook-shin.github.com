---
layout: post
title: java Proxy pattern 실습해보기
---

오늘은 java로 Proxy pattern을 실습해본 자료입니다.

프록시 패턴은 프록시 자체의 뜻과 비슷하게 대신해주는 패턴입니다.

간단한 일을 프록시 클래스에서 대신 수행하고 복잡한 일은 본 클래스에서 수행하게 됩니다.

```java
public interface Value {
	public void getValue();
}
```

우선 인터페이스로 공통된 메소드를 추상화합니다.

```java
public class RealValue implements Value{
	
	private String val;
	
	public RealValue(String val) {
		setVal(val);
		save(val);
	}

	private void save(String val) {
		System.out.println("Load "+val);
	}

	@Override
	public void getValue() {
		System.out.println("get Real Value");
	}
	
	public void setVal(String val) {
		this.val = val;
	}
}
```

값을 저장할 변수를 만들고 private으로 접근지정자로 지정합니다.

그리고 생성자로 변수에 값을 저장하며 출력합니다.

인터페이스에 추상화된 메소드로 구현합니다.

```java
public class ProxyValue implements Value{
	
	private String val;
	RealValue realvalue;
	
	public ProxyValue(String val) {
		setVal(val);
	}

	@Override
	public void getValue() {
		if(realvalue == null)
			realvalue = new RealValue(val);
		realvalue.getValue();
	}
	
	public void setVal(String val) {
		this.val = val;
	}

}
```

프록시 클래스에서는 대부분 기능을 그대로 할 수 있겠지만 중요한 메소드는 넘겨줍니다.

이 코드는 getValue 메소드가 넘겨주게됩니다.

생성된 객체가 없다면 인자를 넣어 만들어주고, 있다면 그대로 getValue 메소드를 넘겨줍니다.

```java
public class Main {
	public static void main(String[] args) {
		ProxyValue test = new ProxyValue("one value");
		
		test.getValue();
	}
}
```

메인 클래스는 테스트의 형태입니다.

프록시 객체에서 해당 메소드를 실행해도 프록시 클래스에서는 실행하지않고 넘겨줍니다.