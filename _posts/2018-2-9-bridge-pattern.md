---
layout: post
title: java Bridge Pattern 실습해보기
---

오늘도 Bridge Pattern을 java로 구현해보려고 합니다.

구현부에서 추상층을 분리하여 각자 독립적으로 변형할 수 있게 하는 패턴입니다.

adapter 패턴과 매우 흡사합니다.


## 코드

이번 패턴은 잘 정리된 위키피디아에서 인용했습니다.

```java
interface Api {
    public void draw(double x,double y,double radius);
}
```

우선 우리가 흔히 알고 있는 API로서 기본 기능을 추상화합니다.

이로인해 인터페이스, 즉 규격이 생겼습니다.

```java
class RealApi implements Api {
    public void draw(final double x, final double y, final double radius) {
        System.out.println("API1"+ " "+ x + " " + y+ " "+ radius);
    }
}
```

이 인터페이스를 실제로 구현한 클래스를 만듭니다.

```java
abstract class Shape {
    Api Api;

    Shape(Api Api){
        this.Api = Api;
    }

    public abstract void Realdraw();
}
```

추상 클래스를 만들어서 인터페이스 API 타입의 변수를 만들고, 생성자를 만들어서 들어오는 인자를 API 타입의 변수에 넣습니다.

그리고, 추상 클래스에 추상 메소드를 넣어줍니다.
이는 위 API 클래스에 있는 기능을 실제 제품에 넣는 역할을 합니다.

```java
class CircleShape extends Shape {
    private double x, y, radius;
    
    public CircleShape(double x,double y,double radius,Api drawingAPI) {
        super(drawingAPI);
        this.x = x;  
        this.y = y;  
        this.radius = radius;
    }


    public void Realdraw() {
    	Api.draw(x, y, radius);
    }
}
```

이 클래스에서만 쓰는 변수를 접근 지정자 private으로 만들어주고, 생성자를 만들어줍니다.

생성자는 api 타입의 변수까지 작성합니다.

그리고 이 클래스 기능을 넣을 때는 위 api 클래스에 있는 기능을 넣습니다.

이 부분이 마치 adapter 패턴과 유사합니다.

```java
public class Main {

	public static void main(String[] args) {
		CircleShape shapes =  new CircleShape(1, 2, 3, new RealApi());

		shapes.Realdraw();
	}
}
```

메인 클래스에서는 테스트차원에서 만들었습니다.

우선 기능 구현부 클래스를 타입으로한 객체를 생성하여 인자를 넣습니다.

마지막으로 api에서 추상화한 메소드를 포팅한 기능 메소드를 실행합니다.