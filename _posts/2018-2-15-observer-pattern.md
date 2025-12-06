---
layout: post
title: java Observer pattern 실습해보기
---

오늘은 java로 Observer pattern을 실습해보았습니다.

옵저버, 즉 관찰자 패턴라고하며, 이 관찰자의 목록이 객체에 등록되어 상태에 변화가 있을때마다 메소드로 통지되는 패턴입니다.

주로 분산 이벤트 핸들링 시스템을 구현하는 데 사용된다고 합니다.

## 방식

이벤트가 발생하면 각 옵저버는 콜백을 받고, notify 함수는 관찰 대상이 발행한 메시지등을 전달합니다.

notify 함수를 구현함으로써 이벤트가 발생했을 때 처리할 각자의 동작을 정의할 수 있습니다.

## 코드 

```java
package observer;

import java.util.Observable;

public class Button extends Observable implements Runnable{

	@Override
	public void run() {
		setChanged();
		notifyObservers();
	}
}
```

Runnable 메소드를 오버라이딩하면서 Observable에서 메소드를 사용합니다.

```java
package observer;

import java.util.Observable;
import java.util.Observer;

public class Main {

	public static void main(String[] args) 
    {
		Button button = new Button();

		button.addObserver(new Observer() {

			@Override
			public void update(Observable o, Object arg) {

				System.out.println(o+" is Clicked");
			}
		});

		button.run();

		button.run();
		
        button.run();
		
	}

}

```

우선 Observable과 Observer를 import합니다.

메인 클래스에서 버튼 객체를 만들고, 버튼 이 객체에서 addObserver 메소드를 불러옵니다.

다만, 업데이트 메소드를 오버라이드해주어야 합니다.

테스트겸 run 메소드를 실행합니다.
