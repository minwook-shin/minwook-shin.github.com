---
layout: post
title: C++ 스터디 2주차 과제 수행
---

오늘도 우분투 한국 커뮤니티 c++ 기초 스터디의 3주차까지 해야 될 과제를 수행하면서 (간략히 정리하는 차원에서) 기록을 남겨보려합니다.

이 과제는 돌아오는 주의 수요일까지 해야되는 과제입니다.

## 수행할 과제

* 배열
1. 1차원 정수 배열의 합과 평균을 찾는 프로그램
1. 정수 1차원 배열의 첫 번째 요소와 마지막 요소를 바꾸는 프로그램
1. 배열의 가장 큰 요소와 가장 작은 요소를 찾는 프로그램
* 함수
1. 두 개의 정수를 인수로 받아 그 합을 반환하는 함수를 작성
1. 정수를 인수로 받아 팩토리얼 값을 계산하는 함수를 작성
1. 두 숫자를 인수로 받고 이 두 숫자 사이의 모든 소수를 표시하는 함수를 작성
1. 두 개의 정수 인수가 매개변수로 전달되는 zero_small() 함수를 작성하고 두 숫자 중 작은 숫자를 0으로 설정하는 함수를 작성

## 과제 코드 복습

* 1차원 정수 배열의 합과 평균을 찾는 프로그램을 작성하십시오.

```c++
#include<iostream>
#define len 10
using namespace std;
```

우선 선언할 내용을 먼저 적어줍니다.

```c++
int main() {
	int *iarry = new int[len];
```

동적할당으로 정수형 배열을 만듭니다.

```c++

	for (int i = 0; i < len; i++) {
		iarry[i] = i;
	}
```

내용을 집어넣습니다.

```c++
	int sum = 0;
	for (int k = 0; k < len; k++) {
		sum += iarry[k];
	}
	cout << sum << endl;
	cout << sum/len << endl;
```

배열의 모든 수를 합하고, 나눠서 평균을 냅니다.

```c++
	delete[] iarry;
	return 0;
}
```

마무리로 반드시 동적 변수를 메모리에서 해제합니다.

* 정수 1차원 배열의 첫 번째 요소와 마지막 요소를 바꾸는 프로그램을 작성하십시오.

```c++
#include<iostream>
#define len 10

using namespace std;
```

선언할 내용을 선언해줍니다.

```c++
int main() {
	int *iarray = new int[len];
```

동적 변수를 할당해줍니다.

```c++
	for (int i = 0; i < len;i++) {
		iarray[i] = i;
	}
```

0부터 전처리기로 선언한 길이까지 넣어줍니다.

```c++
	int temp = 0;
	temp = iarray[0];
	iarray[0] = iarray[len - 1];
	iarray[len - 1] = temp;
```

두 수를 바꿔줍니다.

```c++
	for (int j = 0; j < len;j++) {
		cout << iarray[j] << endl;
	}
```

출력합니다.

```c++
	delete[] iarray;
	return 0;
}
```

반드시 메모리에서 해제해줍니다.
* 배열의 가장 큰 요소와 가장 작은 요소를 찾는 프로그램을 작성하십시오.

```c++
#include<iostream>
#include<algorithm>
#define len 10

using namespace std;
```

선언할 내용을 적습니다.

```c++
int main() {
	int iarray[len] = { 1,6,4,2,7,9,3,5,8,0 };
	sort(iarray, iarray + len);
```

정수형 배열을 생성하고 임의의 값으로 초기화합니다.
그리고, 알고리즘 헤더로 정렬합니다.

처음 값과 마지막 값을 적습니다.

```c++
	cout << iarray[0] << '\t' << iarray[len-1] << endl;
	return 0;
}
```

작은 값과 큰 값을 출력합니다.

* 두 개의 정수를 인수로 받아 그 합을 반환하는 함수를 사용하여 프로그램을 작성하십시오. main()에서 이 함수를 호출하고 결과를 main()에서 출력하십시오.

```c++
#include<iostream>
using namespace std;
```

헤더와 네임스페이스를 미리 선언합니다.

```c++
int sum(int a, int b);
```

sum함수를 미리 선언해둡니다.

```c++
int main() {
	cout << sum(1, 6) << endl;

	return 0;
}
```

메인에서 1과6을 sum함수로 건내줍니다.

```c++
int sum(int a, int b) {
	int sum = a + b;
	return sum;
}
```

합산합니다.
* 정수를 인수로 받아 팩토리얼 값을 계산하는 함수를 작성하십시오. main()에서 이 함수를 호출하고 결과를 main()에서 출력하십시오.

```c++
#include<iostream>
using namespace std;
```

헤더와 네임스페이스를 선언합니다.

```c++
int prime(int n);
```

함수를 미리 선언합니다.

```c++
int main() {
	int start_num, end_num;
	cout << "enter start number : ";
	cin >> start_num;
	cout << "enter end number : ";
	cin >> end_num;
	for (start_num; start_num < end_num; start_num++) {
		if (prime(start_num) == 1) {
			cout << start_num << endl;
		}
	}
	return 0;
}
```

메인에서 시작 값과 끝 값을 받아서 시작부터 끝까지의 수를 for로 반복하며 소수를 찾는 함수로 건내줍니다.

건내준 값은 1과 0으로 다시 돌아오는데, 1로 오는 값들만 출력합니다. 

```c++
int prime(int n) {
	if (n <= 1) {
		return 0;
	}
	else {
		for (int i=2; i <= n / 2; i++) {
			if (n % i == 0) {
				return 0;
			}
		}
		return 1;
	}
}
```

1이하이면 자동으로 소수가 아니므로 넘어가고, 2부터 구해야 되는 수의 절반까지 for문으로 반복하여 나머지 값이 존재하면 소수로 판정합니다.
* 두 개의 정수 인수가 매개변수로 전달되는 zero_small() 함수를 작성하고 두 숫자 중 작은 숫자를 0으로 설정하십시오. main()에서 이 함수를 호출하고 결과를 main()에서 출력하십시오.

```c++
#include<iostream>
using namespace std;
```

헤더와 네임스페이스 선언합니다.

```c++
void zero_small(int &a, int &b);
```

함수를 미리 선언합니다.

```c++
int main() {
	int a = 0;
	int b = 0;
```

메인에서 a와 b를 초기화합니다.

```c++
	cin >> a;
	cin >> b;
```

입력을 받습니다.

```c++
	zero_small(a, b);
	cout << a << b << endl;
	return 0;
}
```

함수에 a와 b를 건내주고 출력합니다.

```c++
void zero_small(int &a,int &b) {
	if (a > b) {
		b = 0;
	}
	else {
		a = 0;
	}
}
```

a와 b의 주소값을 받아서 작은 수만 0으로 처리합니다.

주소값으로 가져오지 않으면, main에 있는 지역 변수에 영향을 주지 못합니다.