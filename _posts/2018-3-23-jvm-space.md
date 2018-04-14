---
layout: post
title: Java 가상머신안 영역 알아보기
---

자바 가상 머신은 물리적인 실제의 기계 장치가 아니라 추상적인 장치입니다.

그레서  JAVA가 OS에 상관하지 않고, 클래스 파일을 실행하는 역할을 합니다.

자바 가상 머신은 클래스 영역, 자바 스택 영역, 힙 영역 그리고 네이티브 메소드 스택 영역으로 구성되어 있습니다.

## 클래스 영역

실행에 필요한 클래스를 저장하는 공간으로서, 불러들인 클러스의 변수들을 메소드 영역에 저장하게 합니다.

물론 클래스 코드도 저장됩니다.

## 스택 영역

프로그램이 발생하는 메소드 호출과 복귀에 대한 정보를 생성하여 저장합니다.

Last In First Out (LIFO)입니다.

메서드 수행이 끝나면 프레임별로 삭제가 됩니다.

## 힙 영역

객체를 동적으로 할당할 때 공간을 힙 영역에 할당하게 됩니다.

자바의 가바지 컬렉션 기능으로 자동으로 청소되는 영역입니다.

## 네이티브 메소드 스택 영역

하드웨어를 제어해야 할 때 쓰이는 공간으로서, c언어와 같은 타 언어의 기능을 빌려 쓰면서 JNI 기술이 기록하는 공간입니다.
