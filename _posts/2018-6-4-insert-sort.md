---
layout: post
title: 삽입 정렬에 대하여 알아보기
---

오늘은 삽입 정렬에 대하여 알아보려고 합니다.

## 개요

정렬되어 있는 집합에 정렬할 새로운 원소들을 알맞은 위치를 찾아 삽입하는 방식을 삽입 정렬이라고 합니다.

기본적으로 정렬할 자료가 두개의 정렬된 집합과 정렬되지 않은 집합이 있다고 가정합니다.

앞부분 원소부터 정렬을 수행하면서 정렬된 앞부분의 원소가 정렬된 집합으로 되고, 아직 정렬되지 않은 나머지 원소들의 집합을 정렬되지 않은 부분 집합으로 됩니다.

정렬되지 않은 집합의 원소를 하나씩 꺼내어 이미 정렬된 집합의 마지막 원소부터 비교하며 위치를 찾아 하나씩 삽입합니다.

정렬되지않은 집합의 공간이 공집합이 되면 삽입 정렬이 완성됩니다.

## 알고리즘

```java
insert(arrr[],n){
    for(i<-1;i<n;i<-i+1) do{
        tmp <- arr[i];
        j <- i;
        if(arr[j-1]>tmp) then{
            move <- true;
        }else{
            move <- false;
        }
        while(move){
            arr[j] <- arr[j-1];
            j <- j-1;
            if(j>0 and arr[j-1]>tmp)then{
                move <- true;
            }else{
                move <- false;
            }
        }
        arr[j] <- tmp;
    }
}
```

n개의 자료를 정렬할 때에는 n개의 메모리 공간이 사용되며, 최소의 전체 비교 횟수는 O(n)되지만, 모든 원소가 역순으로 되어 있다면, O(n^2)가 됩니다.

평균 비교횟수는 n(n-1)/4 가 되어서 결국 평균 복잡도는 O(n^2)이 됩니다.