---
layout: post
title: flutter에서 사용하는 dart 언어의 비동기 stream 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 비동기를 이용한 stream에 대해서 작성합니다.

## stream

stream은 비동기 데이터 시퀀스를 제공하므로 단일 구독, 브로드캐스트등 형식으로 비동기 이벤트를 구현할 수 있습니다.

```dart
Stream<int> count(int i) async* {
  for (int j = 1; j <= i; j++) {
    yield j;
  }
}
```

함수를 async*로 작성하고, yield문을 사용하여 값을 전달합니다.

```dart
Future<int> adder(Stream<int> i) async {
  var sum = 0;
  await for (var v in i) {
    sum += v;
  }
  return sum;
}
```

전달받은 Stream타입의 값을 await for로 스트림 이벤트를 반복하는 작업을 합니다.

이벤트를 받아서 계속 더해주고 있습니다.

```dart
main() async {
  var sum = await adder(count(10));
  print(sum);
}
```

adder 함수의 작업이 완료되었으면 합계를 출력합니다.