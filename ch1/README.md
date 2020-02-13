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


|메소드명|의미|CRUD와 매핑되는 역할|
|--------|----|--------------------|
|GET|리소스 취득| Read
|POST|리소스 생성, 리소스 데이터 추가|Create
|PUT|리소스 변경|Update
|DELETE|리소스 삭제|Delete
|HEAD|
|OPTIONS|
|TRACE|
|CONNECT|

#### GET

지정한 URL의 정보를 가져오는 메소드

#### POST, PUT

`POST` 리소스를 생성하는 메소드
`PUT` 리소스를 변경하는 메소드

#### DELETE

`DELETE` 리소스를 삭제하는 메소드

### 1.3.3 GET, POST 메소드

GET

URL 부분의 `?` 뒤에 `이름=값` 쌍으로 이어붙여 보낸다.

```
GET http://docs.djangoproject.com/search/?q=forms&release=1 HTTP/1.1
```

POST

GET에서 URL에 포함시켰던 파라미터들을 아래 예시처럼 요청 메시지의 바디에 넣는다.

```
POST http://docs.djangoproject.com/search/  HTTP/1.1
Content-Type: application/x-www-form-urlencoded

q=form&release=1
```

### 1.3.4 상태코드 Status Code

#### 상태 코드 분류

|메소드명|의미|CRUD와 매핑되는 역할|
|--|--|--|
|1xx|Informational| 임시적인 응답
|2xx|Success|클라이언트의 요청이 서버에서 성공적으로 처리됨
|3xx|Redirection|완전한 처리를 위해서 추가적인 동작을 필요로 함
|4xx|Client Error|클라이언트의 요청 메시지 내용이 잘못된 경우
|5xx|Server Error|서버측 사정에 의해 메시지 처리에 문제가 발생한 경우

#### 자주 사용되는 상태코드

2xx, Success
* 200
  * OK
* 201
  * Created
  * 새로운 리소스 생성
* 202
  * Accepted
  * 요청 접수, 처리 완료 x

3xx, Redirection
* 301
  * Moved Permanently
  * 지정한 리소스가 새로운 URI로 이동
* 303
  * See Other
  * 다른 위치로 요청
* 307
  * Temporary Redirection
  * 임시로 리다이렉션 요청이 필요함

4xx, Client Error
* 400
  * Bad Request
  * 요청의 구문 오류
* 401
  * Unauthorized
  * 지정한 리소스에 대한 엑세스 권한이 없음
* 403
  * Forbidden
  * 지정한 리소스에 대한 엑세스가 금지됨
* 404
  * Not Found
  * 지정한 리소스를 찾을 수 없음

5xx, Server Error
* 500
  * Internal Server Error
* 502
  * Bad Gateway
* 503
  * Service Unavailable

## 1.4 URL 설계

URL 구성 항목
* URL 스킴: URL에 사용된 프로토콜을 의미
* 호스트명: 웹 서버의 호스트명, 도메인명 또는 IP 주소로 표현됨
* 포트번호: 웹 서버 내의 서비스 포트 번호. 생략시 디폴트 포트번호 (http-80, https-443)
* 경로: 파일이나 에플리케이션 경로
* 쿼리스트링: 질의 문자열, &로 구분된 `이름=값` 쌍 형식으로 표현
* 프라그먼트: 문서 내의 조각을 지정

### 1.4.1 URL을 바라보는 측면

RPC, REST
