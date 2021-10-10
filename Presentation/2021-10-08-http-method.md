---
marp: true
---

# HTTP

by 홍석주

---

## HTTP의 중요성

IT에 있는 자는 분야를 막론하고 HTTP 메서드에 대해 알아야 한다.

- 웬만한 통신은 이제 HTTP를 통해 이루어지기 때문
- HTML, TEXT
- IMAGE, 음성, 영상, 파일
- JSON, XML

현재는 버전이 UDP로 동작하는 3까지 있지만 **버전 1.1**만 알아도 이해할 수 있다.

> 메시지 구조와, 메서드, 상태 코드에 대해 알아보자.

---

## HTTP 메시지 구조

구조는 다음과 같다.

||요청|응답|
|--|--|--|
|start-line|GET /search?q=hello HTTP/1.1|HTTP/1.1 200 OK|
|header||Content-Type: text/html|
|empty line|||
|message body||내용|

empty line은 message body와 구분을 위해 필수로 있어야 한다.

---

## HTTP 메서드

알아야 할 메서드는 5가지 정도가 있다.

- GET: 리소스 조회
- POST: 요청 데이터를 처리, 등록에 사용
- PUT: 리소스를 대체, 해당 리소스가 없으면 생성
- PATCH: 리소스 부분 변경
- DELETE: 리소스 삭제

---

## GET

- 리소스 조회에 사용
- 서버에 전달하고 싶은 데이터는 query(쿼리 파라미터, 쿼리 스트링)를 통해 전달
- 메시지 바디를 사용해서 전달할 수 있지만 권장하지 않음

```text
GET /search?q=hello&lan=ko HTTP/1.1
HOST: www.google.com
```

---

## POST

- 메시지 바디를 통해 서버로 요청 데이터를 전달
- 전달된 데이터로 신규 리소스 등록 또는 프로세스 처리에 사용

```text
POST /members HTTP/1.1
Content-Type: application/json

{
    "username": "hello",
    "age": 20
}
```

---

## PUT

- 리소스를 대체: 있으면 대체하고 없으면 생성한다.
- 클라이언트가 리소스 위치를 알고 URI를 지정한다.

```text
PUT /members/100 HTTP/1.1
Content-Type: application/json

{
    "username": "hello",
    "age": 20
}
```

---

## PATCH

부분적으로 변경을 할 때 사용. 지원 안해주는 서버가 있으면 POST 사용

```text
PATCH /members/100 HTTP/1.1
Content-Type: application/json

{
    "age": 5p
}
```

## DELETE

리소스를 제거해준다.

```text
DELETE /members/100 HTTP/1.1
Host: localhost:8080
```

---

## HTTP 상태코드

상태 코드는 클라이언트가 보낸 요청의 처리 상태를 응답에서 알려주는 기능이다.

- 1XX(Informational): 요청이 수신되어 처리중, 거의 사용하지 않는다.
- 2XX(Successful): 요청 정상 처리
- 3XX(Redirection): 요청을 완료하려면 추가 행동 필요
- 4XX(Client Error): 잘못된 문법등으로 서버가 요청을 수행할 수 없음
- 5XX(Server Error): 서버가 정상 요청을 처리하지 못함

---

## 2XX 성공

클라이언트의 요청을 성공적으로 처리하면 보내는 코드

- 200 OK
- 201 Created
- 202 Accepted
  - 요청이 접수되었으나 처리가 완료되지 않음
  - 배치 처리같은 곳에 사용
- 204 No Content
  - 서버가 요청을 성공적으로 수행했지만, 응답 본문에 보낼 데이터가 없음
  - 웹 문서 편집기에서 save 버튼 등을 눌렀을 때 사용

---

## 3XX 리다이렉션

요청을 완료하기 위해 유저 에이전트(웹 브라우저)의 추가 조치 필요
헤더에 Location을 추가해주어 Location 위치로 자동 이동하게 된다.
ex) /event에 들어오는 사람들을 /new-event로 자동 이동

- 301, 308: 영구 리다이렉션, 특정 리소스의 URI가 영구적으로 이동, 원래의 URL을 사용하지 않고, 검색 엔진 등에서도 변경을 인지
  - 301 Moved Permanently: 리다이렉트시 요청 메서드가 GET으로 변하고, 본문이 제거될 수 있음
  - 308 Permanent Redirect: 301과 같지만 요청 메스드와 본문을 유지한다.
- 302, 303, 307: 일시적인 리다이렉션, 검색 엔진 등에서 URL이 변경되지 않는다.

> PRG(Post/Redirect/Get) 패턴: 새로고침으로 문제됨을 방지

---

## 4XX 클라이언트 오류

클라이언트의 요청에 잘못된 문법 등으로 서버가 요청을 수행할 수 없을 때 쓴다

- 400 Bad Request: 요청 구문, 메시지 등 오류
- 401 Unauthorized: 클라이언트가 해당 리소스에 대한 인증 필요
- 403 Forbidden: 서버가 요청을 이해했지만 승인 거부
- 404 Not Found: 요청 리소스를 찾을 수 없음

---

## 5XX 서버 오류

서버 문제로 오류가 발생했을 때 사용한다.

- 500 Internal Server Error: 서버 문제로 인한 오류
- 503 Service Unavailable: 서버가 일시적인 과부하 또는 예정된 작업으로 잠시 요청을 처리할 수 없음