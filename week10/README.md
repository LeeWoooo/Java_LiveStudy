Java Live Study
===
## 10주차 과제


### 학습 목록

1. Thread 클래스와 Runnable 인터페이스
2. 쓰레드의 상태
3. 쓰레드의 우선순위
4. Main 쓰레드
5. 동기화
6. 데드락

---

<br>

## Thread

프로레스란 간단히 말해서 실행중인 프로그램이다. 프로그램을 실행하면 OS로 부터 실행에 필요한 자원을 할당받아 프로세스가 된다. <br>

**프로세스는 프로그램을 수행하는데 필요한 데이터와 메모리 등의 자원 그리고 쓰레드로 구성되어 있다.** <br>

그래ㅔ서 모든 프로세스에는 최소한 하나 이상의 쓰레드가 존재하며 둘 이상의 쓰레드를 가진 프로세스를 멀티쓰레드 프로세스 라고한다.<br>

멀티 쓰레드의 장점

1. CPU의 사용률을 향상시킨다.
2. 자원을 보다 효울적으로 사용할 수 있다.
3. 사용자에 대한 응답성이 향상된다.
4. 잡업이 분리되어 코드가 간결해진다.

<br>

이와 같이 멀티 쓰레드는 장점을 가지고 있다 하지만 장점만 존재하는 것이 아니다. 여러 쓰레드는 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에 발생할 수 있는 동기화(Synchronuzation), 교착상태(deadlock)와 같은 문제를 발생시킬 수 있다.

## Thread 클래스와 Runnable 인터페이스

쓰레드를 구현하는 방법은 Thread class를 상속받는 방법과 Runnable인터페이스를 구현하는 방법 두가지가 있다. 단 **Thread class를 상속받아서 사용하면 다른 class를 상속받을수 없기 때문에 주로 인터페이스를 구현해서 사용한다.** <br>

1. Thread class를 상속받는 경우

<img src = https://user-images.githubusercontent.com/74294325/105458060-34ddce00-5ccb-11eb-8cfe-f2147a43fa74.png>


```java
//1.Thread를 상속받는다.
public class Myclass extends Thread{

//2.run method()를 Override 하고 동시에 처리되어야 할 code 정의
@Overrid
public void run(){
    //동시에 처리되어야 할 code 정의
}

//3.자식 class로 객체를 생성한다. (부모인 Thread가 생성된다.)
Myclass mc = new Myclass();

//4.Thread class의 start()를 호출하여 run()를 호출한다.
mc.start(); //start에서 run을 호출하면 Override한 자식의 run()이 호출된다.
}
```

2. Runnable 인터페이스를 구현

<img src = https://user-images.githubusercontent.com/74294325/101603988-ae836a80-3a43-11eb-962c-45b95f0971fa.png>

```java
//1. Runnable Interface를 구현한다.
public class Myclass implements Runnable{

//2.run method()를 Override 하고 동시에 처리되어야 할 code 정의
@Overrid
public void run(){
    //동시에 처리되어야 할 code 정의
}

//3.자식 class로 객체를 생성한다. (부모인 Thread가 생성된다.)
Myclass mc = new Myclass();
}

//4.Thread class를 이용해 객체를 만들고 매개변수로 Runnable을 구현한 class를 넣어준다.
Thread t = new Thread(mc); 
//Thread class의 생성자 중 매개변수로 Runnable을 받는 생성자가 있다. mc와 Runnable은 is-a관계

//5.Thread class의 start()를 호출하여 run()를 호출한다.
t.start(); //Has-A관계로 인해 Override한 run()이 호출된다.

//Runnable 인터페이스는 Single Abstract Method이기 때문에 람다로 구현가능하다.
@FuncationalInterface
Runnable test = () ->{
    //작업 내용
}
```

<br>

쓰레드를 이렇게 생성했다고 자동으로 실행이 되는 것이 아니다. start()의 method를 호출해야지만 쓰레드가 실행된다. **정확히 이야기 하자면 바로 실행이 되는 것이 아니라 자신의 차례가 되었을 때 실행이 된다.**<br>

또 중요한 것은 **쓰레드는 한번 실행되었다가 종료가 되면 다시 실행할 수 없다.**<br>

하나의 쓰레드에는 단 한번의 start()만 사용할 수 있다. 간단하게 이야기 하자면 동일 쓰레드를 반복해서 실행을 해야 한다면 한번 실행 후 다시 생성해서 실행하고를 반복 하면 된다.

<br>

```java
class Test extends Thread{
    public void run(){
        for(int i = 0; i < 10; i++){
            System.out.println(getName()); //getName은 쓰레드의 이름을 반환한다.
        }
    }
}

Test t = new Test();
t.start();
//t.strat() //한번 더 실행하면 오류 발생한다.
```

