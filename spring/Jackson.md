# Jackson
https://mommoo.tistory.com/83
## Json Library?
- json : 데이터 구조를 표현하는 파일형식
- 웹 개발에서 일반 html 형식 파일 사용하는 경우 외에 데이터 전달만을 목적으로 할 때 json 사용
- 웹 개발 외에도 데이터 전달을 위해 사용된다.
  - 데이터 분석에서 파일 입출력 등
- 비슷한 기능을 하는 xml 형식이 있으나 더 가벼운 json을 요즘 많이 사용함
- 개발 시 전달할 정보(클래스 등)를 json형식으로 변환해주는 라이브러리
 
## Why Jackson
> google의 GSON이나, SimpleJSON 과 같은 라이브러리도 있는데, 이에 반해 Jackson이 가지는 이점은?
> 
> Spring과 친하다
> 고용량의 Json 데이터를 처리할 때 성능이 gson보다 좋다
> non-blocking 환경에서도 encode/ decode(parsing)를 지원해준다.(gson은 지원 X) -> WebFlux에서 활용 가능

- 다른 라이브러리들처럼, ObjectMapper API를 사용하고 객체에 데이터 셋팅해줘야 하는 건 똑같으나
- Spring 3.0 이후 Jackson과 관련된 API를 제공하여 *자동화 처리*가 가능해짐

## 동작 원리

- Spring 3.0 이후, 컨트롤러 리턴이 @RequestBody라면 Spring은 MessageConverter API를 통해 컨트롤러가 리턴하는 객체를 후킹할 수 있습니다.
- Jackson은 JSON 데이터를 **출력**하기 위한 **MappingJacksonHttpMessageConverter**를 제공합니다.
- 만약 우리가 이를 spring의 Message Converter로 등록한다면,
- 컨트롤러가 리턴하는 객체를 다시 뜯어(자바의 리플렉션)
- Jackson의 ObjectMapper API로 JSON 객체를 만들고 난 후, 출력하여 JSON 데이터를 완성합니다.
- 이 과정이 3.0 이후 자동화 되어 있습니다.

## Jackson 활용법
### Property 기준으로 동작한다.
- Java에는 Property를 제공하는 문법은 없으나 이를 대신해 보통 getter와 setter로 대신합니다.
- *즉 Jackson을 쓴다면 Getter에 신경써야 함 !*
  - Lombok과 같이 사용할 때 명명 규칙에 따라 의도치 않은 버그가 생길 수 있음
    ex) Java와 Lombok의 네이밍 규약이 달라 생기는 버그(https://velog.io/@cateto/java-spring-boot-lombok-%EC%82%AC%EC%9A%A9-%EC%8B%9C-%EC%A3%BC%EC%9D%98%EC%A0%90)

### Getter 기준이 아닌 멤버변수 기준으로 데이터 매핑을 하고 싶다면?
**1. @JsonProperty**
- 예시
~~~
Public class Person{
  @JsonProperty("name")
  private String myName;
}
~~~ 
**2. Data Mapping 규칙 변경하기**
- **@JsonAutoDetect**를 이용하면 일일이 **@JsonProperty**를 붙이지 않아도 됩니다.
- https://fasterxml.github.io/jackson-annotations/javadoc/2.9/com/fasterxml/jackson/annotation/JsonAutoDetect.Visibility.html
- 예시
~~~
@JsonAutoDetect(fieldVisibility=JsonAutoDetect.Visibility.ANY)
public class Person{
  private String myName;
}
~~~ 

### 특정 변수/ 프로퍼티를 제외하고 싶다면?
**1. @JsonIgnore**
~~~
public class Person{
  private String name;
  private String job;
  @JsonIgnore
  public String getJob(){
    return this.job;
  }
}
~~~

> 혹은, 위와 마찬가지로 규칙을 변경해도 됩니다.

**2. Data Mapping 규칙 변경하기**
~~~
@JsonAutoDetect(fieldVisibility = JsonAutoDetect.Visibility.ANY, getterVisibility = JsonAutoDetect.Visibility.NON_PRIVATE)
public class Person {
  private String myName = "Mommoo";       
  public String getJob() {        
    return "Developer";    
  }
}
~~~

### 데이터 상태에 따라 포함/ 제외하고 싶다면?
**@JsonInclude**
~~~
@JsonInclude(JsonInclude.Include.NON_NULL)
public class Person {    
  private String myName = "Mommoo";        
  public String getJob() {        
    return "Developer";    
  }
}
~~~
- 아래와 같이 특정 프로퍼티/ 변수에만 따로 적용할 수도 있습니다.
~~~
​public class Person {    
  private String myName = "Mommoo";
  
  @JsonInclude(JsonInclude.Include.NON_NULL)    
  public String getJob() {        
    return "Developer";    
  }
}
~~~