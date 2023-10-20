---
layout: post
title: "[HTTP웹기본지식_인프런] HTTP 기본"
date: 2023-09-19 00:00:00 +0530
categories: http
---

<br/>

#### HTTP (HyperText Transfer Protocol)

- HTTP 메시지에 모든 것을 전송 가능
- HTML, TEXT, IMAGE, 음성, 영상, 파일, JSON, XML(API) 거의 모든 형태의 데이터 전송 가능
- 서버간의 데이터를 주고 받을 때도 대부분 HTTP 사용

<br/>

**HTTP 특징**

- 클라이언트 서버 구조로 동작
- 무상태 프로토콜(스테이스리스), 비연결성
- HTTP 메시지를 통해 통신
- 단순함, 확장 기능

<br/>

#### 클라이언트 서버 구조

- Request Response 구조
- 클라이언트: 서버에 요청을 보내고 응답을 대기
- 서버: 요청에 대한 결과를 만들어서 응답
  [클라이언트] -> 요청 ㅡ 응답 <- [서버]

<br/>

**클라이언트와 서버를 개념적으로 분리하는 것이 중요**

- 클라이언트: UI, UX, 사용성에 집중
- 서버: 비즈니스 로직, 데이터 처리에 집중
  => 클라이언트, 서버는 각각 독립적으로 진화 가능

<br/>

#### 무상태 프로토콜(스테이스리스)

- 서버가 클라이언트의 상태를 보존X
- 장점: 서버 확장성 높음(스케일 아웃)
- 단점: 클라이언트가 추가 데이터 전송

<br/>

**상태 유지 - Stateful**

- 항상 같은 서버가 유지되어야 한다.
- 중간에 서버가 장애나면? 처음부터 다시 해야한다.

<br/>

**무상태 - Stateless 차이**

- 아무 서버나 호출해도 된다. (상태를 보관하지 않고 응답만 한다.)
- 중간에 서버가 장애나면? 다른 서버가 응답한다.(상태 보관 x)

<br/>

#### 실무 한계

- 가능한 무상태로 설계하고, 상태 유지는 최소한으로 사용
- 무상태 > 로그인이 필요없는 단순한 서비스 소개 화면

<br/>

**상태 유지 > 로그인**

- 로그인 사용자의 경우, 로그인 정보를 서버에 유지
- 일반적으로 브라우저 쿠키, 서버 세션 등을 사용하여 상태 유지

<br/>

#### 비 연결성

- HTTP는 기본이 연결을 유지하지 않는 모델(최소한의 자원 사용)
  -> 서버는 연결을 계속 유지하게 되면 서버 자원 소모가 된다.
- 일반적으로 초 단위 이하의 빠른 속도로 응답
- 1시간 동안 수천명이 서비스를 사용해도 실제 서버에서 동시에 처리하는 요청은 수십개 이하로 매우 작음
  예) 웹 브라우저에서 계속 연속해서 검색 버튼을 누르지는 않는다.
- 서버 자원을 매우 효율적으로 사용할 수 있음

<br/>

#### 비 연결성 - 한계와 극복

**한계**

- TCP/IP 연결을 새로 맺어야 함 - 3 way handshake 시간 추가(연결 + 요청/응답 + 종료)
- 웹 브라우저로 사이트를 요청하면 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등 수많은 자원이 함께 다운로드

<br/>

**극복**

- 지금은 HTTP 지속 연결(Persistent Connections)으로 문제 해결
- HTTP/2, HTTP/3에서 더 많은 최적화

<br/>

**HTTP 초기 - 연결, 웅답, 종료 낭비**

- HTTP 초기에는 비 연결성으로 인해 각각의 자원에 대해 연결, 요청/응답, 종료를 반복함으로써, 대략적으로 1초가량 소모

<br/>

**HTTP 지속 연결(Persistent Connections)**

- 클라이언트는 서버와 연결 후 HTML 뿐만 아니라 자바스크립트, css, 추가 이미지 등 수많은 자원이 함께 다운로드 받는다.
  초기와 차이점으로는 연결이 종료되지 않고, 필요한 자원들을 모두 다운로드 받을때까지 요청/응답이 반복된 후 종료된다.
- 속도 증가, 시간 단축

<br/>

**스테이리스를 기억하자**

- 서버 개발자들이 가장 어려워하는 업무
- 정말 같은 시간에 딱 맞추어 발생하는 대용량 트래픽
  예) 선착순 이벤트, 한국시리즈 예매, 명절 기차 예매, 수강신청 -> 수만명 동시 요청으로 인해 과부화가 걸리는 경우 발생
- 이 경우, 무상태 페이지를 활용해 페이지 접속인원을 분산해 대용량 트래픽을 분산시키면 좋다.

<br/>

#### HTTP 메시지

**HTTP 메시지 구조**

- start-line 시작 라인
- header 헤더
- empty line 공백 라인 (CRLF)
- message body

<br/>

**HTTP 헤더 용도**

- HTTP 전송에 필요한 모든 부가 정보 ex) 메시지 바디의 내용, 메시지 바디의 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보 등등

<br/>

**HTTP 메시지 바디 용도**

- 실제 전송할 데이터 ex) HTML문서, 이미지, 영상, JSON 등 byte로 표현할 수 있는 모든 데이터 전송 가능

<br/>

**HTTP 메시지 [공식 스펙]**

HTTP-message = start-line
\*( header-field CRLF )
CRLF
[ message-body ]

<br/>

**HTTP 요청 메시지**

- ex)

```javascript
GET /search?q=hello&hl=ko HTTP/1.1  --> (start-line = request-line)
Host: www.google.com  --> (request-line)
```

<br/>

**request-line = method SP(공백) request-target SP(공백) HTTP-version CRLF(개행)**

- method: 서버가 수행해야 할 동작을 지정 ex) GET, POST, PUT, DELETE
- request-target: 요청 대상
- HTTP-version: HTTP 버전

<br/>

**HTTP 응답 메시지**

- ex)

```javascript
HTTP/1.1 200 OK  --> (start-line = status-line)
Content-Type: text/html;charset=UTF-8  --> (Content-Type)
Content-Length: 3423  --> (Content-Length)

<html>
  <body>...</body>
</html>
```

<br/>

**status-line = HTTP-version SP(공백) status-code SP(공백) reason-phrase CRLF(개행)**

- HTTP-version: HTTP 버전
- status-code: HTTP 상태 코드 ex) 200*성공, 400*클라이언트 요청 오류, 500\_서부 내부 오류
- reason-phrase: 짧은 상태 코드 설명 글

<br/>

<br/><br/><br/>
**_개인적인 이해를 바탕으로 작성한 글 입니다. <br/>
잘못된 내용, 피드백은 언제든 환영합니다!_** 🥺🥺🥺
<br/><br/><br/>

### 참고 사이트

<hr>
[3.HTTP 기본](https://catsbi.oopy.io/5c0b482c-b427-4052-9030-d2be0810eeb6)
<br/>
