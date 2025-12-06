---
layout: post
title: Java 후위 표기법 실습하기
---

후위 표기법(postfix notation)은 연산자를 연산 대상의 뒤에 쓰는 연산 표기법으로서, 스택으로 구현가능합니다.

사람이 식을 보면 이해하기 어려워서 컴퓨터에게 계산을 시킬 때 주로 사용된다고 합니다.

## 방법

1. 피연산자가 보이면 스택에 삽입(push)합니다.

1. 연산자가 보이면 필요한 피연산잘를 스택에서 꺼내(pop) 연산하고, 연산 결과를 다시 스택에 삽입(push)합니다.

1. 수식이 계산되면, 마지막으로 pop하여 출력합니다.

알고리즘 가상 코드는 아래와 같습니다.

## 가상 코드

```java
func(exp)
 while (true) do {
     text <- get_text(exp);
     case {
        text = num :
            push(text);
        text = opera :
            opr2 <- pop();
            opr1 <- pop();
            result <- opr1 op(text) opr2;
            push(result);
        text = null:
            print(pop(stack));
     }
 }
end func
```

계속 반복하는 코드로서, 피연산자와 연산자를 구별하며 push와 pop을 수행합니다.

마지막에는 결국 결과가 나오고, 결과가 pop되면 스택에는 결과값만 남으므로 pop하여 출력하게 됩니다.