#Reactive Programming
* 1990년대에 제시된 데이터 흐름과 전달에 관한 프로그래밍 패러다임
* [ReactiveX](http://reactivex.io/)
  * Document 주요 클래스에 대해서는 한글설명을 제공하고있다.
  * 마블다이어그램의 이해가 필요하다.
* [Reactive programming Wiki](https://en.wikipedia.org/wiki/Reactive_programming)

## 자바와 리액티브
* 자바의 Pull 방식을 Push방식으로 개념을 바꿈
* 함수형 프로그래밍 지원을 받음
* 자바의 콜백, 옵저버 패턴은 멀티스레드 환경에서 데드락, 동기화 등의 문제가 있음

## 함수형 프로그래밍
* side effect가 없음
* 순수 함수 지향
* 멀티 스레드 환경에서 안전

## RxJava
* 넷플릭스에서 만듬
* 동시성 처리
* 비동기 흐름을 조합(당시 Java8의 CompletableFuture같은 기능 부재)
* 콜백 지옥 해결
* Rxjava 1.x 패키지 : rx.xxx
* Rxjava 2.x 패키지 : io.reactivex
