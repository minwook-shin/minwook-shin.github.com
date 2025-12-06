---
layout: post
title: java Prototype pattern 실습해보기
---

지난 시간에 이어서 이번에도 prototype pattern을 실습하며 배워보려합니다.

생산 비용이 높은 인스턴스를 복사로 쉽게 생성 할 수 있도록 하는 패턴입니다.

종류가 많아서 클래스로 정리되지 않는 경우와 클래스로부터 인스턴스 생성이 어려운 경우를 생산 비용이 높은 인스턴스라고 생각할 때, 또 다른 객체를 생성할 시 복제를 할 수 있습니다.

## java 

복사할 수 있게 준비하고 사용하는 패턴이므로 java에서는 메소드가 준비되어 있어서 편합니다.

## 코드

웹사이트 코드를 복제한다고 가정해봅니다.

```java
public class Clone implements Cloneable {
	
	private String id = "readl id";

	public String getId() {
		return id;
	}
	
	public void setId(String id) {
		this.id = id;
	}

	@Override
	protected Object clone() throws CloneNotSupportedException {
		// TODO Auto-generated method stub
		return super.clone();
	}
}
```

웹사이트의 고유 이름을 id라고 가정하고, 이 고유적인 객체를 복제할 수 있게 자바에서 기본으로 제공해주는 Cloneable 추상 메소드를 불러와서 구현합니다.

그리고, getter와 setter를 생성하여 private 접근 지정자를 외부에서 접근 가능하게 합니다.

```java
	
	private int password;

	public Website(int password) {
		super();
		this.password = password;
	}

	public void setPassword(int password) {
		this.password = password;
	}
	
	public int getPassword() {
		return password;
	}
	
	Clone copy() throws CloneNotSupportedException {
		Clone Website = (Clone) clone();
		return Website;
	}

}
```

예제 코드이기에 간단하게 비밀번호라고 예시를 지었습니다.

위와 마찬가지로 private 변수를 생성하고 getter와 setter를 생성해줍니다.

그리고 생성자를 이용해서 메인 클래스에서 인자를 조작할 수 있도록 합니다.

마지막으로 중요한 복사 메소드를 작성합니다.
위의 최상단 클래스의 clone 메소드를 이용하여 해당 웹사이트 객체를 반환해줍니다.

```java
public class Main {

	public static void main(String[] args) throws CloneNotSupportedException {
		Website test = new Website(1234);
		Website test_copy = (Website) test.copy();
		
		System.out.println(test.getId()+"\n"+test_copy.getId() + "\n" + test.getPassword()+ "\n" + test_copy.getPassword());
	}

}
```

메인 클래스에서 원본 객체를 만들면서 비밀전호 인자를 1234로 임의로 넣었습니다.

이렇게 넣게되면 해당 객체의 비밀번호는 1234가 됩니다.

이 상태로 두번째 복사 객체를 만듭니다. 
이번에는 웹사이트 클래스의 copy 메소드를 이용하여 clone한 객체를 반환 받습니다.

그렇게 출력하게 되면 별도로 설정하지 않아도 비밀번호 변수는 같아지게 됩니다.