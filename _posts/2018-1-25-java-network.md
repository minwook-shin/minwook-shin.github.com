---
layout: post
title: java network 실습해보기
---

Java에서 네트워크 관련해서 간단히 알아보았습니다.

들은 이야기로는 java가 다른 언어에 비해 네트워크 기능이 뛰어나다고 합니다.

## InetAdress

네트워크상의 정보를 얻어오는 클래스입니다.

```java
import java.net.InetAddress;
import java.net.UnknownHostException;
```

우선 필요한 것들은 import합니다.

위와 같은 코드는 이클립스에서 자동으로 채워지지만, 어느 집합에 속해있는지 보시면 좋습니다.

네트워크 관련된 것들은 주로 java.net에 위치합니다.

그래서 주로 ```java.net.*```로 대체하기도 합니다.

```java
public class InetAdress_test {

	public static void main(String[] args) {
		InetAddress test = null;
```

InetAddress 타입의 변수를 null로 초기화하면서 만들어 줍니다.

```java
		try {
			test = InetAddress.getLocalHost();
		} catch (UnknownHostException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
        System.out.println(test.getHostName() + " " + test.getHostAddress());
```

getLocalHost() 메소드를 통하여 로컬 호스트, 즉 본인의 컴퓨터에 대한 정보를 얻어옵니다.

얻어온 정보를 위에 선언하면서 초기화했던 InetAddress 타입의 변수로 저장해줍니다.

그리고 System.out.println으로 InetAddress 타입의 변수에 있는 호스트 이름과 호스트 주소를 가져옵니다.

여기서 try catch문은 예외처리를 위한 구문으로, try를 실행하면서 오류 e를 catch하여 던져줍니다.

try catch문이 없으면 컴파일 오류가 일어 납니다.

```java
		InetAddress google_address = null;
```

이제 로컬호스트로만 실헙하지 말고, 구글 주소로 실험해보기 위해 다시 InetAddress 타입의 변수를 선언하고 초기화합니다.

```java
		try {
			google_address = InetAddress.getByName("www.google.com");
		} catch (UnknownHostException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
		System.out.println(google_address);
	}
}
```

InetAddress 타입의 변수에 getByName() 메소드로 구글의 주소를 입력합니다.

getByName() 메소드는 문자열로 받아서 처리하게 됩니다.

이 역시 예외처리 구문이 필요합니다.

마지막으로 InetAddress 타입의 변수를 출력합니다.

## URLConnection

URL 클래스 : dns를 통한 ip 정보를 이용해 객체를 만든 뒤, 네트워크와 URL 정보를 출력합니다.

URLConnection 클래스 : 객체가 추상클래스로부터 생성됩니다. URL 클래스의 openConnetion()메소드를 이용합니다.

URLConnection 클래스가 만들어진 뒤에 정보가 오고갑니다.

```java
import java.io.IOException;
import java.io.InputStream;
import java.net.MalformedURLException;
import java.net.URL;
import java.net.URLConnection;
import java.util.Scanner;
```

위 InetAdress처럼 필요한 것들은 import합니다.

위와 같은 코드는 이클립스에서 자동으로 채워지지만, 어느 집합에 속해있는지 보시면 좋습니다.

네트워크 관련된 것들은 주로 java.net에 위치합니다.

그래서 주로 ```java.net.*```로 대체하기도 합니다.

다만 이번에는 입출력 코드가 들어가기 때문에 java.io 부분도 건들이게 됩니다.

```java
public class URLConnection_test {

	public static void main(String[] args) {
		URL url;
		InputStream in_s;
		Scanner in_scan;
		String code;
```

URL 타입의 객체와 Scanner 객체를 선언하고, 나머지 변수들도 만들어 줍니다.

```java
		try {
			url = new URL("http://www.google.com");
			URLConnection con = url.openConnection();
			in_s = con.getInputStream();
			in_scan = new Scanner(in_s);
```

이미 선언한 URL 타입의 객체에 구글의 주소로 만들어주고, 그 객체의 openConnection()메소드를 이용해 정보를 URLConnection 변수에 넣어줍니다.

그리고 입력 스트림을 사용하여 입력 받을 준비를 합니다.

```java
			while(in_scan.hasNext()) {
				code = in_scan.next();
				System.out.println(code);
				}
			in_scan.close();
```

while 문으로 hasNext() 메소드를 이용하여 다음 줄이 있을 때만 다음 라인을 출력합니다.

다 되었으면 Scanner 객체를 종료합니다.

```java
		} catch (MalformedURLException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		} catch (IOException e) {
			// TODO Auto-generated catch block
			e.printStackTrace();
		}
	}
}
```

마지막으로 이 역시 예외처리 구문이 필요합니다.

## socket

네트워크상에서 서로 다른 호스트 사이의 통신을 위한 수단입니다.

서버와 클라이언트간 i/o 스트림으로 연결하고 소켓을 이용하게 됩니다.

방법을 간단히 설명해보자면, 아래와 같습니다.

1. 서버에서 (포트를 열어서) ServerSocket(port) 으로 서버 소켓을 만들고 클라이언트의 요청을 기다립니다.

1. 클라이언트에서 (포트를 열어서) Socket(ip, port) 으로 소켓을 만들고 getInputStream()과  getOutputStream() 메소드를 사용하여 i/o 스트림을 만들어 서버에 요청합니다.

1. 서버에서 accept() 메소드로 클라이언트의 요청을 받아 소켓을 만들고 getInputStream() 과 getOutputStream() 메소드를 사용하여 i/o 스트림을 만듭니다.

1. readLine() 메소드로 서로 데이터를 주고 받습니다.

1. close() 메소드로 소켓을 정상적으로 닫습니다.