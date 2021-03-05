Java Live Study
===
## 15주차 과제


### 학습 목록

1. 람다식 사용법
2. 함수형 인터페이스
3. Variable Capture
4. 메소드, 생성자 레퍼런스

<Br>

---

<br>

## 람다식이란?

람다식은 간단히 말해서 메서드를 하나의 식으로 표현한 것이다. 함수를 간략하면서도 명확한 식으로 표현할 수 있게 해준다. 모든 메서드는 클래스에 포함되어야 하므로 클래스도 새로 만들어야 하고, 객체도 생성해야만 비로소 이 메서드를 호출할 수 있다 .그러나 람다식은 이 모든 관정없이 오직 람다식 자체만으로도 이 메서드의 역할을 대신할 수 있다. 

<Br>

또한 람다식은 **메서드의 매개변수로 전달되어지는 것이 가능하고, 메서드의 결과로 반환될 수도 있다, 람다식으로 인해 메서드를 변수처럼 다루는 것이 가능해진다는 것이다. 


<br>

---

<Br>

## 람다식 작성하기

람다식은 '익명 함수'답게 메서드에서 이름과 반환타입을 제거하고 매개변수 선언부와 몸통{} 사이에 -> 를 추가한다.

```java
//기존 메서드
반환타입 메서드이름(매개변수 선언){
    문장들
}

//람다식
(매개변수 선언) -> { 문장들 }
```

<br>

반환값이 있는 메서드의 경우 return문 대신 식(expression)으로 대신 할 수 있다. 식의 연산결과가 자동적으로 반환값이 된다. 이때는 문장이 아닌 식으로 끝나기 때문에 ;를 붙이지 않는다.

```java
//기존문법
(int a, int b) -> {return a>b ? a:b;} 

//return생략
(int a, int b) -> a>b ? a:b
```

<Br>

람다식에 선언된 매개변수의 타입은 추론이 가능한 경우는 생략할 수 있는데 대부분의 경우 생략이 가능하다. 람다식에 반환타입이 없는 이유도 항상 추론이 가능하기 때문이다. 단 매개변수가 두 개 이상일 때 어느 하나의 타입만 생략하는 것은 불가능하다.

```java
//기본 문법
(int a, int b) -> a>b ? a:b

//타입 생략
(a,b) -> a>b ? a:b
```

이 외에 매개변수가 하나일 경우 ()를 생략할 수 있으며 {} 안의 문장이 하나일 경우에는 {}가 생략가능하며 이런한 경우는 ;를 붙이지 않는다.

<br>

---

<Br>

## 함수형 인터페이스

람다식은 익명 클래스의 객체와 동등하다는 성격을 가지고 있다.

```java
(int a, int b) -> a>b ? a : b

//익명 클래스의 객체
new Object() {
    int max(int a, int b){
        return a>b ? a : b;
    }
}
```

아래의 예제를 살펴보면서 하나씩하나씩 이해해보기를 원한다. <Br>

하나의 인터페이스가 있고 인터페이스 안에는 하나의 method가 있다.

```java
interface MyFunction{
    public abstract int max(int a, int b);
}
```

그리고 이 인터페이스를 구현한 익명 클래스의 객체는 다음과 같이 생성할 수 있다.

```java

MyFunction f = new MyFunction{
                    public int max(int a, int b){
                        return a>b ? a: b;
                    }
                };

int result = f.max(5,3);
```

인터페이스를 생성하려면 인터페이스를 구현한 객체를 이용하여 생성할 수 있는데 람다 또한 익명 클래스의 객체와 동등한 성격을 가지고 있다. 그렇기 때문에 아래와 같이 가능하다.

```java
MyFunction f = (int a, int b) -> a>b ? a : b ;
```

그 이유는 interface에 있는 method의 max(int a, int b)와 람다식 (int a, int b)-> a>b ? a : b 의 선언부가 동일하기 때문이다. 그렇기 때문에 람다식이 MyFunction을 구현했다고 볼 수 있으며 람다식을 사용함으로 인터페이스를 구현할 수 있게 되는 것이다. **간단하게 다시 정리하자면 람다식도 실제로는 익명객체이고 MyFunction인터페이스를 구현한 익멱 객체의 메서드 max()와 람다식의 매개변수의 타입과 개수, 그리고 반환값이 일치하기 때문이다.** <br>

이와 같이 람다식을 다루기 위한 인터페이스를 함수형 인터페이스라고 부른다. 여기서 함수형 인터페이스는 제약을 가지고 있는데 오직 하나의 추상 method만 정의되어 있어야 한다는 것이다. 그래야 람다식과 1:1로 연결될 수 있기 때문이다. <br>

우리가 자주사용하는 쓰레드의 Runnable같은 경우도 함수형 인터페이스에 속한다 구조를 살펴보자면

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run();
}
```

이와 같이 run method를 하나만 가지고 있다. 이때 위에 annotation으로 @FunctionalInterface를 가지고 있는데 @FunctionalInterface를 선언하게 되면 컴파일러가 함수형 인터페이스를 올바르게 정의하였는지 확인하여준다.

<br>

---

<br>

## Variable Capture

람다식에서 외부 지역변수를 참조하는 것을 Variable Capture라 한다. 또한 람다식 내에서 참조하는 지역변수는 final이 붙지 않아도 상수로 간주된다. 아래의 예제를 보면서 더 살펴보자면 

```java
@FunctionalInterface
interface MyFunction{
    void myMethod();
}

class Outer{
    int val = 10; //Outer.this.val

    class Inner{
        int val = 20; //this.val

        void method(int i){
            int val = 30;
            //i = 10; error 

            MyFunction f = (i)->{ //error 외부 지역변수와 이름이 중복되면 안됨
                System.out.println(val);
                System.out.println(i);
                System.out.println(++Outer.this.val);
                System.out.println(++this.val);
            } 
        }
    }
}
```

예제 코드를 보면 람다식 내에서 지역변수 i와 val을 참조하고 있으므로 람다식 내에서나 다른 어느곳에서도 이 변수들의 값을 변경하는 일은 허용되지 않는다.

간단하게 정리하면 람다식에서 참조하고 있는 변수는 final로 간주되며 그 어디서도 값 변경이 불가능하다.

<br>

---

<br>

## 메소드, 생성자 레퍼런스

## 메소드 레퍼런스

하나의 메서드만 호출하는 람다식은 클래스이름::메소드이름 또는 참조변수::메소드이름으로 바꿀 수 있다.

예를 들어 아래와 같은 코드를 확인하면 

```java
Function<String,Integer> f = (String s) -> Integer.parseInt(s);

//메소드 레퍼런스 적용
Function<String,Integer> f = Integer::parseInt;
```

위 메소드 참조에서 람다식의 일부가 생략되었지만, 컴파일러는 생략된 부분을 우변의 parseInt메소드의 선언부로부터, 또는 좌변의 Function인터페이스에 지정된 제네릭타입으로 부터 쉽게 알아 낼 수 있다.

### 생성자

생성자를 호출하는 람다식도 메소드 참조로 변환할 수 있다.

```java
Supplier<MyClass> s = () -> new MyClass(); //람다식
Supplier<MyClass> s = MyClass::new; //메서드 참조
```

매개변수가 있는 생성자라면, 매개변수의 개수에 따라 알맞은 함수형 인터페이스를 사용하면 된다. 필요하다면 함수형 인터페이스를 새로 정의해야한다.


---

<br>

---

## 참조

남궁성 자바의 정석