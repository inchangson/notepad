# @Valid 와 @Validated
[출처: https://mangkyu.tistory.com/174]

## @Valid
- Bean Validator를 이용해 객체의 제약 조건을 검증하도록 지시하는 어노테이션

### Spring에서의 동작
- 일종의 **어댑터?**인 LocalValidatorFactoryBean가 제약조건 검증을 처리
- 즉 LocalValidatorFactoryBean을 등록해주어야 사용할 수 있는데
- SpringBoot에서는 아래 코드를 통해 의존성만 추가해주면 된다

~~~
//https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-validation

implementation group: 'org.springframework.boot', name: 'spring-boot-starter-validation'
~~~

### 활용
- 각 클래스에 명시된 어노테이션들을 통해 칼럼의 값 유효성을 검증한다.
~~~
@Email
@NotBlank
@NotNull
@Min(12)
~~~

### 세부 동작원리
- (spring) 모든 요청은 Front Controller인 Dispatcher Servlet을 통해 Controller로 전달된다
- 전달 과정에서 컨트롤러 메서드 객체를 만들어주는 ArgumentResolver가 동작하는데
- @Valid 역시 ArgumentResolver에 의해 처리된다
- @RequestBody는 Json 객체로 변환하는 작업이 ArgumentResolver의 구현체인 RequestResponseProcessor가 처리
- Processor가 @Valid로 시작하는 어노테이션이 있는 경우에 유효성 검사를 진행
  - 때문에 @ValidTemp 식으로 적어도 동작한다
- 만약 @ModelAttribute를 사용중이라면 ModelAttributeMethodProcessor에 의해 @Valid가 처리된다
- 그리고 검증에 오류가 있으면 MethodArgumentNotValidException이 발생한다
- Dispatcher Servlet에 기본으로 등록된 Exception Resolver인 DefaultHandlerExceptionResolver에 의해 400 BadRequest 에러가 발생한다

## @Validated
- 위 동작원리 때문에, @Valid는 컨트롤러 레이어에서만 처리된다
- 그리고 값의 검증 또한 최대한 Controller에서 해주는 것이 좋다
- 하지만 개발 시 불가피하게 다른 곳에서 검증해야하는 경우도 있을 수 있다
- 이에, spring은 AOP기반으로 유효성 검증을 해주는 @Validated를 제공하고 있다
- 다만 **@Validated는 JSR 표준 기술이 아니며 Spring 프레임워크에서 제공하는 어노테이션 및 기능이다**

### 활용
- 사용할 클래스에 @Validated를 붙여주고
- 유효성 검증할 메서드 파라미터엔 @Valid를 붙여준다
- 그리고 예외 발생 시 MethodArgumentNotValidException이 아닌 ConstraintViolationException 예외가 발생한다

## @Validated의 또 다른 기능(유효성 검증 그룹 지정)
- 동일한 클래스에 대해 제약조건이 요청에 따라 달라질 때

### 활용
- 제약조건이 다른 검증그룹을 내용이 없는 마커 인터페이스로 지정한다
- 클래스를 정의할 때 각 컬럼에 따라 어느 제약조건에서 사용될 지를 지정한다
- 사용할 부분에서 어떤 검증그룹인지 명시한다
~~~
public interface CustomerValidationGroup{}
public interface AdminValidationGroup{}

public class User {
    @NotEmpty(groups = {CustomerValidationGroup.class, AdminValidationGroup.class})
    private String name;
    
    @NotEmpty(groups = CustomerValidationGroup.class)
    private String usrId;

    @NotEmpty(groups = AdminValidationGroup.class)
    private String adminCode;

}

@Controller
...
@PostMapping("/users")
public ResponseEntity<Void> addUser(
    @RequestBody @Validated(UserValidationGroup.class UserRequest req)
    ){
        ...
}
...
~~~

