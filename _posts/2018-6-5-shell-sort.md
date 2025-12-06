---
layout: post
title: 셀 정렬에 대하여 알아보기
---

오늘은 셀 정렬에 대하여 알아보고자 합니다.

## 개요

일정한 간격으로 떨어져 있는 자료들을 집합을 구성하고, 각 집합에 있는 원소들에 대해 삽입 정렬를 수행하는 것을 반복하면서 전체 원소를 정렬하는 방식을 셀 정렬이라고 합니다.

전체 원소에 대하여 삽입 정렬를 시도하는 것보다 부분 집합으로 나누어 정렬하게 되면 비교와 교환 연산의 횟수가 감소합니다.

셀 정렬에서 집합을 만드는 기준인 간격은 매개변수 h에 저장되며, 한 단계가 수행될 때마다 h의 값이 감소되고, 셀 정렬을 재귀적으로 호출합니다.

이는 h가 1이 될 때가지 반복하며, 일반적으로 h의 값은 원소 갯수의 1/2을 사용하고 한단계 수행될 때마다 h의 값을 반으로 감소시키면서 반복 수행합니다.

셀 정렬의 성능은 매개변수 h의 값에 따라 달라집니다.

## 알고리즘

```java
intervalSort(arr[],begin,end,interval){
    for(i<-begin+interval;i<=end;i<-i+interval)do{
        item <- arr[i];
        for(j<-i-interval;j>=begina and item<arr[j];j<- j-interval){
            arr[j+interval] = aarr[j];
        }
        arr[j+interval] <- item;
    }
}

sellSort(arr[],n){
    interval <- n;
    while(interval >= 1) do{
        interval <- interval/2;
        for(i<-0;i<interval;i<-i+1)do{
            intervalSort(arr[],i,n,interval);
        }
    }
}
```

셀 정렬은 삽입정렬의 시간 복잡도인 O(n^2)보다 개선된 O(n^1.25) 로 측정된다고 합니다.