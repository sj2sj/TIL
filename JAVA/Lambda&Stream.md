
# 람다와 스트림 (Lambda & Stream)

## 1. 람다식 (Lambda expression)
> 메서드를 하나의 식으로 표현한 것 (=익명 함수)

<br/>

### (1) 람다식 작성 방법
~~~java
//기본 자바 문법으로 사용
int max(int a, int b) {
    return a > b ? a : b;
}

/* 작성 방법 */
//1. 메서드의 이름과 반환타입을 제거하고 '->'를 블록{} 앞에 추가
(int a, int b) -> {
    return a > b ? a : b;
}

//2. 반환값이 있는 경우, 식이나 값만 적고 return문 생략 가능 (끝에 세미콜론 X)
(int a, int b) -> a > b ? a : b

//3. 매개변수 타입이 추론 가능할 경우 생략 가능 (대부분 생략 가능)
(a, b) -> a > b ? a : b



/* 작성 시 주의 사항 */
//1. 매개변수가 하나인 경우 괄호 생략 가능 (단, 타입이 없을 때만!!)
a -> a * a //OK!!
int a -> a * a //타입이 있기 때문에 생략 불가능!! 에러

//2. 블록 안의 문장이 하나뿐 일때, 괄호 생략 가능! (단, 끝에 세미콜론 X!!)
(int i) -> System.out.println(i)

~~~

<br/>

### (2) 함수형 인터페이스 (Functional Interface)
- 단 하나의 추상 메서드만 선언된 인터페이스 (람다식과 인터페이스의 메서드가 1:1로 연결되기 위해)
- 람다식을 다루기 위한 인터페이스
- `@FunctionalInterface`를 붙이면 함수형 인터페이스를 올바르게 정의했는지 확인해줌
~~~java

//람다식을 다루기 위한 참조변수의 타입은 함수형 인터페이스!!
MyFunction f = (a, b) -> a > b ? a : b;

int value = f.max(3, 5);
System.out.println("value: " + value); //value: 5

//함수형 인터페이스
@FunctionalInterface
interface MyFunction {
    public abstract int max(int a, int b);
}
~~~

<br/>

### (3) java.util.function 패키지
> 자주 사용되는 다양한 함수형 인터페이스를 제공

|함수형 인터페이스|메서드|매개변수|반환값|
|-|-|:-:|:-:|
| java.lang.Runnable |void run()|X|X|
|Supplier\<T>|T get()|X|O|
|Consumer\<T>|void accept(T t)|O|X|
|Function\<T, R>|R apply(T t)|O|O|
|Predicate\<T>|boolean test(T t)|1|boolean|

<br/>


> 매개변수가 2개인 함수형 인터페이스

|함수형 인터페이스|메서드|매개변수|반환값|
|-|-|:-:|:-:|
|BiConsumer\<T, U>|void accept(T t, U u)|2|X
|BiPredicate\<T, U>|boolean test(T t, U u)|2|boolean
|BiFunction\<T, U, R>|R apply(T t, U u)|2|O


<br/>

> 매개변수의 타입과 반환타입이 일치하는 함수형 인터페이스

|함수형 인터페이스|메서드|설명
|-|-|-|
|UnaryOperator\<T>|T apply(T t)|Function의 자손, 매개변수와 결과의 타입이 같음|
|BinaryOperator\<T>|T apply(T t, T t)|BiFunction의 자손, 매개변수와 결과의 타입이 같음|


<br/>

> 컬렉션 프레임워크와 함수형 인터페이스

|인터페이스|메서드|설명|
|-|-|-|
|Collection|boolean removeIf(Predicate\<E> filter)|조건에 맞는 요소 삭제
|List|void replaceAll(UnaryOperator\<E> operator)|모든 요소를 변환하여 대체
|Iterable|void forEach(Consumer\<T> action)|모든 요소에 작업 action을 수행
|Map|V compute(K key, BiFunction\<K, V, V>) f|지정된 키의 값에 작업 f를 수행
||V computeIfAbsent(K key, Function\<K, V> f)|키가 없으면, 작업 f 수행 후 추가
||V computeIfPresent(K key, BiFunction\<K, V, V> f)|지정된 키가 있을 때, 작업 f를 수행
||V merge(K key, V value, BiFunction\<V, V, V> f)|모든 요소에 병합작업 f를 수행
||void forEach(BiConsumer\<K, V> action)|모든 요소에 작업 action을 수행
||void replaceAll(BiFunction\<K, V, V> f)|모든 요소에 치환작업 f를 수행

