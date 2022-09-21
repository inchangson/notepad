# @Valid 와 @Validated
[출처: https://mangkyu.tistory.com/174]

## @Valid
- Bean Validator를 이용해 객체의 제약 조건을 검증하도록 지시하는 어노테이션

### Spring에서의 동작
- 일종의 **어댑터?**인 LocalValidatorFactoryBean가 제약조건 검증을 처리
- 즉 LocalValidatorFactoryBean을 등록해주어야 사용할 수 있는데
- SpringBoot에서는 아래 코드를 통해 의존성만 추가해주면 된다

~~~
// https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-validation
implementation group: 'org.springframework.boot', name: 'spring-boot-starter-validation'
~~~

### 활용
- 각 클래스에 명시된 어노테이션들을 통해 칼럼의 값 유효성을 검증한다.
  