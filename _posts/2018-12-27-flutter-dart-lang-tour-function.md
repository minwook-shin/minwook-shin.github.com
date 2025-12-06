---
layout: post
title: flutter에서 사용하는 dart 언어의 함수 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 함수에 대해서 작성합니다.

## 함수

함수도 function 타입의 객체이므로 함수를 변수에 할당할 수 있고, 인자로도 건내줄 수 있습니다.

```dart
void main() {
  void test() {
    print("hello, world!");
  }
  test();

  test2() {
    print("hello, world!");
  }
  test2();

  void test3() => print("hello, world!");
  test3();
```

기본적으로 함수는 위와 같은 형태로 작성되고, 타입은 생략되어 유추될 수 있습니다.

식이 한 줄이면 화살표로 축약할 수 있습니다.

```dart
  void test4(bool trig,bool option){
    if(trig){
      print(option);
    }
  }
  test4(true, false);
  
  void test5({bool trig, bool option}) {
    if(trig){
      print(option);
    }
  }
  test5(trig: true, option: false);
```

기본적으로 인자를 다 채워서 사용해야되지만, ```{}```로 감싸면 이름을 지정할 수 있습니다.

```dart
  String test6(bool trig, [String str]){
    if(trig){
      return str;
    }
    return "";
  }
  test6(false);
```

```[]```로 감싸면 인자를 옵션으로 사용할 수 있습니다.

```dart
  void test7({String str = "hello,world!"}){
    print(str);
  }
  test7();

  void test8({List<int> list = const [1,2,3]}){
    print(list);
  }
  test8();
```

```{}```로 감싸서 기본 인자 값을 상수로 지정할 수 있습니다.

그래서 const 키워드가 필요할 때가 있습니다.

```dart  
  void printArr(int element) {
    print(element);
  }
  var list = [1, 2, 3];
  list.forEach(printArr);
```

함수를 인자로 넣을 수 있습니다.

```dart
  var testList = ['1', '2', '3'];
  list.forEach((item) {
    print(testList[item-1]);
  });
```

익명 함수로 만들어서 사용할 수도 있습니다.

```dart
  Function adder(int n){
    return (int i)=> n + i;
  }
  var tmp = adder(5);
  print(tmp(5));
```

함수도 객체이기 때문에 함수를 반환하는 함수로 변수에 할당하여 사용할 수 있습니다.

```dart
  foo() {}
  print(foo() == null);
}
```

만약 함수의 반환값이 없다면 null로 출력됩니다.