<br>

<img src = https://user-images.githubusercontent.com/74294325/105459620-e3830e00-5ccd-11eb-8b89-f3103a65af9f.JPG>

<br>

이와같이 한번 더 실행시키면 IllegalThreadStateException이 발생한다.

<br>

한번 더 쓰레드를 실행 시켜야 할 때는 이렇게 하자.
```java
Test t = new Test();
t.start();
t = new Tesst();
t.start();
```

<br>

---

<br>

## start() , run()

쓰레드를 실행시킬 때 run()이 아닌 start()를 호출하는 것에 대해서 알아보자.<br>

**main method에서 run을 호출하는 것은 생성된 쓰레드를 실행 하는 것이 아니라 클래스 안에 있는 method를 실행하는 것 뿐이다.**

<br>

<img src =https://user-images.githubusercontent.com/74294325/105462106-838e6680-5cd1-11eb-8554-c3427b67349c.JPG>

run()을 호출 했을 때의 호출stack이다.<br>

<br>

반면에 start()는 새로운 쓰레드가 작업을 실행하는데 필요한 **호출stack을 생성한 다음에 run()을 호출해서 생성된 호출stack에 run()이 첫번째로 올라가게 한다.** <br>

모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출stack을 필요로 하기 때문에, 새로운 쓰레드를 생성하고 실행시킬 때마다 **새로운 호출 스택이 생성되고 쓰레드가 종료되면 호출stack은 소멸된다.** <br>

<br>

<img src = https://user-images.githubusercontent.com/74294325/105463311-390de980-5cd3-11eb-8ea9-822e7183a339.JPG>

<br>

1. main method에서 쓰레드의 start()를 호출한다.
2. start()는 새로운 쓰레드를 생성하고 쓰레드가 작업하는데 필요한 호출stack을 생성한다.
3. 새로 생성된 호출stack에 run()이 호출되어 쓰레드의 독립된 공간에서 작업을 수행한다.
4. 이제는 호출 스택이 2개이므로 스케줄러가 정한 순서에 의해서 번갈아 가면서 실행된다.

<br>

---

<br>

## main Thread

main method의 작업을 수행하는 것도 쓰레드 이며, 이를 main Thread라고 한다. **main Thread는 프로그램이 시작되면 가장 먼저 시작되는 스레드이다.** 다른 쓰레드들은 메인 쓰레드에서 생성이 되며 다른 쓰레드가 생성되지 않고 메인 쓰레드만 실행이 된다면 싱글 쓰레드로 메인 쓰레드의 작업이 종료되는 순간 프로세스도 종료된다. 반대로 **메인 쓰레드가 여러 개의 쓰레드를 실행한다면 멀티 쓰레드로 메인 쓰레드가 종료되어도 다른 쓰레드 들이 종료되기 전까지 프로세스가 종료되지 않는다.**

>실행중인 쓰레드가 하나도 없을 시 프로그램은 종료된다.

<br>

---

<br>

## Thread의 우선순위

쓰레드는 우선순위라는 속성(멤버변수)을 가지고 있는데 이 우선순위의 값에 따라 쓰레드가 얻는 실행시간이 달라진다. 쓰레드가 수행하는 작업의 중요도에 따라 쓰레드의 우선순위를 서로 다르게 지정하여 특정 쓰레드가 더 많은 작업시간을 갖도록 할 수 있다.<br>

### Thread의 우선순위 지정하기

쓰레드의 우선순위와 관련된 method와 상수는 다음과 같다.
```java
void setPriority(int newPriority); //쓰레드의 우선순위를 지정한 값으로 변경한다.
int getPriority(); //쓰레드의 우선순위를 반환한다.

public static final int MAX_PRIORITY = 10; //최대 우선순위
public static final int MIN_PRIORITY = 1; //최소 우선순위
public static final int NORM_PRIORITY = 5; //보통 우선순위
```
<br>

쓰레드가 가질 수 있는 우선순위의 범위는 1~10이며 숫자가 높을수록 우선순위가 높다.<br>
**한가지 더 알아두어야 할 것은 쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다.** main method를 수행하는 쓰레드는 우선순위가 5이므로 main method내에서 생성하는 thread의 우선순위는 자동적으로 5가 된다. <br>


<br>

---

<br>

## Thread의 실행

### Thread 스케줄링

우선순위를 통해 쓰레드 간 스케줄링을 할 수 있지만 이것만으로는 멀티쓰레드 프로그램을 만들기에는 조금 부족하다. 스케줄링을 잘 하기 위해서는 쓰레드의 상태와 관련된 method를 잘 알아야 한다.

