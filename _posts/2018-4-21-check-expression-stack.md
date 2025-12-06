---
layout: post
title: Stack을 사용한 수식의 괄호 검사
---

오늘은 스택을 사용하여 수식의 괄호를 검사해보겠습니다.

## 개요

수식은 일반적으로 연산자와 피연자로 나누어져 있으며, 왼쪽에서 오른쪽 순서대호 처리합니다.

수식에 사용되는 연산자의 우선 순위가 다를 경우 괄호를 사용하여 우선순위를 표현합니다.

이 때 괄호는 일반 괄호, 중괄호, 대괄호가 쓰입니다.

괄호는 왼쪽 괄호와 오른쪽 괄호가 한 쌍을 이루면서 여러개가 중첩되면 가장 안쪽의 괄호부터 처리하게 됩니다.

이를 나중에 들어가면 먼저 나오는 스택의 특성을 이용하여 문제를 풀어봅니다.

수식을 읽으면서 왼쪽 괄호를 만나면 스택에 push하여 넣어주고, 오른쪽 괄호를 만나면 스택을 pop하여 꺼냅니다. 

현재 잡혀있는 오른쪽 괄호와 pop하여 꺼낸 왼쪽 괄호가 같은 종류이라면 한 쌍으로 성립되며 수식이 다 종료되었을 때 스택에 공백 상태가 되면 괄호의 갯수가 맞아서 검사를 마무리하게 됩니다.

## 알고리즘

```java
func()
    exp <- Expression;
    stack <- null;
    while(true) do{
        char <- getc(exp);
            case {
                char = "(" or "[" or "{" :
                    push(symbol);
                char = ")":
                    tmp <- pop();;
                    if(tmp != "(") then return false;
                char = "[":
                    tmp <- pop();;
                    if(tmp != "]") then return false;
                char = "{":
                    tmp <- pop();;
                    if(tmp != "}") then return false;
                char = "null":
                    if(isempty()) then return true;
            }
    }
end()
```