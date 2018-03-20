# 스케줄러

  * subscribeOn, observeOn 함수를 사용하여 데이터 흐름을 발생하는 스레드와 구독자에게 발행하는 스레드 분리
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
