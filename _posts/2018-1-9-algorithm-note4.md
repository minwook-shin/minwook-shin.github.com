---
layout: post
title: 알고리즘 공부-4 (링크드리스트)
---

알고리즘 공부의 연장선으로 링크드리스트(Linked list)에 대한 내용을 공부해보았습니다.

아래는 오늘 공부한 내용을 요약한 노트입니다.

즉석에서 필기한 내용이라 잘 정리가 안된 포스팅입니다. 양해부탁드립니다.

## 개요

* 구성 요소 : 노드(데이터), 링크(포인트)

```c++
struct node
{
    t data;
    node *next;
}
```

* 특징 : 동적 자료구조로서 불연속적으로 메모리를 사용하며 순차적 접근만 가능합니다.

동적 자료구조 : 필요할때 할당하고 필요없으면 해제하며, 크기 지정이 필요없습니다.

불연속 메모리 : 물리적 순서없고 불연속으로 저장하며, 링크에 의해 순서가 유지됩니다.

순차적 접근 : 임의적으로 접근이 불가능합니다. 반드시 iterator를 이용합니다.

## 클래스 템플릿

이용하는 이유 : 링크드리스트는 다른 데이터타입을 수용하는 컨테이너 역활이므로,
타입에 따라 재정의하지 않고 사용할 수 있어야하기 때문입니다.

```c++
template <class T> class node
{
    private:
        T data;
        node *next;
    public:
        T& getdata();
};

template <class T> T& node<T>::getdata()
{
    return data;
}
```

위와 같이 정의해주고, 아래처럼 코딩하면 int로 재정의하지 않아도 사용할 수 있습니다.

```c++
node<int> intnode;
intnode.getdata();
```

## 예외 처리

exception을 throw하면 try절에서 catch합니다.

리턴하지 못하는 상황에서 쓰입니다.

## 싱글 링크드리스트

* 이해 : 단순한 단방향 링크드리스트로서, 데이터와 다음을 가리키는 포인터로 구성되있습니다.

초기화와 제거, 다음 데이터 삽입/ 삭제 그리고 iterator로 이루어져 있습니다.

* 구현

```c++
typedef void *point;
template <class T> class List
{
    public:
        enum_except{
            invalid_point,
            empty_point
        }
    protected:
        struct node{
            T data;
            node *next;
        };
        node *head;
        node *tail;
        int count;
}
```

초기화 : 머리와 꼬리를 새로 만들어서 모두 꼬리를 가리키게 합니다.

```c++
head = new node;
tail = new node;

head->next = tail;
tail->next = tail;

count = 0
```

삽입 : 새로 노드를 만들어 데이터를 넣고, 그 노드를 노드의 다음 순서를 가리키게 합니다.

```c++
node *node = (node*) point;

node *newnode = new node;
newnode->data = new_data; // new_data는 인자로 받음
newnode->next = node->next;
node->next = newnode;

count++;

return newnode;
```

삭제 : 삭제한 노드를 delnode로 정의하고 이를 가리키는 것을 다다음 것으로 넘겨버립니다. 그리고 삭제합니다.

```c++
node *node = (node*) point;

node *delnode = node->next;
node->next = node->next->next;

TYPE rt = delnode->data;
delete delnode;

count--;

return rt;
```

## 더블 링크드리스트

* 정의 : 양방향이 연결된 리스트입니다.

데이터와 다음 포인터, 이전 포인터로 구성되어 있습니다.

초기화와 삭제, 데이터 다음/이전 삽입, 삭제 그리고 iterator로 구성되어있습니다.

싱글 링크드리스트와 눈에 띄게 다른 점은 이전/다음이 가능하다는 것입니다.

* 구현

```c++
typedef void *point;
template <class T> class List
{
    public:
        enum_except{
            invalid_point,
            empty_point
        }
    protected:
        struct node{
            T data;
            node *next;
            node *prev;
        };
        node *head;
        node *tail;
        int count;
}
```

초기화 : 양방향이기때문에 서로를 기리킵니다.

```c++
head = new node;
tail = new node;

head->next = tail;
head->next = head;
tail->next = tail;
tail->next = head;

count = 0
```

삽입, 삭제 : 이전에 기록해둔 단일 링크드리스트와 거의 흡사하나, 양방향이 서로 연결되는 점이 다릅니다.