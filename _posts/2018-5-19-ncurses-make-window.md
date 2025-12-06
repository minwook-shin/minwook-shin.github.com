---
layout: post
title: ncurses 새로운 윈도우 만들기
---

오늘은 ncurses로 새로운 윈도우를 생성하고 생성한 윈도우를 꾸미고 윈도우를 소멸시키는 방식에 대하여 포스팅해보려 합니다.

## WINDOW*

특정 윈도우를 가리키는 포인터 데이터 타입입니다.

stdscr는 기본 윈도우의 포인터입니다.

## newwin()

새로운 윈도우를 생성하는 함수입니다.

새 윈도우 사이즈와 위치를 인자로 넣어주어야 하며, 생성시 메모리가 할당됩니다.

인자는 차래대로 행 크기, 열 크기, 시작 x좌표, 시작 y좌표 순서로 넣습니다.

```c++
win1 = newwin(10, 10, 1, 1);
```

10*10 크기의 윈도우가 1,1 좌표에서 출력됩니다.

## wbkgd()

한 속성으로 특정 윈도우에게 적용하게 하는 함수입니다.

윈도우 객체와 COLOR_PAIR()쌍이 인자로 들어가게 됩니다.

## wattron()

특정 윈도우에 적용할 속성을 적용하게 하는 함수힙니다.

이 역시 윈도우 객체와 COLOR_PAIR()쌍이 인자로 들어가게 됩니다.

## mvwprintw()

특정 윈도우의 특정 위치에서부터 문자열을 출력합니다.

저번 포스팅에서 언급한대로 화면을 새로고치기전에는 화면에 나오지는 않습니다.

## wborder()

특정 윈도우의 경계를 설정해주는 함수로서 box()와 유사하게 작동합니다.

다만 이 함수는 왼쪽, 오른쪽, 위쪽, 아래쪽 모양을 인자로 주어 다양하게 꾸밀 수 있습니다.

```c++
wborder(window, '*','*','*','*','*','*','*','*');
```

## wrefresh()

특정 윈도우를 새로고치는 함수입니다.

인자로는 새로고칠 화면의 윈도우 객체를 넣어줍니다.

## delwin()

윈도우가 필요없어졌을 떄에 할당된 메모리를 해제합니다.

인자로 삭제할 윈도우 객체를 넣습니다.

## 예제

```c++
#include <ncurses.h>
int main()
{
    WINDOW *win;
    initscr();
    start_color();
    init_pair(1, COLOR_WHITE, COLOR_BLACK);
    refresh();
    win = newwin(10, 10, 1, 1);
    wbkgd(win, COLOR_PAIR(1));
    wattron(win, COLOR_PAIR(1));
    mvwprintw(win, 1, 1, "A new window");
    wborder(win, '*','*','*','*','*','*','*','*');
    wrefresh(win);
    getch();
    delwin(win);
    endwin();
}
ret
```