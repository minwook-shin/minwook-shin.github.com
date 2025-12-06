---
layout: post
title: java doubly linkedlist 알아보기
---

doubly linkedlist, 즉 이중 연결리스트는 단순 연결리스트와 다르게 이전 노드를 가리킬 수 있습니다.

양방향성이라서 탐색에 좀 더 효율적이나, 이전 노드에 대한 정보를 유지해야하므로 더 많은 메모리의 공간을 잡습니다.

이전에 만들어둔 [연결리스트(linked list) 실습 자료](https://minwook-shin.github.io/java-linkedlist/)가 있으니 오늘은 주요 달라진 코드만 언급해보겠습니다.

```java
	private class Node {
		private Object data;
		private Node next;
		private Node prev;
		
		public Node(Object e) {
			this.data = e;
			this.next = null;
			this.prev = null;
		}
```

이전을 가리키는 노드가 생겼습니다.

이로인해 메모리를 더 잡아먹게 되는 것입니다.

Node 생성자에 이전 노드를 가리키는 변수도 초기화해줍니다.

```java
	public boolean add_fisrt(Object e) {
		Node new_node = new Node(e);
		new_node.next = head;
		
		if(head.prev != null)
			head.prev = new_node;
		
		head = new_node;
		size++;
		if(head.next == null)
			tail = head;
		return true;
	}
```

처음 노드를 추가할 때는 만약 머리(head)가 있다면 기존의 머리(head)의 이전 노드를 새로 추가한 노드에 연결해야 합니다.

```java
public boolean remove_first() {
		 Node temp = head;
		 head = head.next;
		 temp = null;
		 
		 if(head != null)
			 head.prev = null;
		 
		 size--;
		 
		 return true;
	 }
```

처음 값을 삭제할 때 이미 머리(head)가 존재하면 이전 노드를 초기화합니다.

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
			
			if(temp_next != null)
				temp_next.prev = new_node;
			new_node.prev = temp;
			
			size++;
			if(new_node.next == null)
				tail = new_node;
		 }
		 
		 return true;
	 }
```

원하는 위치에 추가할 때는 추가한 위치의 다음 노드의 이전과 이전 노드의 다음을 새로운 노드에 연결해주어야 합니다.

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
			 
			 if(temp.next != null)
				 temp.next.prev = temp;
			 
			 size--;
		 }
```

원하는 위치를 삭제할 때는 기존에 삭제할 위치의 이전 노드에서 다음의 다음 노드를 연결했기 때문에 이를 활용합니다.

삭제할 위치의 이전과 이후의 노드를 연결합니다.

```java
public boolean add_last(Object e) {
		Node new_node = new Node(e);
		if(size == 0)
			add_fisrt(e);
		else {
			tail.next = new_node;
			
			new_node.prev = tail;
			
			tail = new_node;
			size++;
		}
		
		return true;
	}
```

마지막에 추가할 때는 새로운 노드의 이전 노드를 설정해줘야 하므로 꼬리(tail)을 이전 노드라고 칭합니다.

이는 아직 꼬리(tail)이 새로운 노드가 추가되기 전의 꼬리 그대로여서 가능한 것입니다.

```java
protected Node node(int index) {
		
		if(index<size/2) {
			Node n = head;
			for(int i = 0; i<index;i++)
				n = n.next;
			return n;
		}
		else {
			Node n = tail;
			for (int i = size-1; i > index; i--)
				n = n.prev;
			return n;
		}	
	}
```

노드를 탐색할 때는 단순 연결 리스트와 다르게 이전으로 접근이 가능하므로. 절반을 기준으로 우측에 위치하면 역순으로 탐색합니다.

단순 연결리스트보다 탐색이 빠른 이유입니다.

추가로 반복자(iterator)는 이전에 실습한 단순 연결리스트와 다르게 이전 노드로 이동하는 것이 가능합니다.