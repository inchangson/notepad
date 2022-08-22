# spring5 reactive(web flux)

# 출처
[https://taes-k.github.io/2019/05/21/about-spring-reactive/]

# Reactive 프로그래밍
## Reactive 프로그래밍이란?(위키피디아)
- 데이터 스트림이나 네트워크(변화의 전달)에 집중한 declarative programming(선언적 프로그래밍) 패러다임이다.
- 정적/ 동적 데이터 스트림을 쉽게 표현할 수 있고
- 통신할 수 있다.
- 연계된 실행 모델와 함께 inferred dependency가 존재하는 상황에서
- 데이터 플로우의 변화를 자동으로 전파하기 용이한

## Reactive 프로그래밍의 목적
- 리소스의 효율적 사용
  - 멀티 스레드를 통한 병렬처리를 한 기존 방식
  - cpu, 메모리 제한으로 인한 어려움
  - non-blocking 은 일반적인 스레드를 통한 비동기 작업과는 다르게, 스레드 점유하지 않고 작업을 수행하여 하나의 스레드 내에서 동시에 많은 작업 수행 가능
    - 너무 많은 작업 몰리면 안 되기 때문에 back-press를 통해 요청 횟수 제한
    - 이를 통한 고가용성 확보
  - 고가용성?
    - non-blocking을 통해 어플리케이션의 실행속도가 개선된다는 의미보단 더 적은 고정된 수의 스레드와 메모리로 최대의 효율을 내며 확장할 수 있다는 것이 특징
# Spring WebFlux
## Spring MVC와 Spring WebFlux
- 가장 큰 차이점
  - mvc 에서는 어플리케이션이 스레드를 차단할 수 있다는 가정하에 가장 큰 스레드풀을 가지고 있고
  - web flux는 스레드가 차단되지 않아 작은 스레드풀을 사용하여 request를 처리


|spring mvc|both|spring webflux|
|---|---|---|
|Imperative logic|@controller|Fuctional endpoints|
|simple to write and debug|reactive clients|event loop concurrency model|
|JDBC, JPA, blocing devs|Tomcat, Jetty, Undertow|Netty|

### WebFlux 도입 고려 사항
- non-blocking web을 운영 중이면 Spring WebFlux 도입이 도움이 된다
- Java8 lambda, kotlin과 함꼐 사용하기 위한 가볍고 기능적인 웹프레임워크를 찾는 경우 적합
- MSA에서 MVC, WebFlux 모두 사용 가능하다
- JPA, JDBC 같은 blocking 기반 지속성 api를 사용하거나 네트워크 api 를 사용하는 경우 Spring MVC가 좋다
- Spring MVC에서 원격 서비스를 사용하는 경우 reactivce webclient를 사용해보는 것도 좋다

## Spring Webflux
- HttpHandler : non-blocking I/O 와 Reactive Stream을 통한 HTTP Request 처리를 위한 기본 규약으로 Reactor Netty, Undertow, Tomcat, Jetty, and any Servlet 3.1+ container가 포함되어 있음
- WebHandler API : request process를 위한 좀더 높은수준의 범용 웹 API


