---
layout: post
title: java arraylist 구현해보기
---

array list는 java에서 가장 많이 쓰이는 자료구조라고 전해져오고 있습니다.

우선 컬렉션으로 실습해보는 단계는 [이 곳](https://minwook-shin.github.io/java-collection/)에서 이미 실습해보았으므로, 오늘은 클래스로 직접 구현하여 알아보겠습니다.

## 특징

array의 특성으로 인해 인덱싱을 할 때 속도가 빠릅니다.
다만, linked list처럼 서로 연결되있는 게 아니라 메모리에 서로 array로 연결되어있습니다.

## 구현

## class

```java
public class Arraylist {
	private Object e_data[] = new Object[100];
```

먼저 private으로 해당 클래스에서만 사용하는 오브젝트 타입의 배열을 100의 크기로 생성합니다.

```java
	private int size = 0;
```

그리고 크기를 구하기 위한 private 변수를 만듭니다.

해당 변수는 변조를 막기위해 getter로만 얻어갈 예정입니다.

```java
	public boolean add_last(Object e){
		e_data[size] = e;
		size ++;
		
		return true;
	}
```

맨 뒤 배열에 값을 추가하는 메소드를 구현했습니다.

인자로 받은 오브젝트 타입의 e를 현재 크기, 즉 마지막 위치의 배열에 추가해줍니다.

그리고 추가해준 그 뒤의 값으로 증가 연산합니다.

```java
	public boolean add(int index, Object e) {
		for(int i = size-1; i >= index;i--) {
			e_data[i+1] = e_data[i];
		}
		e_data[index] = e;
		size++;
		
		return true;
	}
```

원하는 위치에 값을 추가하려면 for문으로 마지막 값을 i로 잡고 원하는 위치까지 증감 연산을 이용하여 수행합니다.

array, 즉 배열의 특성상 중간에 값을 넣으려면 그 뒤에 존재하는 모든 값들을 한 칸씩 미뤄야 되기 때문에 마지막 칸부터 +1로 대입해줍니다.

for문이 끝나게 되면 원하는 위치는 비어있으므로, 오브젝트 타입의 e를 대입해줍니다.

마지막으로 크기가 메소드를 실행하기전에 보다 한칸 증가했으므로 증가 연산을 사용하여 한 칸을 증가시킵니다.

```java
	public boolean add_first(Object e) {
		return add(0, e);
	}
```

처음에 값을 넣는 것도 생각해보면 첫번째의 위치가 0이므로, add() 메소드를 사용하여 리턴해주면 됩니다.

```java
	@Override
	public String toString() {
		String text = "[";
		for (int i = 0; i < size; i++) {
			text += e_data[i];
			if(i< size-1)
				text += ",";
		}
		return text + "]";
	}
```

기존의 toString 메소드는 arraylist의 값을 나타내지 못하므로 기존의 메소드를 오버라이딩합니다.

for문으로 string에 += 하여 값을 붙혀줍니다.

```java
	public boolean remove(int index) {
		for(int i = index + 1;i<=size-1;i++ ) {
			e_data[i-1] = e_data[i];
		}
		size--;
		e_data[size] = null;
		return true;
	}
```

원하는 위치의 값을 제거하는 메소드립니다.

for문으로 삭제를 원하는 위치의 다음 인덱스를 이전 인덱스로 대입합니다.

이 작업을 마지막 값의 위치까지 진행합니다.

그리고 크기를 증감 연산으로 한 칸 줄입니다.

마지막 값을 null로 채워서 없앱니다.

```java
	public boolean remove_first() {
		return remove(0);
	}
```

처음 값을 없앨 때는 이전의 remove 메소드를 이용하여 0번째 값을 지워줍니다.

```java
	public boolean remove_last() {
		return remove(size -1 );
	}
```

마지막 값도 첫번째 값처럼 해줍니다.

```java
	public Object get(int index) {
		return e_data[index];
	}
```

getter를 만들어서 해당 위치에 있는 값을 구해줍니다.

```java
	public int size() {
		return size;
	}
```

크기도 size 변수가 private 변수이므로 getter를 만들어줍니다.

```java
	public int indexOf(Object e) {
		for(int i =0; i<size; i++) {
			if(e.equals(e_data[i])){
				return i;
			}
		}
		return -1;
	}
```

for문을 이용하여 0부터 배열의 마지막 값까지 원하는 오트젝트 타입의 값을 비교해서 찾으면 위치를 반환해줍니다.

못찾으면 음수를 반환합니다.

```java
	public Iterator iterator() {
		
		return new Iterator();
	}
```

iterator, 즉 (한국어로) 반복자를 만들어줍니다.

새로운 반복자를 생성해줍니다.

iterator()를 실행하면 새로운 반복자가 반환되므로 메인 메소드에서 Iterator 타입의 변수에 넣어주면 됩니다.

```java
	class Iterator{
		private int next_i =0;
```

private 변수로 0으로 초기화 해줍니다.

```java
		public Object next() {
			Object return_val = e_data[next_i];
			next_i++;
			return return_val;
		}
```

다음 수를 리턴해줍니다.

시작점이 0이므로 먼저 배열의 0번째 수를 리턴 값에 넣고 위에 선언된 변수를 증가 연산하여 1을 더해줍니다.

이렇게 또 다시 next() 메소드를 불러오면 그 다음의 수를 불러오게 됩니다.

```java
		public boolean hasnext() {
			return next_i < size;
		}
```

다음의 수가 있는지 검사합니다.

배열의 크기보다 진행된 위치가 앞에 존재하면 true입니다.

```java
		public Object prev() {
			next_i--;
			Object return_val = e_data[next_i];
			return return_val;
		}
```

이전 수를 리턴해줍니다.

먼저 다음 수의 변수를 감소시키고, 그 감소된 변수의 수에 위치한 배열의 값을 오브젝트 타입의 변수에 대입해주고 리턴합니다.

```java
		public boolean hasprev() {
			return next_i > 0;
		}
```

hasnext() 메소드와 비슷하게 0보다 크면 true입니다.

```java
		public boolean add(Object e) {
			Arraylist.this.add(next_i, e);
			next_i++;
			return true;
		}
```

반복자가 수형한 위치에 추가하려면 다음 수의 위치를 지정한 변수를 활용합니다.

그리고 추가했으니 다음 수의 위치도 증가시킵니다.

```java
		public boolean remove() {
			Arraylist.this.remove(next_i-1);
			next_i--;
			return true;
		}
	}
}
```

삭제도 위와 비슷합니다.

## main

```java
public class Main {

	public static void main(String[] args) {
		Arraylist test = new Arraylist();
```

위에서 직접 구현한 Arraylist의 객체를 생성합니다.

```java
		test.add_last(10);
		test.add_last(20);
		test.add_last(30);
		test.add_last(50);
```

마지막에 추가하므로 ```[10,20,30,50]``` 이 됩니다.

```java
		test.add(3,40);
```

3번째 위치에 추가하므로 ```[10,20,30,40,50]``` 이 됩니다. 

```java
		test.add_first(0);
		System.out.println(test.toString());
```

첫번째에 추가하므로 ```[0,10,20,30,40,50]```가 되고, 출력하게 됩니다.

```java
		test.remove(4);
		
		System.out.println(test.toString());
```

4번째를 삭제하게 되므로 ```[0,10,20,30,50]```가 되고 출력하게 됩니다.

```java
		test.remove_first();
		
		System.out.println(test.toString());
```

첫번째를 삭제하게 되므로 ```[10,20,30,50]```가 되고 출력하게 됩니다.

```java
		test.remove_last();
		
		System.out.println(test.toString());
```

마지막을 삭제하게 되므로 ```[10,20,30]```가 되고 출력하게 됩니다.

```java
		System.out.println(test.get(0));
		System.out.println(test.get(1));
		System.out.println(test.get(2));
```

각 위치의 값을 출력하게 됩니다.

다만, 위와 같이 반복적으로 코딩하는 것보다 for문이나 while문으로 작성하는 것이 좋습니다.

```java
		System.out.println(test.size());
```

이 Arraylist의 객체 크기를 구합니다.

```java
		System.out.println(test.indexOf(30));
```

30이 어느 위치에 있는지 구합니다. 

이 코드 같은 경우 2라고 표시하게 됩니다.

```java
		Arraylist.Iterator it = test.iterator();
```

iterator, 즉 반복자를 생성합니다.

iterator() 메소드를 실행하여 Iterator 클래스 타입의 메소드에 접근할 수 있는 반복자 객체를 생성합니다.

```java
		System.out.println(it.hasnext());
		System.out.println(it.next());
		System.out.println(it.hasnext());
		System.out.println(it.next());
		System.out.println(it.hasnext());
		System.out.println(it.next());
		System.out.println(it.hasnext());
		System.out.println(it.next());
```

다음 위치에 수가 있는지 판별하고 다음 수를 출력합니다.

이렇게 코딩하는 것보다 ```while(it.hasnext())```로 반복시켜주는게 좋습니다.

```java
		System.out.println(it.hasprev());
		System.out.println(it.prev());
		System.out.println(it.hasprev());
		System.out.println(it.prev());
		System.out.println(it.hasprev());
		System.out.println(it.prev());
		System.out.println(it.hasprev());
		System.out.println(it.prev());
		System.out.println(it.hasprev());
```

이 역시 이렇게 코딩하는 것보다 ```while(it.hasprev())```로 반복시켜주는게 좋습니다.

```java
		it.add(100);
		System.out.println(test.toString());
```

반복자가 수행한 위치에 100을 추가시켜줍니다.

지금 이 코드에는 next와 prev가 수행되며 0번째 위치로 다시 돌아왔기 때문에 0번째에 들어가게 됩니다.

```java
		it.remove();
		System.out.println(test.toString());
	}
}
```

0번째 그대로에서 삭제해주면 위에 추가한 100이 삭제됩니다.