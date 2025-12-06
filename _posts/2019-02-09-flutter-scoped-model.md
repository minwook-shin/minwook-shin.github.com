---
layout: post
title: flutter에서 Scoped Model 적용하기
---

오늘은 flutter 프로젝트에서 Scoped Model 구조로 만드는 방법을 작성하려 합니다.

Scope Model은 구글의 새로운 운영체제인 퓨시아(Fuchsia)에서 사용되는 gui인 flutter에 Model의 구조로 사용되고 있다고 합니다.

이 방법으로 앱의 구조를 작성하면 StatefullWidget과 State를 사용할 필요가 없어지고, 독립된 모델로 구성할 수 있게 됩니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  scoped_model:
```

앱의 구조를 Scoped Model로 만들기 위해 scoped_model 패키지를 추가해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:scoped_model/scoped_model.dart';
import 'dart:async';
```

머티리얼 디자인의 앱을 만들기 위한 material 패키지와 scoped model을 적용하기 위한 scoped_model 패키지를 가져옵니다.

```dart
class CounterModel extends Model {
  var value = 0;
```

Model을 만들어주고 카운팅을 위한 value 정수형 변수를 만들어줍니다.

```dart
  increment() {
    value += 1;
    notifyListeners();
  }

  decrement() {
    value -= 1;
    notifyListeners();
  }
```

Model에 increment와 decrement 함수를 만들어줍니다.

```dart
  static CounterModel of(BuildContext context) =>
      ScopedModel.of<CounterModel>(context);
}
```

ScopedModel.of를 랩핑해줍니다.

ScopedModel.of로 반환하면 ScopedModelDescendant와 달리 화면을 다시 그리지 않습니다.

```dart
void main() {
  var counter = CounterModel();
  runApp(ScopedModel<CounterModel>(model: counter, child: MyApp()));
```

main 함수에서 ScopedModel을 MyApp 위젯에 전달해줍니다.

```dart
  Timer.periodic(Duration(seconds: 5), (timer) => counter.increment());
}
```

타이머로 5초 간격으로 counter 모델의 increment 함수를 실행하여 value 변수의 값을 증가시킵니다.

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
		title: 'Demo', 
		home: MyHomePage());
  }
}
```

StatelessWidget에서 머티리얼 디자인으로 감싼 MyHomePage을 반환합니다.

```dart
class MyHomePage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
        body: Center(
            child: ScopedModelDescendant<CounterModel>(
                builder: (context, child, counter) =>
                    Text(counter.value.toString()))),
```

MyHomePage도 StatelessWidget이지만, notifyListeners 메소드로 인하여 모델이 변경되었다는 것을 알려주어 화면이 다시 그려지게 됩니다.

ScopedModelDescendant로 위젯 트리에서 모델을 찾아서 이용합니다.

```dart
        floatingActionButton: FloatingActionButton(
            onPressed: () => CounterModel.of(context).decrement(),
            tooltip: 'Increment',
            child: Icon(Icons.remove)));
  }
}
```

floating 버튼을 누르면 CounterModel.of로 랩핑된 ScopedModel.of를 이용하여 모델의 함수를 실행합니다.

이렇게 모델을 사용하게 되면 화면을 다시 그리지 않습니다.