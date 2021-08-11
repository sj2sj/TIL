
<br/>

# 지네릭스 (Generics)
> 컴파일 시 타입을 체크해주는 기능 (complie-time type check) <br/>
> 객체의 타입 안정성 ↑, 형변환의 번거로움은 ↓
<hr>
<br/>

## 1. 용어
~~~JAVA
Class Box<T> {}
~~~
- Box<T>: 지네릭 클래스. 'T의 Box' 또는 'T Box' 
- T: 타입 변수 또는 타입 매개변수
- Box: 원시 타입

<br/> <hr> <br/>

## 2. 객체 생성과 사용

~~~java
// 1. 참조변수와 생성자에 대입된 타입이 일치해야 함.
Box<Apple> appleBox = new Box<Apple>(); //OK
Box<Apple> appleBox = new Box<Grape>(); //에러

// 2. Apple이 Fruit의 자손일 때
// 대입된 타입이 다르면 에러
Box<Fruit> appleBox = new Box<Apple>(); //에러

// 3. 단, 두 지네릭 클래스가 상속 관계에 있을 때
// Box가 부모, FruitBox가 자식일 때 다형성 OK
Box<Apple> appleBox = new FruitBox<Apple>(); //Ok

// 4. Fruit의 자손들은 add 메서드의 매개변수가 될 수 있음
Box<Fruit> fruitBox = new Box<Fruit>();
fruitBox.add(new Fruit());
fruitBox.add(new Apple()); //OK!!
~~~

<br/> <hr> <br/>

## 3. 제한된 지네릭 클래스
- 타입 매개변수 T에 지정할 수 있는 타입의 종류를 제한할 수 있다.
~~~java
//특정 타입의 자손들만 대입할 수 있게 제한하자 (extends 사용)
class FruitBox<T extends Fruit> { //Fruit 클래스의 자손만 타입으로 지정할 수 있게 됨
    ArrayList<T> list = new ArrayList<>();
}

FruitBox<Apple> appleBox = new FruitBox<>(); //OK!
FruitBox<Toy> appleBox = new FruitBox<>(); //Toy는 Fruit 클래스의 자손이 아니기 때문에 에러!
~~~
~~~java
//인터페이스 구현의 제약이 필요할 때에도 'extends'를 사용한다.
interface Eatable {}
class FruitBox<T extends Eatable> {...}
~~~
~~~java
//클래스 Fruit의 자손이면서 Eatable인터페이스도 구현해야 하는 경우 '&' 기호를 사용한다.
class FruitBox<T extends Fruit & Eatable> {...}
~~~

<br/> <hr> <br/>

## 4. 와일드 카드

> 하나의 참조변수로 대입된 타입이 다른 객체를 참조할 수 있게 하는 것
- 사용법
~~~
<? extends T> : 와일드 카드의 상한 제한. T와 그 자손들만 가능
<? super T> : 와일드 카드의 하한 제한. T와 그 조상들만 가능
<?> : 제한 X, 모든 타입 가능 = <? extends Object>
~~~
~~~java
ArrayList<Product> list = new ArrayList<Tv>(); //대입된 타입이 불일치하기 때문에 에러 발생!

//와일드 카드를 사용
ArrayList<? extends Product> list = new ArrayList<Tv>(); //OK, Product와 그 자손들도 가능

//메서드의 매개변수에도 와일드 카드 사용 가능
static Juice makeJuice(FruitBox<? extends Fruit> box) {
    ...
}
~~~

<br/> <hr> <br/>

## 5. 지네릭 메서드
> 지네릭 타입이 선언된 메서드 (타입 변수는 메서드 내에서만 유효함) <br/>
- 클래스의 타입 매개변수 `<T>`와 메서드의 타입 매개변수 `<T>`는 별개
- 메서드를 호출할 때 마다 타입을 대입해야 함 **(대부분 생략가능)**


<br/> <hr> <br/>

## 6. 지네릭 타입의 형변환
- 지네릭 타입과 원시 타입 간의 형변환 (가능, 하지만 경고. 바람직하지 않음)
~~~java
Box box = null; //원시 타입
Box<Object> objBox = null; //지네릭 타입

box = (Box)objBox; //OK, 지네릭 -> 원시
objBox = (Box<Object>)box; //OK, 원시 -> 지네릭
~~~

<br/>

- 대입된 타입이 다른 지네릭 타입 간의 형변환
~~~java
Box<Object> objBox = null;
Box<String> strBox = null;

objBox = (Box<Object>)strBox; //에러
strBox = (Box<String>)objBox; //에러

/* 와일드 카드를 사용하면 가능 */
Box<? extends Object> wBox = new Box<String>(); //형변환 OK
~~~


<br/> <hr> <br/>

남궁성, *자바의 정석* 책과 강의를 보며 공부한 내용을 정리한 문서입니다.