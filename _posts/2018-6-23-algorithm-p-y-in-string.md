---
layout: post
title: 프로그래머스 문자열 내 p와 y의 개수 알고리즘 문제 풀기
---

오늘도 프로그래머스에서 "문자열 내 p와 y의 개수" 라는 알고리즘 문제를 풀어보았습니다.

## 문제 설명

대문자와 소문자가 섞여있는 문자열 s가 주어집니다. s에 'p'의 개수와 'y'의 개수를 비교해 같으면 True, 다르면 False를 return 하는 solution를 완성하세요. 

'p', 'y' 모두 하나도 없는 경우는 항상 True를 리턴합니다. 단, 개수를 비교할 때 대문자와 소문자는 구별하지 않습니다.

예를들어 s가 pPoooyY면 true를 return하고 Pyy라면 false를 return합니다.

# 제한사항

* 문자열 s의 길이 : 50 이하의 자연수

* 문자열 s는 알파벳으로만 이루어져 있습니다.

## 입출력 예

* 'p'의 개수 3개, 'y'의 개수 3개로 같으므로 true를 return 합니다.

* 'p'의 개수 1개, 'y'의 개수 2개로 다르므로 false를 return 합니다.

## 코드

```c++
#include <string>
#include <iostream>
using namespace std;

bool solution(string s)
{
    int isP = 0;
    int isY = 0;
    bool answer = true;
    
    for(int i = 0;i<s.length();i++)
    {
        if(s[i]=='y'||s[i]=='Y')
        {
            isY++;
        }
        if(s[i]=='p'||s[i]=='P')
        {
            isP++;
        }
    }
    if(isP == isY)
    {
        return true;
    }

    return false;
}
```

p의 갯수를 대소문자 다 합쳐서 세고, y의 갯수를 대소문자 다 합쳐서 센 다음에 이 두 수가 같으면 참을 반환하고, 다르면 거짓을 반환합니다.