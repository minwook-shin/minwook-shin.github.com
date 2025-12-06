---
layout: post
title: C++ 스터디 3주차 과제 수행
---

오늘도 우분투 한국 커뮤니티 c++ 기초 스터디의 4주차까지 해야 될 마지막 과제를 수행하면서 기록을 남깁니다.

이 과제는 돌아오는 주의 수요일까지 해야되는 과제입니다.

## 수행할 과제

* 링크드리스트 초기화 코드 작성

* 1부터 1000까지의 난수로 100개의 정수를 이진 탐색하기

* 파일 입력

## 과제 코드 복습

* 링크드리스트 초기화 코드 작성

```c++
	void del_all() {
		node *prev = new node;
		node *curr = new node;
		curr = head;
		while (curr) {
			prev->next = curr->next;
			delete curr;
			curr = prev->next;
		}
		head = NULL;
		tail = NULL;
		// 모든 노드를 지우는 코드를 작성합니다.
	}
};	
int main() {
	list a;
	a.del_all(); //ASSIGNMENT "del_all()"
	a.display();

	return 0;
}
```

```void del_all()```만 말씀드리자면,

```c++
node *prev = new node;
node *curr = new node;
```

이전 노드와 현재 노드를 만듭니다.

```c++
curr = head;
```

현재 노드를 앞으로 둡니다.

```c++
while (curr) {
			prev->next = curr->next;
			delete curr;
			curr = prev->next;
		}
```

현재 노드가 남아있는 동안, 현재 노드의 다음 것을 이전 노드의 다음과 연결합니다.
그리고, 떨어져 나온 현재 노드를 삭제합니다.

다시 이전 노드의 다음을 현재노드로 만듭니다.

```c++
head = NULL;
tail = NULL;
```

Head와 Tail을 NULL로 초기화합니다. 

 * 1부터 1000까지의 난수로 100개의 정수를 이진 탐색하기

```c++
#include<iostream>
#include<vector>
#include<random>
#include<algorithm>
#define min 0
#define max 1000

using namespace std;
```

선언해줄 헤더와 전처리기로 최소 최댓값을 정합니다.

```c++
int main() 
{
	random_device r;
	mt19937_64 rnd(r());

	uniform_int_distribution<int>range(min, max);
```

난수를 하드웨어 시드로 0부터 1000까지 생성합니다.

```c++
	vector<int>random_vec = {};

	for(int i = 0; i<100;i++)
	random_vec.push_back(range(rnd));

	for (int j = 0; j < random_vec.size(); j++) {
		cout << random_vec[j] << "\t";
	}
	cout << endl;
```

벡터를 생성해서 for문으로 push_back해서 생성된 난수를 벡터에 넣습니다.

```c++
	sort(random_vec.begin(),random_vec.end());

	for (int k = 0; k < random_vec.size(); k++) {
		cout << random_vec[k] << "\t";
	}
	cout << endl;
```

채워진 벡터를 이진탐색을 위해 정렬한 다음, 출력합니다.

```c++
	for (int l = 0; l < max; l++)
	    if(binary_search(random_vec.begin(), random_vec.end(), l))
		cout << l << "을 찾았습니다." << endl;

	return 0;
}
```

이진 탐색을 테스트하기 위해 0부터 최댓값까지 찾습니다.

* 파일 입력

```c++
#include <fstream>
#include <iostream>

using namespace std;

int  main() {
	ofstream File("NOTES.txt");
	for (int i = 1; i < 100+1; i++) {
		File << i << endl;
	}
	File.close();
}
```

ofstream으로 쓸 파일을 열고, 1부터 100까지 기입하고 닫습니다. 

## 마무리

단기 스터디라서 이번이 마지막 과제일듯하며, 다음 주차부터 마지막 6주차까지는 해커톤형식으로 개별적으로 프로그램을 만들게 됩니다.