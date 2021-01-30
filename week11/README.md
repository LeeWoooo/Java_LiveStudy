Java Live Study
===
## 11주차 과제


### 학습 목록

1. enum 정의하는 방법
2. enum이 제공하는 메소드 (values()와 valueOf())
3. java.lang.Enum
4. EnumSet

---

<br>

## enum이란?

enum(열거형)이란 서로 관련된 상수를 편리하게 선언하기 위한 것으로 여러 상수를 정의할 때 사용하면 유용하다. JDK 1.5부터 새로 추가되었다. <br>
C언어는 타입이 다르더라도 값이 같으면 조건식 결과가 참으로 나오지만 **자바의 열거형은 타입에 안전한 열거형이기 때문에 실제의 값이 같아도 타입이 다르면 컴파일 오류가 발생한다.**<br>

코드로 예를 들어보자면
```java 
class Card {
    static final int CLOVER = 0;
    static final int HEART = 1;
    static final int DIAMOND = 2;
    static final int SPADE = 3;

    static final int TWO = 0;
    static final int THREE = 1;
    static final int FOUR = 2;
}
```
<br>

이와같이 정의된 상수들을 열거형을 쓰면 아래와 같이 정의할 수 있다.
```java
class Card {
    enum Kind {CLOVER, HEART, DIAMOND, SPADE}
    enum VALUE {TWO, THREE, FOUR}
}
```

<br>

---

<br>

## enum 선언하기

1. 하나의 java파일로 만들어서 선언하기

```java
public enum enum명{
    상수1,상수2,상수3,상수,,,
}

//ex
public enum Type{
    WALKING, RUNNING, TRACKING, HIKING
}

public class shoes{
    public String name;
    public int size;
    public Type type; //인스턴스를 생성해서 사용
}
```

<br>

2. class안에서 선언하기
```java
public class class명{
    멤버변수1;
    멤버변수2;
    ,,,
    public enum enum명{
        상수1,상수2,상수3,상수,,,
    }
}

//ex
public class shoes{
    public String name;
    public int size;
    public enum Type{
    WALKING, RUNNING, TRACKING, HIKING
    }
}
```

<br>

3. class 밖에서 선언하기
```java
enum enum명 {
    상수1,상수2,상수3,상수,,,
}

public class class명{
    public enum명 instance명;
}
```

<br >

열거된 순서에 따라 0부터 순서 값을 가지게 된다. 차례대로 증가하며 enum 열거형으로 지정된 상수는 대문자를 사용하며 **마지막 끝에 ; 를 붙이지 않는다.**

<br>

---

<br>

## value(), valueOf(), ordinal()

1. value() : 열거된 모든 원소를 배열에 담아 순서대로 반환한다.
```java
enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}
public class Shoes {
    public String name;
    public int size;
    public Type type;
     
    public static void main(String[] args) {
        for(Type type : Type.values()) { //열거형에 있는 원소를 순서대로 배열에 담아 반환
            System.out.println(type);
        }
    }
}
```

<br>

2. 인자로 주어진 string과 열거형에 일치하는 이름을 갖는 원소를 반환한다. (**만약 주어진 인자와 일치하는 원소가 없을 경우 IllegalArgumentException예외 발생**)

```java
enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}
public class Shoes {
     
    public static void main(String[] args) {
        Type tp1 = Type.WALKING;
        Type tp2 = Type.valueOf("WALKING");
         
        System.out.println(tp1);
        System.out.println(tp2);
    }
}
```

<br>

3. ordinal() : 원소에 열거된 순서를 정수 값으로 반환
```java
enum Type {
    WALKING, RUNNING, TRACKING, HIKING
}
public class Shoes {
    public String name;
    public int size;
    public Type type;
     
    public static void main(String[] args) {
        Shoes shoes = new Shoes();
         
        shoes.name = "나이키";
        shoes.size = 230;
        shoes.type = Type.RUNNING;
         
        System.out.println(shoes.type.ordinal());//결과로 1이 반환
         
        Type tp = Type.HIKING;
         
        System.out.println(tp.ordinal()); //결과로 3이 반환
    }
}
```
<br>

---


<br>

## 열거형에서 값을 가져오는 방법

1. enum형 객체를 만들어서 값 가져오기
```java
Type type = Type.WALKING;
```

2. valueOf() method를 이용해서 가져오기
```java
Type.valueOf("WALKING");
```

<br>

## 열거형 상수의 비교

열거형 상수의 비교에는 ==와 compareTo()를 사용할 수 있다. (등가 비교 연산자가능) 하지만 **비교연산자는 사용할 수 없다 만약 사용하게 된다면 컴파일 오류가 발생한다.** <br>

왜 사용할 수 없냐면 **enum의 상수들은 각각의 객체이다.** 그렇기 때문에 객체와 객체를 서로 비교연산자를 사용 할 수 없다.

<br>

---

<br>

## java.lang.Enum

모든 enum은 내부적으로 java.lang.Enum class를 부모 class로 갖는다.

method | 설명
:--- | : ---
Class getDeclaringClass() | 열거형의 Class객체를 반환
String name() | 열거형 상수의 이름을 문자열로 반환
int ordinal() | 열거형 상수가 정의된 순서를 반환(0부터 시작)
T valueOf(Class enumType, String name) | 지정된 열거형에서 name과 일치하는 열거형 상수 반환
compareTo(E o) | 지정된 객체보다 작은 경우 음의 정수, 동일한 경우 0 , 크면 양의 정수를 반환한다.

<br>

---

## 열거형 상수를 다른 값과 연결하기
```java
enum Type {
    // 상수("연결할 문자")
    WALKING("워킹화"), RUNNING("러닝화")
    , TRACKING("트래킹화"), HIKING("등산화")
     
    final private String name;
     
    private Type(Stirng name) { //enum에서 생성자 같은 역할
        this.name = name;
    }
    public String getName() { // 문자를 받아오는 함수
        return name;
    }
}
public class Shoes {
    public static void main(String[] args) {
        for(Type type : Type.values()){
            System.out.println(type.getName());
        }
    }
}
```

<br>

---

<br>

## EnumSet

set 인터페이스를 기반으로 하면서 Enumeration type을 사용하는 방법<br>

method | 설명
:--- | :---
allOf(Class elementType) | 인자로 들어온 enum을 그대로 enum set 생성
complementOf(EnumSet s) | 인자로 들어온 enum set에서 없는 요소들만 다시 enum set 생성
of(E e1,E e2,E e3,E e4,E e5) | 초기값으로 지정한 값들로 enum set 생성
range(E for,,, E to) | 처음과 끝을 입력하면 그 사이에 있는 값들로 enum set 생성

<br>

```java
import java.util.EnumSet;

enum Gfg { CODE, LEARN, CONTRIBUTE, QUIZ, MCQ };

public class EnumSetExample {
    public static void main(String[] args) {
        // Creating a set
        EnumSet<Gfg> set1, set2, set3, set4;

        // Adding elements
        set1 = EnumSet.of(Gfg.QUIZ, Gfg.CONTRIBUTE, Gfg.LEARN, Gfg.CODE); 
        set2 = EnumSet.complementOf(set1); 
        set3 = EnumSet.allOf(Gfg.class); 
        set4 = EnumSet.range(Gfg.CODE, Gfg.CONTRIBUTE); 

        System.out.println("Set 1: " + set1);
        System.out.println("Set 2: " + set2);
        System.out.println("Set 3: " + set3);
        System.out.println("Set 4: " + set4);
  }  
}
```