Java Live Study
===
## 5주차 과제


### 학습 목록
1. 클래스 정의하는 방법

2. 객체 만드는 방법 (new 키워드 이해하기)

3. 메소드 정의하는 방법

4. 생성자 정의하는 방법

5. this 키워드 이해하기 
 
---

## Class

* Class란 객체를 정의해놓은 것, 객체의 설계도 또는 틀 이라 할 수 있으며 Class는 객체를 생성하는데 사용된다. 객체는 Class에 정의된 대로 생성된다.

* Class는 사용자 정의 자료형, 참조형 DataType이라고 불린다.

* 사용자정의 자료형(DataType)

    * 기존에 제공되는 데이터 형을 조합하여 새로운 일므으로 묶어 사용하는 DataType

* 참조형 DataType (Reference Type)

    * 값은 메모리 heap에 생성되고 그 시작주소를 stack에 저장 한다.
    * heap를 사용하려면 "new" 라는 객체 생성용 keyword를 사용해야 한다.

* class의 종류

    종류 | 기능
    :--- | :---
    class | 일을 하기 위한 class
    Abstract class | 상속을 하기 위한 부모class
    Interface | 다중상속의 장점을 가지고 있으며 약결합을 구현해 객체 간 유연성 증가
    Inner Class | Event handling 목적(class를 instance변수처럼 사용)
    Nested Class | Event handling 목적(class를 Static변수처럼  사용) 
    Local Class | class를 지역변수처럼 사용하기 위해

### Class의 작성순서

    1. 대상을 선정한다 ( 현업의 업무 )

    2. 객체를 모델링 ( 추상화 작업 )

    3. Class 작성

    4. 객체 생성

    5. 생성된 객체를 사용

### Class의 작성 문법

```java
접근지정자 class class명 extends 부모class명 implement 부모interface명,,,
```

* class의 접근지정자

    * public : package 외부에서도 접근 가능.
    * package (default) : 패키지 내부에서만 접근가능.
    * Abstract : 추상 class의 접근지정자.
    * final : 상속을 막기 위한 접근 지정자

---

## Class의 구조

    class의 구조는 생성자,field,method로 구성되어 있다.

<br>

### 생성자

* 객체가 생성 될 때 가지고 있어야 할 초기화 값, 코드를 정의하는 method의 일종이다.

* `생성자는 첫번째 줄에서만 사용 할 수 있다.`

* 생성자는 method를 호출하듯 `직접 호출을 할 수 없다.`(new로만 호출 가능)

* `생성자를 하나도 정의하지 않으면 Compler가 매개변수 없는 기본 생성자 (default constructor를 생성해준다.)`

* 매개변수가 있는 생성자를 하나라도 정의하면 Compler는 기본 생성자를 생성하지 않는다.

* 생성자 안에서 this(), super()와 같이 다른 생성자를 호출할 수 있다.

* 생성자를 작성할 때 생성자의 이름은 class명과 동일해야 한다.

 * 생성자는 상속이 되지 않는다.

### 작성 문법

```java
접근지정자 class명(매개변수,,,,){
객체가 가져야 할 초기값, 객체가 생성되면서 실행되어야 할 코드
}
```

* 생성자의 접근지정자

    * public : class외부에서 객체 생성가능
    * protecte : 같은 package의 다른class에서 생성가능<br>
    (다른 package의 상속관계에 있는 class에서 생성가능)
    * private : class안에서만 객체 생성가능 
    * default : 같은 package내에서만 생성 가능.

<br>

## this

* 자신class와 관련된 일만 수행한다.

* 객체 내부에서 필드에 접근하기 위해 this를 사용할 수 있다.

* 사용 방식은 keyword방식, method방식이 있다.

1. method 방식 : 자신class의 생성자를 호출할 때 사용한다.
```java
this(); //자신class의 기본 생성자를 호출
this(인자,,,,,) //자신class의 매개변수가 있는 생성자 호출
```
    
