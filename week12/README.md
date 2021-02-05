Java Live Study
===
## 11주차 과제


### 학습 목록

1. 애노테이션 정의하는 방법
2. @retention
3. @target
4. @documented
5. 애노테이션 프로세서
---

<br>

## 애노테이션이란?

프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것을 애노테이션이라 한다. 애노테이션은 주석(comment)처럼 프로그래밍 언어에 영향을 미치지 않으며서도 다른 프로그램에게 유용한 정보를 제공할 수 있다는 장점을 가지고 있다. <br>

애노테이션은 JDK에서 기본적으로 제공하는 것과 다른 프로그램에서 제공하는 것들이 있다. JDK에서 제공하는 표준 애노테이션은 주로 컴파일러를 위한 것으로 컴파일러에게 유용한 정보를 제공한다. 그리고 새로운 애노테이션을 정의할 때 사용하는 메타 애노테이션을 제공한다.
>JDK에서 제공하는 애노테이션은 'java.lang.annotation' 패키지에 포함되어 있다.

## 표준 애노테이션

JDK에서 제공하는 애노테이션으로 종류와 기능은 이러하다<br>

애노테이션 | 설명
:--- | :---
@Override | 컴파일러에게 오버라이딩하는 method임을 명시해준다.
@Deprecated | 앞으로 사용하지 않을 것을 권장하는 대상에게 붙인다.
@SuppressWarnings | 컴파일러의 특정 경고메세지가 나타나지 않게 해준다.
@SafeVarargs | 제네릭 타입의 가변인자에 사용한다(JDK 1.7)
@Functionalinteface | 함수형 인터페이스라는 것을 알려준다(JDK 1.8)
@Native | native method에서 참조되는 상수 앞에 붙은다.(JDK 1.8)

<br>

1. @Override

    * method 앞에만 붙일 수 있는 애노테이션으로, 부모의 method를 오버라이딩하는 것이라는걸 컴파일러에게 명시해 줄 수 있다. 애노테이션을 명시하면 컴파일러가 현재 method가 부모에게 있는지 확인해주고 없으면 error메세지를 출력한다.

    ```java
    class Parent{
        void parentMethod(){

        }
    }

    class Child extends Parent{
        @Override
        void parentMethod(){

        }
    }
    ```

<br>

2. @Deprecated

    * 새로운 버전의 JDK가 나올때 새로운 기능이 추가 될 뿐만 아니라 기존의 부족했던 기능들도 개선하게 된다. 이 과정해서 기존의 기능을 대체할 것들이 추가되어도 이전 버전에서 사용되고 있는 기능들을 함부로 삭제할 수 없다. 그래서 이전 버전의 기능들을 사용하지말고 개선된 기능을 사용하라고 권장하는 애노테이션이다. <br>

    @Deprecated이 붙은 것을 사용하면 컴파일러가 아래와 같은 메시지를 나타낸다.
    ```java
    Note: 실행한 소스파일명 uses or overrides a deprecated API
    Note: Recompile with -Xlint:deprecated for details.
    ```
    -Xlint:deprecated를 붙여 다시 컴파일 하면 더 자세한 내용을 알 수 있다는 내용이다.

<br>

3. @SuppressWarnings

    * 컴파일러가 보여주는 경고메세지가 나타나지 않게 억제해준다. 경우에 따라 경고가 발생하는 것을 알면서도 묵인해야 할 때가 있는데 이러한 때에 사용을 한다. 주로 묵인하는 경고는 deprecation, unchecked,rawtypes,vararge정도 이다. 

        1. deprecation : @Deprecated이 붙은 대상을 사용했을 때.
        2. unchecked : 제네릭타입을 지정하지 않았을 때.
        3. rawtypes : 제네릭을 사용하지 않았을 경우.
        4. vararge : 가변인자의 타입이 제네릭의 타입일 경우.

<br>

---

<br>

## 애노테이션 정의

애노테이션을 정의하는 방법은 @기호를 붙이는 것을 제외하면 인터페이스를 정의하는 것과 동일하다.<br>

```java
@interface 애노테이션명 {
    타입 요소이름(); //애노테이션의 요소를 선언한다.
    ,,,
}
```

<br>

### 애노테이션의 요소

**애노테이션 내에 선언된 method를 애노테이션 요소라고한다.** 애노테이션의 요소는 반환값이 있고 매개변수가 없는 추상 method의 형태를 가지며 상속을 통해 구현하지 않아도 된다. 다만 애노테이션을 적용할 때 이 요소들의 값을 빠짐없이 지정해줘야 한다.  <br>

* 예제 코드

