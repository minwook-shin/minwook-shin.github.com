---
layout: post
title: flutter에서 사용하는 dart 언어의 비동기 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 비동기에 대해서 작성합니다.

## 비동기

단일 쓰레드로 실행되기 때문에 특정 동작을 기다리면 멈출 수 있습니다.

그래서 async, await 혹은 Future API를 사용하여 비동기 작업으로 해야 합니다.

```dart
void main() {
  hello() => Future.delayed(Duration(seconds: 1),()=>"hello, world");
  
  Future func() async{
    var test = await hello();
    print(test);
  }
  func();
  print("delayed <hello,world>");
```

우선 처음으로 1초 동안 잠시 딜레이되고 문자열이 반환되는 hello라는 함수를 만듭니다.

그리고 await를 이용하여 hello 함수에서 반환되는 순간까지 기다리다가 출력하게 합니다.

그러므로 delayed hello,world라는 문자열이 우선 출력되고 func가 수행됩니다. 

```dart
  var fail = 0;
  error() => Future.delayed(Duration(seconds: 1),() => fail == null);

  Future handleError() async{
    var test = await error();
    try{
      assert(test);
    }
    catch(e){
      print(e);
    }
  }
  handleError();
  print("delayed <assert>");
```

try catch문으로 예외처리할 때에도 오류를 기다리다가 수행될 수 있습니다.

이 역시 delayed assert 문자열이 우선 출력됩니다.

```dart 
  Future<String> future() => Future.delayed(Duration(seconds: 1),()=>"future api");

  Future<void> printDelayed() {
    return future().then((str)=>print(str));
  }
  printDelayed();
  print("delayed <future api>");
}
```

async, await가 없을 때는 future api로 동일한 코드를 작성할 수 있습니다.

딜레이를 일으키는 future() 함수로 수행할 작업을 then 콜백으로 지정합니다.

결과적으로 printDelayed라는 함수는 future라는 함수가 수행되기 전까지 기다리다가 반환이 되는 순간에 문자열이 출력되게 됩니다.
