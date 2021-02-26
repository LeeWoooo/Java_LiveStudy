Java Live Study
===
## 14주차 과제


### 학습 목록

1. 제네릭 사용법
2. 제네릭 주요 개념 (바운디드 타입, 와일드 카드)
3. 제네릭 메소드 만들기
4. Erasure

<Br>

---

<br>

## 제네릭이란?

다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 **컴파일 시의 타입체크(compile-time type check)를 해주는 기능이다.** 객체의 타입을 컴파일 시에 체크하기 때문에 **객체의 타입 안정성을 높이고 형변환의 번거로움이 줄어든다.** <br>

타입 안정성을 높인다는 것은 의도하지 않은 타입의 객체가 저장되는 것을 막고, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 잘못 형변환되어 발생하는 오류를 줄여준다는 뜻이다.

>제네릭의 장점 <br>
1 - 타입의 안정성을 제공한다.<br>
2 - 타입체크와 형변환을 생략할 수 있으므로 코드가 간결해진다.

<br>

---

<Br>

## 제네릭 사용법

제네릭은 class와 method에 선언을 할 수 있다. <br>

1. class에 선언하기

class를 선언할 때 class명 옆에 &lt;T>를 선언해주면 된다. &lt;T>안에 T는 기호일 뿐 이 안에 객체의 타입을 지정해주면 된다. 코드로 확인을 해보자면

```java
class Box<T>{
    T item;

    void setItem(T item){this.item = item}
    T getItem{return item}
}

class Main{

    public static void main(String[] args){
        Box<String> stringBox = new Box();
        Box<Integer> integerBox = new Box();

        stringBox.setItem("String");
        integerBox.setItem(261);
    }
}
```

<br>

이와 같이 &lt;>안에 참조형 타입을 넣어서 class를 생성하여 사용할 수 있으며 생성할 때 제네릭으로 어떠한 참조형 타입으로 설정할 것인지 생성하면된다. 간단하게 말하면 T인 부분이 전부 설정한 제네릭으로 바뀐다.

```java
//Box<String> stringBox = new Box()를 하면 아래와 같은 class로 바뀐다 생각하면 된다.
class Box{
    String item;

    void setItem(String item){this.item = item}
    String getItem{return item}
}
```

<br>

---

<Br>

## 제네릭 주요 개념 (바운디드 타입, 와일드 카드)

<br>

### 바운디드 타입

바운드 타입은 특정 타입의 서브타입으로 제한하는 것을 이야기 한다. 클래스나 인터페이스 설계할 때 가장 흔하게 사용할 정도로 많이 볼 수 있는 개념이다. <br>

먼저는 extends와 supper의 개념을 알고가자 바운디드 타입에서 extends와 supper가 사용이 되는데 코드로 확인해보면

```java
<? extends 상위타입> //와일드 카드의 상한제한 T와 그 자손들만 가능
<? supper 하위타입> //와일드 카드의 하한 제한 T와 그 조상들만 가능

//예제
class Box<T extends Number>{
    T item;
    void setItem(T item){this.item = item};
    T getItem{return item};
}
```

<br>

위와 같이 예제코드를 확인해보면 T에들어갈 수 있는 것은 Number의 서브타입만 올 수 있게 된다. Nuber의 서브클래스는 API를 확인하면 알 수 있다.

* api

    <img src = https://user-images.githubusercontent.com/74294325/109282567-09697880-7861-11eb-832d-6d6d1f4dc6a0.JPG>

<br>

### 와일드 카드

와일드 카드는 위의 바운디드 타입의 ?와 연결이 되는데 위에서 정리한 것이 와일드 카드에서 사용된다

```java
<? extends 상위타입> //와일드 카드의 상한제한 T와 그 자손들만 가능
<? supper 하위타입> //와일드 카드의 하한 제한 T와 그 조상들만 가능
```

<br>

와일드 카드를 사용하는 목적은 한번 제네릭을 설정해 버리면 그 참조형 타입에 묶여버린다는 것이다. 그렇기 때문에 나중 유지보수나 다른 값을 입력하고 싶을 때 힘들어진다. 와일드 카드를 사용하면 이러한 점을 사용할 수 있는데 예제를 통해 확인해보자

```java
public class Person{
    ,,,
}

public class Woogil extends Person{
    ,,,
}
```

<Br>

여기서 와일드 카드 개념을 적용하면 다음과 같다.

```java
List<? extends Person> list = new ArrayList<Woogin>();
```
이 처럼 extends를 사용한 경우 List의 제네릭으로 Person을 상속하고 있는 모든 자식 class가 들어갈 수 있게 된다. 만약 extends가 아닌 super를 사용할 경우 위의 예제코드는 ERROR가 발생한다.

<br>

---

<br>

## 제네릭 메소드 만들기

메서드의 선언부에 제네릭 타입이 선언된 메서드를 의미한다. <br> 

대표적으로는  Collections 클래스의 sort 메서드이다. sort method를 확인해보면 

```java
public static <T> void sort(List<T> list, Comparator<? super T> c) {
    list.sort(c);
}
```

이와 같은데 선언부의 &lt;T>와 매개변수의 &lt;T>는 생김세만 같지 전혀 다른 놈이라는 것을 주의해야한다. 이 method의 사용법은 다음과 같다.

```java
List<String> stringList = new ArrayList<>();
List<Integer> integerList = new ArrayList<>();

Collection.<String>sort(stringList);

Collection.<Integer>sort(integerList);
```

여기서 method를 호출할 때마다 제네릭을 명시해줬는데 매개변수로 들어온 타입을 추정해서 컴파일러가 처리해주기 때문에 생략을 할 수 있게 된다.


<br>

---

<br>

## Erasure

컴파일러는 제네릭릭 타입을 이용해서 소스파일을 체크하고 필요한 곳에 형변환을 넣어준 후 제네릭 타입을 제거한다. 즉 컴파일된 파일에는 제네릭 타입에 대한 정보가 없다는 것이다. <br>

이렇게 하는 이유는 제네릭이 도입되기 이전의 소스 코드와의 호환성을 유지하기 위해서이다.
 
위의 예제 코드처럼 <? extends Person>라면 ?는 Person으로 치환된다.  이와 같이 제네릭이 타입을 체크하고 컴파일이 되면 class 파일에서 지워지게된다. 즉 컴파일시에는 타입의 제약이 존재하지만 런타임에는 제약이 사라진다는 의미이다.






