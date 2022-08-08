# HashMap

> https://d2.naver.com/helloworld/831311

- Java Collections Framework에 속한 구현체 클래스(Java 2 ~)
- Map interface는 Java 5에서 Generic이 적용된 정도의 변화 외엔 현재까지도 그대로
- 단 HashMap은 성능 향상을 위해 지속적인 변화 있었음
  - amortized constant time을 위하여 hash collision 가능성을 줄인 과정

## HashMap과 HashTable
- HashTable(JDK 1.0 ~ , Java API) HashMap(Java2 ~, Java Collection Framework)
 - HashTable extends Dictionary<K, V> implements Map<K, V> Clonable, Serializable
 - HashMap extends AbstractMap<K, V> implements Map<K, V> Cloneable, Serializable
- 둘 모두 Map interface를 상속 받음, 즉 기능의 차이는 없는 셈
  - 되려 지금의 HashTable의 역할은 해시 맵 자체의 기능이라기보다 JRE 1.0, 1.1 환경의 하위 호환성을 제공함에 있기 때문에 둘 간의 기능/ 성능 비교는 무의미하다 볼 수 있다
- 대신 HashMap은 보조 해시 함수(Additional Hash Function)를 사용
  - 이 덕분에 해시 충돌이 덜 일어나, 성능이 더 좋다

## HashMap(HashTable, Map, Dictionary, Symbol Table)의 정의
- 키에 대한 해시 값을 사용하여 값을 저장하고 조회하며 키-값 쌍의 개수에 따라 동적으로 크기가 증가하는 associate array
- 
