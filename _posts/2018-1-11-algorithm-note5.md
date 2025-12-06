---
layout: post
title: 알고리즘 공부-5 (스택, 큐)
---

알고리즘 공부의 연장선으로 스택과 큐에 대한 내용을 공부해보았습니다.

아래는 오늘 공부한 내용을 요약한 노트입니다.

즉석에서 필기한 내용이라 잘 정리가 안된 포스팅입니다. 양해부탁드립니다.

스택과 큐는 배열과 리스트로 구현가능하고 STL 컨테이너로도 할 수 있지만, 이 포스팅에서는 STL로만 직접 구현해보았습니다.

## 스택

입구와 출구가 하나인 자료구조로서 last in first out (선입후출)의 형태를 가지고 있습니다.

* 배열 구현 : arraystack

데이터 집합을 배열에 저장하며, 스택의 크기가 미리 정해져있습니다.

배열과 top의 위치를 저장하는 맴버 변수가 필요합니다.

* 리스트 구현 : liststack

데이터집합을 링크드리스트에 저장하며, 스택 크기가 자유롭습니다.

단순 링크드리스트로 구현됩니다.

## STL

* empty () : 스택이 비어 있는지 여부를 리턴합니다.

* size () : 스택의 크기를 리턴합니다.

* top () : 스택의 최상위 요소에 대한 참조를 리턴합니다.

* push (g) : 스택 상단에 'g'요소를 추가합니다.

* pop () : 스택의 최상위 요소를 삭제합니다.

```c++
#include<iostream>
#include<stack>

using namespace std;

int main() {
	stack<int> stack_1;
	stack_1.push(10); // IN 10
	stack_1.push(20); // IN 20
	stack_1.push(30); // IN 30
	stack_1.push(40); // IN 40

	stack<int> view= stack_1;
	while (!view.empty()) { // 출력 코드
		cout << view.top() << "\t";
		view.pop();
	}

	cout << stack_1.size() << endl;
	return 0;
}
```

결과는 40 30 20 10 그리고 크기인 4가 같이 출력됩니다.

나중에 넣은 숫자가 먼저 나오면서 시작됩니다.

## 큐

입구와 출구가 따로 있는 자료구조이며, first in first out (선입선출)입니다.

* 배열 구현 : circularqueue

배열에 저장되며, 배열의 크기는 정해져 있습니다.

* 리스트 구현 : listqueue

링크드리스트에 저장되며, 크기제한이 없습니다.

## STL

```c++
#include<queue>
#include<iostream>

using namespace std;

int main() 
{
	queue<int> sample;

	sample.push(10); // IN 10
	sample.push(20); // IN 20
	sample.push(30); // IN 30
	sample.push(40); // IN 40
	sample.push(50); // IN 50

	while (!sample.empty()) { // 출력 코드
		cout << sample.front() << "\t";
		sample.pop();
	}
	return 0;
}
```

결과는 10 20 30 40 50 으로 먼저 넣은 숫자가 먼저 출력됩니다.

## 문자열을 넣으려면

```<std::string>``` 혹은 ```<char *>```을 넣으면 된다고 합니다.

## 번외 이야기

스택과 큐를 공부하다가... 이전에 우분투 한국 커뮤니티 c++ 기초 스터디때 나온 이야기가 있었습니다.

```
정수기 옆에 있는 종이컵 홀더에 큐로 만들어 놨는데, 사람들 중 스택으로 빼가는 사람이 있다.
``` 

(저만 듣기엔 아까운 명언이여서 블로그에 기록해둡니다! ㅎㅎ)