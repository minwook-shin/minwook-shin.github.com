---
layout: post
title: flutter에서 사용하는 dart 언어의 변수와 타입 알아보기
---

오늘은 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 변수와 타입부터 작성합니다.

## 언어 컨셉

* 변수에 넣을 수 있는 모든 것은 객체이며, 모든 객체는 클래스의 인스턴스입니다.

* 타입이 강력하게 형식화되어 있지만, 유추할 수 있습니다. 
유형이 없다고 선언하려면 dynamic을 사용합니다.

* ```<dynamic>```와 같은 제네릭 형식을 지원합니다.

* 함수에서 함수를 만들 수 있습니다.

* 클래스 또는 객체에 연결된 변수 뿐만 아니라 최상위 변수를 지원합니다. 

* public, protected 및 private 키워드가 없지만, 식별자가 밑줄로 시작하면 비공개로 의미합니다.

* 표현식과 명령문을 모두 가지고 있습니다.

* 프로그램 실행할 때에 경고 및 오류로 나타냅니다.

## 변수

```dart
void main() {
  var hello = 'hello,world';
  print(hello);
```

변수에는 문자열 객체에 대한 레퍼런스가 들어있습니다.

var은 타입을 알아서 유추하여 지정하게 됩니다.

print를 하면 문자열을 출력합니다.

```dart
  dynamic variable;
  print(variable);
  variable = "10";
  print(variable);
```

dynamic을 이용하면 타입이 지정되지 않을 때 사용할 수 있습니다.

```dart
  int year = 10;
  print(year);
  String name = "string";
  print(name);
```

타입을 int,String과 같이 지정할 수 있습니다. 

```dart  
  int lineCount;
  print(lineCount == null);
```

기본적으로 객체에는 null이 들어있으며, 변수에 할당할 때에 타입이 지정됩니다.

```dart  
  final test = 'test';
  final String testStr = 'test';
  print(test);
  print(testStr);

  const test2 = 'test';
  const String testStr2 = 'test';
  print(test2);
  print(testStr2);
```

final과 const는 값을 바꿀 수 없다는 점은 동일하지만, const는 컴파일할 때에 결정되므로 상수와 같이 사용됩니다.

```dart 
  double d = 1;
  print(d == 1.0);
```

더블로 변환됩니다.

```dart
  var ip = int.parse('1');
  print(ip == 1);
  var dp = double.parse('1.1');
  print(dp == 1.1);
```

문자를 int또는 double로 파싱할 수 있습니다.

```dart
  String ts = 1.toString();
  print(ts == '1');
  String tsf = 3.14159.toStringAsFixed(2);
  print(tsf == '3.14');
```

toString도 존재합니다.

```dart  
  var empty = '';
  print(empty.isEmpty);

  var nan = 0 / 0;
  print(nan.isNaN);
```

빈 값인지, NaN인지 판별할 수 있습니다.

```dart  
  var list = [1, 2, 3];
  print(list);
```

리스트를 나타낼 수 있습니다.

```dart  
  var literals = {
  'first': '1',
  'second': '2',
  'third': '3'
	};
  print(literals);
  
  var constructor = Map();
  constructor['first'] = '1';
  constructor['second'] = '2';
  constructor['third'] = '3';
  print(constructor);
```

리터럴로 맵을 생성하는 방식과 맵 생성자로 생성한 뒤에 값을 삽입하는 방식이 존재합니다.

```dart  
  #symbol;
}
```

선언된 연산자 또는 식별자라고 하며, 그 외에도 룬이라고하는 utf-32로 이루어진 문자들도 있습니다.