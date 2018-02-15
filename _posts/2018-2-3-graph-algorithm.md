---
layout: post
title: 그래프 알고리즘 알아보기
---

오늘은 그래프 알고리즘에 대한 개념을 간단히 알아보려고 합니다.

## 개념

그래프 G = (V, E), 즉 무방향성입니다.

V : 노드 자체이며, {1,2,3,4,5}로 표현됩니다.

E : 노드들을 연결하는 링크이며 {(1,2),(2,3),(3,4)}로 표현됩니다.

(노드는 정점이라고 불리며, 링크는 엣지라고도 불립니다.)

오브젝트들 간의 이진 관계를 표현합니다.

## 경로

무방향 그래프에서 G= (v,e)에서 노드 u와 노드 v를 연결하는 경로가 존재하면 서로 연결한 것입니다.

연결된 그래프들을 연결요소라고 하며,
모두 연결된 그래프틑 연결된 그래프라고 말합니다. 

## 방향 그래프

링크 (U,V)는 U로 부터 V로의 방향을 가집니다.

인접 행렬은 비대칭이며, 인접 리스트는 m개의 노드를 가지게 됩니다.

## 가중치 그래프

링크마다 가중치가 지정됩니다.

만약 인접 행렬로 표현한다면 링크의 존재를 나타내는 값을 1 대신 링크의 가중치를 저장하게 됩니다.

주로 가중치가 거리 혹은 비용에 대한 의미 있는 경우 링크가 없을 때는 무한을 쓰거나, 대각선은 0을 씁니다.

만약 용량을 의미한다면 반대로 쓰는게 자유롭다고 합니다.

## 인접 행렬

N*N 행렬로 표현

A,B 사이에 링크가 있으면 1, 없으면 0으로 표기한 대각선을 기준으로 대칭행렬을 인접 행렬이라고 합니다.

시간복잡도로 최악의 경우를 찾으면, 어떤 노드 V에 인접한 모든 노드를 찾으려면 O(N)의 시간이 걸리며, 어떤 링크(U,V)가 존재하는 지 찾으려면 O(1)시간이 걸립니다.

그리고 공간 복잡도는 O(N^2)입니다.

## 인접 리스트

노드 집합을 표현하는 하나의 배열이며, 각 노드마다 인접한 노드들의 연결리스트입니다.

N개의 연결리스트를 하나의 배열에 저장하는 그래프입니다.

하나의 링크에 서로 중복하여 표시되므로 인접 리스트의 노드 갯수는 2M개 입니다.

그러므로 공간 복잡도는 O(N+M)입니다.

그리고 어떤 노드 V에 인접한 모든 노드를 찾는 것은 O(degree(V))이며, 어떤 링크(U,V)가 존재하는지 찾는 것은 O(degree(u))입니다.

최악의 경우는 o(n-1)입니다.