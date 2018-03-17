# 결합연산자
  * Observable 조합 활용
  > zip(), combineLatest(), merge(), concat()

## zip()
  * 2개 이상의 Observable 결합
  * 2개의 Observable 에서 모두 데이터 발행해야 결합가능
  * 최대 9개까지 Observable 결합 가능

```java
@SchedulerSupport(SchedulerSupport.NONE)
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
## combineLatest()
  * 여러 Observable 각각의 값이 변경 되었을 떄 갱신

```java
@SchedulerSupport(SchedulerSupport.NONE)
public static <T1, T2, R> Observable<R> combineLatest(
  ObservableSource<? extends T1> source1,
  ObservableSource<? extends T1> source2,
  BiFunction<? super T1, ? super T2, ? extends R> combiner)
)
```

```java
String[] hanguls = {"가","나","다","라"};
String[] numbers = {"1","2","3"};

Observable<String> observable = Observable.combineLatest(
  Observable.fromArray(hanguls)
    .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS),
      (hangule, notUsed) -> hangul),
  Observable.fromArray(numbers)
    .zipWith(Observable.interval(150L, 200L, TimeUnit.MILLISECONDS),
    (number, notUsed) -> number), (v1, v2) -> v1+v2);
observable.subscribe(Log::i);

실행결과
가1
나1
다1
다2
라2
라3
```
## merge()
  * 여러 Observable 에 대해서 먼저 입력되는 데이터를 그대로 발행

```java
String[] num1 = {"1","4"};
String[] num2 = {"2","5","6"};

Observable<String> observable1 = Observable.interval(0L, 100L, TimeUnit.MILLISECONDS)
  .map(Long::intValue)
  .map(idx -> num1[idx])
  .take(num1.length);
Observable<String> observable2 = Observable.interval(50L, TimeUnit.MILLISECONDS)
    .map(Long::intValue)
    .map(idx -> num2[idx])
    .take(num2.length);

실행결과
1
2
4
5
6
```

## concat()
  * 여러 Observable를 이어 붙여줌
  * Observable에 onComplte 발생하면 다음 Observable 구독

```java
@SchedulerSupport(SchedulerSupport.NONE)
public static <T> Observable<T> concat()
  ObservableSource<? extends T> source1,
  ObservableSource<? extends T> source2)
```

```java
Action onCompleteAction = () -> Log.d("onComplete()");

String[] num1 = {"1","4"};
String[] num2 = {"2","5","6"};

Observable<String> observable1 = Observable.fromArray(num1)
  .doOnComplete(onCompleteAction);

Observable<String> observable2 = Observable.interval(100L, TimeUnit.MILLISECONDS)
    .map(Long::intValue)
    .map(idx -> num2[idx])
    .take(num2.length)
    .doOnComplete(onCompleteAction);

Observable<String> observable = Observable.concat(observable1, observable2)
  .doOnComplete(onCompleteAction);

observable.subscribe(Log::i);

실행결과
1
4
onComplete
2
4
5
onComplete
onComplete
```
