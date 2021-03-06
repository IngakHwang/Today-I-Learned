# ReactiveX

> 옵저버 패턴으로 구독자에게 변경사항을 알려주는 '비동기 이벤트 기반 프로그래밍'



**변화의 전파와 데이터 흐름**과 관련된 **선언적 프로그래밍** 패러다임

- 변화의 전파와 데이터 흐름



Observer 역할 - 다른 Observable에게 구독하여 item 수신

Observable 역할 - Observable 에게 수신받은 item 재배출 또는 새로운 item배출



데이터 발행에 대한 흐름

데이터가 중심

Cold Stream -> 데이터가 정해져 있는 것

Hot Stream -> 예) 터치이벤트, 능동적인 데이터





## 요약

3가지

첫번째, 주요요소인 일련의 값들을 발행하는 Observable (관찰될 수 있는 것, 관찰되는 대상이란 뜻)

​	예를 들어 Observable 에서 1~20 까지 정수를 반환하는 데, 

​	이렇게 연속적으로 발행되어 나오는 값들을 Stream (흐름) 이라고 한다.

​	이 Stream을 타고 흐르는 값들은 Pipe 라는 배관을 타고 

두번째, 주요요소인 Operator, 연산자들의 손을 거치게 된다.

​	Operator들은 작업을 처리하기 위한 순수 함수들이다.

마지막으로 Observer, 관찰자란 뜻

​	Oberserver는 Pipe만 쳐다보며 값을 기다리다가 뭔가 나오는 대로 최종 작업을 실행 

​	Oberserver가 Pipe를 주시하며 발행물을 기다리는 것을 ReactiveX 에서는 subscribe, 구독한다고 표현한다.

​	

ReactiveX의 장점은 

평면적인 배열, 1차원적인 값들 뿐 아니라

시간의 흐름, 사용자의 동작, 네트워크 요청의 결과까지 

전부 Stream 으로 만들어서 

위와 같은 형식으로 

PipeLine으로 흘려보내 처리한다는 것

----

Reactive Program은 옵저버 패턴으로 구독자에게 변경사항을 알려주는 '비동기 이벤트 기반 프로그래밍'

​	

Observer 패턴 + Iterator 패턴 + Functional 프로그래밍	 	

​	**Reactive (비동기 이벤트 방식) + X (이벤트 처리 방식)**



필요에 의해 Data 요청 -> Data 가공이 아닌, 데이터 관리주체 (Observable)에 데이터 변경 시 요청을 받겠다는 구독신청을 하고, 변경사항 (event) 발생 시 전달받는 방식

​	-> Reactive Programming은 하나의 값을 반환하기 보다 Data Stream을 반환한다.



Reactive (반응형) 프로그래밍과 Functional (함수형) 프로그래밍의 조합



**Reactive Programming?**

> 로직이나 데이터를 하나의 흐름 (Stream)으로 봄으로써 비동기 작업을 쉽게 다루는 것을 의미

| **기존 비동기 방식**                                         | **Reactive 비동기 방식**                                     |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 비동기 작업 A를 완료하면 B를 호출(실행) (B는 Callback 또는 Interface 함수) | 비동기 작업 A가 이벤트를 발행하면 B가 구독을 수행 (B는 Observer 또는 Consumer) |



**Functional Programming?**

> 주어진 데이터에서 새로운 데이터를 반환 (과정을 알려주지 않음) 새로운 데이터를 반환하는 함수의 연속

| **명령형 프로그래밍 (절차형, 객체지향 .. 일반적인 방식)**    | **선언형 프로그래밍**                                        |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| 작업을 이렇게 저렇게 수행해라데이터 상태를 변경해서 결과를 반환 (알고리즘을 명시) | 특정 데이터가 되어라 (목적을 명시) 주어진 데이터에서 새로운 데이터를 반환하는 함수의 연속 |



## RxKotlin



`build.gradle`

```
implementation 'io.reactivex.rxjava2:rxkotlin:2.4.0'
```



### Observable

> 구독 대상자

