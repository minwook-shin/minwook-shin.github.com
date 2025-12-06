---
layout: post
title: java collection 실습해보기
---

이전에 c++로 STL로 자료구조를 살짝 느껴본 내용을 바탕으로 자바에서 (C++의 STL처럼) 컬렉션을 실습해보았습니다.

이론 내용은 거의 건너뛰고 바로 실습으로 넘어갔습니다.

## array list

```java
import java.util.ArrayList;
```

우선 ArrayList를 임포트합니다.

이클립스로 코딩한다면, 알아서 작성됩니다.

```java
public class Main {

	public static void main(String[] args) {
		ArrayList<Integer>int_arr = new ArrayList<Integer>();
```

ArrayList 객체를 생성합니다.

제네릭으로 정수형을 선언했습니다.

```java
		int_arr.add(1);
		int_arr.add(2);
		int_arr.add(3);
		int_arr.add(4);
		int_arr.add(5);
```

add로 값을 넣습니다.

순서, 즉 인덱스로 판별할 수 있습니다.

```java
		System.out.println(int_arr.get(0));
```

특정 인덱스를 get하여 값을 알아낼 수 있습니다.

```java
		int_arr.set(0, 10);
```

0번째의 값을 10으로 바꿉니다.

```java
		System.out.println(int_arr.size());
```

이 자료구조의 크기를 나타냅니다.

```java
		int_arr.remove(4);
		
		System.out.println(int_arr.toString());
```

4번째의 값을 지웁니다.

여기서 4번째라 함은 일반적으로 5번째 값이라는 소리입니다.
(0부터 셈을 하기때문에...)

```java
		int_arr.clear();		
		System.out.println(int_arr.toString());
	}
}
```

마지막으로 지워주면 ```[]```만 표시됩니다.

모두 삭제하려면 null로 해주어야 합니다.

## Linked List

```java
import java.util.LinkedList;
```

우선 LinkedList를 임포트합니다.

이클립스로 코딩한다면, 알아서 작성됩니다.

```java
public class Main {

	public static void main(String[] args) {
		LinkedList<Integer> linked_int = new LinkedList<Integer>();
```

LinkedList 객체를 생성합니다.

제네릭으로 정수형을 선언했습니다.

```java
		linked_int.add(1);
		linked_int.add(2);
		linked_int.add(3);
		linked_int.add(4);
		linked_int.add(5);
        System.out.println(linked_int.toString());
```

add로 값을 넣습니다.

```java
		linked_int.add(0, 10);
		
		System.out.println(linked_int.toString());
```

해당하는 인덱스(0)에 값을 넣기 위해서는 그 뒤에 있는 인덱스들을 밀어내고 집어 넣습니다.

```java
		linked_int.set(4, 50);
```

해당하는 인덱스(4)에 값을 50으로 변경합니다.

```java
		System.out.println(linked_int.size());
```

이 자료구조의 크기를 나타냅니다.

```java
		linked_int.remove(0);
		
		System.out.println(linked_int.toString());
```

0번째의 값을 지웁니다.

```java
		linked_int.clear();
		
		System.out.println(linked_int.toString());
	}
}
```

마지막으로 지워주면 ```[]```만 표시됩니다.

모두 삭제하려면 null로 해주어야 합니다.

## HashMap

```java
import java.util.HashMap;
```

우선 HashMap를 임포트합니다.

이클립스로 코딩한다면, 알아서 작성됩니다.

```java
public class Main {

	public static void main(String[] args) {
		HashMap<Integer, String> htest = new HashMap<Integer, String>();
```

HashMap 객체를 생성합니다.

제네릭으로 정수형을 선언했습니다.

```java
		htest.put(0, "number0");
		htest.put(1, "number1");
		htest.put(2, "number2");
		htest.put(3, "number3");
		htest.put(4, "number4");
		System.out.println(htest.toString());
		
```

put으로 키와 값을 넣습니다.

여기서는 키로 인덱싱을 하게됩니다.

```java
		System.out.println(htest.get(4));
```

get으로 키로 값을 찾습니다.

```java
		htest.remove(0);
		System.out.println(htest.toString());
```

특정 키에 위치한 값을 지워줍니다.

```java
		htest.clear();
		System.out.println(htest.toString());
	}

}
```

모두 지워줍니다.

## HashSet

```java
import java.util.HashSet;
```

우선 HashSet를 임포트합니다.

이클립스로 코딩한다면, 알아서 작성됩니다.

```java
public class Main {

	public static void main(String[] args) {
		HashSet<Integer> hint = new HashSet<Integer>();
```

HashSet 객체를 생성합니다.

제네릭으로 정수형을 선언했습니다.

```java
		hint.add(0);
		hint.add(4);
		hint.add(2);
		hint.add(5);
		hint.add(2);
		hint.add(5);
		hint.add(1);
		hint.add(0);
		System.out.println(hint.toString());
```

add로 값을 넣습니다.

set 특징으로 인해 중복된 값은 없어지며, 순차적으로 정렬되어 출력됩니다.

```java
		hint.remove(0);
		System.out.println(hint.toString());
	}
}
```

지울 수 있습니다.

## Iterator

Iterator로 자료구조 유형을 순차적으로 접근할 수 있습니다.

```java
import java.util.Iterator;
```

임포트해주고

```java
        Iterator<string> iterator = collection.iterator();
 
        while (iterator.hasNext()) {
        System.out.println(iterator.next());
        }
```

위와 같이 선언 후, hasNext()로 다음 내용이 존재하는지 검사합니다.

만약 있다면, next()로 다음 내용을 출력해줍니다.