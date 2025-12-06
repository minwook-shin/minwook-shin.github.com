---
layout: post
title: c++ 스터디 1주차 기록
---

오늘은 우분투 한국 커뮤니티에서 주관하고, 제가 주최한 c++ 스터디의 1주차 기록을 간략히 남겨보려합니다.

참고로 스터디는 매주 수요일에 합니다 ㅎㅎ; (단순히 제가 늦게 기록 남기고 있는거랍니다.) 

## 이번 주차에서 배운 것

* 기초 내용 : 출력, 연산자, 변수, 조건문, 난수, 반복문 

* 수행된 과제 : 이름 출력, 원의 둘레 구하기, 변수 출력하기, 성적 등급 판별하기, 기상 예측 프로그램 만들기(랜덤함수), 극장 선택기 

## 추가로 배운 것

* rand 함수를 쓰려면 <stdlib.h> 헤더가 있어야 합니다.

(저는 비주얼 스튜디오를 쓰고 있어서 몰랐던 내용이였습니다.)

rand() Reference : https://msdn.microsoft.com/en-us/library/398ax69y.aspx

## 수행한 스터디 과제 코드

아래는 제가 수행한 과제입니다.

타 스터디원분들의 코드는 [위키](https://wiki.ubuntu-kr.org/index.php/C%2B%2B_Basic_Study)에서 보실 수 있습니다.

* hello, world!

```c++
#include<iostream>

int main() {
	std::cout << "hello, world!" << std::endl;
	return 0;
}
```

* 이름 출력하기

```c++
#include<iostream>

using namespace std;

int main() {
	cout << "minwook-shin" << endl;
	return 0;
}
```

* 변수 타입 연습하기

```c++
#include<iostream>

using namespace std;

int main() {
	int a = 0;
	bool b = true;
	short c = 1;
	float d = 3.14;
	double e = 3.14;

	cout << a << "\n" << b << "\n" << c << "\n" << d << "\n" << e << endl;
	return 0;
}
```

* 연산자 익히기

```c++
#include<iostream>

using namespace std;

int main() {
	double num = 0;

	int radius = 17;

	num = 3.14 * 2 * radius;

	cout << num << endl;

	return 0;
}
```

* if를 이용한 성적 판별하기

```c++
#include<iostream>

using namespace std;

int main() {
	int score = 0;
	cout << "enter score : ";
	cin >> score;

	if (100 >= score && score >= 80) {
		cout << "A" << endl;
	}
	else if (80 > score && score >= 60) {
		cout << "B" << endl;
	}
	else if (60 > score && score >= 40) {
		cout << "C" << endl;
	}
	else if (40 > score && score >= 0) {
		cout << "F" << endl;
	}

	return 0;
}
```

* 난수를 이용한 기상 예측하기

```c++
#include<iostream>
#include<time.h>
#include<stdlib.h>

using namespace std;

int main() {
	srand((unsigned int)time(0));

	double percent = 0;

	percent = rand() % 10000 / 100.f;

	cout << "random percent : " << percent << endl;;
	if (0 <= percent && percent <= 30) {
		cout << "rain" << endl;
	}
	else if (30 < percent && percent <= 41) {
		cout << "snow" << endl;
	}
	return 0;
}
```

* 열거체와 switch를 이용하여 극장 선택하기

```c++
#include<iostream>

using namespace std;

enum theater {
	theater_1 = 1,
	theater_2,
	theater_3
};

int main() {
	int a = 0;
	cout << "enter number(1-3) : ";
	cin >> a;

	switch (a) {
	case theater_1:
		cout << "selected first theater" << endl;
		break;
	case theater_2:
		cout << "selected second theater" << endl;
		break;
	case theater_3:
		cout << "selected third theater" << endl;
		break;
	default:
		cout << "selected other number" << endl;
		break;
	}

	return 0;
}
```

## 앞으로 수행할 스터디 과제

스터디원(저 포함)은 다음주 스터디 주차 전까지 해당 과제를 해야합니다.

* while : while문을 이용한 AI(?) 가위바위보 게임 만들기

* for : A 모양 출력하기

* 그리고... + 추가 과제