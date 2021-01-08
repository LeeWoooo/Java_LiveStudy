Java Live Study
===
## 8주차 과제


### 학습 목록

1. 인터페이스 정의하는 방법
2. 인터페이스 구현하는 방법
3. 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법
4. 인터페이스 상속
5. 인터페이스의 기본 메소드 (Default Method), 자바 8
6. 인터페이스의 static 메소드, 자바 8
7. 인터페이스의 private 메소드, 자바 9

---

## 인터페이스란?

* 인터페이스는 일종의 추상class이다. 인터페이스는 추상class처럼 추상 method를 갖지만 추상class보다 추상화의 정도가 높다.

* 인터페이스는 일반method(body를 가진)나 멤버변수를 구성원으로 가질 수 없다.(JDK 1.8 이전 기준)

* 인터페이스의 사용목적

    * 다중상속의 장점을 사용하기 위해서(상속은 하나의 부모만 가질 수 있지만 인터페이스는 한 class에서 여려개를 구현 가능하다.)

    * 약결합을 구현하여 **객체간의 유연성을 높이기 위해서.**

    * 동시적인 개발을 통해 **개발 시간을 단축할 수 있다.**

    * 관계가 없는 class간의 공통적인 부분이 있을 경우 인터페이스를 통해 **공통 부분 관계 형성이 가능하다.**


<br>

---

<br>

## 인터페이스 정의하는 방법.


* 인터페이스를 작성하는 것은 class를 작성하는 것과 같다. class keyword 대신 interface를 사용한다.

* 인터페이스의 접근 지정자는 **public, default**를 사용할 수 있다.

* 인터페이스는 **생성자가 없다.**


* 구조

    <img src = https://user-images.githubusercontent.com/69107255/98751432-70266b80-2403-11eb-95b6-4fb58e3261a4.PNG>

* 문법

    ```java
    interface interface명 {
        public static final datatype 상수명 = 값;
        public abstract 반환값 method이름(parameter);
    }

    //interface 예시
    public interface PlayingCard{
        public static final int SPADE = 4;
        final int DIAMOND = 3;
        static int HEART = 2;
        int CLOVER = 1;

        public abstract String getCard();
        String getCardKind();
    }
    ```

    * **모든 멤버변수는 public static final 이어야 하며 이를 생략하고 "datatype 상수명 = 값" 으로만 표기 가능하다.**
    * **모든 method는 public abstract 이어야 하며 이를 생략하고 반환형 method이름(parameter); 으로만 표기 가능하다.**

* 인터페이스는 다른 인터페이스들을 상속 받을 수 있다.

    ```java
    //ex
    interface Fightable extends Movable, Attackable{}
    interface Movable{ void move(int x, int y)}
    interface Attackable { void attack (String attackpoint) }
    ```

    * 이 경우 Fightable를 구현하는 자식 class는 move,attack의 추상 method를 반드시 구현해야 한다.


<br>

---

<br>

## 인터페이스 구현하는 방법.

* <mark>implements</mark>라는 예약어를 class선언할 때 사용하여 구현할 수 있다.

    * **인터페이스는 상속과 달리 여러개의 인터페이스를 한 class에서 구현할 수 있다.**

* **인터페이스는 생성자가 없고 단독으로 Instance화를 할 수 없다.(구현한 자식class로 Instace화 가능)**

* 문법

    ```java
    public class class명 implements 인터페이스명 {
        //인터페이스에서 정의된 추상 method를 반드시 구현해야한다.
    }

    //구현 예시
    public class User implements PlayingCard{
        public String getCard(){
            //자식 class에서 구현 할 code
        }
        public String getCardKind(){
            //자식 class에서 구현 할 code
        }

        public void betting(){
            ...
        }
    }
    ```
* 인터페이스를 구현하는 class는 상속과 인터페이스의 구현을 동시에 할 수 있다.

<br>

---

<br>

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법.

* 인터페이스 타입의 참조변수로 이를 구현한 class의 인스턴스를 참조할 수 있으며 interface 타입으로 casting도 가능하다.

* 코드로 확인해보자.

    ```java
    public interface Car{
        void Speed();
    }
    ```

    ```java
    public class void SportsCar(){
        @Override
        public void speed(){
            System.out.println("스포츠카는 빠르다.")
        }

        public void name(){
            System.out.println("페라리")
        }
    }
    ```

    ```java
    public void Main{
        public static void main(String[] args){
            Car car = new SportsCar();

            car.speed();
            //car.name(); 사용 불가능

            (SportsCar)car.name(); //casting으로 사용이 가능하다.
        }
    }
    ```

* class에서 method를 선언시 반환형으로 인터페이스를 주는 경우에는 method가 해당 인터페이스를 구현한 class를 반환한다는 것을 의미.


<br>

---

<br>

## 인터페이스 상속

* 인터페이스는 class와 달리 다중 상속이 가능하다.

* 인터페이스는 부모 인터페이스를 여러개 가질 수 있다. 하지만 부모 인터페이스 중 method명과 parameter의 형식은 같지만 return이 다른 method가 있다면 다중 상속이 불가능 하다.