<br/> <hr> <br/>

### (4) 메서드 참조
> '메서드 참조'로 람다식 사용을 더 간단히 할 수 있다.

|종류|람다|메서드 참조|비고|
|-|-|-|-|
|static메서드 참조|(x) -> ClassName.method(x)|ClassName::method
|인스턴스 메서드 참조|(obj, x) -> obj.method(x)|ClassName::method
|특정 객체의 인스턴스 메서드 참조| (x) -> obj.method(x)|obj::method|자주 사용되지 X

<br/>

**static메서드 참조**
~~~java
// 1. 기존 method 사용방법
Integer method(String s) {
    return Integer.parseInt(s);
}

// 2. 람다식
Function<String, Integer> f = (String s) -> Integer.parseInt(s);

// 3. 메서드 참조
Function<String, Integer> f = Integer::parseInt;
~~~

<br/>

**생성자의 메서드 참조**
~~~java
/* 생성자 호출 */
//1. 람다식
Supplier<MyClass> s = () -> new MyClass();

//2. 메서드 참조
Supplier<MyClass> s = MyClass::new;

/* 배열 생성 */
//1. 람다식
Function<Integer, int[]> f = x -> new int[x];

//2. 메서드 참조
Function<Integer, int[]> f = int[]::new;
~~~

<br/> <hr> <br/>

## 2. 스트림 (Stream)
> 데이터 소스가 무엇이던 간에 같은 방식으로 다룰 수 있게 하는 것
- 스트림은 데이터 소스를 변경하지 않는다.
- 스트림은 `일회용`이다. (필요할 경우 스트림을 다시 생성해야 한다.)
- 스트림은 작업을 `내부 반복`으로 처리한다. 
- `지연된 연산`: 최종 연산이 수행되기 전까지 중간 연산이 수행되지 않는다.
- `병렬 스트림`: 스트림의 작업을 병렬로 처리한다. *(parallel() 메서드 사용)*
- `기본형 스트림`: 데이터 소스의 요소를 기본형으로 다루는 스트림, IntStream, LongStream 등이 제공된다.

<br/>

### (1) 스트림 만들기


#### **a. 컬렉션**
- Collection 인터페이스의 stream()으로 컬렉션을 스트림으로 변환

~~~java
Stream<E> stream() //Collection 인터페이스의 메서드

List<Integer> list = Arrays.asList(1, 2, 3, 4, 5);
Stream<Integer> intStream = list.stream(); //list를 스트림으로 변환

intStream.forEach(System.out::println); //스트림의 모든 요소를 출력한다.
~~~

<br/>

#### **b. 배열**
- Stream과 Arrays에 static 메서드로 정의되어 있음

~~~java
//객체 배열로부터 스트림 생성
Stream<T> Stream.of(T... values) //가변인자
Stream<T> Stream.of(T[])
Stream<T> Arrays.stream(T[])
Stream<T> Arrays.stream(T[ array, int startInclusive, int endExclusive])

//기본형 배열로부터 스트림 생성
IntStream IntStream.of(int... values)
IntStream IntStream.of(int[])
IntStream Arrays.stream(int[])
IntStream Arrays.stream(int[] array, int startInclusive, int endExclusive)
~~~

<br/>

#### **c. 특정 범위의 정수**
- IntStream과 LongStream은 지정된 범위의 연속된 정수를 스트림으로 생성해서 반환하는 메서드를 가짐
~~~java
//end가 범위에 포함 X
IntStream intStream = IntStream.range(int begin, int end) 

//end가 범위에 포함 O
IntStream intStream = IntStream.rangeClosed(int begin, int end)
~~~

<br/>

#### **d. 난수**
- Random 클래스에 정의된 메서드를 사용하면 크기가 정해지지 않은 무한 스트림을 반환
- 따라서 limit()도 같이 사용해서 스트림의 크기를 제한 (유한스트림으로 만든다.)
~~~java
IntStream intStream = new Random().ints(); //무한 스트림
intStream.limit(5).forEach(System.out::println) //5개 요소만 출력