method | 설명
:-- | :---
static void sleep(long millis) | 지정된 시간(ms단위)동안 쓰레드를 일시정지시킨다. 지정 시간이 지나면 자동적으로 다시 실행
void join(),void join(long millis) | 지정된 시간동안 쓰레드가 실행되도록 한다. 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 쓰레드로 돌아와 실행을 계속한다.
void interrupt() | sleep()이나 join()에 의해 일시정지상태인 쓰레드를 깨워서 실행대기상태로 만든다. 해당 쓰레드 에서는 InterruptedException이 발생함으로 일시정지상태를 벗어나게 된다.
void stop() | 쓰레드를 즉시 종료시킨다.
static void yield() | 실행 중에 자신에게 주어진 실행시간을 다른 쓰레드에게 양보하고 자신은 실행대기상태가 된다.

<br>

이외 resume(),suspend()도 있지만 쓰레드를 교착상태로 만들기 쉽기 때문에 deprecated 되었다.


### Thread의 상태

쓰레드는 생성 된 이후 여러가지의 상태를 가질 수 있다.

상태 | 설명
:---: | :---:
NEW | 쓰레드가 생성되고 아직 start()가 호출되지 않은 상태.
RUNNABLE | 실행 중 또는 실행 가능한 상태
BLOCKED | 동기화 블럭에 의해서 일시정지된 상태(lock이 플릴 때 까지 기다리는 상태)
WAITING | 쓰레드의 작입이 종료되지는 않았지만 실행 가능하지 않은(runnable) 일시정지 상태.
TIMED_WAITING  | 일시정지시간이 지정된 경우.
TERMINATED | 쓰레드의 작업이 종료된 상태

<br>

<img src = https://user-images.githubusercontent.com/74294325/105472789-b0497a80-5cdf-11eb-934c-f7edac426d45.JPG>

<br>

쓰레드를 생성하고 start()를 호출하면 바로 실행되는 것이 아니라 **실행대기열에 저장되어 자신의 차례가 될 때까지 기다려야 한다.** 실행대기열은 큐(queue)와 같은 구조로 먼저 실행되기열에 들어온 쓰레드가 먼저 실행된다. <br>

실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다. <br>

주어진 실행시간이 다되거나 yield()를 만나면 다시 실행대기상태가 되고 다음 차례의 쓰레드가 실행된다. <br>

실행중인 쓰레드가 일시정지되게 되면 잠깐 동작을 정지했다가 일시정지 시간이 다 되거나 강제로 다시 깨울 경우 다시 실행대기상태로 돌아간다. <br>

실행을 모두 마친 쓰레드는 소멸된다.

<br>

---

<br>

## 쓰레드의 동기화

멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스 내의 자원을 공유해서 작업하기 때문에 서로의 작업에 영향을 주게된다. 만일 쓰레드 A가 작업하던 도중에 다른 쓰레드B에게 제어권이 넘어갔을 때 쓰레드A가 작업하던 공유데이터를 쓰레드B가 임의로 변경하였다면 다시 쓰레드A가 제어권일 받아서 나머지 작업을 완료하였을 때 사용자가 원하는 결과가 나오지 않을 수 있다. <br>

이러한 일이 발생하는 것을 방지하기 위해 쓰레드가 특정 작업을 끝마치기 전까지 다른 쓰레드에 의해 방해받지 않도록 하는 것이 필요하다. 그래서 도입된 것이 임계구역(critical section)과 잠금(lock)이다. <br>

**공유데이터(객체)를 사용하는 코드 영역을 임계구역으로 설정하고 공유데이터(객체)가 가지고 있는 lock를 획득한 단 하나의 쓰레드만 이 영역 내의 코드를 수행할 수 있게한다.** <br>

해당 쓰레드가 객체의 이용이 끝나고 lock를 반납하면 다른 쓰레드가 lock를 획득하여 객체를 사용하게된다.<br>

**이처럼 한 쓰레드가 진행중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는것을 쓰레드의 동기화라고 한다.**<br>

임계영역을 지정하는 방법은 두가지가 있다.

1. method 전체를 임계영역으로 지정
```java
public synchronized void test(){
    //method 안이 전부 임계영역
}
```

쓰레드는 metghod가 호출된 시점부터 해당 metghod를 포함된 객체의 lock을 얻어 작업을 수행하다가 method가 종료되면 lock를 반환한다.

<br>

2. 특정 영역을 임계영역으로 지정
```java
synchronized(객체 참조변수){
    //블럭 내부가 임계영역
}
```
이 블럭의 영역안으로 들어가면서 부터 쓰레드는 지정된 객체의 lock를 얻게되고 이 블럭을 벗어나면 lock를 반납하게된다.

<br>

**모든 객치에는 lock를 하나씩 가지고 있으며, 해당 객체의 lock을 가지고 있는 쓰레드만 임계 영역의 코드를 수행할 수 있다. 그리고 다른 쓰레드들은 lock를 얻을 때까지 기다리게된다.**

