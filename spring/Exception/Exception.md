# Exception

> 아래 4개 아티클 내용을 정리한 문서입니다.

> https://tecoble.techcourse.co.kr/post/2021-10-25-spring-exceptioin-handle/

> https://tecoble.techcourse.co.kr/post/2021-05-10-controller_advice_exception_handler/

> https://tecoble.techcourse.co.kr/post/2020-08-17-custom-exception/

> https://tecoble.techcourse.co.kr/post/2020-07-28-global-exception-handler/

## Custom exception을 언제 써야할까?
### Custom exception의 장점
1. 이름만 봐도 어떤 경우에 대한 예외인지 쉽게 알 수 있다.
  - 추후에 나오겠지만 단순히 이런 장점을 위해 사용하는 것은 과한 구현이다.
2.  error가 발생하는 지점이나 catch하는 지점에서 따로 exception을 구분해서 처리해줄 수 있다.
    - 여기서 처리의 의미는 추가로 에러에 관한 정보(exception이 발생한 메소드에 어떤 파라미터로 들어왔는지)를 넣는 경우 혹은
    - 여러 타입의 exception이 발생할 수 있는 코드에서 한 가지 exception type으로 묶어서 처리하는 경우
    - 를 의미한다.
3. 또한 단순히 메시지를 던져주더라도, 더 상세한 메시지 정보를 일관성 있게 줄 수 있다.
     - 예시) 배열 범위 밖의 인덱스를 사용했을 때
  - 표준 예외만 사용하면 상세한 메시지 정보를 주기 위해 다음과 같이 사용해야 한다.
  ~~~
  throw newIndexOutOfBoundsException("범위를 벗어났습니다." + "size: " + myList.size() + " index: " + idx);
  ~~~
  - 하지만 custom exception을 사용하면 생성자에서 편하고, 안전하게 생성할 수 있다.
  - (표준 예외의 위의 경우와 같이 코드를 짜면 같은 종류의 에러 메시지를 여러 곳에 작성했을 때 메시지 형태가 바뀌면 일일이 바꿔줘야하고 오타의 위험도 있다)
    ~~~
    public class MyIllegalIndexException extends IndexOutOfBoundsException{
      private static final String message = "범위를 벗어났습니다."
      public IllegalIndexException(List<?> target, int index){
        super(message + " size: " + target.size() + " index: " + index);
      }
    }
    ~~~
4. 예외 발생 후처리가 용이하다.
  - ControllerAdvice를 통해 전역적인 예외처리가 가능하다.
  - 재사용성이 높다는 표준예외의 장점이 되려 단점이 될 수 있다.
    - 정확히 어느 위치에서 예외가 발생하는 지 파악하기 어렵기 때문이다.

5. 예외 생성 비용을 절감한다.
  - 예외 생성 비용 = stack trace
    - stack trace : 예외 발생 시 call stack에 있는 메소드 리스트를 저장하고
    이를 통해 예외가 발생한 정확한 위치를 알 수 있다.
  - try/catch나 Advice를 통해 예외를 처리하면 해당 예외의 stack trace는 사용하지 않을 때가 만다. 
  - stack trace의 생성은 부모 클래스 중 Throwable의 fillInStackTrace() 메소드를 통해 이루어지는데 이를 Override함으로 stack trace의 생성 비용을 줄일 수 있다.
  - 즉 단순한 메시지만 던져주는 경우에도 fillInStackTrace()의 Override를 통한 성능 향상이 있을 수 있다.
   
### 웬만하면 안 써도 된다(표준 예외를 적극 활용하자)
#### 예시 : 사용자 이름이 비어있는 경우
- API에서 사용자 이름이 필요하나, 해당 값이 넘겨주지 않은 경우, 즉 클라이언트의 잘못된 입력으로 예외가 생기는 경우다.
- 이를 위해 굳이 UserNameEmptyException이라고 custom exception을 만들기 보다
- 이는 유효하지 않은 입력(인자)값에 대한 예외이므로
- IllegalArgumentException이란 표준 예외를 사용하고
- 메시지에만 예외사항을 재정의해주면 된다.



## 표준 예외
> https://docs.oracle.com/javase/8/docs/api/?java/lang/RuntimeException.html

### IllegalArgumentException
### IllegalStateException
### UnsupportedOperationExcpetion

## Spring Boot에서 예외 처리하는 방법
1. Controller에서 ExceptionHandler 정의
2. ControllerAdvice에 ExceptionHandler 정의
3. Exception에 ResponseStatus 어노테이션 추가

### Controller에서 ExceptionHandler 정의


### ControllerAdvice에 ExceptionHandler 정의


### Exception에 ResponseStatus 어노테이션 추가
