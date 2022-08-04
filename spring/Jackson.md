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
> **"Spring과 친하다"**

- 다른 라이브러리들처럼, ObjectMapper API를 사용하고 객체에 데이터 셋팅해줘야 하는 건 똑같으나
- Spring 3.0 이후 Jackson과 관련된 API를 제공하여 *자동화 처리*가 가능해짐

## 동작 원리

- Spring 3.0 이후, 컨트롤러 리턴이 @RequestBody라면 Spring은 MessageConverter API를 통해 컨트롤러가 리턴하는 객체를 후킹할 수 있습니다.
- 
