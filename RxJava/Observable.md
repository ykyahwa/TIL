#Observable
 * 현재는 관찰되지 않았지만 앞으로 관찰될 가능성을 의미
 * Rx1 Observable -> Rx2 Observable, Maybe, Flowable 클래스로 구분됨
   * Maybe - 데이터 발생될수 있거나, 발행되지 않고도 완료 되는 경우 (reduce(), firstElement()
   * Flowable - Observable 발행 속도 > 구독 속도 일때 발생하는 배압 이슈 대응 하는 기능 추가
 * Observable 알림
   * onNext - 데이터 발행 알림
   * onComplete - 데이터 발행 완료 알림. 한번만 발생. 이후 onNext 발생해선 안됨
   * onError - 에러 발생 알림. 이후 onNext, onComplete 발생하지 않음. Observable 종료


## just()
 * 인자로 넣은 데이터 차례로 발행하는 Observable 생성
 * 최대 10개
 <img src="http://reactivex.io/documentation/operators/images/just.c.png"/>
'''java
public static <T> Observable<T> just(T item)
public static <T> Observable<T> just(T item1, T item2)
'''

'''Java
  public void emit() {
    Observable.just("A","B","C")
    .subscribe(System.out::println);
  }

  실행결과
  A
  B
  C
'''
 * subscribe() 호출해야 데이터 발행
   * subscribe() 는 Disposable 인터페이스 리턴
   * Disposable - void dispose(), boolean isDisposed()

## create()
  * onNext(), onComplete(), onError() 직접 호출해 줘야함
<img src="http://reactivex.io/documentation/operators/images/create.c.png"/>
'''java
Observable<T> create(ObservableOnSubscribe<T> source)
'''
'''java
Observable<String> observable = Observable.create(
  (ObservableEmitter<String> emitter) -> {
    emitter.onNext("A");
    emitter.onNext("B");
    emitter.onNext("C");
    emitter.onComplete();
  });
  observable.subscribe(System.out::println);

  실행결과
  A
  B
  C
'''
## fromArray()
'''Java
 String[] array = {"A","B","C"};
 Observable<String> observable = Observable.fromArray(array);
 observable.subscribe(System.out::println);

 실행결과
 A
 B
 C
'''
 * int[]로 입력하면 안됨. Integer[]사용

## fromIterator()
  * Iterator<E> - ArrayList, ArrayBlockingQueue, HashSet, LinkedList, Vector 등
'''Java
List<String> lang = new ArrayList<>();
names.add("Java");
names.add("Kotlin");
names.add("C++");

Observable<String> observable = Observable.fromIterator(lang);
observable.subscribe(System.out:println);

실행결과
Java
Kotlin
C++
'''

## fromCallable()
 * Callable - Executor 인자로 활용 되어 다른 스레드에서 실행 될수 있음
'''Java
Callable<String> callable = () -> {
  Thread.sleep(1000);
  return "Hello";
}

Observable<String> observable = Observable.fromCallable(callable);
observable.subscribe(System.out::println);

샐행결과
Hello
'''

## fromFuture()
 * Executor 에서 Future 반환. get() 호출하면 Callable 에서 구현한 결과 나올때 까지 블록킹
 * Executor - newSingleThreadExecutor, FixedThreadPool, CachedThreadPool 지원
 '''Java
 Future<String> future = Executors.newSingleThreadExecutor().submit(() -> {
   Thread.sleep(1000);
   return "Hello";
   });
 Observable<String> observable = Observable.fromFuture(future);
 observable.subscribe(System.out::println);

 실행결과
 Hello
 '''
