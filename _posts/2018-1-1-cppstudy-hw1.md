---
layout: post
title: C++ 스터디 1주차 과제 수행
---

2018년 무술년입니다.
새해 福 많이 받으세요!

오늘은 우분투 한국 커뮤니티 c++ 기초 스터디의 2주차까지 해야 될 과제를 수행하면서 (간략히 정리하는 차원에서) 기록을 남겨보려합니다.

이 과제는 이번주 수요일까지 해야되는 과제입니다.

## 수행해야 될 과제

* 가위바위보 게임

* A 모양 출력

* 추가 과제

## 과제 코드 복습

* 가위바위보 게임

```c++
#include<iostream>
#include<time.h>
```
우선, iostream과 time.h를 가져옵니다.
```c++
using namespace std;
```
네임스페이스를 지정해줍니다.
```c++
struct selected
{
	int user;
	int bot;
};
```
구조체로 사용자와 봇 변수를 만들어줍니다.
```c++
enum rps
{
	r = 1,
	p,
	s
};
```
가위 바위 보를 각각 숫자로 할당해주는 단계를 열거체로 구현했습니다.
```c++
double random() {
	double percent = rand() % 100 + 1;
	return percent;
}
```
랜덤을 함수로 분할했습니다.

double형으로 리턴되는 함수입니다.
```c++
int core() {
	int select_key;
	selected game = {};
	cout << "Rock Paper Scissor!" << endl;
	cout << "Rock : 1 / Paper : 2 / Scissor : 3" << endl;
	cout << "your turn :";
	cin >> select_key;
	switch (select_key)
	{
	case(1):
		cout << "you selected \"rock\"" << endl;
		game.user = 1;
		cout << "--------\n| |  |  |\n| |  |  |\n| |  |  |\n--------" << endl;
		break;
	case(2):
		cout << "you selected \"paper\"" << endl;
		game.user = 2;
		cout << "--------\n|\t|\n|\t|\n|\t|\n--------" << endl;
		break;
	case(3):
		cout << "you selected \"scissor\"" << endl;
		cout << "\t   /\n\t  /\n    -------------\n\t /\n\t/" << endl;
		game.user = 3;
		break;
	default:
		exit(0);
	}
	int percent = random();
	if (percent < 33) {
		game.bot = 1;
		cout << "bot selected \"rock\"" << endl;
		cout << "--------\n| |  |  |\n| |  |  |\n| |  |  |\n--------" << endl;
	}
	else if (33 <= percent < 66) {
		game.bot = 2;
		cout << "bot selected \"paper\"" << endl;
		cout << "--------\n|\t|\n|\t|\n|\t|\n--------" << endl;
	}
	else if (66 <= percent <= 100) {
		game.bot = 3;
		cout << "bot selected \"scissor\"" << endl;
		cout << "\t   /\n\t  /\n    -------------\n\t /\n\t/" << endl;
	}
```
게임의 대부분을 작성한 함수로서, 사용자의 입력을 받으면 game.user에 값을 저장하고 봇은 랜덤 확률로 game.bot에 저장합니다.

```c++
	if (game.user == game.bot) {
		cout << "tie!" << endl;
	}
	else if (game.user == 1 && game.bot == 2) {
		cout << "you loose!" << endl;
	}
	else if (game.user == 1 && game.bot == 3) {
		cout << "you won!" << endl;
	}
	else if (game.user == 2 && game.bot == 1) {
		cout << "you won!" << endl;
	}
	else if (game.user == 2 && game.bot == 3) {
		cout << "you loose!" << endl;
	}
	else if (game.user == 3 && game.bot == 1) {
		cout << "you loose!" << endl;
	}
	else if (game.user == 3 && game.bot == 2) {
		cout << "you won!" << endl;
	}
}
```
game.user와 game.bot를 비교하여 최종적으로 누가 이겼는지 출력됩니다.



```c++
int main()
{
	srand((unsigned int)time(0));
	core();
	return 0;
}
```
메인 함수는 랜덤 시드를 생성하고, 코어 함수를 불러옵니다.

(참고로 비주얼 스튜디오로 안하면 난수때문에 ```stdlib.h```를 include 해야합니다)

* A 모양 출력

```c++
#include <iostream>
using namespace std;

int main()
{
	for (int i = 0; i < 6;i++ ) {
		for (int j = 0; j < 6 - i; j++) {
			cout << " ";
		}
		cout << "*";
```
우선 좌측 공백 부분부터 채우고, A자의 왼쪽 라인을 그려줍니다.
```c++
		for (int k = 0; k < i *2-1;k++) {
			if (i == 3) {
				cout << "*";
			}
			else {
				cout << " ";
			}
		}
```

그리고, A자의 중간 공백을 채우되 3번째 라인만 그려줍니다.
```c++
		if (i == 0) {
			cout << "";
		}
		else {
			cout << "*";
		}
		cout << endl;
	}
	return 0;
}
```
마지막으로 A자의 오른쪽 라인도 채워줍니다. 
(0번째는 좌우가 교차하는 지점이므로 중복되면 안되서 if문으로 처리합니다.)

* 추가 과제

(분량 문제로 추가 과제는 이글에 게시하지 않습니다.)