* 코드로 확인해보자

    ```java
    interface Test1 {void temp1()}
    interface Test2 {void temp1(); int temp2()}
    interface Test3 {int temp1()}

    interface Test4 extends Test1, Test2{} //가능
    //interface Test5 extends Test1, Test3{} 불가능
    ```
    

<br>

---

<br>

## 인터페이스의 기본메소드 (Default Method), 자바 8

* JDK 1.8부터 인터페이스에 Default Method의 추가가 가능해졌다. 이미 구현이 된 인터페이스에 Default Method를 추가되어도 자식 class를 변경하지 않아도 된다.

* Default Method는 method 앞에 default를 붙이며 일반 method와 같이 body를 가지고 있어야 한다.( 접근지정자는 public 이며 생략이 가능하다. )

* Default Method는 인터페이스에서 선언이 되지만 인터페이스에서 바로 사용할 수 없다.

* **Default Method는 추상 method가 아닌 인스턴스 method이기 때문에 구현을 해야 사용이 가능하다**

* 문법

    ```java
    interface Test{
        void temp();
        default void newMethod(parameter,,,){
            //method로 실행할 일을 정의
        }
    }
    ```

* 만약 Default Method의 이름이 중복되어 충돌이 발생할 시

    1. class에서 Default Method를 재정의했다면 첫번째 우선순위

    2. 상속구조의 인터페이스에서 자식 인터페이스가 부모 인터페이스의 Default Method를 재정의 했다면 두번째 우선순위

    3. 우선순위가 없을 경우라면 구현 class에서 Override해주면 된다.
        * 이 경우 선언할 때 명시적으로 인터페이스명.super.method명으로 선언

<br>

---

<br>

## 인터페이스의 static 메소드, 자바 8

* JDK 1.8부터 인터페이스에 static method가 추가되었다.

* Default Method는 Override가 가능하지만 static method는 불가능하다.

* 코드로 확인해보자

    ```java
    interface Test{
        public static int sum(int i, int y){
            return i+y;
        }
    }
    ```
    ```java
    public class Main{
        public static void main(String[] args){
            int sum = Test.sum(20,6);
            System.out.println(sum);
        }
    }
    ```
    * 이와 같이 반드시 인터페이스명.method명();으로 호출해서 사용한다.

<br>

---

<br>

## 인터페이스의 private 메소드, 자바 9

* JDK 9부터 인터페이스에 private method가 추가되었다.

* 인터페이스 안에서 사용이 가능하며 Default method와 private method의 공통 코드를 공유할 수 있다.

* private method와 static private method가 있다.

    * static private method는 static영영이다보니 Instace영역의 자원을 사용할 수 없다.

* 코드로 확인해보자

    ```java
    public interface Test{
        default void lunch(){
            setting();
            System.out.println("점심을 먹는다");
        }

        private void setting(){
            System.out.println("식사를 준비한다.");
        }
    }
    ```

<br>

---

<br>

## 인터페이스 이해하기

* 먼저 인터페이스를 이해하기 위해서는 class를 사용하는쪽(USER)와 제공하는 쪽(Provider)이 있다.

* method를 호출하는 쪽(USER)에서는 사용하려는 method(Provider)의 선언부만 알면 된다.

```java
class A{
    public void methodA(B b){
        b.methodB();
    }
}

class B{
    public void methodB(){
        System.out.println("methodB()")
    }
}

class InterfaceTest{
    public static void main(String[] args){
        A a = new A();
        a.methodA(new B);
    }
}
```

<p>
위와같은 경우 A class를 작성하려면 B class가 이미 작성되어 있어야 한다. 그리고 class B의 methodB()의 선언부가 변경되면 이를 사용하는 class에서도 변경이 되어야 한다. 이와 같이 직접적인 관계는 한쪽이 수정이 되면 다른 한쪽도 반드시 수정되어야 된다는 단점이 있다. 
</p>

<br>

<p>
하지만 인터페이스를 매개체로 해서 class A가 인터페이스를 통해 Class B에 접근하게 한다면 class B에 변경사항이 생기더라도 class A에는 전혀 지장이 없을 것이다. 코드로 확인하자.
</p>

<br> 

```java
class A{
    public void methodA(I i){
        i.methodB();
    }
}

interface I{
    void methodB();
}

class B implements I{
    public void methodB{
        System.out.println("인터페이스를 매개체로 접근")
    }
}

class Main{
    public static void main(String[] args){
        A a = new A();
        a.methodA(new B);
    }
}
```

<br>

<p>
이와 같이 methodA의 매개변수로 인터페이스를 지정해준 후 인터페이스 I를 구현한 class B의 인스턴스를 넣어준다면 B가 변경되더라도 methodA를 사용하는데는 지장이 없다. 이렇게 설정된 관계를 간접적인 관계라고 하며 이때 인터페이스의 역할은 class B를 감싸고 있는 껍데기 역할이라 할 수 있다.
</p>

<br>

<p>
class A는 인터페이스를 통해 실제로 사용하는 class의 이름은 몰라도 되고 심지어는 실제로 구현된 class가 존재하는지 않아도 문제가 없다. class A는 오직 직접적인 인터페이스의 영향만 받는다.
