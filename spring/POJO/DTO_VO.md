# DTO vs VO
> https://parkadd.tistory.com/53

## DTO
- Layer 간 데이터 교환을 위한 객체
- getter, setter 외 기능 가지지 않음
  - 단 정렬, 직렬화 등 데이터 표현을 위한 기능은 가능
- mutable(값을 유연하게 바꿀 수 있음)
- **데이터 캡슐화를 통해 유연한 대응 가능**

## VO
- Immutable
- 값이 같다면 동일한 객체
  - 즉 같은 값이면 같은 객체
    - 이를 위해 equals(), hashCode()를  override 해줘야함

## DTO vs VO
| DTO   |   VO  |
| :---: | :---: |
| mutable | immutable|
| Layer <-> Layer 에서 사용 | 모든 Layer 사용 |
| 필드가 같아도 다른 객체 | 필드만 같으면 같은 객체 |
| 데이터 접근 관련 기능만 | 특정 비즈니스 로직 또한 가질 수 있음|

