# CHAPTER 1

웹 프로그래밍의 이해

## 1.1 웹 프로그래밍이란?

HTTP(S) 프로토콜로 통신하는, 클라이언트와 서버를 개발하는 것.

* 웹 서버
  * 웹 프레임워크를 활용하여 웹 서버를 개발함.
* 웹 클라이언트
  1. 웹 브라우저를 사용하여 요청
  2. 리눅스 ```curl``` 명령을 사용하여 요청
  3. Telnet을 사용하여 요청
  4. 직접 만든 클라이언트로 요청

## 1.2 다양한 웹 클라이언트

```www.example.com``` 도메인의 웹 서버를 대상으로 HTTP Request와 Response를 확인해본다.

### 1.2.1 웹 브라우저를 사용하여 요청

웹 브라우저에 ```www.example.com```을 입력

### 1.2.2. 리눅스 curl 명령을 사용하여 요청

```curl``` 명령은 HTTP/HTTPS/FTP 등 여러 가지의 프로토콜을 사용하여 데이터를 송수신할 수 있는 명령이다.
인자로 넘오온 URL로 HTTP 요청을 보내는 웹 클라이언트의 역할을 수행한다.

```bash
$ curl http://www.example.com
```

### 1.2.3 Telnet을 사용하여 요청

리눅스의 ```telnet``` 프로그램을 사용하여 HTTP 요청을 보낸다.

```bash
$ telnet www.example.com 80
```

telnet 명령은 터미널 창에서 입력하는 내용을 그대로 웹 서버에 전송한다.

### 1.2.4 직접 만든 클라이언트로 요청

파이썬 프로그램으로 간단한 웹 클라이언트를 만들어 요청을 보낸다.


example.py
```python
import urllib.request
print(urllib.request.urlopen("http://www.example.com").read().decode('utf-8'))
```

```bash
$ python example.py
```

## 1.3 HTTP 프로토콜

HTTP
* Hypertext Transfer Protocol은 웹 서버와 웹 클라이언트 사이에서 데이터를 주고받기 위해 사용하는 통신 방식
* TCP/IP 프로토콜 위에서 동작
* 모든 데이터를 전송 가능
* http request와 response 메시지를 주고 받는다.

### 1.3.1 HTTP 메시지의 구조

HTTP 메시지 구조

| 스타트라인        |    헤더     |       빈줄       |     바디     |
| ------------ | :-------: | :------------: | :--------: |
| 요청라인 또는 상태라인 | 헤더는 생략 가능 | 헤더의 끝을 빈 줄로 식별 | 바디는 생략  가능 |

스타트라인은 요청 메시지일 때 요청라인(request line)이라고 하고, 응답 메시지일 때 상태라인(status line)이라고 한다.
헤더는 각 행의 끝에 CRLF(Carriage Return Line feed)가 있으며 헤더와 바디는 빈 줄로 구분한다.
바디에는 텍스트, 바이너리 데이터가 들어갈 수 있다.

#### 요청 메시지

바디가 없음

```
get /Book/shakespear HTTP/1.1
Host: www.example.com:8080
```

첫 번째 줄은 요청라인으로, `요청 방식(method), 요청 URL, 프로토콜 버전` 으로 구성된다
두 번째 줄은 헤더로, `이름: 값` 형식으로 표현하며 헤더는 여러 줄도 가능하다.

#### 응답 메시지

```
HTTP/1.1 200 OK
Content-Type: application/xhtml+xml; charset=utf-8

<html>
...
</html>
```

첫 번째 줄의 상태 라인은 `프로토콜 버전, 상태 코드, 상태 텍스트`로 구성됨
`200 OK`이므로 정상적으로 처리되었음을 알 수 있다.

헤더

바디는 보통 HTML 텍스트가 포함되어있다.

### 1.3.2 HTTP 요청 방식



