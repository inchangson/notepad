# Reactive Patterns
[https://zorba91.tistory.com/291]

# pub-sub
- 객체(subject)가 변할 때마다 observer에게 정보를 알려주는(notify) observer pattern과 다르게 pub-sub구조는 발행된 메시지의 정해진 범주에 따라 해당 범주에 대한 구독을 신청한 수신자에게만 메시지를 전달한다
- 수신자는 발행자에 대한 지식이 없어도 원하는 메시지만 수신할 수 있다

# pub-sub vs observer
- 가장 큰 차이점 : pub(=subject)와 sub(observer)이 직접적으로 통신하지 않는다
  - subscription(broker, message broker, event bus)를 통해서 연결된다

## subscription이 메시지를 처리하는 방식?
- ..

# study
- future의 경우 get 함수있는 code line 에서 blocking 