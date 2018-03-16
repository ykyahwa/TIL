# 결합연산자
  * Observable 조합 활용
  > zip(), combineLatest(), merge(), concat()

## zip()
  * 2개 이상의 Observable 결합
  * 2개의 Observable 에서 모두 데이터 발행해야 결합가능
  * 최대 9개까지 Observable 결합 가능

```java
@SchedulerSupport(SchedulerSupport.NON)
public static <T1, T2, R> Observable<R> zip(
  ObservableSource<? extends T1> source1,
  ObservableSource<? extends T2> source2,
  BiFunction<? super T1, ? super T2, ? extends R> zipper>
)
```

```java
String[] hanguls = {"가","나","다"};
String[] engs = {"A","B","C"};

Observable<String> observable = Observable.zip(
  Observable.fromArray(hanguls),
  Observable.fromArray(engs),
  (hangul, eng) -> hangul + eng);

  observable.subscribe(System.out::println)
)

실행결과
가A
나B
다C
```

```java
Observable<Integer> observable = Observable.zip(
  Observable.just(100,200,300),
  Observable.just(10,20,30),
  (a,b) -> a + b)
    .zipWith(Observable.just(1,2,3), (ab, c) -> ab + c);

Observable.subscribe(System.out::println);

실행결과
111
222
333
```
