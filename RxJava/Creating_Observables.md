# 생성연산자
  * 데이터의 흐름을 만듬
  > just(), fromXXX(), create(), interval(), range(), timer(), intervalRange(), defer(), repeat()

## interval()
  * 일정 시간 가격으로 데이터 흐름 생성
  * 영원히 지속 실행, 폴링 용도로 많이 사용
  <img src="http://reactivex.io/documentation/operators/images/interval.c.png"/>

```java
@SchedulerSupport(SchedulerSupport.COMPUTATION)
public static Observalbe<Long> interval(long period, TimeUnit unit)
public static Observable<Long> interval(long initialDelay, long period, TimeUnit unit)
```
```java
Observable<Long> observable = Observable.interval(100L, TimeUnit.MILLISECONDS)
  .map(data -> (data + 1) * 100)
  .take(5);
observable.subscribe(System.out::println);

실행결과
100
200
300
400
500
```

## timer()
  * 한번만 실행
  * 일정 시간이 지난 후 onComplete() 호출
  <img src="http://reactivex.io/documentation/operators/images/timer.c.png"/>

```java
@SchedulerSupport(SchedulerSupport.COMPUTATION)
public static Observable<Long> timer(long delay, TimeUnit unit)
```

```java
Observable<String> observable = Observable.timer(1000L, TimeUnit.MILLISECONDS)
  .map(notUsed -> {
    return new SimpleDataFormat("yyyy/MM/dd").format(new Date());
  });
  observable.subscribe(System.out::println);

실행결과
2018/03/14
```

## range()
 * n부터 m개 Integer 발행
 * 현재 쓰레드에서 실행

<img src="http://reactivex.io/documentation/operators/images/range.c.png"/>

```java
@SchedulerSupport(SchedulerSupport.NONE)
public static Observable<Integer> range(final int start, final int count)
```

```java
Observable<Integer> observable = Observable.range(1,5)
  .filter(num -> num % 2 == 0);
observable.subscribe(System.out::println);

실행결과
2
4
```

## intervalRange()
 * interval() + range()
 * 일정한 시간 간격으로 n부터 m개 발행하고 onComplete() 호출

```java
@SchedulerSupport(SchedulerSupport.COMPUTATION)
public static Observable<Long> intervalRange(long start,
  long count, long initialDelay, long period, TimeUnit unit)
```

```java
Observable<Long> observable = Observable.intervalRange(1, 3, 100L, 100L, TimeUnit.MILLISECONDS);
observable.subscribe(System.out::println);

실행결과
1
2
3
```

## defer()
 * subscribe() 호출 할때 새로운 Observable 생성

<img src="http://reactivex.io/documentation/operators/images/defer.c.png"/>

```java
@SchedulerSupport(SchedulerSupport.NONE)
public static <T> observable<T> defer(Callable<? extends ObservableSource<? extends T>> supplier){}
```

```java
Iterator<String> nums = ArrayList.asList("1","3","5").iterator();
private observableDefer(){
    Observable<String> observable = Observable.defer(()-> getObservable);

    observable.subscribe(data-> Log.i("#1 = " + data));
    observable.subscribe(data-> Log.i("#2 = " + data));
}

private Observable<String> getObservable() {
  if (num.hasNext()) {
    String num = nums.next();
    return Observable.just(num+"가",num+"나",num+"다");
  }
  return Observable.empty();
}

실행결과
#1 = 1가
#1 = 1나
#1 = 1다
#2 = 3가
#2 = 3나
#2 = 3다
```
## repeat()
  * 단순 반복 실행

<img src="http://reactivex.io/documentation/operators/images/repeat.c.png"/>

```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fromArray(hanguls)
  .repeat(3);

observable.doOnComplete(() -> Log.d("onComplete"))
  .subscribe(Log::i);

실행결과
가
나
다
가
나
다
가
나
다
onComplete
```