/* Random 클래스 메서드의 매개변수에 사이즈 지정이 가능하다 */
IntStream intStream2 = new Random().ints(5); //5개의 난수가 들어있는 스트림

/* 난수 범위 지정도 가능하다. */
IntStream intStream3 = new Random().ints(5, 1, 10); //1부터 9까지의 난수가 5개 들어있는 스트림
~~~

<br/>

#### **e. 람다식**
- Stream 클래스의 iterate()와 generate()는 람다식을 매개변수로 받아, 람다식에 의해 계산되는 값들을 요소로 하는 무한 스트림을 생성한다.
    * `iterate()`: 이전 요소를 seed로 해서 다음 요소를 계산함 <br/>
    static \<T> Stream\<T> iterate(T seed, UnaryOperator\<T> f)
    * `generate()`: seed를 사용하지 않음 <br/>
    static \<T> Stream\<T> generate(Supplier\<T> s)
> ❗️ iterate()와 generate()에 의해 생성된 스트림은 기본형 스트림 타입(IntStream 등)의 참조변수로 다룰 수 없다.
~~~java
//iterate() 사용
Stream<Integer> evenStream = Stream.iterate(0, n -> n+2); //0, 2, 4, 6, ...

//generate() 사용
Stream<Double> randomStream = Stream.generate(Math::random);
~~~

<br/>

#### **f. 파일**
- java.nio.file.Files의 list()는 지정된 디렉토리(dir)에 있는 파일의 목록을 스트림으로 생성해서 반환
~~~java
Stream<Path> pathStream = Files.list(Path dir)
~~~

<br/>

#### **g. 빈 스트림**
- 요소가 하나도 없는 비어있는 스트림 생성
~~~java
Stream emptySteam = Stream.empty();
~~~

<br/> <hr> <br/>

### (2) 스트림의 중간 연산
> 연산결과가 스트림인 연산, `반복적으로 적용` 가능

<br/>

#### **a. 스트림 자르기 - skip(), limit()**
- 스트림의 일부를 잘라낼 때 사용한다.
    * Stream\<T> skip(long n)
    * Stream(T) limit(long maxSize)
~~~java
IntStream intStream = IntStream.rangeClosed(1, 10); //1~10을 가진 스트림

//앞의 3개 요소를 skip하고, 스트림의 요소를 5개로 제한함
intStream.skip(3).limit(5).forEach(System.out::print); //45678
~~~

<br/>

#### **b. 스트림의 요소 걸러내기 - filter(), distinct**
- `distinct()`: 스트림에서 중복 요소 제거
    * Stream\<T> distinct()
- `filter()`: 주어진 조건에 맞지 않는 요소를 걸러냄 (조건식에 맞는 요소가 반환됨)
    * Stream\<T> filter(Predicate\<? super T> predicate)
~~~java
//distinct()
IntStream intStream = IntStream.of(1, 2, 2, 3, 3, 3, 4, 5, 5, 6);
IntStream.distinct().forEach(System.out::print); //123456

//filter() - 매개변수로 람다식 사용 가능
IntStream intStream = IntStream.rangeClosed(1, 10) //1~10
intStream.filter(i -> i%2 == 0).forEach(System.out::print); //246810 (짝수만 반환)
~~~

<br/>

#### **c. 정렬 - sorted**
- 스트림을 정렬할 때 사용한다.
    * Stream\<T> sorted() : 기본 정렬(Comparable)
    * Stream\<T> sorted(Comparator\<? super T> comparator)
~~~java
Stream<String> strStream = Stream.of("dd", "aaa", "CC", "cc", "b");
strStream.sorted().forEach(System.out::print); //CCaaabccdd
~~~

<br/>

<br/> <hr> <br/>

### (3) 스트림의 최종 연산
> 연산결과가 스트림이 아닌 연산, `단 한번만 적용` 가능 (스트림의 요소를 소모함)

<br/> <hr> <br/>

남궁성, *자바의 정석* 책과 강의를 보며 공부한 내용을 정리한 문서입니다.