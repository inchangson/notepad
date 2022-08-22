# 정리1
[https://godekdls.github.io/Reactive%20Spring/springwebflux/]
[https://docs.spring.io/spring-framework/docs/5.2.6.RELEASE/spring-framework-reference/web-reactive.html]

## declarative programming vs imperative programming


# 정리2
[https://spring.io/reactive]
[https://appleg1226.tistory.com/16]
[https://spring.io/guides/gs/reactive-rest-service/]

## reactive processing?
- non-blocking, asynchronous 어플리케이션을 만들 수 있게 하는 패러다임이다.
- back-pressure(flow control)을 핸들링할 수 있다

## why reactive on spring?
- 현대 프로세서에 좀 더 적합하다
- back-pressure의 inclusion(포섭)은 decoupled component 간의 resilience(탄력성)을 증대시킨다
- 더 적은 리소스로 많은 데이터를 처리할 수 있다
- 따라서 같은 동접자 수도 적은 micro service instance로 관리할 수 있다
- MongoDB, Redis, Cassandra는 Spring Data를 통해서 반응형 기술을 제공하고
- 많은 관계형 db들이(postgres, MS SQL, MySQL, H2, Google Spanner) R2DBC를 통해서 반응형 기술을 제공한다.

## WebFlux?
- Java에는 RxJava, Reactor 등 Reactive Streams API를 구현한 라이브러리들이 있습니다
- spring에선 Reactive Streams API의 구현체 중 하나인 Reactor를 이용하여 Spring Webflux를 제공합니다
- 대부분 메서드 들을 **Mono**와 **Flux**로 반환해야 하며, 함수형 프로그래밍에 익숙해야 합니다
- 

## Tutorial
> spring webflux를 통한 hello world REST API 구현

### Initializr
- [https://starts.spring.io] 접속
- Dependencies에서 Spring Reactive Web 추가
- 생성

### POJO
- 전달할 데이터 형식을 갖춘 클래스 생성
  
### WebFlux Handler
- Spring Reactive 에서는 요청/ 응답 단계에서 handler를 무조건 통해야 한다.
- 즉 해당 POJO에 부합하는 Handler를 만들어준다
- 이 때 데이터를 가진 ServerResponse를 **Mono Object**에 담아야 한다 

### Router
- 데이터가 노출될 URL에만 route하는 router를 생성시켜야 한다
- 해당 라우터는 해당 경로의 모든 데이터 트래픽을 감지하고 reactive hanlder class를 통해 값을 반환한다

### Web Client
- Spring RestTemplate는 blocking 기반이기 때문에 반응형 웹을 만들기 위해선 spring에서 제공하는 WebClient 클래스를 따로 만들어야 한다
- 이 WebClient 객체는 mono를 사용하기 때문에 reactive의 특성을 지닌다
  - Mono는 getMessage를 통해 데이터를 hold하고
  - 이는 function API로, imperative하지 않다
- 또한 WebClient는 non-reactive의 애플리케이션(Spring MVC)와도 함께 쓸 수 있단 장점이 있다

# 정리3(with kotlin)
[https://spring.io/blog/2019/04/12/going-reactive-with-spring-coroutines-and-kotlin-flow]
