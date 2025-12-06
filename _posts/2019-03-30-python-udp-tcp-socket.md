---
layout: post
title: Python socket으로 UDP와 TCP 소켓 통신해보기
---

오늘은 Python으로 UDP와 TCP 소켓 프로그래밍을 체험해보려 합니다.

## 용어 정리

- udp : 신뢰성이 없는 데이터그램으로서 데이터가 중복되거나 누락될 수 있지만 속도가 빠릅니다.

- tcp : 신뢰성이 있는 바이트 스트림 지향으로서 메시지 수신을 확인하며 통신하게 됩니다.

* 소켓 : 프로세스 통신의 종착점입니다.

## udp

클라이언트와 서버 모두 소켓을 열어주고, 클라이언트에서 서버 호스트와 포트로 데이터그램을 생성합니다.

서버에서는 생성된 데이터 그램을 읽어들여서 소켓을 이용하여 서버 호스트와 포트로 반환합니다.

다시 클라이언트에서는 소켓으로 받은 데이터 그램을 읽어 값을 얻게 됩니다.

아래 예시 코드에서는 모든 문자열을 대문자로 바꾸어주게 됩니다.

- server

```python
import socket
```

socket 라이브러리를 사용합니다.

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```

udp socket을 생성합니다.

```python
sock.bind(("127.0.0.1", port))
```

로컬호스트의 포트를 바인드해줍니다.

int형인 port 변수는 이미 사용중인 포트만 아니면 됩니다.

```python
while True:
    data, addr = sock.recvfrom(1024)
```

소켓으로부터 메시지를 읽고 클라이언트의 주소도 같이 가져옵니다.

```python
    data = data.decode().upper()
    sock.sendto(data.encode(), addr)
```

메시지를 디코드해서 대문자로 바꾸어주고, 클라이언트로 다시 돌려줍니다.

```python
sock.close()
```

소켓을 닫습니다.

- client

```python
import socket
```

socket 라이브러리를 사용합니다.

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)
```

서버에 전송할 udp 소켓을 만듭니다.

```python
while True:
    msg = input("->")
    sock.sendto(msg.encode(), ("127.0.0.1", 5005))
```

키보드에서 입력을 받고, 인코드된 해당 문자열을 호스트의 포트로 보냅니다.

```python
    recvMsg, addr = sock.recvfrom(2048)
    print(recvMsg.decode())
```

서버로부터 받은 문자열과 주소를 저장하고 출력합니다.

```python
sock.close()
```

소켓을 닫습니다.

## tcp

서버에서 소켓을 열고, 연결 요청을 기다립니다.

이 때에 클라이언트에서 소켓을 열고 호스트와 포트로 서버에 연결을 요청합니다.

서버에서 소켓을 통해 요청을 수락하면 새로운 소켓이 생성되고, 해당 소켓으로 통신하게 됩니다.

아래 예시 코드에서는 모든 문자열을 대문자로 바꾸어주게 됩니다.

- server

```python
import socket
```

socket 라이브러리를 사용합니다.

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
```

서버에서 사용할 tcp 소켓을 생성합니다.

```python
sock.bind(('127.0.0.1', Port))
sock.listen()
```

소켓에 호스트의 포트를 바인딩하고, tcp 연결 요청을 기다립니다.

int형인 port 변수는 이미 사용중인 포트만 아니면 됩니다.

```python
con, addr = sock.accept()
```

기다리다가 클라이언트에서 연결 요청이 오면 새로운 소켓을 생성하여 반환합니다.

```python
while True:
    data = con.recv(1024).decode()
    if not data:
        break
```

소켓에서 바이트를 읽어 디코드합니다.

```python
    capitalized = data.upper()
    con.send(capitalized.encode())
```

받은 데이터를 모두 대문자로 바꾸어 클라이언트로 보냅니다.

```python
sock.close()
```

소켓을 닫습니다.

- client

```python
import socket
```

socket 라이브러리를 사용합니다.

```python
sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)
sock.connect(('127.0.0.1', 5005))
```

클라이언트에서 사용할 tcp 소켓을 생성하고 호스트의 포트로 연결합니다.

```python
while True:
    msg = input("->")
    sock.send(msg.encode())
```

키보드에서 입력을 받고, 인코드된 해당 문자열을 호스트의 포트로 보냅니다.

```python
    data = sock.recv(1024)
    print(data.decode())
```

소켓으로 데이터를 받아서 디코드한 뒤에 출력합니다.

```python
sock.close()
```

소켓을 닫습니다.
