Java Live Study
===
## 7주차 과제


### 학습 목록

1. package 키워드
2. import 키워드
3. 클래스패스
4. 접근지시자
 
---

## Package keyword

* package란 class, interface를 그룹 단위로 묶어 놓아 효율적으로 관리할 수 있도록 하는 개념이다.

* 하나의 소스파일에 첫 번째 문장으로 단 한번 선언된다.

* 모든 class는 반드시 하나의 package에 속해야 한다.

* package는 . 을 이용하여 계층구조로 구성할 수 있다.

* package명은 class명과 구분하기 쉽게 소문자로 작성을 원칙으로 하고있다.

* package를 선언하지 않은 class는 자바에서 제공하는 unnamed package에 속하게 된다.

---
## import keyword

* 소스 코드를 작성할 때 다른 package의 class를 사용하려면 package명.class명으로 사용해야 한다. 하지만 import를 사용하면 class명으로만 사용이 가능하다.

* **import의 역할은 컴파일러에게 소스페일에 사용된 class package에 대한 정보를 제공하는 역할을 한다.**

* import문은 package문 다음에 선언이 된다.

### static import

* static import문을 사용하면 static멤버를 호출할 때 class의 이름을 생략할 수있다.

```java
//ex
import static javalang.math.*;

public static void main(String[] args){
    System.out.println(random());
}
```
---

## class path

* class를 찾기위한 경로로 **JVM이 프로그램을 실행할 때 class 파일을 찾게되는데 class파일을 찾을 때 class path를 기준으로 한다.**

*  java runtime(java 또는 jre)으로 이 .class 파일에 포함된 명령을 실행하려면, 먼저 이 파일을 찾을 수 있어야 한다. 이때 .class 파일을 찾을 때 classpath에 지정된 경로를 사용한다.

### class path를 지정하는 방법

1. 환경 변수 Classpath를 사용

* 제어판에서 CLASSPATH를 통해 설정할 수 있으며 JVM이 시작될 때 환경 변수가 설정되어 있는 디렉토리에 있는 class들이 JVM에 로드된다.<br>(CLASSPATH는 필수 class들이 위치한 디렉토리를 등록한다.)

2. java runtime에 -classpath plag를 사용하는 방법
```java
javac <options> <souce files>
```

* 컴파일러가 컴파일 하기 위해서 필요로 하는 참조할 클래스 파일들을 찾기 위해서 컴파일시 파일 경로를 지정해주는 옵션

```java
//Hello.java파일이 C:\Java 디렉터리에 존재하고,
//필요한 클래스 파일들이 C:\Java\Engclasses에 위치한다면,
javac -classpath C:\Java\Engclasses C:\Java\Hello.java


//만약 참조할 클래스 파일들이 그 외의 다른 디렉터리, 그리고 현 디렉토리에도 존재한다면,
javac -classpath .;C:\Java\Engclasses;C:\Java\Korclasses C:\Java\Hello.java
// . 은 현 디렉토리, .. 은 현 디렉토리에서 상위 디렉토리를 의미한다.

```

---
## 접근 지시자

* 접근 지시자는 멤버 또는 class에서 사용이 되며 해당하는 맴버 또는 class를 외부에서 접근하지 못하도록 제한하는 역할을 한다.

* 접근 지시자가 default임을 알리기 위해 실제로 default를 붙이지 않는다. class나 멤버변수, method, 생성자에 접근 지시자가 지정되어 있지 않다면 default를 의미한다.

### 접근 지시자의 종류

| 지시자 | 같은 class | 같은 package | 자식 class | 전체
| :---: | :---: | :---: | :---: | :---: |
public | O | O | O | O
Protected | O | O | O | X
(Default) | O | O | X | X
private | O | X | X | X

* public : 접근 제한이 없어서 어디서든 접근이 가능하다.

* protected : 같은 Package, 같은 class에서 사용이 가능하며 package에 관계 없이 상속 받은 자식 class에서만 접근이 가능하다.

