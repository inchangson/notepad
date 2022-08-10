# URI(Uniform Resource Identifier)

## URI? URL? URN?
>"URI는 로케이터(Locator), 이름(Name) 또는 둘 다 추가로 분류될 수 있다."

- 즉 URI가 가장 상위 개념, 나머지를 포함한다.
- 대신 URN은 사용하기도, 보기도 힘들어서 거의 안 씀 -> url만 있다~

## 정의(URI)
- Uniform : 리소스를 식별하는 **통일된** 방식
- Resource : 자원, URI로 식별할 수 있는 모든 것(제한 없음)
- Identifier : 다른 항목과 구분하는데 필요한 정보

## URL VS URN
- URL : 리소스가 있는 위치를 지정
- URN : 리소스에 이름을 부여
- 위치는 변할 수 있지만 이름은 변하지 않는다.
- 하지만 이름만으로 실제 리소스를 찾을 수 있는 방법이 보편화 되지 않기 때문에 앞으로 강의에서 URL과 URI를 같은 의미로 사용

## URL 분석
> 대상 : https://www.google.com/search?q=hello&hl=ko

> 문법 : scheme://[userinfo@]host[:port][/path][?query][#fragment]

### scheme
- 주로 포로토콜 사용
- http, https, ftp
- http(80), https(443) 처럼 port가 명확하면 후에 port 부문 생략 가능
### userinfo
- url에 사용자 정보 포함해서 인증할 때
- 사용 안 함
### host
- 호스트명
- 도메인명, IP 주소를 직접 사용 가능
### port
- 일반적으로 생략 가능
- 접속 포트

### path
- 리소스 경로, 주로 계층적 구조로 되어 있다

### query
- key, value 형태
- ?로 시작
- &로 추가
- query parameter, query string 등으로 불림
### fragment
- html에서 내부 북마크 용도로 사용
- 실제 서버에 가는 정보는 아님

# 웹 브라우저 요청 흐름

## 