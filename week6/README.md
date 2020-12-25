Java Live Study
===
## 6주차 과제


### 학습 목록

1. 자바 상속의 특징

2. super 키워드

3. 메소드 오버라이딩

4. 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

5. 추상 클래스

6. final 키워드

7. Object 클래스
 
---

## 자바 상속의 특징

* 상속이란 기존의 class를 재사용하여 새로운 class를 작성하는 것을 의미한다.<br>
상속을 통해서 class를 작성하면 보다 적은 양의 코드로 새로운 class를 작성할 수 있고,코드의 유지보수가 매우 용이하다.

* 어떠한 class를 상속받지 않은 class의 부모는 모두 Object class이다.
    
    * Object class는 모든 class의 상속계층도의 최상위에 있는 조상 class이다.


* 상속을 이야기 할 때 부모class와 자식class로 나뉘어 진다.

    * 자식class는 extends라는 키워드를 사용하여 부모class를 상속받게 된다.

```java
class Parent{
    //부모 class의 code
}

class Child extends Parent{
    //자식 class의 code
}
```
* 상속관계에 있는 자식class는 부모class의 자원을 자기것 처럼 가져다 사용할 수 있게된다.

    * 자식 class는 부모class의 모든 멤버를 상속 받기 때문에 부모보다 같거나 많은 멤버들을 가지고 있다.

    * `생성자와 초기화 블럭은 상속되지 않는다.`

    * `자식class의 접근 지정자는 부모class의 접근지정자보다 더 넓은 범위로 지정해야 한다.`

    * `부모 class의 private 자원은 직접 가져다 사용할 수 없다.`

* 상속받은 자식class가 instance를 생성하면 먼저 부모class의 생성자가 불린 후 자식class의 생성자를 호출한다.

    * 자식class의 필드에는 기본적으로 super()라는 부모class의 생성자가 생략되어있다

### HAS-A 관계, IS-A관계

* 상속 관계를 IS-A관계라고 한다. ~는 ~이다, 자식 class는 부모class이다가 성립이 된다.

* 포함 관계를 Has-A관계라고 한다. ~는 ~를 가지고 있다. 한class에서 다른 class를 Instance화 하여 가지고 있는것을 의미한다.

```java
class Circle{
    point c = new point(); //Circle는 point를 가지고 있다.
    int r;
}//Circle

class Circle extends point{//Circle는 point 이다.
    int r;
}
```
### 단일상속

* 다른 객체지향언어들은 여러 조상 class로부터 상속을 받는 것이 가능하지만 Java에서는 단일 상속만 허용한다.

    * 다중 상속을 받으면 여러 기능들을 한 class에 담을 수 있다는 장점이 있지만 class관계가 매우 복잡해진다.

    * 자바에서 상속은 단일상속이지만 Interface는 여러개를 한 class에서 구현이 가능하다.


---


## super 키워드

* super는 자식class에서 부모class로부터 상속받은 멤버를 참조하는데 사용되는 참조 변수이다.

* super는 부모class Instance의 시작주소를 가지고 있다.

* this와 사용방식이 동일함 부모class의 멤버와 자식class의 멤버의 이름이 같을 때 super,this를 이용하여 구분

    * 부모와 자식의 멤버가 동일한 명을 가지고 있을 때는 super만 사용하는 것이 좋다

### super의 사용방식

1. method 형식

```java
super() //부모 class의 기본 생성자를 호출한다.

super(argument,,,,,) //부모 class의 매개변수가 있는 생성자를 호출한다.

```

2. Keyword 방식

* 부모 class의 Instance 주소를 가지고 있다.

* 자식 class의 멤버와 구분하기 위하여 사용한다.

---

## 메소드 오버라이딩

* 부모 class로부터 상속받은 method의 내용을 변경하는 것을 Overriding라고 한다.

* 상속 받은 method를 그대로 사용하기도 하지만 자식class에 맞게 변경해서 사용해야 하는 경우 사용한다.

```java
public class Run {

    public String runSpeed(){
        return "Fast";
    } 
}
public class Run2 extends Run{

     public String runSpeed(){ //Overriding
        return "Very Fast";
    } 
}
```

### Overriding의 조건

    1. method의 이름이 같아야 한다.
    2. 매개변수가 같아야 한다.(매개변수 명은 같지않아도 가능하다.)
    3. 반환타입이 같아야 한다.
    4. 접근지정자는 부모method보다 작은 범위로 변경할 수 없다.(큰 범위로만 가능)
    5. 부모 class보다 많은 수의 예외를 선언 할 수 없다.(같거나 적어야 한다.)
    6. Instance method를 static으로 혹은 static을 Instance method로 변경 불가.

### Overriding 과 Overloading의 차이점

* Overloading은 기존에 없는 새로운 method를 추가하는 개념

