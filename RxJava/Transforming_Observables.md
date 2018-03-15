# 변환 연산자
  * 데이터 흐름을 변형
  > map(), flatMap(), concatMap(), switchMap(), scan(), groupBy()

## concatMap()
  * flatMap - 데이터 처리 중 새로운 데이터가 들어오면 나중에 들어온 데이터가 먼저 처리 될 수도 있음
  * concatMap - 들어온 데이터 순서대로 처리 보장

```java
String[] hanguls = {"가", "나", "다"};
Observable<String> observable = Observable.interval(100L, TimeUnit.MILLISECONDS)
  .map(Long::intValue)
  .map(idx -> hanguls[idx])
  .take(hanguls.length)
  .concatMap(hangul -> Observable.interval(200L, TimeUnit.MILLISECONDS)
    .map(notUsed -> hangul + "!@")
    .take(2)
  );
observable.subscribe(Log::it)

실행결과
가!@
가!@
나!@
나!@
다!@
다!@
```

## switchMap()
  * 순서를 보장하기 위해 기존에 진행 중이던 작업을 중단
  * 여러개 값이 발행되었을때 마지막 값만 처리하고 싶을 떄 사용

```java
String[] hanguls = {"가", "나", "다"};
Observable<String> observable = Observable.interval(100L, TimeUnit.MILLISECONDS)
  .map(Long::intValue)
  .map(idx -> hanguls[idx])
  .take(hanguls.length)
  .switchMap(hangul -> Observable.interval(200L, TimeUnit.MILLISECONDS)
    .map(notUsed -> hangul + "!@")
    .take(2)
  );
observable.subscribe(Log::it)

실행결과
다!@
다!@
```

## groupBy()
  * 단일 Observable을 여러 개로 이루어진 Observable 그룹으로 만듬

<img src="http://reactivex.io/documentation/operators/images/groupBy.c.png"/>

```java
String[] hanguls = {"나2","가1","다2","가2","다1","나1"}
Observable<GroupedObservable<String, String>> observable = Observable.fromArray(hanguls).groupBy(Utils::getHangul);

observable.subscribe(hangul -> {
  hangul.subscribe(val -> System.out.println("Key = " +obj.getKey()+" Value = " + val));
})

실행결과
Key = 나 Value = 나2
Key = 가 Value = 가1
Key = 다 Value = 다2
Key = 가 Value = 가2
Key = 다 Value = 다1
Key = 나 Value = 나1
```

## scan
  * reduce()는 데이터 종합하여 마지막 데이터만 발행
  * 중간 결과 최정 결과 발행

```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fromArray(hanguls)
  .scan((hangul1, hangul2) -> hangul2 + hangul1);
observable.subscribe(Log::i);

실행결과
가
나가
다나가
```
