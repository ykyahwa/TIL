# Subject
 * cold Observable - > hot Observable로 바꿔줌
 * Observable 속성과 구독자 속성이 모두 있음

## AsyncSubject
 * Observable에서 발행된 마지막 데이터를 얻어올 수 있음
 * onComplete() 이후 onNext()는 무시됨
 
<img src="http://reactivex.io/documentation/operators/images/S.AsyncSubject.png"/>

```Java
public abstract class Subject<T> extends Observable<T> implements Observer<T>
```
```Java
AsyncSubject<String> subject = AsyncSubject.create();
subject.subscribe(data -> System.out.println("#1 = " + data));
subject.onNext("A");
subject.onNext("B");
subject.subscribe(data -> System.out.println("#2 = " + data));
subject.onNext("C");
subject.onComplete();

실행결과
#1 = C
#2 = C
```
```java
String[] hanguls = {"가","나","다"};
Observable<String> observable = Observable.fronArray(hanguls);

AsyncSubject<String> subject = AsyncSubject.create();
subject.subscribe(data -> System.out.println("#1 = " + data));

observable.subscribe(subject);

실행결과
#1 = C
```

## BehaviorSubject
 * 구독 하면 최근 값 또는 기본값을 넘겨줌
 
<img src="http://reactivex.io/documentation/operators/images/S.BehaviorSubject.png"/>

```java
BehaviorSubject<String> subject = BehaviorSubject.createDefault("D");
subject.subscribe(data -> System.out.println("#1 = " + data));
subject.onNext("A");
subject.onNext("B");
subject.subscribe(data -> System.out.println("#2 = " + data));
subject.onNext("C");
subject.onComplete();

실행결과
#1 = D
#1 = A
#1 = B
#2 = B
#1 = C
#2 = C
```

## PublishSubject
 * 구독 한 시점 부터 발생한 데이터 받음
 
<img src="http://reactivex.io/documentation/operators/images/S.PublishSubject.png"/>

```java
PublishSubject<String> subject = PublishSubject.create();
subject.subscribe(data -> System.out.println("#1 = " + data));
subject.onNext("가");
subject.onNext("나");
subject.subscribe(data -> System.out.println("#2 = " + data));
subject.onNext("다");
subject.onComplete();

생행결과
#1 = 가
#1 = 나
#1 = 다
#2 = 다
```

## ReplaySubject
 * cold Observer처럼 동작
 * 구독자가 생기면 데이터의 처음부터 끝까지 발행
 * 메모리 누수 발생 가능성
 
<img src="http://reactivex.io/documentation/operators/images/S.ReplaySubject.png"/>

```java
ReplaySubject subject = ReplaySubject.create();
subject.subscribe(data -> System.out.println("#1 = " + data));
subject.onNext("가");
subject.onNext("나");
subject.subscribe(data -> System.out.println("#2 = " + data));
subject.onNext("다");
subject.onComplte();

실행결과
#1 = 가
#1 = 나
#2 = 가
#2 = 나
#1 = 다
#2 = 다
```