- 이벤트 발행 주체 (Consumer 가 소비하는 값을 생성하는 역할)
- Observer 가 구독하는 대상 (subcribe() 으로 구독 등록)
- Observable은 값을 생성 후 Consumer 들에게 push 방식으로 값 전달
- Observable Type (타입)
  - Observable : 최상위 기본 타입 (Default)
  - Single : 1개의 데이터만 반환 (null 반환 시 예외 발생)
  - Maybe : Null 가능성 있는 1개의 데이터 반환 (single + Nullable)
  - Completable : 반환값 없이 단순히 수행 후 종료 (Nullable)
  - Flowable : Backpressure 지원 Observable (발행/구독의 속도 차이 발생 시 속도 조절)

`예시`

```kotlin
Observable.just("One","Two")											// 1
		.subscribeOn(Scheduler.io())									// 2
		.observeOn (AndroidSchedulers.mainThread())		// 3
		.subscribe (																	// 4
      {value -> println("Result : $value")},			// 5
      {error -> println ("Error : $error")},			// 6
      {println("onComplete")}											// 7
    )
// Result : One
// Result : Two
// onComplete
```

```
1. Observable 선언, just() 는 내부 요소 순서대로 전달
2. Observable, Observer 실행 Thread 변경 (io 를 Worker 쓰레드, 백그라운드)
3. Observer 실행 Thread 변경 (UI 쓰레드)
4. 구독 등록, 3개 의 param
5. 데이터 발행 시 호출
6. 예외 발생 시 호출
7. 모든 데이터 발행 후 호출
```

Observable 생성 함수는 `create()` , `just()` 등 여러개가 존재한다.

`just()` 는 인자로 받은 데이터를 순서대로 발행(emit) 한다.

`subscribeOn` 은 선언 위치에 상관없이 Observable 과 Observer의 실행 Thread 를 설정하는 함수이다.

`observeOn` 선언 아래부분에만 선언한 Thread를 설정하는 함수

subscribeOn 보다 onserveOn의 우선순위가 높기 때문에 각각 Thread 영역이 다르게 설정

​	Observable - Io Thread (Worker Thread / Background Thread)

​	Observer - Main Thread (UI Thread)

`subscribe()` 는 구독자를 등록하는 함수로 Observer 인스턴스로 할당해도 되고 예시처럼 등록도 가능



#### Observable 주요 이벤트

- `onSubScribe(d : Disposable)` : 구독을 신청 시 호출
  - `Disposable` 은 Observer가 구독을 해제 할 때 사용 , `dispose()`
- `onNext(item : T)` : 값을 발행(emit) 할 때 호출하여 값을 넘겨준다.
- `onError(e : Throwable)` : Observable에서 에러 발생 시 호출
- `onComplete()` : 값을 모두 발행 (emit) 하면 호출, End



이 Callback 함수들을 구현해서 Observable에 등록하면 Observable이 전달하는 이벤트를 받는다. 

```kotlin
val observer : Observer<Int> = object : Observer<Int> {
  override fun onSubscribe(d : Disposalbe){ // 구독 시 호출
  	println("onSubscribe() - $d")
  }
  
  override fun onNext(item : Int){					// Observable이 값을 발행(emit) 시 호출
   	println("onNext() - $item")
  }
  
  override fun onError(e : Throwable){			// Observable이 에러 발생 시 호출
    println("onError() - $e")
  }
  
  override fun onComplete(){								// Observable이 값을 모두 발행(emit) 하면 호출
    println("onComplete()")
  }
}
```



#### Observable 생성

- `just()` : 함수 선언 요소들을 순서대로 발행, 최대 10개 까지 값 등록
- `create()`  : `just()` 는 순서대로 발행, create는 직접 `onNext()` 를 호출해야 함
- `range()` : 특정 범위만큼 수를 생성하여 전달 
- `empty()` : 반환값이 없는 Observable, onSubscribe, onComplete 호출
- `interval()` : 특정 시간 간격으로 0부터 숫자를 증가시켜 값을 발행
- `timer()` : 특정 시간이 되면 한번만 값을 발행
- `fromXXX()` : 기존 구조체로부터 Observable을 생성
  - `fromIterable()` : Iterable이 존재하는 Colletion, Item을 순서대로 하나씩 전달
  - `fromCallable()` : Callable 객체 -> Observable 변경, Callable의 `call()` 값을 전달 
  - `fromFuture()` : Future 객체 -> Observable 변경, Future의 `get()` 값 전달



