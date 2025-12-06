---
layout: post
title: java linkedlist 구현해보기
---

linked list는 각각의 노드들이 연결되어 있기때문에 데이터의 추가와 삭제에 유용합니다.

우선 컬렉션으로 실습해보는 단계는 [이 곳](https://minwook-shin.github.io/java-collection/)에서 이미 실습해보았으므로, 오늘은 클래스로 직접 구현하여 알아보겠습니다.

## 구현

* 클래스

```java
public class Linkedlist {
	private Node head;
	private Node tail;
	private int size = 0;
```

우선 노드 타입의 머리(시작)와 꼬리(끝)을 private으로 생성하고, 크기를 정하는 변수도 생성합니다.

```java
	private class Node {
		private Object data;
		private Node next;
```

노드에는 데이터를 보관하는 장소와 다음 데이터를 가리키는 공간이 있습니다.

```java
		public Node(Object e) {
			this.data = e;
			this.next = null;
		}
```

생성자를 만들어줘서 처음에 오브젝트 타입의 변수를 데이터에 넣어주고 다음 값은 null로 만들어줍니다.


```java
	public boolean add_fisrt(Object e) {
		Node new_node = new Node(e);
		new_node.next = head;
		head = new_node;
		size++;
		if(head.next == null)
			tail = head;
		return true;
	}
```

첫번째에 값을 추가합니다.

먼저 새로 만들 노드를 생성하면 위에 만들 생성자로 인해 값이 넣어집니다.

그리고 새로운 노드의 다음을 head(시작점)로 연결하고, 새로운 노드를 head(시작점)로 설정합니다.

그리고 추가한 1개만큼 크기를 한칸 증가시킵니다.

만약 시작점의 다음 노드가 없다면, 꼬리(tail)와 머리(head)를 동일하게 합니다.

```java
	public boolean add_last(Object e) {
		Node new_node = new Node(e);
		if(size == 0)
			add_fisrt(e);
		else {
			tail.next = new_node;
			tail = new_node;
			size++;
		}
		
		return true;
	}
```

마지막에 연결할 때는 앞에 추가할때와 마찬가지로 새로운 노드를 생성합니다.

만약 처음 추가되는 상황이라면 이전에 만들어진 메소드를 활용합니다.

마지막에 추가하는 것이니 꼬리(tail)의 다음 노드를 새로운 노드로 연결시키고, 새로운 노드를 꼬리(tail)로 만듭니다.

마지막으로 크기를 한칸 키웁니다.


```java
	protected Node node(int index) {
		Node n = head;
		for(int i = 0; i<index;i++)
			n = n.next;
		return n;
	}
```

node() 메소드를 만들어서 이후에 만들 메소드에 도움을 줍니다.

탐색을 위한 메소드입니다.

```java
	 public boolean add(int index, Object e) {
		 if(index == 0)
			 add_fisrt(e);
		 else {
			Node temp = node(index-1);
			Node temp_next = node(index);
			Node new_node = new Node(e);
			temp.next = new_node;
			new_node.next = temp_next;
			size++;
			if(new_node.next == null)
				tail = new_node;
		 }
		 
		 return true;
	 }
```

원하는 위치에 값을 추가하려면, 원하는 위치의 이전 노드를 알아둬야 합니다.

그리고 새로운 노드를 만들어서 알아둔 원하는 위치의 이전 노드의 다음으로 연결해야 합니다.

들어간 새로운 노드의 다음을 기존 인덱스 위치에 존재한 노드를 연결합니다.

마지막으로 크기를 키우고, 넣은 노드의 다음은 검사해서 null값이라면 tail(꼬리)로 연결합니다.

```java
	 @Override
	 public String toString() {
		 if(head == null)
			 return "[]";
		 else {
			 	Node temp = head;
			 	String str = "[";
			 	while(temp.next != null) {
			 		str += temp.data + ",";
			 		temp = temp.next;
			 		if(temp == tail)
			 			str+= temp.data;
			 	}
			 	
			 	return str + "]";
		 }
		}
```

toString() 메소드를 오버라이드하여 이 linked list를 보여줍니다.

머리(head)부터 시작해서 while문으로 temp의 다음이 없을때까지 str 변수에 누적시킵니다.

```java 
	 public boolean remove_first() {
		 Node temp = head;
		 head = head.next;
		 temp = null;
		 size--;
		 
		 return true;
	 }
```

처음 값을 삭제하려면 머리(head)를 기준으로 head의 다음을 head로 설정하고, 기존 head는 제거시킵니다.

그리고 크기를 줄입니다.

```java
	 public boolean remove(int index) {
		 if(index == 0)
			 return remove_first();
		 else {
			 Node temp = node(index-1);
			 Node del = temp.next;
			 temp.next = temp.next.next;
			 if(del == tail)
				 tail = temp;
			 del = null;
			 size--;
		 }
		 
		 return true;
	 }
```

원하는 위치에서 삭제하려면 삭제할 위치의 이전 노드를 알아야합니다.

이전 노드의 다음을 이전 노드의 다음과 다음으로 연결합니다.

그리고 삭제할 위치가 마지막이라면 삭제할 위치의 다음은 없으므로, 마지막을 꼬리(tail)로 연결합니다.

마지막으로 삭제할 노드를 null로 초기화하며 삭제하고 크기를 줄입니다.

```java
	 public boolean remove_last() {
		 return remove(size-1);
	 }
```

마지막을 제거하는 것은 마지막 위치를 제거해주면 됩니다.

```java	 
	 public int size() {
		 return size;
	 }
```

private 변수 size의 getter 메소드를 만듭니다.

```java
	 public Object get(int i) {
		 Node temp = node(i);
		 
		 return temp.data;
	 }
```

위에 만든 node 메소드를 이용하여 해당 위치의 값을 알아낼 수 있습니다.

```java
	 public int indexOf(Object e) {
		 Node temp = head;
		 int index = 0;
		 while(temp.data != e) {
			 temp = temp.next;
			 index ++;
			 if(temp == null)
				 return -1;
		 }
		 return index;
	 }
```

해당 오브젝트 변수가 어디에 있는지 알 수 있는 메소드입니다.

머리(head)를 기준으로 해당 변수가 맞을때까지 탐색하다가 찾은 값을 리턴합니다.

```java
	 public List_it iterator() {
		 return new List_it();
	 }
```

iterator, 즉 반복자를 만드는 메소드입니다.

```java
	 public class List_it{
		 private Node next;
		 private Node current;
		 private int next_i;
```

반복자 클래스입니다.

다음 노드와 현재 노드, 그리고 다음에 오는 위치를 저장할 변수를 private 변수로 만듭니다.

```java
		 List_it(){
			next = head; 
		 }
```

생성자로 처음에 head(머리)가 기준이 되게 합니다.

```java
		 public Object next() {
			 current = next;
			 next = next.next;
			 next_i++;
			 
			 return current.data;
		 }
```

반복자에서 다음 노드를 리턴하기위한 메소드입니다.

현재 노드를 다음 노드로 한 뒤, 다음의 다음 노드를 다음 노드로 바꿔줍니다.

이렇게하면 다음에 next() 메소드를 부르는 순간, 한 칸 전진하게 됩니다.

마지막으로 다음에 올 수의 위치를 증가시켜줍니다.

```java
		 public boolean hasnext() {
			 return next_i < size;
		 }
	 }
	 
}
```

현재까지 증가되어온 위치 값과 크기를 비교합니다.

크기가 같거나 작으면 다음 노드는 없는거나 마찬가지입니다.

* 메인

```java
public class Main {

	public static void main(String[] args) {
		Linkedlist test = new Linkedlist();
```

Linkedlist 에서 test 객체를 생성했습니다.

```java
		test.add_fisrt(0);
		test.add_fisrt(10);
		test.add_fisrt(20);
		test.add_fisrt(30);
```

앞에서 추가했으므로 stack과 유사하게 선입후출입니다.

```java
		test.add_last(10);
		test.add_last(20);
		test.add_last(30);
```

뒤에서 추가했으므로 queue와 유사하게 선입선출입니다.

```java
		test.add(2, 0);
```

2번째 위치에서 0을 추가했습니다.

```java
		System.out.println(test.toString());
```

```[30,20,0,10,0,10,20,30]```이 출력됩니다.

```java
		test.remove_first();
		
		System.out.println(test.toString());
```

처음 값이 지워졌으므로 ```[20,0,10,0,10,20,30]```이 출력됩니다.

```java
		test.remove(1);
		
		System.out.println(test.toString());
```

1번째 값이 지워졌으므로 ```[20,10,0,10,20,30]```이 출력됩니다.

```java
		test.remove_last();
		
		System.out.println(test.toString());
```

마지막 값이 지워졌으므로 ```[20,10,0,10,20]```이 출력됩니다.


```java
		System.out.println(test.get(3));
```

3이 출력됩니다.

```java
		Linkedlist.List_it it = test.iterator();
```

반복자를 생성하는 메소드로 객체를 만듭니다.

```java
		it.next();
		it.next();
		it.next();
		it.next();
		it.next();
		System.out.println(it.hasnext());
	}
}
```

위와 같이 next 메소드를 배열의 크기만큼 진행하고 다음 값을 확인하면 false가 출력됩니다.

