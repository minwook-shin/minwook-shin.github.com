---
layout: post
title: ncurses를 ubuntu에 설치하기
---

오늘은 우분투에서 터미널 그래픽 라이브러리인 NCURSES를 설치하고 테스트하는 방법을 포스팅하려 합니다.

## 설치

```bash
sudo apt-get update
sudo apt-get install libncurses5-dev libncursesw5-dev
```

저장소를 업데이트하고, libncurses5-dev와 libncursesw5-dev 패키지를 내려받아 설치합니다.

## 예쩨 코드

```c++

#include <ncurses.h>

using namespace std;

int main()
{
    initscr(); 
    printw("Hello World"); 
    refresh();                 
    getch();                  
    endwin();
    return 0;
}

```

ncurses.h 헤더를 include하고, 메인을 작성합니다.

initscr로 화면을 띄울 준비하고, printw으로 기록합니다.

기록한 문자열은 바로 출력되는게 아니고, refresh로 화면을 새로 고쳐야지만 문자열이 출력됩니다.

출력된 화면을 그대로 유지하기위하여 getch로 입력 받는 상태로 대기합니다.

endwin로 마무리 짓습니다.

## 컴파일

```bash
g++ test.cpp -lncurses
```

전처리 과정에서 헤더파일을 검색하는 -l 옵션으로 ncurses 라이브러리를 사용한다고 선언합니다.

## 출력

윈도우와 색상을 지정하지 않아서 평범하게 Hello World가 출력됩니다.