**예시**

##### `just()`

받은 인자를 차례대로 순서 맞춰 발행하는 Observable을 생성

List나 Map을 전달받던 객체 자체를 그대로 전달하며, 최대 개수는 10개로 10개 이상 선언 시 예외 발생

<img src="https://blog.kakaocdn.net/dn/NLKUW/btqDvYkMwXO/vGKKN25BOmwGDiEXchAI00/img.png" alt="img" style="zoom:50%;" />

```kotlin
val observer : Observer<Any> = object : Observer<Any>{
  override fun onSubscribe(d: Disposable) = println("onSubscribe() - $d")
	override fun onNext(item: Any) = println("onNext() - $item")
	override fun onError(e: Throwable) = println("onError() - ${e.message}")
	override fun onComplete() = println("onComplete()")
}

val list = listOf(1,2,3,4,5)	// List
val num = 100									// Int
val str = "just Test"					// String
val map = mapOf(1 to "One", 2 to "Two", 3 to "Three")	// map

Observable.just(list,num,str,map)			//just() 함수로 Observable 생성
				.subscribe(observer)					//구독

//결과
onSubscribe() - io.reactivex.internal.operators.observable.ObservableFromArray$FromArrayDisposable@66133adc
onNext() - [1, 2, 3, 4, 5]
onNext() - 100
onNext() - just Test
onNext() - {1=One, 2=Two, 3=Three}
onComplete()
```



##### `create()`

`just()` 와 다르게 선언 시 `ObservableEmitter` 를 직접 호출해야 한다. 

​	`ObservableEmitter` =? `onNext(), onError(), onComplete` 

<img src="https://blog.kakaocdn.net/dn/bfWg9s/btqDvyNutTM/WxplC9Ki2aYDZWQ3qcQ6QK/img.png" alt="img" style="zoom:50%;" />

```kotlin
Observable.create<Int>{							// Create 예시
  it.onNext(1)
  it.onNext(2)
  it.onComplete()
}.subscribeBy{
  	onNext = {println("first $it - onNext()")}
  	onComplete = {printlnt("first onComplete()\n")}
}

Observable.create<Int>{							// Error 예시
  it.onNext(10)                
  it.onNext(20)
  it.onError(Exception("Error Now"))
 }.subscribeBy(
     onNext = { println("second $it - onNext()") },
     onError = { println("second $it - onError()") },
     onComplete = { println("onComplete()") }
 )

//결과
first 1 - onNext()
first 2 - onNext()
first onComplete()

second 10 - onNext()
second 20 - onNext()
second java.lang.Exception: Error Now - onError()
```



##### `range()`

주어긴 값(n) ~ (m)개의 Integer 객체를 발행

<img src="https://blog.kakaocdn.net/dn/dvwpCX/btqDwdaZkLV/TkAswmXqMBpoiYak0kM2QK/img.png" alt="img" style="zoom:50%;" />

```kotlin
Observable.range(5,5)
			.subscribeBy{
        	onNext = {println("$it - onNext()")},
        	onComplete = {println("onComplete")}
      }

//결과 : 주어진 값(5) 부터 5개의 Integer 객체 발행
5 - onNext()
6 - onNext()
7 - onNext()
8 - onNext()
9 - onNext()
onComplete()
```



##### `empty()`

아무값을 전달하진 않지만 `onComplete()` 를 마지막에 호출

```kotlin
Observable.empty<Unit>()
			.subscribeBy{
        	onNext = {println("$it - onNext()")},
        	onComplete = {println("onComplete")}
      }

//결과 : 아무 값도 반환하진 않았지만 onComplete 호출
onComplete()
```



##### `interval()`

