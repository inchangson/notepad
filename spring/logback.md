# Logback
> https://logback.qos.ch/
> https://tecoble.techcourse.co.kr/post/2020-07-30-use-logger/

## 사용하는 이유
- System.out.println() 의 성능 저하
  - I/O operation이라 synchronized가 되어 있어 성능 저하가 일어난다.
- 상세한 로그 설정
  - 로그 파일 및 메시지 형식
  - 로그 레벨 지정

## 설정
1. logback.xml 로 설정한다.
2. 단, spring boot의 경우 logback-spring.xml을 사용한다.
  - spring boot의 경우 logback.xml을 쓰면 spring boot 설정 전에 logback이 설정되어 로그 제어가 어려워질 수 있다.
  - 사실 properties 설정 파일(application.properties or application.yml)에서 logging.config=classpath:logback-${spring.profiles.active}.xml으로 따로 설정 가능하다.
  - 간단한 설정은 properties 파일로도 가능하다.
3. appender/ logger/ layout으로 크게 나뉜다.
   - appender: 로그를 남기는 형식
   - logger: 해당 로거가 사용될 패키지와 로그 레벨 설정
4. 로그 레벨은 TRACE/ DEBUG/ INFO/ WARN/ ERROR 순으로 기록된다.


# 설정 태그