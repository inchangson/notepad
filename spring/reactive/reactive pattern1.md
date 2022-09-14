# Reactive Programming Patterns

[https://phantasmicmeans.tistory.com/entry/Observer-Iterator-Reactive-Stream]

# Iterable Pattern
- 컬렉션 내부 구조를 노출시키지 않으면서 컬렉션에 들어있는 모든 항목에 접근할 수 있도록 해주는 패턴
- 컬렉션마다 모든 항목에 접근하기 위한 방식이 다르다 
  - 이를 알려줄 무언가 필요하다 = iterator
- 따라서 Iterator를 이용하면 client는 내부 구조와 데이터의 수와 같은 정보 없이 모든 항목을 순회할 수 있다
  - 컬렉션은 Iterator를 반환하는 메서드(ex. hasNext, next, ...)를 포함하는 Iterable Interface를 구현
~~~
Iterator<Integer> iter = List.of(1, 2, 3).iterator();
while(iter.hasNext()){
    // iterator pattern은 pulling 방식으로 동작
    // 즉 데이터를 client가 원하는 시점에 가지고 온다
    System.out.println(iter.next());
}
~~~

# Observer Pattern
- 상태가 업데이트 되면 관찰하고 있던 옵저버들에게 바뀐 상태를 알려주는 패턴
- Reactive Programming은 특정 데이터나 이벤트에 반응하는 프로그래밍 방식이기 때문에 Observer Pattern이 기반이 되는 중요한 패턴이다
- 직접 구현하기엔 오류처리, 비동기 실행, 스레드 안정성 처리가 어렵기 때문에 Java에서 제공하는 Observable/ Observer(현재는 Deprecated)를 주로 사용한다
- Subjecter와 Observer가 서로를 인지하기 때문에 결합도가 높다
- 데이터 제공을 마치는 Complete나 예외 발생을 Observer에게 알릴 수단이 없다
  - 문자열을 통한 인위적인 전달은 가능하다
  - Pub-Sub이라는 이러한 단점이 보완된 패턴이 등장한다
- Observer가 Client 입장이고 Subject에 register만 해두면 Subject가 상태 변경 시 Observer들의 update 호출한다
  - 즉 Iterator pattern의 next() 호출 = Observer pattern의 update()
- Observer 입장에서는 가만히 있다가 데이터를 받는 Pushing 방식으로 동작한다

|Subject|
|---|
|register(observer o)|
|unregister(observer o)|
|notify()|
✓ subject는 관찰하고자 하는 상태가 있는 쪽에서 사용하는 인터페이스
✓ 상태가 바뀌면 notify()를 통해 update()를 호출한다

▽

|Observer|
|---|
|update()|
✓ 상태가 변경되는 걸 지켜보다가 변경되면 실행할 내용을 update에 작성


# Observer + Iterator = Reactive Stream(RxJava- ReactiveX)
## Observer 단점
- 데이터 스트림의 끝을 알리는 기능이 없고
- 에러 전파 매커니즘 또한 없다
- consumer가 준비 되기 전 producer가 이벤트 생성하는 것 또한 좋지 않다


## Reactive Stream
- Iterator로 끝을 나타내고 Observer pattern의 async한 이벤트 실행을 결합하면 reactive stream 구현이 가능하다
~~~
public interface RxObserver<T>{
    void onNext(T next);
    void onComplete();
    void onError(Exception e);
}
~~~

### Reactive stream
- netflix, vmware, redhat, twitter
- async + non-blocking, backpressure

- subscription 과 subscriber 는 1:1?
- processor