주어진 시간 간격으로 0부터 1씩 증가하는 Long 객체 생성

<img src="https://blog.kakaocdn.net/dn/bf3D4L/btqDx4RskPV/UFM10W9hzonFVYzb3PwXk0/img.png" alt="img" style="zoom:50%;" />

```kotlin
Observable.interval(100, TimeUnit.MILLISECONDS)
			.take(5)			//5개 까지만 발행 
			.subscribeBy{
        	onNext = {println("$it - onNext()")},
        	onComplete = {println("onComplete")}
      }

Thread.sleep(1000)	// 동작 완료할 때 까지 Main 쓰레드 대기

//결과 : 0.1초마다 값을 발행, 종료를 위해 5개까지만 발행 설정
0 - onNext()
1 - onNext()
2 - onNext()
3 - onNext()
4 - onNext()
onComplete()
```



##### `timer()`

`interval()` 과 비슷하지만, 주어진 시간이 지나면 한개의 데이터를 발행하고 `onComplete()` 이벤트 발생

<img src="https://blog.kakaocdn.net/dn/wJ2bk/btqDx6V2or8/yKlgLdnUX38XaiGKS8cjk1/img.png" alt="img" style="zoom:50%;" />

```kotlin
println(SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(Date()))

Observable.timer(3000L, TimeUnit.MILLISECONDS)
			.map{SimpleDateFormat("yyyy/MM/dd HH:mm:ss").format(Date())}
			.subscribeBy{
        	onNext = {println("$it - onNext()")},
        	onComplete = {println("onComplete")}
      }

Thread.sleep(5000)

//결과 : 주어진 시간 (3초) 지나면 값을 한번만 전달하고 종료
2022/04/19 02:29:15
2022/04/19 02:29:18 - onNext()
onComplete()
```



**fromXXX()**

from을 이용 시 기존 구조체로 부터 Observable을 생성 할 수 있다.

<img src="https://blog.kakaocdn.net/dn/pyFTD/btqDx4YcaZR/y2aMk5jCR5AfxMzG8NKKm1/img.png" alt="img" style="zoom:50%;" />



##### `fromArray()`

Array 내부 데이터를 하나씩 발행하는 Observable 반환

```kotlin
val arr = arrayOf("one", "two", "three")
Observable.fromArray(arr)
        .subscribe { println(it) }

//결과
one
two
three
```



##### `fromIterable()`

iterable을 지원하는 구조체를 Observable 형태로 변경

Collection 아이템이 하나씩 발행

Iterable 클래스들 - List, ArrayList, ArrayBlockingQueue, HashSet, LinkedList, Stack, TreeSet 등

```kotlin
val names = ArrayList<String>()
names.add("손오공")
names.add("나루토")
names.add("이치고")

Observable.fromIterable(names)
			.subscribeBy{
       		onNext = {println("$it - onNext()")},
        	onComplete = {println("onComplete")}
      }

//결과
onNext() - 손오공
onNext() - 나루토
onNext() - 이치고
onComplete()
```



##### `fromCallable()`

Callable 객체를 Observable 형태로 변경

Callable의 `call()` 함수 return 값이 Observer에 전달

```kotlin
val callable = {
  sleep(500)
  "500ms End sleep !"
}

Observable.fromCallable(callable)
			.subscribeBy{
        	onNext = {println("onNext() - $it")},
        	onComplete = {println("onComplete()")}
      }

//결과
onNext() - 500ms End sleep !
onComplete()
```



##### `fromFuture()`

Future 객체를 Observable 형태로 변경

Future의 `get()` 함수 return 값이 Observer에 전달

```kotlin
val future = object : Future<String> {
    override fun get() = "Future get()"		// 구독(subscribe) 시 전달될 값
    
    override fun get(timeout: Long, unit: TimeUnit) = "Future get(timeOut)"
    
    override fun isDone() = true
    
    override fun cancel(mayInterruptIfRunning: Boolean) = false
    
    override fun isCancelled() = false
}

Observable.fromFuture(future)
			.subscribeBy{
        	onNext = {println("onNext() - $it")},
        	onComplete = {println("onComplete()")}
      }

//결과
onNext() - Future get()
onComplete()
```



