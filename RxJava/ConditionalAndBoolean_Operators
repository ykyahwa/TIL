# 조건 연산자
  * Observable의 흐름 제어
  > amb(), takeUntil(other), sipUntil(other), all()

## amb()
  * ambiguous - 모호한
  * 여러 Observable 중 가장 먼저 데이터 발행한 Observable 1개 선택, 다른 Observable 에서 발행하는 데이터 무시

```java
@SchedulerSupport(SchedulerSupport.NONE)
public static <T> Observable<T> amb(Iterable<? extends ObservableSource<? extneds T>> sources)
```

```java
String[] data1 = {"가","나","다"};
String[] data2 = {"A","B","C"};

List<Observable<String>> observables = Arrays.asList(
  Observable.fromArray(data1)
    .doOnComplete(() -> Log.d("Observable #1 : onComplete()")),
    Observable.fromArray(data2)
      .delay(100L, TimeUnit.MILLISECONDS)
      .doOnComplete(() -> Log.d("Observable #2 : onComplete()"))
);

Observable.amb(observables)
  .doOnComplete(() -> Log.d("Result : onComplete()"))
  .subscribe(Log::i);

실행결과
가
나
다
Observable #1 : onComplete()
Result : onComplete()
```

## takeUntil()
  * 인자로 받은 Observable 이 값을 발행하면 현재 Observable은 데이터 발생 중단하고 onComplete() 이벤트 발생 시킨다.

```java
@SchedulerSupport(SchedulerSupport.NONE)
public final <U> Observable<T> takeUntil(ObservableSource<U> other)
```

```java
String[] data = {"A","B","C","D","E"};

Observable<String> observable = Observable.fromArray(data)
  .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS), (val, notUsed) -> val)
  .takeUntil(Observable.timer(500L, TimeUnit.MILLISECONDS));

observable.subscribe(Log::i);

실행결과
A
B
C
D
```

## skipUntil()
  * other Observable 가 데이터를 받을 때까지 값을 건너 띔

```java
String[] data = {"A","B","C","D","E","F"};

Observable<String> observable = Observable.fromArray(data)
  .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS), (val, notUsed) -> val)
  .skipUntil(Observable.timer(500L, TimeUnit.MILLISECONDS));

observable.subscribe(Log::i);

실행결과
E
F
```

## all()
  * 조건에 맞는 데이터 발행할 때만 true, 맞지 않는 데이터 발행 시 false 반환

```java
@SchedulerSupport(SchedulerSupport.NONE)
public final Single<Boolean> all(Predicate<? super T> Predicate)
```

```java
int[] data = {1,2,3,4};

Single<Boolean> observable = Observable.fromArray(data)
  .map(num -> num * 2)
  .all(val -> val < 10)
observable.subscribe(Log::i);

실행결과
true
```
