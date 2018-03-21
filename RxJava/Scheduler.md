# 스케줄러

  * subscribeOn, observeOn 함수를 사용하여 데이터 흐름을 발생하는 스레드와 구독자에게 발행하는 스레드 분리
  * subscribeOn 한번 결정하면 다시 호출해도 바뀌지 않음
  * observeOn 호출 다음 스레드 바뀜
```java
int[] nums = {1,2,3,4}
Observable<Integer> observable = Observable.fromArray(nums)
  .doOnNext(data -> Log.d("Original = " + data))
  .subscribeOn(Schedulers.newThread())
  .observeOn(Schedulers.newThread())
  .map(num -> num + 1);
observable.subscribe(Log::i)
```

> 뉴 스레드 스케줄러 - newThread()</br>
  싱글 스레드 스케줄러 - single()</br>
  계산 스케줄러 - computation()</br>
  IO 스케줄러 - io()</br>
  트램펄린 스케줄러 - trampoline()</br>
  메인 스레드 스케줄러 - 지원안함</br>
  테스트 스케줄러 - 지원안함

## 뉴 스레드 스케줄러
  * 새로운 스레드 만들어 동작

```java
String[] engs = {"A","B","C"}
Observable.fromArray(engs)
  .doOnNext(data -> Log.d("Original = " + data))
  .map(data  -> "<" + data + ">")
  .subscribeOn(Schedulers.newThread())
  .subscribe(Log::i)

Observable.fromArray(engs)
  .doOnNext(data -> Log.d("Original = " + data))
  .map(data  -> "@@" + data + "@@")
  .subscribeOn(Schedulers.newThread())
  .subscribe(Log::i)
```

## 계산 스케줄러
  > 계산 스케줄러, IO 스케줄러, 트램펄린 스케줄러 사용 추천

  * IO작업을 하지 않는 스케줄러
  * CPU 개수 만큼 스레드 생성

```java
@SchedulerSupport(SchedulerSupport.CUSTOM) //원하는 스케줄러 지정가능
public static Observable<Long> interval(long period, TimeUnit unit, Schedulers Scheduler)
```

```java
String[] numbers = {"1","2","3"};
Observable<String> observable = Observable.fromArray(numbers)
  .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS), (a. b) -> a);

observable.map(item -> "!!" + item + "!!")
  .subscribeOn(Schedulers.computation());
  .subscribe(Log::i);

observable.map(item -> "@@" + item + "@@")
  .subscribeOn(Schedulers.computation());
  .subscribe(Log::i);

실행결과
RxComputationThreadPool-1 value = !!1!!
RxComputationThreadPool-2 value = @@1@@
RxComputationThreadPool-1 value = !!2!!
RxComputationThreadPool-2 value = @@2@@
RxComputationThreadPool-1 value = !!3!!
RxComputationThreadPool-2 value = @@3@@
```

## IO 스케줄러
  * 네트워크 요청 처리, 입출력 처리
  * 필요 시 스레드 계속 생성

```java
.subscribeOn(Schedulers.io())
```

## 트램펄린 스케줄러
  * 현재 스레드에 Queue 생성 스케줄러

```java
String[] numbers = {"1","2","3"};
Observable<String> observable = Observable.fromArray(numbers)
  .zipWith(Observable.interval(100L, TimeUnit.MILLISECONDS), (a. b) -> a);

observable.subscribeOn(Schedulers.trampoline())
  .map(item -> "!!" + item + "!!")
  .subscribe(Log::i);

observable.subscribeOn(Schedulers.trampoline())
  .map(item -> "@@" + item + "@@")
  .subscribe(Log::i);

실행결과
main value = !!1!!
main value = !!2!!
main value = !!3!!
main value = @@1@@
main value = @@2@@
main value = @@3@@
```

## 싱글 스레드 스케줄러
  * Rxjava 내부 단일 스레드에서 구독 작업
  * rxjava1 의 immediate() 개선

```java
Observable<Integer> obervable1 = Observable.range(10,3);
String engs = {"A","B","C"};
Observable<String> obervable2 = Observable.fromArray(engs);

observable1.subscribeOn(Schedulers.single())
  .subscribe(Log::i);
observable2.subscribeOn(Schedulers.single())
  .subscribe(Log::i);

실행결과
RxSingleScheduler-1 value = 10
RxSingleScheduler-1 value = 11
RxSingleScheduler-1 value = 12
RxSingleScheduler-1 value = A
RxSingleScheduler-1 value = B
RxSingleScheduler-1 value = C
```

## Executor 변환 스케줄러
  * executor 를 변환하여 스케줄러 생성
  * executor 재사용할때 활용

```java
String[] nums = {"1","2","3"};
Observable<String> observable = Observable.fromArray(nums);
Executor executor = Executors.newFixedThreadPool(10);

observable.subscribeOn(Schedulers.from(executor))
  .subscribe(Log::i);
observable.subscribeOn(Schedulers.from(executor))
  .subscribe(Log::i);

실행결과
thread-1 value = 1
thread-1 value = 2
thread-1 value = 3
thread-2 value = 1
thread-2 value = 2
thread-2 value = 3
```
