---
layout: post
title: Stack을 사용한 수식의 후위 표기법 계산
---

어제에 이어서 오늘은 스택을 이용하여 후위 표기법으로 이루어진 수식을 연산해보려고 합니다.

컴퓨터에서는 후위 표기법 수식을 스택으로 계산할 수 있습니다.

## 방식

1. 피연산자를 만나면 스택에 push하여 넣습니다.

1. 연산자를 만나면 필요한 정도만 피연산자를 스택에서 pop하여 꺼냅니다.

1. 꺼내진 피연산자와 연산자로 계산합니다.

1. 결과를 다시 스택에 넣습니다.

1. 모든 수식이 탐색되면 스택을 pop하여 최종 결과를 꺼냅니다.

## 알고리즘

```java
func(){
    while(true) do{
        tmp <- get(exp);
        case {
            tmp = operand :
                push(tmp);
            tmp = operator :
                o1 <- pop();
                o2 <- pop();
                result <- o1 tmp o2;
                push(result);
            tmp = null:
                print();
        }
    }
}
```