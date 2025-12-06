---
layout: post
title: java Factory Method Pattern 실습해보기
---

지난 시간에 이어서 이번에도 Factory Method Pattern을 실습하며 배워보려합니다.

Factory method는 상위 클래스에 알려지지 않은 구체 클래스를 생성하는 패턴이며. 하위 클래스가 어떤 객체를 생성할지를 결정하도록 하는 패턴이라고 합니다.

## 특징

공장처럼 각 세부 스펙을 추상으로 두고 자세한 스펙은 각 제품에 구현합니다.

그리고 공장에서 제품을 출하합니다.

## 코드 

```java
public abstract class Super {
	public abstract void price();
	public abstract void size();
}
```

우선 제품의 가격과 크기를 추상 클래스에 추상 메소드로 선언합니다.

```java
public class Minicar extends Super {
	
	private int price;
	private String size;
	
	Minicar(int price, String size){
		this.price = price;
		this.size = size;
	}
	
	public int getPrice() {
		return price;
	}

	public void setFuel(int price) {
		this.price = price;
	}

	public String getSize() {
		return size;
	}

	public void setSize(String size) {
		this.size = size;
	}

	@Override
	public void price() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void size() {
		// TODO Auto-generated method stub
		
	}
	
	@Override
	public String toString() {
		String strprice = String.valueOf(this.price);
		return strprice + this.size;
	}

}

```

위는 미니카를 예로 든 클래스입니다.

스펙 수치는 굳이 외부에 공개할 필요가 없으므로, 접근 지정자를 private으로 두고 getter와 setter를 만듭니다.

생성자를 이용하여 공장에서 내려올 인자를 받아 변수에 넣습니다.

그리고 toString을 오버라이드하여 객체가 생성될 때 정상적으로 적용되는지 테스트하기 위해 작성합니다.

```java
public class Miniphone extends Super {
	
	private int price;
	private String size;

	Miniphone(int price, String size){
		this.price = price;
		this.size = size;
	}
	
	public int getPrice() {
		return price;
	}

	public void setPrice(int price) {
		this.price = price;
	}

	public String getSize() {
		return size;
	}

	public void setSize(String size) {
		this.size = size;
	}

	@Override
	public void price() {
		// TODO Auto-generated method stub
		
	}

	@Override
	public void size() {
		// TODO Auto-generated method stub
		
	}
	
	@Override
	public String toString() {
		String strprice = String.valueOf(this.price);
		return strprice + this.size;
	}
}

```

공장이 잘 작동되는지 미니폰를 예로 든 클래스를 미니카처럼 생성합니다.

```java
public class Factory {
	public Super select(String class_name, int i, String size) {
		if("car".equals(class_name)) {
			return new Minicar(i, size);
			}
		else if("phone".equals(class_name)){
			return new Miniphone(i, size);
			}
		return null;
	}
}
```

팩토리 클래스에서는 각 제품을 선택하기위한 메소드를 만들어주면서 각 제품의 새로운 객체를 리턴해주어야 하므로 최상위 타입인 Super로 메소드를 작성합니다.

if문으로 분기점을 만들어주어 (switch문도 무관), 각자의 제품의 객체로 만들어지게 합니다.

다 준비를 끝낸 뒤, 메인 클래스에서 팩토리 클래스를 이용합니다.

```java
public class Main {

	public static void main(String[] args) {
		Factory test1 = new Factory();
		Factory test2 = new Factory();
		
		System.out.println(test1.select("car", 700000, "compact"));
		System.out.println(test2.select("phone", 200000, "compact"));
		
	}

}
```

팩토리 객체를 만들고 선택하는 메소드에서 분기점을 갈라 새로운 미니카 혹은 미니폰에 대한 객체를 생성합니다.

생성하면서 이전에 오버라이드한 toString으로 인해 출력됩니다.