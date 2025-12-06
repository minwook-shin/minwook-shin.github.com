---
layout: post
title: java Abstract factory Pattern 실습해보기
---

Abstract factory pattern은 다양한 구성 요소 별로 '객체의 집합'을 생성해야 할 때 유용합니다.

즉, 수 많은 서브 클래스들을 그룹으로 묶어서 한번에 사용할 수 있다고 합니다.

## 코드

이 코드에서는 윈도우와 우분투라는 OS에서 주요 특징을 메소드로 잡은 상태입니다.

```java
public interface Windows {
	public void gui();
}
```

```java
public interface Ubuntu {
	public void bash();
}
```

위 두 인터페이스 클래스를 만들어서 각 OS의 특징을 선언만 합니다.

```java
public class XP implements Windows {

	@Override
	public void gui() {
		System.out.println("graphic user interface");
	}
}
```

윈도우의 세부 버전의 클래스를 만들어서 위 인터페이스를 오버라이딩합니다.

그러면 선언만 되어 추상화된 메소드를 구현하게 됩니다.

예시 코드이므로 VOID 형태로 했습니다.

```java
public class LTS implements Ubuntu {

	@Override
	public void bash() {
		System.out.println("long term service");
		
	}
}
```

우분투에서도 장기 지원 버전을 세부 클래스로 만들어서 우분투 인터페이스를 상속합니다.

상속받은 추상화된 메소드를 구현합니다.

```java
public interface Ifactory {
	public Ubuntu getUbuntu();
	public Windows getWIN();
}
```

이 패턴의 이름답게 공장(Factory) 인터페이스를 만들어 줍니다.

그리고 각 OS 제품을 가져오기 위한 getter 메소드를 추상화합니다.

```java
public class Factory implements Ifactory {

	@Override
	public Ubuntu getUbuntu() {
		return new LTS();
	}

	@Override
	public Windows getWIN() {
		return new XP();
	}
	
}
```

위 공장 인터페이스를 구현한 공장 클래스입니다.

반환 타입이 인터페이스이며, 반환받는 새로운 객체는 그 인터페이스를 구현한 클래스 객체입니다.

```java
public class Main {

	public static void main(String[] args) {
		Ifactory test = new Factory();
		
		test.getUbuntu().bash();
		
		test.getWIN().gui();
	}

}
```

메인 클래스는 테스트 차원에서 제작되었으며, 간단하게 공장에서 새로운 객체를 생성해 getter로 객체를 얻어온 다음, 각 OS 별 특징 메소드를 불러오게 됩니다.