<br>

코드를 통해 확인해 보자.

```java
class Account {
	private int myMoney = 1000;

	public int getMyMoney() {
		return this.myMoney;
	}
	
	//sub 객체를 쓰레드 2개가 같은 객체를 이용할 때
	public void sub(int money) {
		if (myMoney >= money) {
			try {
				//하나의 쓰레드가 일정시간 멈추는 동안 또다른 쓰레드는 객체의 데이터를 조작
				//만일 if문의 조건문이 참이여서 들어왔지만 멈춰있는동안 같이 사용하는 쓰레드가 
				//공유 자원을 조작하게된다면 일정시간이 지나고 다시 동작할 때 if문의 조건식에 맞지 않더라도
				//이하 코드들이 수행을 해 원하는 결과를 얻기 힘들다.
				Thread.sleep(1500); 
			} catch (Exception e) {
				e.printStackTrace();
			}
			myMoney-=money;
		}
	}
}

class RunableTest implements Runnable {
	Account acc = new Account();
	public void run() {
		while(acc.getMyMoney()>0) {
			int money = (int)(Math.random()*3 +1)*100;
			acc.sub(money);
			System.out.println("myMoney :"+ acc.getMyMoney());
		}
	}
}
	
public class Test {
	public static void main(String[] args) {
		RunableTest rt = new RunableTest();
		Thread t1 = new Thread(rt);
		Thread t2 = new Thread(rt);
		t1.start();
		t2.start();
	}
}
```

<br>

이와 같이 하나의 객체를 두 쓰레드가 동시에 사용한다면 문제가 생길 수 있기에 동기화 처리를 하여 하나의 쓰레드가 사용중일 때 lock를 걸어서 다른 쓰레드의 간섭을 받지 않게 하여 사용한다. <br>

```java
//1. method 전체를 임계영역 처리
public synchronized void sub(int money) {
	if (myMoney >= money) {
		try {
			Thread.sleep(1500); 
		} catch (Exception e) {
			e.printStackTrace();
		}
		myMoney-=money;
	}
}

//2. 특정 영역을 임계영역으로 지정

public synchronized void sub(int money) {
    synchronized(this){
        if (myMoney >= money) {
		    try {
		    	Thread.sleep(1500); 
		    } catch (Exception e) {
		    	e.printStackTrace();
	    	}
		    myMoney-=money;
	    }
    }
}
```

<br>

---

<br>

## DEADLOCK

데드락의 등장 배경은 멀티쓰레드를 환경에서 여러 프로세스나 여러 쓰레드 간의 간섭에 의해 생긴다. <br>

두개 이상의 프로세스나 쓰레드가 서로 끝나기를 기다리는 상태이다. 예를 들어 A라는 쓰레드가 O라는 객체에 대한 lock를 가지고 있는 상태에서 lock를 반납하지 않고 다른 객체를 lock를 얻으려 대기를 하고 B라는 쓰레드는 O 객체의 lock를 얻으려고 대기하는 경우 DeadLock(교착상태)가 발생한다.<br>

### DEADLOCK의 발생조건

1. 상호배제(Mutal Exclusion)

    * 한번에 여러 프로세스나 쓰레드가 한 자원에 접근하지 못하도록 막는다.

2. 점유대기(Hold and Wait)

    * 자원을 가지고 있는 상태에서 다른 프로세스나 쓰레드가 사용하고 있는 객체의 lock를 얻기를 기다리는 상태

3. 자원을 뺏어 오지 못함(No Preemption)

    * 다른 프로세스가 이미 점유한 자원을 강제로 뺏어오지 못함

4. 순환 대기 (Circular Wait)

    * 프로세스나 쓰레드가 다른 프로세스나 쓰레드를 기다리고 있고 이걸 계속해서 반복하다보면 결국 처음 프로세스나 쓰레드가 오는 상황

<br>

### DEADLOCK 예방

1. 예방(Prevention)

    * 위 4가지 조건은 동시에 충족해야 deadlock이 발생한다 그러므로 그 중 하나라도 발생하지 않도록 시스템 차원에서 막아버리면 된다.

2. 회피(Avoidance)

    * 교착상태의 원칙적인 발생가능성은 그냥 두고 발생을 막는 알고리즘을 적용하여 해결하는 방법을 이야기한다. (은행원 알고리즘)

3. 회복(Recovery)

    * 교착상태가 발생하는 것을 막지 않고 발생을 하면 그 때서 해결하는 방법.

4. 무시

    * 데드락을 해결할 때 데드락으로 인한 성능저하보다 이를 해결하는데 성능저하가 심한 경우 무시한다.


<br>


---

<br>

참고

1. 자바의 정석

2. DeadLock : https://webie.tistory.com/99