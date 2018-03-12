# Basic Operations

## map()
 * 함수를 넣어 원하는 값으로 변환

```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fromArray(hanguls)
  .map(hangul -> hangul + "!!");
observable.subscribe(Log::i);

실행결과
가!!
나!!
다!!
```

```java
@CheckReturnValue //반환값 확인
@SchedulerSupport(value="none") // 스케쥴러 미지원
public final <R> Observable<R> map(Function<? super T,? extends R> mapper)
```

> 제너릭 함수형 인터페이스
  * Predicate<T> - T 값을 받아 boolean return
  * Consumer<T> - T 값을 받아 처리. void 리턴 없음
  * Function<T,R> - T 값을 받아서 R 형태 리턴

## flatmap()
 * 함수를 넣어 결과가 Observable로 나옴

<img src="http://reactivex.io/documentation/operators/images/flatMap.c.png"/>

```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fromArray(hanguls)
  .flatMap(hangul -> Observable.just(hangul + "!", hangul + "@"));
observable.subscribe(Log::i);

실행결과
가!
가@
나!
나@
다!
다@
```

```java
@SchedulerSupport(SchedulerSupport.NONE) //현재 쓰레드에서 실행
public final <R> Observable<R> flatMap(Function<? super T, ? extends ObservableSource<? extends R>> mapper)
//ObservableSource - Observable처럼 데이터 발행 할 수 있는 객체
```

## filter()
 * Observable에서 원하는 데이터만 걸러냄

```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fromArray(hanguls)
  .filter(hangule -> hangul.equals("나"));
observable.subscribe(System.out::println);

실행결과
나
```
> first(default) - 첫번째값 필터, 값 없이 완료되면 기본값 리턴</br>
  last(default) - 마지막값 필터, 값 없이 완료되면 기본값 리턴</br>
  take(N) - 최초 N개만 가져옴</br>
  takeLast(N) - 마지막 N개 가져옴</br>
  skip(N) - 최초 N개 건너뜀</br>
  skipLast(N) - 마지막 N개 건너뜀

```java
Integer[] numbers = {1,2,3,4,5};
Single<Integer> single;
Observable<Integer> observable;

//first
single = Observable.fromArray(numbers).first(0);
single.subscribe(num -> System.out.println("first num = " + num));

//last
single = Observable.fromArray(numbers).last(999);
single.subscribe(num -> System.out.println("last num = " + num));

//take(N)
source = Observable.fromArray(numbers).take(3);
source.subscribe(num -> System.out.println("take num = " + num))

//takeLast(N)
source = Observable.fromArray(numbers).takeLast(3);
source.subscribe(num -> System.out.println("takeLast num = " + num))

//skip(N)
source = Observable.fromArray(numbers).skip(2);
source.subscribe(num -> System.out.println("skip num = " + num))

//skipLast(N)
source = Observable.fromArray(numbers).skipLast(2);
source.subscribe(num -> System.out.println("skipLast num = " + num))
```

## reduce()
 * 발행한 데이터를 사용하여 최정결과 데이터 합성

```java
String numbers = {1,2,3,4};
Maybe<String> maybe = Observable.fromArray(numbers)
  .reduce((num1, num2) -> num1 + num2);
maybe.subscribe(System.out.println);

실행결과
10
```
