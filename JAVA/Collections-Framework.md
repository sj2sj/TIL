<br/>

# 컬렉션 프레임워크 (Collections Framework)
> 다수의 데이터를 저장하는 클래스들을 표준화한 것   
> 인터페이스와 다형성을 이용해 재사용성이 높은 코드를 작성할 수 있다.

<hr> <br/>

## → List 인터페이스
> 중복을 허용하고 저장순서가 유지된다.

<br/>

### 1. ArrayList   
- 가장 많이 사용하는 컬렉션 클래스
- 기존의 Vector를 개선한 클래스
- Object 배열을 이용하여 데이터를 순차적으로 저장
- 데이터를 읽어오고 저장할 때의 효율은 좋지만, 용량 변경 시에는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 효율이 떨어진다.
~~~java
import java.util.ArrayList;

class ArrayListEx {
    public static void main(String[] args) {
        ArrayList ar = new ArrayList(10); //생성 시에 용량 설정 가능

        ar.add("a"); //추가
        ar.add("b");
        ar.add("c");

        System.out.println(ar.get(1)); //b 출력
        
        ar.remove(2); //c 삭제
    }
}
~~~

<br/>

### 2. LinkedList
- 배열의 단점을 보완하기 위한 컬렉션 클래스
- 불연속적으로 존재하는 데이터를 서로 연결(link)한 형태
- 각 node들은 자신과 연결된 다음 요소에 대한 참조값 (주소)과 데이터로 구성됨

~~~java
/* 링크드 리스트 */
class Node {
    Node next; //다음 요소의 주소 저장
    Object obj; //데이터 저장
}
~~~
- *따라서,* 데이터 삭제가 간단하고 속도가 빠름
- 그러나 이동방향이 단방향이기 때문에 이전요소에 대한 접근이 어려움. <br/>
--> 이 점을 보완한 것이 **더블 링크드 리스트.**

~~~java
/* 더블 링크드 리스트 */
class Node {
    Node next;  //다음 요소의 주소 저장
    Node previous; //이전 요소의 주소 저장
    Object obj; //데이터 저장
}
~~~
- **실제로 LinkedList의 구현은 더블 링크드 리스트로 되어있음**

<br/>

### 3. Stack
> 마지막에 저장한 데이터를 가장 먼저 꺼내는 LIFO(**L**ast **I**n **F**irst **O**ut) 구조 <br/>
 예를 들어, 0, 1, 2 순서대로 데이터를 넣었다면 꺼낼 때엔 2, 1, 0 순서로 나오게 됨.
- 저장은 push, 추출은 pop
- 활용: 수식괄호검사, 웹브라우저의 뒤로/앞으로 등

<br/>


### 4. Queue
> 처음에 저장한 데이터를 가장 먼저 꺼내는 FIFO(**F**irst **I**n **F**irst **O**ut) 구조 <br/>
> 예를 들어, 0, 1, 2 순서대로 데이터를 넣었다면 꺼낼 때도 0, 1, 2 순서로 나오게 됨.
- 저장은 offer, 추출은 poll
- 활용: 최근사용문서, 버퍼(buffer) 등
- Queue는 인터페이스이므로, 구현체를 사용해야 한다. <br/>
--> **Deque**: Queue의 변형으로, 양쪽 끝에서 추가/삭제가 가능하다. (Stack+Queue)

<br/>
<hr/>
<br/>

## → Set 인터페이스
> 중복을 허용하지 않고, 저장 순서를 유지하지 않는다. <br/>
> 순서를 유지하지 않기 때문에 정렬이 필요하면 List 객체 생성해서 사용할 수 있다.

<br/>

### 1. HashSet
- Set인터페이스를 구현한 대표적인 컬렉션 클래스
- 순서를 유지하려면, LinkedHashSet클래스 사용
- add() 사용 시 기존에 저장된 요소와 같은 것인지 판별하기 위해 equals()와 hashCode()를 호출하기 때문에 두 메서드의 적절한 오버라이딩이 필요하다.

<br/>

### 2. TreeSet
- 범위 검색과 정렬에 유리한 컬렉션 클래스
- 이진 검색 트리(binary search tree)라는 자료구조의 형태로 데이터를 저장하는 컬렉션 클래스
- 이진 트리는 모든 노드가 최대 2개의 하위 노드를 가진다. <br/>
    --> 각 노드가 tree형태로 연결됨
- HashSet보다는 데이터 추가와 삭제에 시간이 더 오래 걸린다.

<br/>
<hr/>
<br/>

## → Map 인터페이스
> 키(key)와 값(value)을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스 <br/>
> 키는 중복될 수 없지만 값은 중복을 허용한다.

<br/>

### 1. HaspMap
- Map인터페이스를 구현한 대표적인 컬렉션 클래스
- 순서를 유지하려면, LinkedHashMap클래스 사용
- ***Hashing***을 사용하기 때문에 많은 양의 데이터를 검색하는데 뛰어난 성능을 보인다.
    > **Hasing**이란? <br/>
    해시 함수를 이용해서 데이터를 해시 테이블에 저장하고 검색하는 것. <br/>
    해시함수는 데이터가 저장된 곳을 알려주기 때문에 원하는 데이터를 빠르게 찾을 수 있다. <br/>
    ✏️ 해시 함수는 같은 key에 대해 항상 같은 해시코드를 반환해야 함! <br/>
    ✏️ 서로 다른 key라도 같은 값의 해시 코드를 반환할 수 있음!

<br/>

### 2. TreeMap
- 범위 검색과 정렬에 유리한 컬렉션 클래스
- TreeSet과 동일한 특성 (Map인 것만 다름)

<br/>
<hr/>
<br/>

## → Iterator
> 컬렉션에 저장된 데이터에 접근하는데 사용되는 인터페이스 <br/>
> 컬렉션에 저장된 요소들을 읽어오는 방법들을 표준화한 것

~~~java
//Collection 객체에 담는 이유: 나중에 ArrayList가 아닌 다른 컬렉션 클래스를 사용할 때 수정하기 편하기 때문에
Collection collection = new ArrayList();
Iterator iterator = collection.iterator();

//iterator는 일회용이기 때문에 요소를 다 읽으면 새로운 iterator를 가져와야함.
while (iterator.hasNext()) { //읽어올 요소가 있는가?
    System.out.println(iterator.next()); //요소 읽어옴
}
~~~
- Map은 Collection의 자손이 아니기 때문에 Map에는 iterator()가 없음. <br/>
--> *따라서,* keySet(), entrySet(), values()를 호출해서 iterator()를 사용해야 한다.

~~~java
Map map = new HashMap();

//map.anteySet()의 실행결과가 Set이기 때문에, iterator()를 호출 가능함.
Iterator iterator = map.entrySet().iterator();
~~~

<br>

남궁성, *자바의 정석* 책과 강의를 정리한 문서입니다.