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
