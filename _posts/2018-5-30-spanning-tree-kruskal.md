---
layout: post
title: 최소 비용 신장 트리 Kruskal 알고리즘에 대하여 알아보기
---

오늘은 최소 비용 신장 트리 Kruskal 알고리즘에 대하여 알아보겠습니다.

## 개요

Kruskal 알고리즘은 가중치가 높은 간선을 제거하며 최소 비용 신장 트리, 즉 minimum cost spanning tree를 만들거나,
가중치가 낮은 간선을 삽입하면서 minimum cost spanning tree를 만듭니다.

## 첫번째 알고리즘

우선 높은 간선을 제거하면서 만드는 방법을 알아보겠습니다.

1. 그래프의 모든 간선을 가중치에 따라 내림차순으로 정렬합니다.

1. 그래프에서 가중치가 가장 높은 간선을 제거합니다.

1. 만약 가중치가 가장 높은 간선이 끊기면 그래프가 다중으로 분리되는 간선이면 제거하지말고 다음 가중치가 높은 것을 제거합니다.

1. 그래프에서 N-1개의 간선만 남을때까지 2번을 반복합니다.

1. 그래프에서 N-1개의 간선만 남게되면 이게 최소 비용 신장 트리입니다.

만약 정점이 7개면, 이 최소 비용 신장 트리는 6개의 간선을 가지게 됩니다.

## 두번째 알고리즘

두번째로 낮은 간선을 제거하면서 만드는 방법도 알아보겠습니다.

1. 그래프의 모든 간선을 가중치에 따라 오름차순으로 정렬합니다.

1. 그래프에 가중치가 가장 작은 간선을 삽입합니다.

1. 사이클이 형성되는 간선이라면 삽입할 수 없으므로 그 다음으로 작은 가중치를 가진 간선을 삽입합니다.

1. 그래프에 N-1개의 간선이 삽입될 때까지 2번째를 반복합니다.

1. 그래프의 간선이 N-1개가 되면 최소 비용 신장 트리가 됩니다. 