2. keyword 방식 
```java
this. 변수명 //생성된 객체의 instance변수를 호출할 때
this. method명();//생성된 객체의 instance method를 호출할 때
```

* keyword방식으로 쓰일 때 this는 생성된 객체의 주소를 가지고 있다.

* instance영역에서만 사용이 가능하다. `static영역에서 사용 불가`

<br>

 
 ## Field

* 객체의 고유 데이터 및 현재 상태의 데이터를 저장하는 곳이다.
 
* class의 { } 안의 영역을 이야기 한다.
 
* 이 영역 안에서 instance 변수, class 변수 , method들이 존재한다.

1. Instacce 변수

    * class field에서 선언하고 객체화 하여 객체명으로 사용하는 변수.

    * 자동 초기화가 된다.

    * class를 객체화하면 객체마다 변수가 메모리(heap)에 생성

    * Static영역에서는 instance 변수를 직접 접근할 수 없다.

    * 생성되는 객체마다 변수의 값을 따로 저장하고 사용해야 할 때 선언한다.

2. class 변수

    * 공용 변수 - 하나의 자원을 여러 대상들이 공유하고 사용한다.

    * 실행중인 JVM에서 하나만 만들어 지고 사용된다.

    * class가 실행되면 memory에 올라간다.

    * 값이 변경되면 변경된 값을 모두가 사용하게 된다.

    * 사용하지 않는 변수를 선언하면 메모리의 손실이 올 수 있다.

* 문법
```java
접근지정자 static 데이터형 변수명;
```

<br>

## Method

* 업무를 구분하여 작성 할 때 사용되며  중복코드를 줄일 수 있다.

* 종류로는 instance method와 static method가 있다.

1. Instance method

    * class의 instance변수를 사용하여 일을 처리하는 method
    * Instance화를 하여 Instacce명.method()로 호출

2. Static method

* 객체가 가지고 있는 값을 사용하여 일을 처리하는 것이 아닌 매개변수로 입력된 값을 가지고 일을 처리.

* Instacce화 하지 않고 class명.method()로 호출하여 사용

* 문법
```java
접근지정자 반환형 method명(매개변수,,,){
    //처리해야할 일
}
```

* method의 접근지정자

    * public : class외부에서 호출가능
    * protected : 같은 package,다른 package에서 상속관계에 있는 class
    * private : class안에서만 호출가능
    * default : 같은 package에서만

    * static : static method를 생성할 때
    * final : Override를 막을 때
    * synchronized : multi thread에서 동시 호출을 막을 때

### 반환형

* return type

* method안에서 업무 처리한 결과를 밖으로 내보낼 때 사용된다.
* method를 호출한 곳에서 결과 값을 받을 수 있다.
* void : 반환값이 없음을 의미한다.

### method의 4가지 형태

1. 반환형 없고 매개변수가 없는 형 - 고정적인 일
```java
public void typeA(){
    //처리해야할 일
}
```

2. 반환형 없고, 매개변수 있는 형 - 동적인 일
```java
public void typeB(매개변수,,,){
    //처리해야할 일
}
```

3. 반환형 있고 매개변수 없는 형 - 고정 값
```java
public 반환형 typeC(){
    //처리해야할 일
    return 반환될 값
}
```

4. 반환형 있고 매개변수 있는 형. - 가변 값.
```java
public 반환형 typeD(매개변수,,,){
    //처리해야할 일
    return 반환될 값
}
```

## INSTANC화(new 이해하기)

* class를 Instance화 하여 Instance를 생성하여 사용하게 된다.
* Instance를 생성할 때 new 라는 keyword를 사용하게된다.
* new를 사용하지 않고 선언했다면 아직 heap에 주소가 없다는 것을 의미하며 null값을 갖게된다.

* new를 사용하여 instance를 만들게 되면 class안에 있는 Instance 변수들이 heap에 올라가게 되고 instance는 stack에 올라가며 instance 변수의 시작주소를 갖게된다.

<img src =  https://user-images.githubusercontent.com/74294325/102606206-bfca2680-4169-11eb-9bda-bb19584307c4.JPG>