---
layout: post
title: flutter에서 사용하는 dart 언어의 제네릭 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 제네릭에 대해서 작성합니다.

## 제네릭

```<``` ```>```으로 타입을 지정할 수 있습니다.

```dart
abstract class StringTest{
  String getTest(String i);
  void setTest(String i);
}
abstract class Test<T>{
  T getTest(T i);
  void setTest(T i);
}
```

기본적으로 타입마다 지정해야 될 수 있었던 상황을 제네릭으로 코드의 중복을 막을 수 있습니다.

```dart
void main() {
  var list1 = <int>[1,2,3];
  var list2 = List<int>();
  var map1 = <int,int>{
    1 : 1,
    2 : 2,
    3 : 3
  };
  var map2 = Map<int,int>();
  print(list1.toString()+list2.toString()+map1.toString()+map2.toString());
```

리스트나 맵처럼 자료구조에서 제네릭을 사용하여 타입을 지정합니다.

리터럴 방식이나 생성자 방식 모두 같습니다.

```dart
class superClass{}
class sub1 <T extends superClass>{
  toString() => "$T";
}
```

```dart
  print(sub1<superClass>());
```

인자의 타입을 제네릭으로 제한할 수 있습니다.

```dart
  T functionTest<T>(List<T> i){
    T tmp = i[0];
    return tmp;
  }
  var f = functionTest<int>(list1);
  print(f);
}
```

제네릭 메소드를 사용하여 메소드와 함수에서 제네릭을 사용할 수 있습니다.