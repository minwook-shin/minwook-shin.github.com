---
layout: post
title: 트리 정렬에 대하여 알아보기
---

오늘은 트리 정렬에 대하여 알아보고자 합니다.

## 개요

이진 탐색 트리를 이용하여 정렬하는 방법을 트리 정렬이라고 합니다.

정렬할 원소들을 이진 탐색 트리로 구성하고 중위 순회 방법을 사용하여 이진 탐색 트리의 원소들을 재귀적으로 꺼내어 오른차순으로 정렬합니다.

정렬되지 않은 자료들을 트리 정렬로 정렬하는 방식은 아래와 같습니다.

## 단계

1. 정렬할 원소들을 차례대로 트리에 삽입하여 이진 탐색 트리를 구성합니다.

1. 이진 탐색 트리를 중위 순회 방법으로 재귀적으로 원소를 저장합니다.

## 알고리즘

```java
insert(bst, x){
    p<-bst;
    while(p!=null) do{
        if(x=p.key) then{
            return;
        }
        q <- p;
        if(x<p.key) then{
            p<-p.left;
        }else{
            p<-p.right;
        }
    }
    n <- getNode();
    n.key <- x;
    n.left <- null;
    n.right <- null;
    if(bst <- null) then{
        bst <- n;
    }else if(x<q.key) then{
        q.left <- n;
    }else{
        q.right <- n;
    }
    return;
}

inorder(t){
    if(t != null) then{
        inorder(t.left);
        print(t.data);
        inorder(t.right);
    }
}

tree(arr[],n){
    for(i<-0;i<n;i<-i+1) do{
        insert(bst,arr[i]);
    }
    inorder(bst);
}
```

트리 정렬에서 원소들을 비교하여 이진 탐색 트리를 구성하는 시간이 O(log2n)이지만, n개의 노드에 대해서는 O(nLog2n)이 됩니다.