---
layout: post
title: Rxjava 실습하면서 알아보기
---

오늘은 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions을
java에서 적용할 수 있는 Rxjava를 설치하고 Visual studio Code 환경으로 실습해보려 합니다.

Rxjava는 비동기 프로그래밍을 구현할 수 있는 Reactive Extensions의 java 라이브러리입니다.

## maven

java의 빌드 도구인 maven을 설치해야 rxjava 라이브러리를 의존성으로 사용할 수 있습니다.

물론 rxjava는 gradle도 지원합니다.

mac에서 maven을 설치하려면 아래와 같이 터미널에 입력합니다.

```
brew install maven
```

만약 우분투라면 apt로 설치해야 합니다.

```
sudo apt install maven
```

## vscode

vscode에서 Java Extension Pack를 설치하면 별도의 에디터없이 간단하게 java 프로젝트를 시작할 수 있습니다.

```
>Java: Create Java Project
```

vscode의 보기 매뉴에서 명령 팔레트를 열어서 위와 같이 입력합니다.

## pom.xml

빌드 정보를 기술하는 pom.xml으로 rxjava jar 파일을 내려받으려고 합니다.

```xml
<?xml version="1.0"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
      <modelVersion>4.0.0</modelVersion>
      <groupId>io.reactivex.rxjava2</groupId>
      <artifactId>rxjava</artifactId>
      <version>2.2.0</version>
      <name>RxJava</name>
      <description>Reactive Extensions for Java</description>
      <url>https://github.com/ReactiveX/RxJava</url>
      <dependencies>
          <dependency>
              <groupId>io.reactivex.rxjava2</groupId>
              <artifactId>rxjava</artifactId>
              <version>2.2.0</version>
          </dependency>
      </dependencies>
</project>
```

위와 같이 작성하고, 아래 명령어를 터미널에서 수행합니다.

```
mvn -f pom.xml dependency:copy-dependencies
```

./target/dependency 디렉토리에 reactive-streams-1.0.2.jar 파일을 내려받을 수 있습니다.

물론 jar 파일을 내려받지 않고, 아래와 같이 간단히 gradle이나 maven 의존성 추가해도 됩니다.

```java
compile 'io.reactivex.rxjava2:rxjava:2.2.0'
```

```xml
<dependency>
    <groupId>io.reactivex.rxjava2</groupId>
    <artifactId>rxjava</artifactId>
    <version>2.2.0</version>
</dependency>
```

## .classpath

내려받은 jar 파일을 프로젝트와 연결해줍니다.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<classpath>
	<classpathentry kind="con" path="org.eclipse.jdt.launching.JRE_CONTAINER/org.eclipse.jdt.internal.debug.ui.launcher.StandardVMType/JavaSE-1.8"/>
	<classpathentry kind="src" path="src"/>
	<classpathentry kind="output" path="bin"/>
	<classpathentry kind="lib" path="target/dependency/rxjava-2.2.0.jar"/>
	<classpathentry kind="lib" path="target/dependency/reactive-streams-1.0.2.jar"/>
</classpath>
```

classpath에 lib를 추가해줍니다.

## hello world!

이제 hello world를 작성하려 합니다.

```java
package app;

import io.reactivex.*;

public class App {
    public static void main(String[] args) {
        Flowable.just("Hello world").subscribe(System.out::println);
    }
}
```

"Hello world"라는 문자열을 Flowable로 변환해서 구독하여 출력하는 코드입니다.

Ctrl+F5로 코드를 수행하거나, main 메소드 위에 출력되는 run 버튼을 누르면 수행됩니다.
(mac인 경우에는 fn+control+F5입니다.)

## Flowable

Flowable은 rxjava2부터 나온 개념으로서 Backpressure buffer를 지원하는 Observable입니다.

이로 인하여 Flowable으로 흐름 제어를 할 수 있습니다.

```java
package app;

import io.reactivex.*;

public class App {
    public static void main(String[] args) {
        Observable.just("Hello world").subscribe(System.out::println);
    }
}
```

작은 범위에서는 위의 코드와 같은 역할을 하지만, Observable는 흐름 제어 함수를 사용하지 못한다는 차이점이 존재합니다.
