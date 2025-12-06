---
layout: post
title: flutter에서 사용하는 dart 언어의 제어문 알아보기
---

오늘도 flutter에서 사용하는 dart언어를 알아보려 합니다.

이번 포스팅에서는 제어문에 대해서 작성합니다.

## 제어문

제어문은 java와 같은 고급언어들을 비교해보면 매우 흡사함을 알 수 있습니다.

```dart
void main() {
  if(true){
    print('hello, world!');
  }
```

if문을 사용할 수 있습니다.

```dart
  var msg = "hello, world";
  for(var i = 0;i<5;i++){
    print(msg[i]);
  }
```

타 언어와 같이 for문을 사용할 수 있습니다.

초기식, 조건, 증감식을 순서대로 나열합니다.

```dart
  var list = [1,2,3];
  list.forEach((i)=>print(i));
  for(var i in list){
    print(i);
  }
```

forEach 메소드로 함수를 넣어 반복되는 문자들을 출력할 수 있습니다.

for in 형식으로도 가능합니다.

```dart
  var count = 0;
  while(count<3){
    print(count);
    count++;
  }

  do{
    print("hello");
  }while(false);
```

while과 do while문을 지원합니다.

```dart
  while(true){
    print("while...");
    break;
  }
```

break로 반복문을 탈출할 수 있습니다.

```dart
  for(var i = 0;i<4;i++){
    if(i==0){
      continue;
    }
    print(i);
  }
```

continue로 다음 반복으로 넘어갈 수 있습니다.

```dart
  var command = 'dart';
  switch (command) {
    case 'c':
    case 'python':
    case 'java':
    case 'go':
      print("go");
      break;
    case 'dart':
      print("dart");
      continue defaultValue;
    defaultValue:
    default:
      print("default");
  }
```

switch문을 사용 할 수 있습니다.

빈 case로 작성하면 아래로 계속 내려가면서 명령이 수행되지만, case에 코드가 작성된다면 다음 case가 오기 전에 break를 작성해야 합니다.

continue는 goto와 비슷하게 레이블을 추가하여 그 case를 실행할 수 있습니다.

```dart
  try{
    assert(command == null);
  }catch(e){
    print(e);
  }finally{
    print("done!");
  }
}
```

만약 assert가 fasle라면 바로 코드가 멈추지만, try catch문으로 코드의 오류를 막을 수 있습니다.