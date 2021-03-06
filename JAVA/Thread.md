<br/>

# 프로세스(Process)와 쓰레드 (Thread)
## 1. 프로세스
- 실행중인 프로그램
- 프로그램을 수행하는데 필요한 데이터와 메모리 등의 자원 + 쓰레드
- 모든 프로그램에는 하나 이상의 쓰레드가 존재하고, 둘 이상의 쓰레드를 가진 프로세스를 **멀티 쓰레드 프로세스**라고 한다.

<br/>

### 멀티 쓰레드의 장점
1. CPU의 사용률 향상
2. 자원을 효율적으로 사용 가능
3. 사용자 응답성 향상
4. 작업이 분리되어 코드가 간결해짐

<br/>

### 멀티 쓰레드의 단점
1. 동기화 (Synchronization)
2. 교착상태 (Deadlock)
3. 기아


<br/> <hr> <br/>

## 2. 쓰레드의 구현과 실행
### (1) Thread Class 상속
~~~java
class MyThread extends Thread {
    //Thread Class의 run() Overriding
    public void run() {/* 작업내용 */}
}
~~~
<br/>

### (2) Runnable Interface 구현
> 📌Thread 클래스를 상속받으면 다른 클래스를 상속받을 수 없기 때문에 Runnable 인터페이스를 구현하는 방법이 일반적이다.
~~~java
class MyThread extends Thread {
    //Runnable Interface의 추상메서드인 run()을 구현
    public void run() {/* 작업내용 */}
}
~~~
<br/>

### (3) 실행 - start()
- run()을 호출하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 단순히 클래스에 선언된 메서드를 호출하는 것이다.
- **start()를 호출**해야 쓰레드가 실행된다.
- start()가 호출되었다고 바로 실행되는 것이 아니라, *OS의 스케줄러가 작성한 스케줄에 의해 실행순서가 결정된다.*
~~~java
Thread t1 = new Thread();
Thread t2 = new Therad();
t1.start();
t2.start();
~~~

<br/> <hr> <br/>

## 3. 데몬 쓰레드
- 다른 일반 쓰레드(데몬 쓰레드가 아닌 쓰레드)의 작업을 돕는 보조적 역할을 하는 쓰레드
- 일반 쓰레드가 종료되면 데몬 쓰레드도 자동으로 종료됨
- 가비지 컬렉터, 자동 저장, 화면 자동갱신 등에 사용됨
- 무한루프와 조건문을 이용해서 실행 후, 대기하다가 특정조건이 만족되면 작업을 수행하고 다시 대기하도록 작성함

### 사용
- 반드시 쓰레드를 실행하기 전에 setDeamon 메서드를 이용하여 데몬쓰레드로 변경해야함
~~~java
Thread t1 = new Thread();
t1.setDaemon(true); //쓰레드를 데몬쓰레드로 만듬
~~~

<br/> <hr> <br/>


## 4. 쓰레드의 실행 제어
### (1) 쓰레드의 상태

~~~
NEW: 쓰레드가 생성되고 아직 start()가 호출되지 않은 상태
RUNNABLE: 실행 중 또는 실행 가능한 상태
BLOCKED: 동기화블럭에 의해 일시정지된 상태(lock이 풀릴 때까지 기다리는 중)
WAITING, TIMED_WAITING: 쓰레드의 작업이 종료되지는 않았으나, 실행가능하지 않은 일시정지 상태.
TERMINATED: 쓰레드의 작업이 종료된 상태
~~~

<br/>

### (2) 쓰레드의 스케줄링과 관련된 메서드
1. sleep(long millis) - 일정시간동안 쓰레드를 멈추게 함
~~~java
static void sleep(long millis)
static void sleep(long millis, int nanos)

try {
    Thread.sleep(1000) //쓰레드를 1초동안 멈추게 한다. 
} catch(InterruptedException e) {}
~~~
- sleep()에 의해 일시정지가 된 쓰레드는 지정된 시간이 다 되거나 interrupt()가 호출되면 InterruptedException 예외가 발생하고, 실행대기 상태가 된다. <br/>
--> 따라서 *try-catch*문으로 예외처리 필요
- sleep()은 항상 현재 실행중인 쓰레드에 대해 작동하기 때문에, th1.sleep(1000) 으로 호출해도 실제 영향을 받는 것은 현재 실행중인 쓰레드이다. <br/>
--> 따라서 *Thread.sleep(1000)* 과 같이 사용해야한다.

<br/>

2. interrupt() / interrupted() - 쓰레드의 작업을 취소함
> 쓰레드가 sleep(), wait(), join()에 의해 일시정지 상태(WATING)에 있을 때, 해당 쓰레드에 대해 interrupt()를 호출하면, Interrupted Exception이 발생하고 실행대기 상태(RUNNABLE)로 바뀐다.
- 진행중인 쓰레드의 작업이 끝나기 전에 취소한다.
- interrupt()는 쓰레드의 작업을 멈추는 것이 아니라 멈춰달라고 요청만 하는 것이다.
- interrupt()를 호출하면 쓰레드의 interrupted상태를 변경시킨다.
- interrupted()는 쓰레드애 대해 interrupt()가 호출 되었는지 알려준다. 호출되었다면 true, 호출되지 않았다면 false를 반환한다.

<br/>

3. suspend(), resume(), stop()
> suspend()와 stop()이 교착상태(deadlock)를 일으키기 쉽게 작성되어있으므로 사용이 권장되지 않음.
- suspend(): sleep()처럼 쓰레드를 멈추게 함
- resume(): suspend()에 의해 정지된 쓰레드를 다시 실행대기 상태로 만듦
- stop(): 호출되는 즉시 쓰레드가 종료

<br/>

4. yield()
- 자신에게 주어진 실행시간을 다음 차례의 쓰레드에게 양보한다.

<br/>

5. join()
- 자신이 하던 작업을 잠시 멈추고, 다른 쓰레드가 지정된 시간동안 작업을 수행하도록 한다.
- 현재 쓰레드가 아닌 특정 쓰레드에 대해 동작한다. (따라서 static이 아님)


<br/> <hr> <br/>

## 5. 쓰레드의 동기화
> 한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것
- 임계 영역: 간섭받지 않아야 할 코드 영역, 락(lock)을 얻은 단 하나의 쓰레드만 출입가능하다.

<br/>

### (1) synchronized를 이용한 동기화
> 가능한 1번 방법보다 2번 방법을 사용하는 것이 효율적일 것이다.
~~~java
/* 1. 메서드 전체를 임계 영역으로 지정 */
public synchronized void calcSum() {
    ...
}

/* 2. 특정 영역을 임계 영역으로 지정 */
synchronized (객체의 참조변수) {
    ...
}
~~~

<br/>

### (2) wait() / notify()
- 동기화 효율을 높이기 위한 방법
- Object 클래스에 정의되어 있음
- 동기화 블럭 내에서만 사용 가능함
~~~
wait(): 객체의 lock을 풀고 쓰레드의 해당 객체를 waiting pool에 넣음
notify(): waiting pool에서 대기중인 쓰레드 중 하나를 깨움
notifyAll(): waiting pool에서 대기중인 모든 쓰레드를 깨움
~~~

<br/> <hr> <br/>

남궁성, *자바의 정석* 책과 강의를 보며 공부한 내용을 정리한 문서입니다.