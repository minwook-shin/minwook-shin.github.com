---
layout: post
title: flutter에서 Bloc 적용하기
---

오늘은 flutter 프로젝트에서 bloc 구조로 만드는 방법을 작성하려 합니다.

Bloc 구조는 인터페이스와 데이터를 처리하는 프로그램 로직을 분리하여 반응형 프로그래밍을 할 때 사용합니다. 

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_bloc:
```

flutter로 작성된 앱의 구조를 bloc 으로 만들기 위해 flutter_bloc 패키지를 추가해줍니다.

## main.dart 앱 코드 작성하기

이제 앱 코드를 보겠습니다.

```dart
import 'package:flutter/material.dart';
import 'package:bloc/bloc.dart';
import 'package:flutter_bloc/flutter_bloc.dart';
```

머티리얼 디자인의 앱을 만들기 위한 material 패키지와 bloc 구조를 위한 flutter_bloc과 bloc 패키지도 가져옵니다.

```dart
enum CounterEvent { increment, decrement }
```

열거형을 이용하여 묶어줍니다.

```dart
class BlocDelegateClass extends BlocDelegate {
  @override
  void onTransition(Transition transition) {}
}
```

이벤트가 전달되고, bloc이 업데이트되기 전에 불러오는 onTransition 메소드를 오버라이드해줍니다.

때에 따라서 onTransition 메소드에서 원하는 기능을 구현할 수 있습니다.

```dart
void main() {
  BlocSupervisor().delegate = BlocDelegateClass();
  runApp(MyApp());
}
```

BlocDelegate 객체가 BlocSupervisor에서 위임을 받아 Blocs의 이벤트들을 처리합니다.

```dart
class MyApp extends StatefulWidget {
  @override
  State<StatefulWidget> createState() => _MyAppState();
}
```

StatefulWidget으로 버튼, 슬라이더, 체크 박스처럼 사용자와 상호작용하여 위젯이 변화하는 것을 묶어줍니다.

createState 메소드를 오버라이딩하여 _MyAppState 객체를 생성해줍니다.

```dart
class CounterBloc extends Bloc<CounterEvent, int> {
  @override
  get initialState => 0;
```

initialState를 오버라이드하여 이벤트가 처리되기 전의 상태를 0으로 초기화해줍니다.

```dart
  @override
  Stream<int> mapEventToState(currentState, event) async* {
    switch (event) {
      case CounterEvent.increment:
        yield currentState + 1;
        break;
      case CounterEvent.decrement:
        yield currentState - 1;
        break;
    }
  }
}
```

Bloc 클래스를 확장할 때에 mapEventToState 메소드를 오버라이드하여 presentation 레이어가 이벤트를 전달해줄 때마다 switch문에 의해 비동기 Stream 형식으로 상태를 반환합니다.

```dart
class _MyAppState extends State<MyApp> {
  final _counterBloc = CounterBloc();
```

Bloc을 확장한 CounterBloc 객체를 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'bloc code',
      home: BlocProvider(
        bloc: _counterBloc,
        child: MyappPage(),
      ),
    );
  }
}
```

BlocProvider는 CounterBloc 객체를 하위 위젯 트리에 제공해줍니다.

```dart
class MyappPage extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    final _counterBloc = BlocProvider.of<CounterBloc>(context);
```

BlocProvider.of로 Bloc을 전달받게 됩니다.

```dart
    return Scaffold(
      body: BlocBuilder(
        bloc: _counterBloc,
        builder: (context, count) {
          return Center(child: Text(count.toString()));
        },
      ),
```

StreamBuilder처럼 비동기로 들어오는 상태를 위젯으로 그려줍니다.

위 코드에서는 Center와 Text 생성자로 인하여 가운데에 count 상태가 문자열로 출력됩니다.

```dart
      floatingActionButton: Column(
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
            child: Icon(Icons.add),
            onPressed: () {
              _counterBloc.dispatch(CounterEvent.increment);
            },
          ),
```

floating 버튼을 만들어줍니다.

이 버튼이 눌리면 mapEventToState를 반응하게 만들어서 increment라는 이벤트로 Bloc에 통지해줍니다.

```dart
          FloatingActionButton(
            child: Icon(Icons.remove),
            onPressed: () {
              _counterBloc.dispatch(CounterEvent.decrement);
            },
          ),
        ],
      ),
    );
  }
}
```

floating 버튼을 하나 더 만들어줍니다.

이 버튼이 눌리면 mapEventToState를 반응하게 만들어서 decrement라는 이벤트로 Bloc에 통지해줍니다.