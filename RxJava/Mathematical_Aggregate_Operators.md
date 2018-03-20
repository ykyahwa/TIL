# 수학 및 기타 연산자

  * RxJava1 에 수학 함수 RxJavaMath가 있으나 RxJava2에서 사용 불가
  * RxJava2 에 [RxJava2Extensions]("https://github.com/akarnokd/RxJava2Extensions") 활용

```java
// Observable 에서 발행한 데이터 개수 발행
public final Single<Long> count()
public static <T extends Comparable<? super T>> Flowable<T> max(Publisher<T> source)
public static <T extends Comparable<? super T>> Flowable<T> min(Publisher<T> source)
public static Flowable<Integer> sumInt(Publisher<Integer> source)
public static Flowable<Double> averageDouble<Publisher<? extends Number> source>
```

```java
Integer[] data = {1,2,3,4,5};

Single<Long> observable = Observable.fromArray(data)
  .count();
observable.subscribe(count -> Log.i("count - " + count));

Flowable.fromArray(data)
  .to(MathFlowable::max)
  .subscribe(max -> Log.i("max - " + max);

Flowable.fromArray(data)
  .to(MathFlowable::min)
  .subscribe(max -> Log.i("min - " + mim);

Flowable<Integer> flowable = Flowable.fromArray(data)
  .to(MathFlowable::sumInt);
flowable.subscribe(sum -> Log.i("sum - " + sum));

Flowable<Integer> flowable2 = Flowable.fromArray(data)
  .toFlowable(BackpressureStrategy.BUFFER)
  .to(MathFlowable::averageDouble);
flowable2.subscribe(avg -> Log.i("avg - "+ avg));
```

## delay()
  * 인자로 받은 time 만큼 Observable 데이터 발행 지연

```java
@SchedulerSupport(SchedulerSupport.COMPUTATION)
public final Observable<T> delay(long delay, TimeUnit unit)
```

```java
String[] data = {"가", "나", "다"};
Observable<String> observable = Observable.fromArray(data)
  .delay(100L, TimeUnit.MILLISECONDS);
observable.subscribe(Log::it)
```

## timeInterval()
  * 값을 발행했을 때 이전 값을 발행한 이후 얼마나 시간이 흘렀는지 알려줌

```java
@SchedulerSupport(SchedulerSupport.NONE)
public final Observable<Timed<T>> timeInterval()

Timed<T>
public T value()
public TImeUnit unit()
public long time();
public long time(TimeUnit unit)
```

```java
String[] data = {"A","B","C"};
Observable<Timed<String>> observable = Observable.fromArray(data)
  .delay(item -> {
    Thread.sleep(new Random().nextInt(100));
    return Observable.just(item);
  })
  .timeInterval();
observable.subscribe(Log::i);
```
