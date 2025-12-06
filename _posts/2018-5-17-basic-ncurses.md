---
layout: post
title: ncurses를 간단히 알아보기
---

오늘은 터미널 그래픽 라이브러리인 NCURSES를 알아보도록 하겠습니다.

## 개요

위키백과에 따르면 ncurses는 프로그래머가 TUI를 터미널 독립 방식으로 기록할 수 있도록 API를 제공하는 라이브러리입니다. 

# 장점

커서를 이동할 수 있으며, 키보드, 마우스로 쉽게 제어가 가능하며, 사용자가 보기 편리하도록 창의 크기나 색을 제어할 수 있습니다.

# 유래 

본래 ncurses는 Cursor optimization에서 유래하며 유닉스 계열 운영체제를 위한 제어 라이브러리 중 하나였으나 1990년대 중반에 curses라이브러리의 개발이 중단된 후 GNU에서 개발했습니다.

# 개념

* 터미널 : 콘솔 실행 화면

* 스크린 : 프로그램 내에서 보여질 화면

* 윈도우 : ncurses 모드 후 스크린에 생성되어 출력될 화면

윈도우는 기본 윈도우와 뉴 윈도우가 있으며, New window는 사용자가 따로 생성해주어야 합니다.

## initscr()

ncurses TUI 모드를 사용하기 시작했다고 알려줍니다.
가장 먼저 호출되어야 하며, 기본 크기의 윈도우를 생성합니다.

## start_color()

ncurses에 색 attribute를 사용한다고 선언합니다.

색을 사용하기 전에 반드시 선언해야 합니다.

## init_pair()

한 쌍의 글자 색과 배경 색 attribute를 지정합니다.

## attron()

적용할 Attribute를 설정합니다.

## printw()

const char 형식의 문자열을 출력합니다.
그러나 아직 우리의 눈에는 보이지 않습니다.

## refresh()

이 함수가 호출되기 전까지 수행했던 작업들을 스크린에 업데이트하게 되며, printw()와 같은 함수들은 이 함수로 인하여 실제로 출력됩니다.


## getch()

문자를 입력했는지 검사합니다.

보통 실행파일이 바로 종료되지 않도록 하기위해서 사용됩니다.

## endwin()

ncurses TUI 모드를 사용 종료합니다.

```c++
#include <ncurses.h>

int main()
{
    initscr();
    start_color();
    init_pair(1, COLOR_RED,COLOR_WHITE);
    attron(COLOR_PAIR(1));
    printw("Hello world");
    attroff(COLOR_PAIR(1));
    refresh();
    getch();
    endwin();
    return 0;
}
```