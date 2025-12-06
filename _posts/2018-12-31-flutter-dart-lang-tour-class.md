---
layout: post
title: flutter에서 사용하는 dart 언어의 클래스 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 클래스에 대해서 작성합니다.

## 클래스

모든 객체는 클래스의 인스턴스이며, 모든 클래스는 오브젝트에서 파생됩니다. 

```dart
class test{
  var a;
  var b;
  var c;
  var d;
  static const e = "";
  
  test(a,b,c){
    this.a = a;
    this.b = b;
    this.c = c;
  }
  
  printValue(){
    print(this.a);
    print(this.b);
    print(this.c);
  }
  
  adder(test t){
    return t.a + t.b + t.c;
  }
  
  test.named(){
    print("identifier");
  }
  
  test.another(a,b,c):this(a,b,c);
}

void main() {
  var tmp = test(1,2,3);
  tmp.printValue();

  tmp.a = 5;
  tmp.printValue();
  
  tmp?.d = 1;
```

객체를 생성해서 변수 필드에 접근하면 값을 대입할 수 있으며, 물음표를 이용하여 not-null이면 값을 넣게 할 수도 있습니다.

```dart  
  print(tmp.adder(tmp));
```

클래스로 객체를 생성해서 메소드를 사용할 수 있습니다.

```dart  
  test.named();
```

생성자에 이름을 부여해서 식별할 수 있습니다.

```dart  
  var tmp5 = test.another(10,10,10);
  print(tmp5.a);
}
```

자기 클래스의 생성자로 넘겨줄 수 있습니다.

```dart
class ImmutableTest {
  final num a, b, c;
  const ImmutableTest(this.a, this.b,this.c);
}
```

```dart  
  var tmp1 = const ImmutableTest(5,5,5);
  var tmp2 = const ImmutableTest(5,5,5);
  print(tmp1 == tmp2);
  
  var tmp3 = test(5,5,5);
  var tmp4 = const ImmutableTest(5,5,5);
  print(tmp3 != tmp4);
```

상수 생성자를 이용하여 동일한 인자를 주면 서로 같다고 판별합니다.

```dart
class superClass {
  String a;

  superClass.method(Map data) {
    print('super');
  }
}

class subClass extends superClass {
  subClass.method(Map data) : super.method(data) {
    print('sub');
  }
}
```

```dart  
  subClass.method({});
```

클래스 상속을 할 수 있습니다.

```dart
abstract class abstractClass{
  void m(){}
}

class implementClass implements abstractClass{
  @override
  m(){
    
  }
}
```

추상 클래스를 생성해서 오버라이드로 구현할 수 있습니다.

추가로 Mixin라는 개념도 있는데, 이전에 일반적으로 추상 클래스를 사용한 이유와 같습니다.

```dart
enum Color { red, green, blue }
```

enum을 사용할 수 있습니다.