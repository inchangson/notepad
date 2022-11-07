# HTTP(Hyper Text Transfer Protocol)
- 문서 간 링크로 이동할 수 있는 Hyper Text를 통신하기 위한 프로토콜(유래)


## 모든 것이 http
- HTML, TEXT
- IMAGE, VOICE, VIDEO, FILE
- JSON, XML(API)
- 등 거의 모든 형태를 http로 
- 심지어 서버간 통신에도 활용

### 기반 프로토콜
- TCP : HTTP/1.1(현재), HTTP/2
- UDP : HTTP/3

### 확인하는 방법
- 개발자 도구
  - 오른쪽 버튼 > protocol 

### 특징
- 서버-클라이언트 구조
- 무상태 프로토콜 지향(stateless)
- 비연결성
- HTTP 메시지
- 단순함, 확장 가능

## 클라이언트-서버 구조(Request Response 구조)
- 클라이언트는 서버에 요청을 보내고 응답 대기
- 서버가 요청에 대한 결과를 만들어서 응답

### 의의
- 클라이언트와 서버가 분리된 것만으로도 의미가 있음
- 클라이언트와 서버가 독립적으로 진화할 수 있다
  - 사용성 향상이 필요하면 클라이언트만
  - 트랙픽이 는 것에 대해 대응하려면 서버만
- 서버는 데이터와 비즈니스 로직
- 클라이언트는 UI 사용성

## Stateful, Stateless

### Stateless
- 서버가 클라이언트 상태를 보존하지 않는다
- 장점 : 서버 확장성이 높음(스케일 아웃)
- 단점 : 클라이언트가 추가 데이터 전송

### 예시(Stateful)
- 고객 : 이 노트북은 얼마인가요?
- 점원A : 100만원 입니다.
- 고객 : 2개 주문할게요
- 점원A : 200만원입니다. 신용카드, 현금 중에 어떻게 구매하시겠어요?
- 고객 : 신용카드로 하겠습니다.
- 점원A : 200만원 결제 완료되었습니다.

### 예시(Stateless)
- 고객 : 이 노트북은 얼마인가요?
- 점원A : 100만원 입니다.
- 고객 : 2개 주문할게요
- 점원B : 어떤 거 말씀이세요?
- 고객 : 신용카드로 하겠습니다.
- 점원C : 무슨 제품을 몇개 구매하시겠어요?

### 예시(Stateless)
- 고객 : 이 노트북은 얼마인가요?
- 점원A : 100만원 입니다.
- 고객 : 노트북 2개 주문할게요
- 점원B : 200만원입니다. 신용카드, 현금 중에 어떻게 구매하시겠어요?
- 고객 : 노트북 2개를 신용카드로 하겠습니다.
- 점원C : 200만원 결제 완료되었습니다.

### Stateless 장점
- 즉 점원이 바뀌어도 괜찮다.
- 이는 곧 확장이 쉬워진다는 의미
  - 트래픽이 갑자기 늘어도(고객이 갑자기 많아져도) 마트에선 점원만 충분히 어느 순간이든 고용하면, 늘어난 고객에 대해 대응이 가능하다(응답 서버를 마음대로 바꿀 수 있다)
  - 갑자기 서버 하나가 뻗어도 대응 가능

### 한계
- 모든 것을 무상태로 설계할 수는 없다
- 무상태
  - 예) 로그인이 필요없는 단순한 서비스 소개화면
- 상태 유지 필요
  - 예) 로그인
- 로그인한 사용자의 경우 로그인했다는 상태를 서버에 유지
- 일반적으로 브라우저 쿠키와 서버 세션등을 사용해서 상태 유지
- 상태 유지는 최소한만 사용
- 데이터가 너무 많이 왔다갔다 한다

## Connectionless(비연결성)
- 요청 주고 받을 때만 TCP 연결하고
- 일 다 봤으면 연결 끊음
- ex 1시간 동안 수천명이 서비스 사용해도 실제 서버에서 동시에 처리하는 요청은 수십 개 이하로 매우 작음(웹 브라우저에서 계속 연속해서 검색버튼을 누를 일은 거의 없다)
- 서버 자원을 매우 효율적으로 사용할 수 있음

### 한계와 극복
- 3way handshake 시간 추가
- 웹브라우저로 사이트를 요청하면 HTML뿐만 아니라 javascript, css, 추가 이미지등 수많은 자원이 함께 다운로드됨
- 지금은 HTTP Persistent Connections(지속 연결, 과거 keep-alive)로 문제 해결
  - HTTP/2, HTTP/3에서 더 많은 최적화
  - HTTP3는 UDP 프로토콜을 통해 연결 속도 또한 빠르게 만듦

### Persistent Connection 없는 경우
- 연결(0.1초)
- 요청/html 응답(0.1초)
- 종료(0.1초)
- 연결(0.1초)
- 요청/자바스크립트 응답(0.1초)
- 종료(0.1초)
- 연결(0.1초)
- 요청/이미지 응답(0.1초)
- 종료(0.1초)
- 전체 0.9초 소요

### Persistent Connection 있는 경우
- 연결(0.1초)
- 요청/html 응답(0.1초)
- 요청/자바스크립트 응답(0.1초)
- 요청/이미지 응답(0.1초)
- 종료(0.1초)
- 전체 0.5초 소요

### Stateless를 기억하자
- 같은 시간에 맞추어 발생하는 대용량 트래픽
  - 선착순 이벤트, 명절 KTX 예약, 학과 수업 등록
- 처음엔 간단한 html 페이지를 줘서 사용자별로 시간 차이가 생길 수 있도록 만드는 트릭으로 일정부분 해소
- 어쨋든 최대한 Stateless하게 설계해야지 대응하기 수월하다!

## http message

### http message 구조
1. start line
2. header
3. empty line(CRLF) : 무조건 있어야 함
4. message body

### ex1 요청 메시지
1. startline : GET/search?q=hello&hl=ko HTTP/1.1
2. header : www.google.com
3. empty line : 
- 요청 메시지도 물론 body 메시지 가질 수 있음

### ex1 요청 메시지
1. startline : HTTP/1.1 200 OK
2. header : Content-Type: text/html;charset=UTF-8 Content-Lengh: 3423
3. empty line : 
4. message body
~~~
<html> 
    <body>...</body>
</html>
~~~

### start line(요청 시)
- **request line**/ status line 으로 구성
- request-line = method SP(공백) request-target SP HTTP-version CRLF
- request-target = path

#### HTTP method
- GET : 리소스 조회
- POST : 요청내역 처리

#### start line(응답 시)
- request line/ **status line** 으로 구성
- status-line = HTTP-version SP status-code SP reason-phrase CRLF
- status-code
  - 200(success), 400(client request error), 500(internal server error)
- reason-phrase
  - 사람이 이해할 수 있는 짧은 상태 코드 설명 글
  - OK

### header
- header-field = field-name ":" OWS field-value OWS
  - OWS : 띄어쓰기 허용
- field-name은 대소문자 구분 없음

#### header의 용도
- HTTP 전송에 필요한 모든 부가 정보
  - 메시지 바디의 종류(html? xml?...중 뭔지), 크기, 압축, 인증, 요청 클라이언트(브라우저) 정보, 서버 어플리케이션 정보, 캐시 관리 정보
- 표준 헤더가 너무 많음
- 필요 시 임의의 헤더 추가 가능

### empty line(CRLF) : 무조건 있어야 함

### message body
- 실제 전송할 데이터