
# Junit

## Junit이란
- 자바 프로그래밍 언어용 **단위 테스트 프레임워크**
- TDD(Test-driven development, 테스트 주도 개발)면에서 중요하다.

<br>


> ❓ 단위 테스트란 <br>
> - 소스코드의 특정 모듈이 정확히 동작하는지 테스트하는 것
> - 메서드에 대해 테스트 케이스를 작성하는 것


<br><hr><br>


## Junit Annotation
### @Test
- @Test가 선언된 메서드는 테스트를 수행하는 메서드이다.
- Junit에서는 각각의 테스트가 서로 영향을 미치지 않고 독립적으로 실행되므로 테스트를 진행할 메서드마다 @Test 어노테이션을 작성한다.

<br>

### @Ignore
- 테스트를 동작하지 않도록 한다.

<br>

### @Before / @After
- 각각 @Test 메서드가 실행되기 전, 후에 실행된다.

<br>

### @BeforeClass / @AfterClass
- @Test 메서드보다 먼저, 나중에 **단 한번만** 실행된다.


<br><hr><br>

## 자주 사용하는 메서드
|메서드|설명|
|-|-|
|assertArrayEquals(a, b)|배열 A와 B가 일치하는지 확인|
|assertEquals(a, b)|객체 A와 B가 같은 값을 가지는지 확인|
|assertEquals(a, b, c)|객체 A와 B가 값이 일치하는지 확인 (a: 예상 값, b: 결과 값, c: 오차 범위)|
|assertSame(a, b)|객체 A와 B가 같은 객체인지 확인|
|assertTrue(a)|조건 A가 참인지 확인|
|assertNotNull(a)|객체 A가 NULL이 아닌지 확인|

> 더 많은 메서드 확인<br> 
> http://junit.sourceforge.net/javadoc/org/junit/Assert.html