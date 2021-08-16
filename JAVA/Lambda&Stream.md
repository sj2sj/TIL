
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
- @FunctionalInterface를 붙이면 함수형 인터페이스를 올바르게 정의했는지 확인해줌
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
|Supplier`<T>`|T get()|X|O|
|Consumer`<T>`|void accept(T t)|O|X|
|Function`<T, R>`|R apply(T t)|O|O|
|Predicate`<T>`|boolean test(T t)|1|boolean|

<br/>


> 매개변수가 2개인 함수형 인터페이스

|함수형 인터페이스|메서드|매개변수|반환값|
|-|-|:-:|:-:|
|BiConsumer`<T, U>`|void accept(T t, U u)|2|X
|BiPredicate`<T, U>`|boolean test(T t, U u)|2|boolean
|BiFunction`<T, U, R>`|R apply(T t, U u)|2|O


<br/>

> 매개변수의 타입과 반환타입이 일치하는 함수형 인터페이스

|함수형 인터페이스|메서드|설명
|-|-|-|
|UnaryOperator`<T>`|T apply(T t)|Function의 자손, 매개변수와 결과의 타입이 같음|
|BinaryOperator`<T>`|T apply(T t, T t)|BiFunction의 자손, 매개변수와 결과의 타입이 같음|


<br/>

> 컬렉션 프레임워크와 함수형 인터페이스

|인터페이스|메서드|설명|
|-|-|-|
|Collection|boolean removeIf(Predicate`<E>` filter)|조건에 맞는 요소 삭제
|List|void replaceAll(UnaryOperator`<E>` operator)|모든 요소를 변환하여 대체
|Iterable|void forEach(Consumer`<T>` action)|모든 요소에 작업 action을 수행
|Map|V compute(K key, BiFunction`<K, V, V>`) f|지정된 키의 값에 작업 f를 수행
||V computeIfAbsent(K key, Function`<K, V>` f)|키가 없으면, 작업 f 수행 후 추가
||V computeIfPresent(K key, BiFunction`<K, V, V>` f)|지정된 키가 있을 때, 작업 f를 수행
||V merge(K key, V value, BiFunction`<V, V, V>` f)|모든 요소에 병합작업 f를 수행
||void forEach(BiConsumer`<K, V>` action)|모든 요소에 작업 action을 수행
||void replaceAll(BiFunction`<K, V, V>` f)|모든 요소에 치환작업 f를 수행

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

남궁성, *자바의 정석* 책과 강의를 보며 공부한 내용을 정리한 문서입니다.