---
layout: post
title: java Builder Pattern 실습해보기
---

지난 시간에 이어서 이번에도 builder Pattern을 실습하며 배워보려합니다.

빌더 패턴은 복잡한 단계를 거처야 생성되는 객체의 구현을 서브 클래스에 넘겨주는 패턴입니다.

보통 생성자만으로만 인자를 넘겨주기 힘들때 쓰이는 패턴으로서 점층적 생성자 패턴과 자바빈 패턴의 장점을 모은 패턴이라고 합니다.

## 코드

우선 컴퓨터에 대한 스펙을 인자로 준다고 가정할 때, 클래스 생성자에 인자로 모든 부품들을 언급하기 힘드므로, 빌더 패턴을 사용해보겠습니다.

```java
public class Computer {
	
	private String HDD;
	private String RAM;

	private boolean GraphicsCard;
	
	public String getHDD() {
		return HDD;
	}

	public String getRAM() {
		return RAM;
	}

	public boolean isGraphicsCard() {
		return GraphicsCard;
	}

	Computer(Build builder) {
		this.HDD=builder.HDD;
		this.RAM=builder.RAM;
		this.GraphicsCard=builder.GraphicsCard;
	}

}
```

코드에서는 간단하게 하드랑 램의 용량과 그래픽 카드 유무를 판별하려고 합니다.

컴퓨터의 클래스에서는 굳이 값을 정해줄 필요가 없으므로, getter만 작성합니다.

그리고, 생성자를 빌더에게 넘겨주기 위해 위와 같이 다음에 짤 빌더 클래스 타입의 인자를 받습니다.

```java
public class Build {
			String HDD;
			String RAM;

			boolean GraphicsCard;
			
			public Build(String hdd, String ram){
				this.HDD=hdd;
				this.RAM=ram;
			}

			public Build setGraphicsCard(boolean GraphicsCard) {
				this.GraphicsCard = GraphicsCard;
				return this;
			}
			
			public Computer build(){
				return new Computer(this);
			}
}
```

빌더 클래스에서는 컴퓨터의 스펙을 동일하게 변수화한 뒤, 생성자를 작성합니다.

꼭 작성할 필요가 없는 항목들은 생성자와 별개로 해당 클래스 타입의 메소드로 작성합니다.

마지막으로 여기서 작성된 정보들을 컴퓨터 클래스에 넘겨주기위한 빌드 메소드를 작성합니다.

빌드 메소드는 당연히 컴퓨터 타입으로 객체를 생성하며 반환해주면 됩니다.

```java
public class Main {

	public static void main(String[] args) {
		Computer test = new Build("hdd 1G", "ram 4G").setGraphicsCard(true).build();
		
		System.out.println(test.isGraphicsCard());
	}

}
```

메인에서 테스트할 때에는 필수로 넣어야 하는 정보들을 생성자 인자로 삽입하고, 그 다음 추가 정보를 넣습니다.

마지막으로 작성한 빌드 메소드를 사용하여 빌드하게 됩니다.