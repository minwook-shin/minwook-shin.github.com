---
layout: post
title: 구글의 JAVA 코딩 가이드 알아보기
---

오늘은 구글에서 제공해주는 문서중에 자바 코딩 가이드 문서를 알아보도록 해보겠습니다.

## 소스 파일 이름 

소스 파일의 이름은 포함하고 있는 최상위 레벨의 대소문자 구분되는 이름으로 이루어집니다.

## 인코딩 

파일 인코딩은 UTF-8으로 인코딩됩니다.

## 공백

공백은 이스케이프 처리되며, 탭 문자는 공백에 사용하지 않습니다.

## 소스 파일 구조

1. 라이선스나 저작권 정보

1. 패키지 문

1. import 문

1. 최상위 클래스

## 중괄호

중괄호는 본문이 비어있고 단일문만 포함되는 경우에도 반목문, 조건문과 함께 사용합니다.

## K & R

K & R 스타일은 여는 궁괄호 앞에 줄 바꿈이 없으며, 그 다음에 줄 바꿈이 있습니다.

다만, 닫는 중괄호는 줄 바꿈을 합니다.

닫는 중괄호가 수행을 종료하는 메소드나 생성자나 클래스면, 중괄호가 닫힐 때 줄을 바꾸나, else나 쉼표가 뒤따라오면 줄 바꿈이 없습니다.

## 들여쓰기

새로운 블록이나 같은 범위의 블록이 열릴 때마다 공백이 두칸 증가합니다.

## 문장 줄 바꿈

각각의 명령줄은 한 줄에 한 명령줄입니다.

## 열 제한

자바 코드의 열 제한은 100자입니다.

## 줄 바꿈 공백

줄 바꿈을 할 때는 원래 줄에서 적어도 4칸은 띄어야 합니다.

## 변수 선언

모든 변수 선언은 하나의 변수만 선언합니다.

int a,b 와 같이 작성하지 않습니다.

## 스위치문

스위치문의 들여쓰기는 2칸으로 들여씁니다.

레이블마다 줄 바꿈이 있으며 들여쓰는 것은 블록이 열리는 것과 같이 2칸이 벌어집니다.

다음 스위치 레이블은 블록이 닫힌 것처럼 이전 들여쓰기 높이로 돌아갑니다.

## 주석

클래스, 메소드 또는 생성자에 적용되는 주석은 문서 블록의 바로 뒤에 작성됩니다.

매개 변수나 지역 변수에 대한 서식의 특정 규칙은 없습니다.

## 숫자 리터럴

long 값의 정수 리터럴은 대문자 L을 사용합니다.

## 공통 네이밍

식별자는 ASCII 문자와 숫자만 사용합니다.

## 패키지 이름

패키지 이름은 모두 소문자로 하며, 연속 단어들은 연결됩니다.

## 클래스 이름

클래스 이름은 upper camel case입니다.

일반적으로 명사로 이루어져 있으며, 대문자로 각각의 연속 단어를 구별합니다.

만약 테스트 클래스라면 이름 끝에 Test라고 끝납니다.

## 메소드 이름

메소드 이름은 lower camel case 입니다.

일반적으로 동사로 나타냅니다.

## 상수 이름

상수 이름은 CONSTANT_CASE로 사용하며, 각 단어들은 밑줄로 구별됩니다.

## 필드 이름

lower camel case이며, 일반적으로 명사로 이루어져 있습니다.

## 매개 변수 이름

lower camel case이며, 한 문자 매개변수명은 피합니다.

## 지역 변수 이름

lower camel case이며, 상수로 지정되지 않습니다.

## 참고 문서

기타 javadoc이나 프로그래밍 연습 문단은 아래 원문에서 확인해보시기 바랍니다.

https://google.github.io/styleguide/javaguide.html
