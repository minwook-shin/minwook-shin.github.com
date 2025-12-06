---
layout: post
title: flutter에서 Redux 적용하기
---

오늘은 flutter 프로젝트에서 redux 구조로 만드는 방법을 작성하려 합니다.

react에서 자주 사용하며 데이터를 처리하기에 효과적입니다.

## pubspec.yaml 작성하기

```yaml
dependencies:
  flutter:
    sdk: flutter
  flutter_redux:
```

flutter로 작성된 앱의 구조를 redux로 만들기 위해 flutter_redux 패키지를 추가해줍니다.

## main.dart 앱 코드 작성하기

```dart
import 'package:flutter/material.dart';
import 'package:flutter_redux/flutter_redux.dart';
import 'package:redux/redux.dart';
```

머티리얼 디자인의 앱을 만들기 위한 material 패지키와 redux를 적용하기 위한 redux와 flutter_redux 패키지도 가져옵니다.

```dart
enum Actions { Increment, Decrement }
```

앱에서 store로 데이터를 보내줄 액션을 만들어줍니다.

이 코드에서는 Increment와 Decrement를 만들어서 정수형 데이터의 값을 조정합니다.

```dart
int counter(state, action) {
  switch (action) {
    case Actions.Increment:
      return state + 1;
      break;
    case Actions.Decrement:
      return state - 1;
      break;
    default:
      return 0;
  }
}
```

이전의 state를 가져와서 액션에 따라 반환값을 달리 해줍니다.

```dart
void main() {
  runApp(MaterialApp(
    title: 'flutter redux', 
    home: ReduxApp()));
}
```

이제 메인 함수에서 runApp으로 StatelessWidget을 구동합니다.

```dart
class ReduxApp extends StatelessWidget {
  final store = Store(counter, initialState: 0);
```

앱의 상태를 저장하고 가져오는 store을 만들어줍니다.

```dart
  @override
  Widget build(BuildContext context) {
    return StoreProvider(
      store: store,
```

StoreProvider로 Redux의 store를 위젯에 전달해줍니다.

```dart
      child: Scaffold(
        body: Padding(
          padding: EdgeInsets.all(30.0),
          child: Column(children: [
            StoreConnector<int, String>(
                converter: (store) => store.state.toString(),
                builder: (context, count) {
                  return Text(count);
                }),
```

StoreConnector로 위젯을 감싸면, store을 모델로 변환해주면서 builder 함수에 전달해줍니다.

전달된 store는 Text 생성자에서 쓰이게 됩니다.

```dart
            StoreConnector<int, VoidCallback>(
              converter: (store) {
                return () => store.dispatch(Actions.Increment);
              },
              builder: (context, callback) {
                return OutlineButton(
                    onPressed: callback, child: Icon(Icons.add));
              },
            ),
```

버튼과 같은 경우는 builder 함수에 전달되는 반환값이 callback 함수이므로 위와 같이 작성해줍니다.

store을 모델로 변환할 때에 dispatch로 상태를 수정하게 됩니다.

이 때는 Increment 함수가 dispatch 인자로 들어가게 됩니다.

```dart
            StoreConnector<int, VoidCallback>(
              converter: (store) {
                return () => store.dispatch(Actions.Decrement);
              },
              builder: (context, callback) {
                return OutlineButton(
                    onPressed: callback, child: Icon(Icons.remove));
              },
            ),
          ]),
        ),
      ),
    );
  }
}
```

이 때는 Decrement 함수가 dispatch 인자로 들어가게 됩니다.

store에서 상태가 변경되었다는 신호를 주면 구독을 관리할 필요없이 위젯이 자동으로 새로 고쳐지게 됩니다.