* private : 같은 class에서만 사용이 가능하기에 가장 높은 제한이다.

* default : 같은 package, 같은 class에서만 접근이 가능하다.

### 대상에 따라 사용할 수 있는 접근 지시자

* Class : public, default

* method, 멤버변수 : public, protected, default, private

* 지역변수 : 없음

### 접근 제어자를 이용한 캡슐화

* class나 member에 접근 제어자를 사용하는 이유는 **class의 내부에 선언된 데이터를 보호하기 위함이다**.

* 데이터가 **유효한 값을 유지하도록, 외부에서 함부로 변경할 수 없도록** 접근을 제안할 때 사용.

* class 내에서만 사용되는 멤버변수나 부분 작업을 처리하기 위한 method 등의 멤버들을 class 내부에 감추기 위함이다.
<br>(외부에서 접근할 필요가 없는 것들은 private으로 지정하여 노출시키지 않음.)

* 멤버변수에 직접 접근하는 것이 아닌 getter,setter를 만들어 유효성을 검사 후 접근하도록 하는 것이 바람직 하다.

### 생성자의 접근 지시자

* 생성자에 접근 지시자를 사용함으로써 Instance의 생성을 제한할 수 있다.

* 생성자의 접근 지시자를 private으로 설정하면 class 외부에서는 Instance를 생성할 수 없다.

* Singleton pattern으로 디자인 할 때 생성자의 접근 지시자를 private으로 주고 getter를 public static 으로 만들어 class에서 생성된 Instance를 가져다 사용하게 디자인 한다.

* 생성자의 접근 지시자가 private이라면 **부모 class가 될 수 없다.** 
<br>이러한 경우 class의 제어자에 final을 붙여 상속할 수 없음을 표기해 주는것이 좋다.

* Singleton 
```java
class Singleton{

    private static singleton s;

    private singleton(){

    }

    public Singleton getInstance(){
        if(s == null){
            s = new Singleton();
        }
        return s;
    }
}
```

### 제어자

* 접근 지시자 이외에 제어자들

* static

    * class를 생성할 때 모든 instance에 공통적으로 사용해야 하는 것에는 static을 지정해준다.

    * static이 붙은 멤버는 instance를 생성하지 않고도 사용이 가능하다. **static이 붙은 멤버는 class가 메모리에 올라갈 때 같이 올라가 생성된다.**

    * **static이 붙은 method에서는 instance영역에 직접 접근을 할 수 없다.** static은 class가 메모리가 올라 갈 때 생성되지만 instance는 생성이 될 때 heap에 올라가기 때문이다.

    * 반대로 instance에서는 static영역에 직접 접근이 가능하다.

    * method작업내용중에 instance 변수를 필요로 한다면 static을 붙일 수 없다. 반대로 instance 변수를 필요로 하지 않는다면 가능한 static을 붙이는 것을 고려해볼 필요가 있다. <br>
    (method 호출 시간이 짧아지기 때문에 효율이 높아진고, static을 안붙인 메서드는 실행시 호출되어야할 메서드를 찾는 과정이 추가적으로 필요하기 때문에 시간이 더 걸린다)

* final

    * class에 사용할 때 : 상속을 할 수 없는 class임을 의미한다.
    * method에 사용할 때 : overriding이 될 수 없는 method임을 의미한다.
    * 변수에 사용할 때 : 변경될 수 없는 상수임을 의미한다.

    * 주로 멤버변수에 final을 사용할 때 static과 함께 사용한다 **instance가 생성될 때마다 새로운 메모리를 사용하지 않고 class 레벨에서 이미 생성된 메모리 공간을 쭉 가져다가 사용하면 되기 때문이다.**

* abstract 

    * 미 완성의 의미를 가지고 있으며 class나 method에서 사용할 수 있으며 추상 class나 추상 method를 선언할 때 사용한다.