#### Observable 구독

- Observable.subscribe() 메서드로 구독 신청

- `subscribe()` : Observable에서 방출하는 값을 받기 위한 함수

- subscribe을 사용한 구독방법 2가지

  - Observer Instance 등록
  - Instance 아닌, 사용자 필요 이벤트만 정의해서 등록

- `subscribe()` parameters

  ![img](https://blog.kakaocdn.net/dn/lEvhA/btqDwEedvIf/9SCbuTHWVlBCJoSCsWcF01/img.png)

  #위에서 부터 1~6번
  
  1~5번 까지 파라미터 유형은 Observer Instance를 등록하는 게 아닌 필요한 이벤트만 정의하는 유형이다.
  
  1~5번 까지 보면 각 파라미터의 타입에 ! 키워드로 내가 원하는 이벤트만 정의가 가능하다.
  
  6번 파라미터 유형은 Observer Instance를 등록하는 방식이다.
  
  모든 유형에 마찬가지로 `subscribe()` 는 Disposable Instance를 반환하기 때문에 이 Disposable로 구독해제 가능하다.



##### Observer Instance 등록

Observer Instance 구현하면 4개의 이벤트를 Override 해야하지만 재활용이 가능하다.

```kotlin
// Observer 선언, 구독자
// 메소드 오버라이드 하기
val observer : Observer<Any> = object : Observer<Any> {
  override fun onSubscribe(d: Disposable) = println("onSubscribe() - $d")
	override fun onNext(item: Any) = println("onNext() - $item")
	override fun onError(e: Throwable) = println("onError() - ${e.message}")
	override fun onComplete() = println("onComplete()")
}
```



##### 필요 이벤트만 정의

필요 메서드만 구현해서 구독할 수 있다.

```kotlin
// 4개 정의
Observable.just(1)
			.subscribe(
    		{ println("onNext() - $it") },			// onNext() 이벤트
        { println("onError() - $it") },			// onError() 이벤트
        { println("onComplete()") },				// onComplete() 이벤트
        { println("onSubscribe() - $it") }	// onSubscribe() 이벤트
      )

// 2개 정의
Observable.just(1)
			.subscribe(
  	  	{ println("onNext() - $it") },			// onNext() 이벤트
        { println("onError() - $it") },			// onError() 이벤트
      )
```



#### 구독해지

- Observer를 subscribe를 통해 구독하면 `onSubscribe(d : Disposable)` 호출로 Disposable Instance를 전달

- Disposable Interface 구조

  ```kotlin
  public interface Disposable {
      // 구독해지 요청 메서드
      void dispose();
      
      // 구독해지 상태확인 메서드
      boolean isDisposed();   
  }
  ```



##### Observer를 통해 구독해지

`onNext()` 에서 if문으로 검사해서 (item 10 이상 and 구독상태확인) 결과확인 및 구독해지 

```kotlin
// Observer 선언, 구독자
val observer : Observer<Long> = object : Observer<Long>{
  lateinit var disposable : Disposable				// 늦은 초기화
  
  override fun onSubscribe(d : Disposable){
    println("onSubscribe() - $d")
    disposable = d														// disposable 초기화
  }
  
  override fun onNext (item : Long){
    println("onNext() - $item")
    if(item >= 10 && disposable.isDisposed == false){	// item 10 이상 and 구독해제가 안되었는지
      disposable.dispose()														// 구독해제
      println("subscribe Dispose!")
    }
  }
  
  override fun onError(e : Throwable){
    println("onError() - ${e.message}")
  }
  
  override fun onComplete() {
    println("onComplete()")
  } 
}

Observable.interval(100, TimeUnit.MILLISECONDS)	//0.1초 마다 값을 발행하는 interval Observable
		.subscribe(observer)

Thread(){
  Thread.sleep(1500)														//1,5초 Thread Block
}.apply{
 	start()
  join()
}

//결과
onSubscribe() - null
onNext() - 0
onNext() - 1
onNext() - 2
onNext() - 3
onNext() - 4
onNext() - 5
onNext() - 6
onNext() - 7
onNext() - 8
onNext() - 9
onNext() - 10
subscribe Dispose!
```



##### Disposable을 통한 구독해지

값 발행이 모두 완료된 것이 아닌 중간에 구독해제 상황

`onComplete()` 미호출

```kotlin
// Observable Instance 생성, 0.1초 간격으로 값 발행
val observable = Observable.interval(100, TimeUnit.MILLISECONDS)

val disposable : Disposable = observable.subscribe(
 	    { println("onNext() - $it")},			// onNext() 정의
      { println("onError() - ${it.message}")},	// onError() 정의
      { println("onComplete()")}			// onComplete() 정의
)

Thread(){
  Thread.sleep(1200)
  disposable.dispose()				//Observable 구독해지, dispose()
}.apply{
  start()											// Thread 실행
  join()											// Thread 종료 까지 대기
}

//결과
onNext() - 0
onNext() - 1
onNext() - 2
onNext() - 3
onNext() - 4
onNext() - 5
onNext() - 6
onNext() - 7
onNext() - 8
onNext() - 9
onNext() - 10
onNext() - 11
```





#### Cold Observable

위의 설명한 Observable들은 subscribe(구독) 신청 시 Observable이 데이터를 순서에 맞춰 전부 내보낸다.

여러번 subscribe 하더라도 처음부터 순서대로 동일한 데이터를 내보내주는 방식

Observable의 데이터는 subscribe 시 소모되는 것이 아닌, 저장된 값을 구독할 때 마다 전부 발행하는 방법

이러한 Observable의 유형을 `Cold Observable` 이라 한다.

- 구독자가 subscribe 시점에 데이터를 배출
- 처음부터 모든 데이터가 순서대로 배출 (모든 데이터 수신 보장)
- 여러번 subscribe 에도 동일한 데이터 배출 (데이터 배출 시 소모 X)



사용 예시 : 웹 요청, DB 관련 요청



#### Hot Observable

반대로 `Hot Observable` 은 데이터 배출 시점이 subscribe 시점이 아닌,

늦게 구독하는 Observer는 이전 데이터를 받지 못하고 subscrbe 이후의 데이터부터 받게 된다.

- subscribe 와 상관 없이 데이터를 배출
- subscribe 시점 이후 데이터부터 전달 받으며, 구독 이전의 데이터 수신X (모든 데이터 수신보장 X)
- Event를 전달 받는 형태로 사용



사용 예시 : 센서 이벤트, 입력 이벤트



##### ConnectableObservable

Hot Observable 중 하나로 `connect()` 함수를 호출 시 배출을 시작하는 Observable

Observer는 subscribe와 상관 없이 구독 신청 시점부터 데이터를 전달 받는다.

`publish()` - Cold Observable -> Hot Observable 변환



ConnectableObservable에 총 3개의 Observer subscribe 한 예시

1,2 번째 Observer를 구독하고 `connect()` 호출 시점에 값이 발행하고,

3 번째 Observer 구독을 했는 데, 이미 `connect()` 값이 모두 발행된 후라 결과를 못받는 걸 확인

`subscribe()` 시점에 값이 발행되지 않는 다는 것이 포인트

```kotlin
//publish() 함수로 ConnectableObservable 생성
val connecttableObservable = (1..4).toObservable().publish()

// 1번째 Observer subscribe
connectableObservable
		.subscribe{println("first Observer - $it")}
println("Add First Observer")

// 2번째 Observer subscribe
connectableObservable
		.subscribe{println("second Observer - $it")}
println("Add Second Observer")

// connect() - Observable 데이터 발행
connectableObservable.connect()

connectableObservable
		.subscribe{println("third Observer - $it")}
println("Add Third Observer")

//결과
Add First Observer
Add Second Observer
first Observer - 1
second Observer - 1
first Observer - 2
second Observer - 2
first Observer - 3
second Observer - 3
first Observer - 4
second Observer - 4
Add Third Observer
```



`interval()` 로 지속발행하는 Observable

`interval()` 로 0.1초 마다 지속적으로 값을 발행하는 Observable을  구현

`publish()` 로 Hot Observable로 변환하여 Observable의 값 발행시점은 `subscribe()` 가 아닌 `connect()` 시점

`connect()` 시점부터 값을 발행해서 1번째 Observer는 0부터 값을 수신

`delay(300)` 으로 인해 0.3초 지연됐던 2번째 Observer는 3부터 값을 수신

```kotlin
// interval() - 0.1초 마다 값 발행, publish() - Hot Observable 변환
val connectableObservable = Observable.interval(100, TimeUnit.MILLISECONDS).publish()

// 1번째 Observer 구독
connectableObservable.subscribe { println("first Observer - $it") }

// connect() - Hot Observable 값 발행 시점
connectableObservable.connect()

// 0.3초 대기 (Thread 점유)
runBlocking { delay(300) }

// 2번째 Observer 구독
connectableObservable.subscribe { println("second Observer - $it") }

// 0.3초 대기 (Thread 점유)
runBlocking { delay(300) }

// 결과
first Observer - 0
first Observer - 1
first Observer - 2
first Observer - 3
second Observer - 3
first Observer - 4
second Observer - 4
first Observer - 5
second Observer - 5
```



### Subject

> Observable + Observer

- `publish()` 처럼 Cold Observable -> Hot Observable 변환하는 클래스
- Subject는 Obervable + Observer
  - Observable 이면서 Observer 역할 수행
  - Observer 역할 - 다른 Observable에게 구독하여 item 수신
  - Observable 역할 - Observable에게 수신받은 item 재배출 또는 새로운 item 배출

##### `PublishSubject`

등록 시점 이후부터 데이터를 수신하는 Subject

`PublishSubject.create<T>()`

<img src="https://blog.kakaocdn.net/dn/stXyy/btqDzk0xeb2/qCCrGtK9SbItmBb0xLnFQk/img.png" alt="img" style="zoom:50%;" />

observable은 `interval()` 로 0.1초마다 지속적으로 값을 생성하는 Cold Observable, `subscribe()` 시 발행

subject가 observable을 subscribe 하는 시점부터 observable의 값이 발행 시작

이후 0.3초 후에 1번째 Observer를 subject에 등록하면, 3번째 데이터 수신

이후 0.3초 후에 2번째 Observer를 subject에 등록하면, 6번째 데이터 수신

​	-> observable은 Cold Observable인데, subject를 통해 Hot Observable 처럼 동작하는 예시

```kotlin
val observable = Observable.interval(100, TimeUnit.MILLISECONDS)

val subject = PublishSubject.create<Long>()	// PublishSubject 생성, Observable + Observer Type

observable.subscribe(subject)								// subject가 구독 (subject - Observer 역할)

runBlocking {delay(300)}										// 0.3초 대기

subject.subscribe {println("1st : $it")}		// subject에 1번째 Observer 등록, (subject - Observable역할)

runBlocking {delay(300)}

subject.subscribe {println("2nd : $it")}		// subject에 2번째 Observer 등록, (subject - Observable역할)

runBlocking {delay(300)}

//결과
1st : 3
1st : 4
1st : 5
1st : 6
2nd : 6
1st : 7
2nd : 7
1st : 8
2nd : 8
```



##### `BehaviorSubject`

등록 시점 이전에 배출된 직전값 하나를 전달받고 시작

Subscribe 시점에 데이터가 아직 없으면 기본값을 전달

`BehaviorSubject.createDefault(defaultValue)` - 데이터가 아직 없으면 Default 값 전달

<img src="https://blog.kakaocdn.net/dn/bbMuK1/btqDwd3fox4/A7sJ4PJXkWhFKPB5iGka7K/img.png" alt="img" style="zoom:50%;" />

PublishSubject에서 수신했던 값과 다르게 바로 직전의 값을 수신해서 출력

1번째 Subscriber는 onNext 호출전이라 데이터가 없으므로 기본값 5를 처음 수신

2번째 Subscriber는 subscribe 이전의 데이터 2를 먼저 수신

```kotlin
val subject = BehaviorSubject.createDefault("5")
subject.subscribe{ data -> println("Subscriber #1 -> $data")}
subject.onNext("1")
subject.onNext("2")

subject.subscribe{data -> println("Subscriber #2 -> $data")}
subject.onNext("3")
subject.onComplete()
```



##### `AsyncSubject`

Observable의 마지막값을 한번만 배출

​	마지막값 -> `onComplete()` 이후 가장 최신 데이터

`AsyncSubejct.create<T>()`

<img src="https://blog.kakaocdn.net/dn/E4O6X/btqDyN9JkIs/yOgzVhklCnqSQedMYGawd0/img.png" alt="img" style="zoom:50%;" />

AsyncSubject에 0.3초 간격을 두고 2개의 Observer를 구독신청

결과로는 2개의 Observer 모두 10값 수신

AsyncSubject는 쉽게 Observable의 마지막 발행된 값을 저장

이후 `subscribe()` 시 저장한 마지막 값을 발행해주는 Subject

```kotlin
// Cold Observable, 1부터 10개의 값 발행
val observable = Observable.range(1,10)

// AsyncSubject 생성
val subject = AsyncSubject.create<Int>()

// observable 값 발행시작
observable.subscribe(subject)			

subject.subscribe { println("1st : $it") }	
runBlocking { delay(300) }			

subject.subscribe { println("2nd : $it") }

// 결과

1st : 10
2nd : 10
```



##### `ReplaySubject`

Cold Observable과 비슷하게 등록 시점 이전값을 모두 수신받은 후 새로 배출되는 값을 전달

`RepleaySubject.create<T>()`

<img src="https://blog.kakaocdn.net/dn/t0wtY/btqDxI2lxjI/g9BOhpo9H0jrNPp6IYABr1/img.png" alt="img" style="zoom:50%;" />

observable은 `interval()` 로 0.1초마다 지속적으로 값을 생성하는 Cold Observable, `subscribe()` 시 발행

subject가 observable을 subscribe 하는 시점 부터 observable의 값이 발행 시작

이후 200ms 이후에 1번째 Observer를 subject에 등록하면, 이전값(0,1) 모두 수신

이후 200ms 이후에 2번째 Observer를 subject에 등록하면, 이전값(0,1,2,3,) 모두 수신

```kotlin
// Cold Observable, 0.1초마다 값 생성 (subscribe 호출 시 발행시작)
val observable = Observable.interval(100,TimeUnit.MILLISECONDS)

val subject = ReplaySubject.create<Long>()	// ReplaySubject, Observable + Observer Type

observable.subscribe(subject)			// subject가 구독(subject - Observer역할)
runBlocking { delay(200) }			// 0.2초 대기

subject.subscribe { println("1st : $it") }	// subject에 1번째 Observer 등록, (subject - Observable역할)
runBlocking { delay(200) }			// 0.2초 대기

subject.subscribe { println("2nd : $it") }	// subject에 2번째 Observer 등록, (subject - Observable역할)
runBlocking { delay(200) }			// 0.2초 대기

// 결과

1st : 0
1st : 1
1st : 2
1st : 3
2nd : 0
2nd : 1
2nd : 2
2nd : 3
1st : 4
2nd : 4
1st : 5
2nd : 5
```



----

# 용어 정리

`observable` : 데이터 흐름에 맞게 알림을 보내 observer가 데이터를 사용할 수 있도록 해줌

`observeOn` : observable이 다음 처리를 진행할 때 사용할 스레드 지정

`subscribe` : observer 는 observable을 구독해 데이터 소비

`subscribeOn` : 구독에서 사용할 스레드 지정

`doOnsubscribe` : observable 이 구독 될 때 호출되는 콜백함수

`doFinally` : observalbe이 종료될 때 실행

`dispoasable` : observer가 더이상 필요 없거나 Data를 받아오지 않을 때를 위한 객체
