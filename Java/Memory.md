# Java의  메모리 관리
> https://dinfree.com/lecture/language/112_java_2.html#m4

## JVM
- JVM은 시스템(OS)로부터 메모리를 할당 받음
- 세 영역으로 나누어서 관리함

### Method area
- 클래스에 대한 정보, 클래스 변수나 static method
- 때문에 static method에서 멤버변수(Instance 변수)를 접근할 수 없는 이유가 메모리 영역이 다르기 때문

### Heap area
- Instance 메모리 공간

### 호출 스택(call stack or execution stack)
- method 실행에 필요한 메모리 공간

## Garbage collection
- 메모리를 정리해주는 작업
- 소규모나 컨테이너 기반에서 동작하는 웹서버(WAS), 안드로이드 등의 프로그램은 개발자가 직접 메모리 해제에 관심을 가질 필요 없다
- GC의 대상
  - 모든 객체 참조가 null인 경우
  - 객체가 블럭 안에서 생성되고 블럭이 종료된 경우
  - 부모객체가 null이 된 경우
  - 객체가 weak 참조만 가지고 있는 경우
  - 객체가 soft 참조지만 메모리부족이 발생한 경우

### 참조(Reference)

#### Strong reference
- new를 통해 생성할 때 발생
- GC 대상에서 제외
  
#### Soft reference
- GC에 수거될 수도 안 될 수도 있음
- 메모리가 충분하다면 수거되지 않음
- out of memory에 가까우면 수거될 확률이 높다
  
#### Weak reference
- 무조건 GC 발생 시 수거됨
- 즉 GC 시점과 사라지는 시점이 동일하여
- 짧은 주기에 자주 사용되는 객체를 캐시할 때 유용

## Java의 Heap 메모리 구조 및 GC 동작
### 메모리 구조
~~~
|Eden   |Survivor0|Old  |Permanent  |
|       |Survivor1|     |           |
~~~
### New/Young Generation
- Eden : 객체들이 최초로 생성되는 공간
- Survivor 0/1 : Eden에서 참조되는 객체들이 저장되는 공간

### Tenured Generation
- Old : New area에서 일정시간 참조되고 있는, 살아남은 객체들이 저장되는 공간
- Eden영역에 객체가 가득차게 되면 GC가 한 번 발생
- Eden에 있는 값들은 Survivor1 영역에 복사되고 이 영역을 제외한 나머지 영역의 객체들은 삭제됨

### Permanent Generation
- 생성된 객체들의 정보의 주소값이 저장된 공간
- class loader에 의해 load되는 class, method등에 대한 meta 데이터가 저장되는 영역