
# Junit

## Junit이란
- 자바 프로그래밍 언어용 **단위 테스트 프레임워크**
- TDD(Test-driven development, 테스트 주도 개발)면에서 중요하다.


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
