---
layout: post
title: "[HTTP웹기본지식_인프런] URI와 웹 브라우저 요청 흐름"
date: 2023-09-16 00:00:00 +0530
categories: http
---

<br/>

#### URI(Uniform Resource Identifier)

- 특정 리소스를 식별하는 통합 자원 식별자(Uniform Resource Identifier)를 의미
- 웹 기술에서 사용하는 논리적 또는 물리적 리소스를 식별하는 고유한 문자열 시퀀스

<br/>

**URI(Uniform Resource Identifier)**

- Uniform: 리소스를 식별하는 통일된 방식
- Resource: 자원
- Identifier : 다른 항목과 구분하는데 필요한 정보

<br/>

#### URL(Uniform Resource Locater), URN(Uniform Resource Name)

- Locater: 리소스 위치 지정
- Name: 리소스 이름 부여
- 위치는 변할 수 있지만 이름은 변하지 않음.
- URN 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않음.
- URI를 URL과 같은 의미로 생각

<br/>

**URL(Uniform Resource Locater)**

- 웹에서 리소스의 위치를 지정하는 데 사용되는 문자열
- 웹 브라우저나 다른 클라이언트 애플리케이션에게 어떤 서버에서 어떤 리소스를 가져와야 하는지 알려줌.

<br/>

**URL 문법**

scheme://[userinfo@]host[:port][/path][?query][#fragment]

- scheme: 프토토콜
  - 리소스에 접근하는 데 사용되는 통신 프로토콜을 지정
  - http: 80 포트, https: 443 포트 주로 사용하며, 생략 가능
  - https는 http에 보안을 추가한 것으로 요즘 대부분의 웹사이트는 https로 동작
- userinfo: 사용자 정보
  - URL에 사용자 정보를 포함해서 인증(거의 사용 안함)
- host : 호스트 명
  - 리소스가 위치한 서버의 도메인 이름이나 IP 주소를 지정
- port: 포트번호
  - 서버에서 리소스에 접근하기 위해 사용되는 포트 번호(일반적으로 생략)
- path: 경로
  - 서버에서 리소스의 경로를 지정
- query: 쿼리 파라미터
  - 추가 매개 변수를 서버로 전달하는 데 사용
  - 검색어를 서버에 전달하거나 필터를 적용할 때 사용
  - key-value 형태
  - ?로 시작, &로 추가
- fragment

  - html 내부 북마크 등에 사용(서버에 전송하는 정보 아님)

- 예시 URL: https://www.example.com:8080/path/to/resource?query=example#section2

<br/>

**URN(Uniform Resource Name)**

- 리소스를 고유하게 식별하는 데 사용되는 이름
- 리소스의 위치나 현재 상태와 관련이 없으며, 리소스가 이동하더라도 그 이름은 유지됨.
- 예시 URN: urn:isbn:0451450523

<br/>
<br/>
<br/>

#### 웹 브라우저 요청 흐름

- 웹 브라우저 = 클라이언트
- 웹 서버 = 서버

<br/>

**웹 브라우저에 URL 입력**

- https://www.google.com/search?q=hello&hI=ko

<br/>

**클라이언트에서 서버로 패킷(헤더+데이터) 전달**

1. HTTP 요청 메시지 생성: 웹 브라우저에 입력된 URL로 부터 IP, PORT 정보를 얻고, 웹 브라우저가 HTTP 요청 메시지를 생성
2. 웹 서버와 연결: 애플리케이션 계층의 소켓 라이브러리를 통해 IP, PORT 정보를 헤더 부분에 담아 연결을 위한 패킷을 만들고 3-way-handshake 로 웹 서버와 연결
3. 소켓 라이브러리를 통해 HTTP 메시지를 TCP/IP 계층으로 전달한다.
4. TCP/IP 패킷(헤더 + 데이터) 생성: 헤더 부분(출발지 IP, PORT, 목적지 IP, PORT 등)과 데이터 부분(HTTP 요청 메시지)을 합쳐 TCP/IP 패킷을 생성
5. 웹 서버 패킷 전달: 웹 브라우저에서 인터넷망(수많은 노드들)을 거쳐 웹 서버로 패킷을 전달

<br/>

**서버에서 클라이언트로 응답 패킷 전달**

1. 데이터 부분 해석하여 HTTP 응답 메시지 생성: 웹 서버는 도착한 패킷의 헤더 부분은 버리고 데이터 부분(HTTP 요청 메시지)를 해석하여 HTTP 응답 메시지 생성
2. TCP/IP 응답 패킷(헤더 + 데이터) 생성: 헤더 부분(출발지 IP, PORT, 목적지 IP, PORT 등)과 데이터 부분(HTTP 응답 메시지)을 합쳐 TCP/IP 응답 패킷 생성
3. 웹 서버에서 인터넷망(수많은 노드들)을 거쳐 웹 브라우저로 응답 패킷 전달

<br/>

**웹 브라우저는 응답 메시지의 데이터 렌더링하여 화면에 표시**

- 웹 브라우저는 도착한 응답 패킷의 헤더 부분은 버리고 HTTP 응답 메시지의 데이터(HTML)을 렌더링하여 화면에 뿌려줌.

<br/><br/><br/>
**_개인적인 이해를 바탕으로 작성한 글 입니다. <br/>
잘못된 내용, 피드백은 언제든 환영합니다!_** 🥺🥺🥺
<br/><br/><br/>

### 참고 사이트

<hr>
[[모든 개발자를 위한 HTTP 웹 기본 지식] 02. URI와 웹 브라우저 요청 흐름 - URI, 웹 브라우저 요청 흐름](https://hseungyeon.tistory.com/427)
<br/>
[URI랑 URL 차이점이 뭔데?](https://www.charlezz.com/?p=44767)
<br/>
