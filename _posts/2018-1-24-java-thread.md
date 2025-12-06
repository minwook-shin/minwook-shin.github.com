---
layout: post
title: java thread 실습해보기
---

java로 쓰레드에 대하여 잠시 알아보겠습니다.

## 정의

하나의 프로그램이 처리되면서 동작하는 것이 프로세스이며, 그 프로세스가 여러개로 나누어져 처리하는 것이 쓰레드입니다.

여러개의 쓰레드를 모아서 멀티 쓰레드로 작업을 하면 시간을 절약합니다.

java는 멀티 쓰레드를 지원합니다.

## 구현 방식

* runnalbe 인터페이스 구현을 통하여 run 메소드 구현

```java
public class Main {

	public static void main(String[] args) {
		Itest test = new Itest();
		Thread test_thread = new Thread(test,"first");

		test_thread.start();
		System.out.println(Thread.currentThread().getName());
	}

}
```

새로운 객체를 만들어서 쓰레드에 부여해줍니다.

그리고 start하여 시동을 겁니다.

마지막으로 메인 쓰레드의 이름을 출력하게합니다.

```java
public class Itest implements Runnable {
	
	int sum = 0;

	@Override
	public void run() {
		System.out.println(Thread.currentThread().getName());
		System.out.println("test_thread");
		
		for (int i = 0; i < 10; i++) {
			if(Thread.currentThread().getName()=="first")
				sum++;
			System.out.println(i+" "+sum);
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
	}

}

```

run메소드를 구현하면서 이 쓰레드의 이름을 출력하고, for문을 반복합니다.

Thread.sleep으로 쓰레드를 잠시 쉬게합니다.

* Thread 메소드를 상속받아서 run 메소드를 구현

```java
public class Main {

	public static void main(String[] args) {
		ctest test = new ctest();
		
		test.start();
		System.out.println(Thread.currentThread().getName());
	}

}
```

객체를 만들어서 시작합니다.

그리고 메인 쓰레드의 이름을 출력합니다.

```java
public class ctest extends Thread{
	@Override
	public void run() {
		System.out.println(Thread.currentThread().getName());
		System.out.println("test_thread");
		
		for (int i = 0; i < 10; i++) {
			System.out.println(++i);
			try {
				Thread.sleep(500);
			} catch (InterruptedException e) {
				// TODO Auto-generated catch block
				e.printStackTrace();
			}
		}
		}
	}

```

Thread를 상속받아서 구현합니다.

이 역시 run메소드를 구현하는 것이며, 현재 쓰레드 이름을 출력한 뒤 for문을 반복하게 됩니다.


## 멀티 쓰레드

* 객체 한개에 n개의 쓰레드를 동작합니다.

```java
public class Main {

	public static void main(String[] args) {
		Itest test = new Itest();
		Thread test_thread = new Thread(test,"first");
		Thread test_thread2 = new Thread(test,"second");
		test_thread.start();
		test_thread2.start();
		System.out.println(Thread.currentThread().getName());
	}

}
```

* 객체 한개에 한개의 쓰레드를 동작합니다.

```java
public class Main {

	public static void main(String[] args) {
		Itest test = new Itest();
		Itest test2 = new Itest();
		Thread test_thread = new Thread(test,"first");
		Thread test_thread2 = new Thread(test2,"second");
		test_thread.start();
		test_thread2.start();
		System.out.println(Thread.currentThread().getName());
	}

}
```


## 특징

main 쓰레드부터 시작하고, 직접 구현한 쓰레드를 시작합니다.

그리고, 멀티 쓰레드에서는 누가 먼저 시작할지 모릅니다.

## 클래스 변수

객체 하나를 쓰레드가 서로 사용하면 값을 같이 공유합니다.

* 1 객체 n 쓰레드 : 클래스에 있는 변수를 같이 사용함.

* 1 객체 1 쓰레드 : 클래스에 있는 변수를 각자 사용함.

## synchronized

먼저 수행되는 쓰레드의 모든 작업이 끝날때까지 다른 쓰레드를 기다리는 방식입니다.

```java
public synchronized void run()
```

위와 같이 run 메소드에 synchronized를 부여하면 됩니다.

결과는 첫번째로 시작되게 된 쓰레드가 전부 연산하고 다음 쓰레드로 넘어가게 됩니다.