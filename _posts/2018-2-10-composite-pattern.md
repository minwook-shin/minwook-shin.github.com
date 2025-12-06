---
layout: post
title: java Composite Pattern 실습해보기
---

오늘 java로 composite Pattern을 실습해보려 합니다.

컨테이너와 내용을 동일시하는 패턴입니다.

## composite

컴포지트(composite)는 객체들의 관계를 트리 구조로 구성한다는 의미입니다.

## 코드

파일과 폴더를 구현해보는 예제가 유명한 것 같아서 따라서 실습해보았습니다.

```java
public interface Feature {
	public void print();
}
```

인터페이스를 이용하여 파일과 폴더에 공통된 메소드를 추상 메소드로 만들어줍니다.

```java
import java.util.ArrayList;
import java.util.List;

public class Folder implements Feature {
	private List<Feature> private_folder = new ArrayList<Feature>();

	@Override
	public void print() {
		for(Feature i : private_folder) {
			i.print();
		}
	}
	
	public void add(Feature t) {
		private_folder.add(t);
	}
}
```

폴더 클래스에서는 우선 폴더에 필요한 배열을 arraylist로 구현해줍니다.

굳이 외부에 보여줄 필요가 없으므로 private으로 접근 지정자를 설정해줍니다.

인터페이스를 구현하기 위해 가져왔으므로 미구현된 메소드를 구현합니다.

배열을 출력하려면 for-each문으로 차례대로 출력하게 했습니다.

마지막으로 배열에 인터페이스 타입의 객체를 추가하기위한 공개 메소드를 추가합니다.

```java
public class File implements Feature{

	@Override
	public void print() {
		System.out.println("print");
	}

}
```

파일 클래스 역시 인터페이스를 구현하기위해 만들어줍니다.

```java
public class Main {

	public static void main(String[] args) {
		 Folder test = new Folder();
		 
		 File t = new File();
		 
		 test.add(t);
		 test.add(t);
		 test.add(t);
		 
		 test.print();
	}

}
```

테스트를 위한 메인 클래스는 간단하게 작성했습니다.

폴더 객체를 만들어주고, 파일 객체도 만듭니다.

폴더에 파일을 추가하고, 출력도 할 수 있습니다. 