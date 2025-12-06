---
layout: post
title: ncurses의 키보드 입력에 대하여 간단히 알아보기
---

오늘은 어제의 기본적인 내용에 이어서 ncurses에 키보드를 입력받는 것도 포스팅하려합니다.

## keypad()

입력 시 키보드 특수 키의 입력을 가능하게 설정하는 함수입니다.

방향키나 esc키가 해당됩니다.

처음에 WINDOW *win 인자가 들어가고 bool으로 사용 가능과 불가능을 조절할 수 있습니다.

## curs_set()

화면에 보이는 커서를 설정할 수 있는 함수입니다.

인자를 0로 하면 커서가 사라지고, 1은 일반적인 커서, 2는 큰 커서로 되게 됩니다.

## noehco()

문자를 입력할 때에 입력한 값을 화면에 출력하지 않는 함수입니다.

echo()와 대조됩니다.

## scanw()

데이터 입력을 받는 함수입니다.

c 스탠다드 라이브러리의 scanf()와 유사한 함수입니다.

## clear()

화면을 모두 빈칸으로 지워버립니다.

```c++
#include <ncurses.h>
int main()
{
    initscr();
    char userName[8];
    keypad(stdscr, TRUE);
    curs_set(0);
    noecho();
    scanw("%s", userName);
    printw("%s\n", userName);
    refresh();
    getch();

    clear();
    refresh();

    getch();
    endwin();
    return 0;
}
```