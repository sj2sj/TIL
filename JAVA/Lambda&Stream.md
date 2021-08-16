
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
- java.lang.Runnable : void run() : 매개변수 X, 반환값 X
- Supplier`<T>` : T get() : 매개변수 X, 반환값 O
- Consumer`<T>` : void accept(T t) : 매개변수 O, 반환값 X
- Function`<T, R>` : R apply(T t) : 매개변수 O, 반환값 O
- Predicate`<T>` : boolean test(T t) : 조건식 표현 / 매개변수 1개, 반환 타입 boolean

<br/>

> 매개변수가 2개인 함수형 인터페이스
- BiConsumer`<T, U>` : void accept(T t, U u) : 매개변수 2개, 반환값 X
- BiPredicate`<T, U>` : boolean test(T t, U u) : 매개변수 2개, 반환값 boolean
- BiFunction`<T, U, R>` : R apply(T t, U u) : 매개변수 2개, 반환값 1개

<br/>

> 매개변수의 타입과 반환타입이 일치하는 함수형 인터페이스
- UnaryOperator`<T>` : T apply(T t) : Function의 자손, 매개변수와 결과의 타입이 같음
- BinaryOperator`<T>` : T apply(T t, T t) : BiFunction의 자손, 매개변수와 결과의 타입이 같음

<br/> <hr> <br/>

<br/> <hr> <br/>

남궁성, *자바의 정석* 책과 강의를 보며 공부한 내용을 정리한 문서입니다.