* Overriding은 부모로부터 상속받은 method의 내용을 변경하는 개념

```java
class parent{
    void parentMethod(){

    }
}

class child extends parent{
    void parentMethod(int i){
        //Overloading
    }

    void parentMethod(){
        //Overriding
    }
}
```
### @Override annotation(주석달기)

* Override된 method 위에 @Override 주석을 달아 사용가능.
* method 위에 사용하며 Override가 제대로 되었는지 확인(Compiler가 확인)
* method가 Override 된 method임을 알리는 역할을 한다.

---

## 다이나믹 메소드 디스패치 (Dynamic Method Dispatch)

* 정적 메소드 디스패치

```java
public class A{
    public void print(){
        System.out.println("A");
    }
}

public class B extends A{
    public void print(){
        System.out.println("B");
    }
}
public class test{
    public static void main(String[] args){
        B b = new B();
        b.print(); //출력 결과는 "B"
    }
}
```

* 메인 함수에서 b.print()를 호출 했을 때 우리는 class B의 Overriding된 method가 불릴 것을 알고있다.<br>
이와 같이 complier 또한 자기사 호출하고 실행 시켜야 할 method를 알고 있는데 이것을 정적 메소드 디스패치라 한다.

* 동적 메소드 디스패치
```java
 class Dispatch {
        static abstract class Service {
            abstract void run();
        }

        static class MyService1 extends Service {
            @Override
            void run() {
                System.out.println("1");
            }
        }

        static class MyService2 extends Service {
            @Override
            void run() {
                System.out.println("2");
            }
        }

        public static void main(String[] args) {
            Service srv = new MyService1();
            srv.run();
        }
    }
```
* 정적 디스패치와 반대로 컴파일러가 어떤 메소드를 호출하는지 모르는 경우이다. 동적 디스패치는 호출할 메서드를 런타임 시점에서 결정한다.

* 위의 코드 결과 런타임에서는 Service class의 run() method를 호출하여 "2"를 출력하게된다. 

---

## 추상 클래스

* 추상 class는 자식class에게 구현을 강제시킬수 있는 class이다.<br>
(자식 class에서 abstract method를 무조건 Overriding해야만 상속이 가능하다.)

* 추상 class를 가지고 Instance를 만들 수 없으며 자식 class가 Instance화 될때에만 객체가 생성된다.

* 여러 class에서 공통적으로 사용될 수 있는 class를 바로 작성하기도 하고<br>
기존의 class에서 공통적인 부분을 뽑아 추상클래스로 만들어 상속하는 경우도 있다.

### abstract method

* method의 body가 없는 method이다.

* 자식class에서 구현을 하여 사용해야하는 일의 목록을 만들 때 사용하게 된다.

* 직접 호출할 수 없다.

```java
public abstract void test(); //body가 없다.
public abstract void test2(int x);//매개변수를 가질 수 있다.
```

## final 키워드

* final은 마지막의 , 변경될 수 없는 의미를 가지고 있으며 거의 모든 대상에 사용될 수 있다.

* class에 final을 사용하면 다른 class에서 부모 class로 지정할 수 없다.

* method에서 final을 사용하면 자식class에서 Overriding을 할 수 없다.

* 변수에 final을 사용하면 값을 변경할 수 없는 상수가 된다.

    * 변수명은 전부 대문자로 설정하여 사용한다.


```java
final class finalTest{ //부모class가 될 수 없다.
    final int MAX = 100; //값을 변경할 수 없는 멤버변수

    final void test{ //자식 class에서 Overriding 할 수 없는 method
        final int LV = MAX; //값을 변경 할 수 없는 지역변수
    }
}
```

* 생성자를 이용한 final 멤버변수의 초기화

    * final 멤버변수의 값을 할당하지 않은 상태로 매개변수 있는 생성자를 이용해 값을 받아서 final 멤버변수의 값을 초기화 할 수 있다.<br>
    (이 방법을 사용하면 `class를 이용하여 생성되는 Instance마다 다른값의 final 멤버변수를 갖을 수 있다.`)

## Object 클래스

* 모든 class 상속계층도의 최상위에 있는 부모class이다. 

Object class의 method | 용도
:--- | :---
protected Object clone() | 객체 자신의 복사본을 반환한다.
public boolean equals(Object obj) | 객체 자신과 객체 obk가 같은 객체인지 알려준다.
public Class getClass() | 객체 자신의 class정보를 담고 있는 class Instance를 반환
public int hashCode() | 객체 자신의 해시코드를 반환
public String toString() | 객체 자신의 정보를 문자열로 반환
public void notify() | 객체 자신을 사용하려고 기다리는 쓰레드 하나만 깨운다.
public void notifyAll | 객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.

* Object class의 멤버들은 모든 class에서 바로 사용이 가능하다.