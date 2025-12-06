---
layout: post
title: ncurses Attribute 설정해보기
---

오늘은 ncurses에서 속성을 제어하는 방식을 매우 간단하게 포스팅하려합니다.

이전 포스팅과 다소 겹치는 부분이 있을 수도 있습니다. 양해 부탁드립니다.

## start_color()

전에 언급한대로 색 attribute를 사용하기 위해 선언하는 함수입니다.

터미널에 색을 입히기 위해서는 반드시 선언되어야 합니다.

## init_pair()

글자 색과 배경 색을 한 쌍으로 색 속성을 지정합니다.

```c++
init_pair(1,COLOR_WHITE,COLOR_BLACK);
```

1번 팔레트에 하얀색 글씨와 검은 배경으로 설정합니다.

## bkgd()

한 Attribute로 윈도우의 전체에 적용하게 되는 함수입니다.

COLOR_PAIR()로 위에서 설정한 팔레트의 색을 가져다가 씁니다.

```c++
bkgd(COLOR_PAIR(1));
```

1번 팔레트에서 속성을 가져와서 적용하는 예시입니다.

## attron()

적용할 Attribute를 설정하는 함수입니다.

bkgd()처럼 사용합니다.

```c++
attron(COLOR_PAIR(1));
```

1번 팔레트를 사용합니다.

## attroff()

적용한 Attribute를 해제하는 함수힙니다.

attron() 함수와 대조되며, 팔레트를 헤제하는데에 사용됩니다.

```c++
attroff(COLOR_PAIR(1))
```

1번 팔레트를 사용 해제하는 예시입니다.

## border()

윈도우의 경계를 설정하며, 왼쪽, 오른쪽, 위쪽, 아랫쪽, 그리고 각 코너를 각각 설정 할 수 있습니다.

```c++
border('@', '@', '@', '@', '@', '@', '@', '@');
```

각 인자 별로 왼쪽 측면, 오른쪽 측면, 위쪽 측면, 아랫쪽 측면, 왼쪽 윗 코너, 오른쪽 위쪽 코너, 왼쪽 아랫쪽 코너, 오른쪽 위쪽 코너 순으로 넣으면 됩니다.

비슷한 함수인 box()도 테두리를 설정할 수 있습니다.

## 예제 코드

```c++
#include <ncurses.h>
int main()
{
    initscr();
    start_color();
    init_pair(1,COLOR_WHITE,COLOR_BLACK);
    bkgd(COLOR_PAIR(1));
    attron(COLOR_PAIR(1));
    mvprintw(1, 1, "hello world!");
    attroff(COLOR_PAIR(1));
    border('*', '*', '*', '*', '*', '*', '*', '*');
    refresh();
    getch();
    endwin();

    return 0;
}
```

위 간단 포스팅을 바탕으로 작성된 예제 코드입니다.

1번 팔레트로 속성을 설정하고 문자열을 출력하며 테두리를 지정함을 볼 수 있습니다.