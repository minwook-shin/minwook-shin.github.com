---
layout: post
title: java 기초 복습하기
---

오늘은 제가 학교에서 (저번 학기때) 배운 객체지향 언어인 java를 (방학 기회를 노려서) 복습해보았습니다.

## 기본

```java
public class java_learn {

	public static void main(String[] args) {
		System.out.println("hello world!");
```

출력을 할때 위와 같이 System.out.println이라고 작성합니다.

System.out.print는 개행을 안할 때 쓰입니다.

```java
		System.out.print("입력 : ");
		Scanner scanner = new Scanner(System.in);
		int i = scanner.nextInt();
		
		System.out.print("입력 : ");
		System.out.println(i);

```

System.in으로 받는 Scanner 객체(scanner)를 생성하여 입력값을 i로 받습니다.

그리고 i를 출력합니다.

```java
		if(i<=1) {
			System.out.println("음수입니다.");
		}else if(i>=i) {
			System.out.println("양수입니다.");
		}
		else {
			System.out.println("0입니다.");
		}
```

if문으로 괄호안에 특정 조건을 쓰며 사용할 수 있습니다.

```java
		switch (i) {
		case 0:
			System.out.println("hello, zero!");
			break;

		default:
			break;
		}
```

스위치문으로 정수를 구분하여 케이스별로 나눌 수 있습니다.

```java
		for(int j = 0;j<10;j++) {
			System.out.println(j);
		}
```

for문으로 조건부 반복할 수 있습니다.

(초기 값;조건;증가 혹은 증감)으로 사용합니다.

```java		
		int array[] = {10,20,30,40};
		System.out.println(array[0]);
		
		int array_two[] = new int[5];
		int array_three[][] = new int[5][5];

		for(int k=0;k<array_two.length;k++) {
			array_two[k] = k;
			System.out.println(array_two[k]);
		}
		
		for (int l = 0; l < array_three.length; l++) {
			for (int n = 0; n < array_three.length; n++) {
				array_three[l][n]=l*n;
			}
		}
		for (int m = 0; m < array_three.length; m++) {
			for (int o = 0; o < array_three.length; o++) {
				System.out.print(array_three[m][o]);
			}
			System.out.println("");
		}
	}
}
```

위와 같이 두가지 방식으로 배열을 생성할 수 있습니다.

만약 new로 선언하면서 생성하면 heap 메모리에 저장되어 가비지 컬렉션이 관리해줍니다.
그러므로, c언어 계열처럼 동적할당해도 따로 삭제하지 않아도 됩니다.

## 클래스

```java
public class oop {
	private int age;
	private String name;
	private int num;
	public static int wallet = 1000;
```

우선 필요한 변수들을 선언합니다.
대부분 변수는 getter로 얻어갈 수 있으므로 굳이 public으로 열어둘 필요가 없습니다.

그리고, wallet은 static으로 선언하여 1000을 다른 객체들이 서로 공유하도록 합니다.

```java
	public static int getWallet() {
		return wallet;
	}
```

static이 부여된 변수를 리턴하려면 위와 같이 해야합니다.

```java
	protected int sum(int i,int j) {
		int result;
		result = i+j;
		return result;
	}
```

protected는 동일 패키지에 속하는 클래스와 상속 관계 클래스에서만 사용가능합니다.

```java
	public void GetInfo() {
		System.out.println(name+age+num);
	}
```

void로 리턴 값 없이 할 수 있습니다.

```java
	public oop() {
		System.out.println("생성자 호출!");
	}

	public oop(String name,int age,int num) {
		this.name= name;
		this.age = age;
		this.num = num;
		System.out.println("생성자 호출!!");
	}
```

생성자 오버로딩을 사용할 수 있으며, this을 이용해서 클래스 변수를 가리킬 수 있습니다.

```java
	public int getAge() {
		return age;
	}

	public void setAge(int age) {
		this.age = age;
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public int getNum() {
		return num;
	}

	public void setNum(int num) {
		this.num = num;
	}
}
```

getter와 setter로 private인 변수에 접근할 수 있습니다.

```java
public class oop_two {
	public static void main(String[] args) {
		oop.wallet -= 100; 
		System.out.println(oop.getWallet());
		oop.wallet -= 100; 
		System.out.println(oop.getWallet());
		oop.wallet -= 100; 
		System.out.println(oop.getWallet());
		oop.wallet -= 100; 
		System.out.println(oop.getWallet());
		oop.wallet -= 100; 
		System.out.println(oop.getWallet());
	}
}
```

static 변수로 선언한 내용은 객체를 생성할 필요없이 변수를 공유합니다.

## 패키지

```java
package test_pack;

public class test {
	private String testname;

	public String getTestname() {
		return testname;
	}

	public void setTestname(String testname) {
		this.testname = testname;
	}
	
}
```

```java
import test_pack.*;
```

만약 타 패키지에 원하는 클래스나 변수가 있다면 import로 가져옵니다.

별표(*)로 전부 가져옵니다.

```java
public class other_oop extends oop {
	public static void main(String[] args) {
		other_oop test_other = new other_oop();
		System.out.println(test_other.getName());
	}
}
```

extends로 상속해줄 수 있습니다.

위에서 name이라는 변수를 private으로 하고 getName을 하게되면 null값이 나오므로 접근 지정자는 적절히 조절해주셔야 합니다.

## 추상

```java
public abstract class main {
	public abstract void red_goods();
	public abstract void blue_goods();
	public abstract void green_goods();
	public abstract void yellow_goods();
	public abstract void black_goods();
}
```

그냥 선언만 하는 추상 클래스입니다.

추상 클래스에는 한개이상의 추상 메소드가 있습니다.

```java
public class detail extends main{

	@Override
	public void black_goods() {
		// TODO Auto-generated method stub
		System.out.println("검은색 물건은 5000원입니다.");
	}
}
```

상속받아서 구현합니다.

다만, 위와 같이 추상 메소드중에 구현되지 않은 것이 남아 있다면 오류가 납니다.
반드시 모두 구현해야합니다.

```java
public class consumer {
	public static void main(String[] args) {
		detail user = new detail();
		user.black_goods();
	}
}
```

위에서 구현을 한 클래스로 객체를 만들어서 사용할 수 있습니다.

## 인터페이스

```java
public interface root_file {
	public static final int test_int = 10;
	public void black_goods();
}
```

```java
public class real_method implements root_file, root_file2 {
	@Override
	public void black_goods() {
		System.out.println("검은색 실제 구현");
	}
	@Override
	public void white_goods() {
		System.out.println("하얀색 실제 구현");
    }
}
```

인터페이스는 다중 상속이 가능하므로 위와 같이 구현할 수 있습니다.

```java
public class main {
	public static void main(String[] args) {
		real_method test = new real_method();
		test.black_goods();
		test.white_goods();
		
		root_file test_inter = new real_method();
		test_inter.black_goods();
		
		root_file2 test_inter2 = new real_method();
		test_inter2.white_goods();
	}
}
```

인터페이스를 타입으로 사용하여 구현된 메소드를 구분하여 작업할 수 있고, 반대로 타입을 통일시켜서도 사용 가능합니다.