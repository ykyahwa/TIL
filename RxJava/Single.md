# Single
 * 오직 1개의 데이터만 발행
 * onSuccess(T value), onError() 로 구성

# just()
```java
Single<String> single = Single.just("Hello");
single.subscribe(System.out::println);

실행결과
Hello
```

## Single 사용

```java
//Observable -> Single
Observable<String> observable = Observable.just("Hello");
Single.fronObservable(observable)
  .subscribe(System.out::println);

//Single() 
Observable.just("Hello")
  .single("default")
  .subscribe(System.out::println);

//first()
String[] hangul = {"가", "나", "다"};
Observable.fronArray(hangul)
  .first("default")
  .subscribe(System.out::println);

//empty()
Observable.empty()
  .single("default")
  .subscribe(System.out::println);

//take()
Observable.just("가", "나")
  .take(1)
  .single("다")
  .subscribe(System.out:println);
```