```java
// 애노테이션 선언
@interface TestInfo{
    int conunt();
    String testedBy();
    String[] testTools();
    TestType testType(); //enum TestType {FRIST, FINAL}
    DateTime testDate();
}

@interface DateTime{
    String yymmdd();
    String hhmmss();
}
```
```java
//애노테이션 사용
@TestInfo{
    count = 3, testBy="Lee",testTools={"Java","Spring"},
    testType = TestType.FIRST,
    testDate = @DateTime(yymmdd ="210205",hhmmss="223900")
}
public class test{,,,}
```

<br>


애노테이션의 각 요소는 기본값을 가질 수 있으며 기본값이 있는 요소는 애노테이션을 적용할 때 지정하지 않으면 기본값을 사용한다.
>null을 제외한 모든 리터럴이 가능하다.

```java
@interface TestInfo{
    int count() default 1;
}

@TestInfo
public class test{,,,}
```

<br>

애노테이션의 요소가 하나 뿐이고 이름이 value인 경우 애노테이션을 적용할 때 요소의 이름을 생략하고 값만 적어도 된다.

```java
@interface TestInfo{
    String value();
}

@TestInfo("passed")
public class test{,,,}
```

<br>

요소의 타입이 배열인 경우, 괄호{}를 사용해서 여러개의 값을 지정할 수 있다.
```java
@interface TestInfo{
    String[] testTools();
}

@TestInfo(testTools = {"java","Spring"}) //값이 여러개인 경우
@TestInfo(testTools="java")//값이 하나인 경우
@TestInfo(testTools={})//값이 없을 경우 괄호{}가 필요하다
```

<br>

## @retention

애노테이션이 유지(retention)되는 기간을 지정하는데 사용된다. 애노테이션의 유지 정책의 종류는 다음과 같다. <br>

유지정책 | 의미
:--- | :---
SOURCE | 소스파일에만 존재. 클래스 파일에는 존재하지 않음.
CLASS | 클래스 파일에 존재. 실행시에 사용불가. 기본값
RUNTIME | 클래스 파일에 존재. 실행시에 사용가능.

<br>

@Override나 @SuppressWarnings 처럼 컴파일러가 사용하는 애노테이션은 유지정책이 SOURCE이다. <br>

유지 정책을 RUNTIME으로 하면 실행시 리플렉션을 통해 클래스파일에 저장된 애너테이션의 정보를 읽어서 처리할 수 있다. <Br>

유지 정책을 CLASS로 하면 컴파일러가 애노테이션의 정보를 class파일에 저장할 수 있게된다. 하지만 클래스 파일이 JVM에 로딩될 때는 애노테이션의 정보가 무시되어 실행 시에 애노텡이션에 대한 정보를 읽을 수 없다.

<br>

---

<br>

## @target

애노테이션이 적용가능한 대상을 지정하는데 사용된다. 예제 코드로 확인해보자.<br>

```java
@Target({TYPE,FIELD,METHOD,PARAMETER,CONSTRUCTOR,LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings{
    String[] value;
}
```

<br>

위의 코드는 SuppressWarnings 애노테이션의 코드이다. SuppressWarnings의 애노테이션을 적용할 수 있는 대상을 Target으로 지정하고 있다. <br>

대상 타입 | 의미
:--- | :---
ANNOTATION_TYPE | 애노테이션
CONSTRUCTOR | 생성자
FIELD | 필드 (멤버변수, enum 상수)
LOCAL_VARIABLE | 지역변수
METHOD | 메소드
PACKAGE | 패키지
PARAMETER | 매개변수
TYPE | 타입(class, 인터페이스, enum)
TYPE_PARAMETER | 타입 매개변수(JDK 1.8)
TYPE_USE | 타입이 사용되는 모든곳(JDK 1.8)

<br>

---

<br>

## @documented

애노테이션의 대한 정보가 javadoc으로 작성한 문서에 포함되도록 한다. 자바에서 제공하는 기본 애노테이션 중에 @Override와 @SuppressWarnings을 제외하고 모두 이 메타 애노테이션이 붙어 있다.

<br>

---

<br>

## 애노테이션 프로세서

애노테이션 프로세서는 컴파일 단계에서 소스를 조작할 수 있다. 대표적으로 Lombok,JPA등 많은 라이브러리에서 사용이 된다. 애노테이션 프로세서를 통해서 실행되기 전에 체크를 하면서 애노테이션이 의도한 대로 이루어지지 않는 경우 에러나 경고를 표시해주기도 하며 소스코드와 바이트코드 파일을 만들어 주기도 한다.

