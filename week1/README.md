Java Live Study
===
## 1주차 과제
---

### 학습 목록
1. JVM이란 무엇인가.
2. 컴파일 하는 방법
3. 실행하는 방법
4. 바이트 코드란 무엇인가?
5. JIT 컴파일러란 무엇이며 어떻게 동작하는지
6. JVM 구성요소
7. JDK와 JRE의 차이

---

### JVM이란 무엇인가 , JVM의 구성요소
 * JVM은 Java Virtual Machine의 줄임말로 어느 환경 <br>
 (Mac, Windows, Linux)이던지 자바 프로그램을 실행할 수 있도록 도와주는 프로그램

 * JVM은 OS가 ByteCode를 이해할 수 있도록 해석해 주는 역할을 한다.

 * image
 
    <img src="https://user-images.githubusercontent.com/74294325/99254812-4bdceb80-2856-11eb-8bcd-e199ac3831d1.JPG" width="500" height="300">

    >출처 (https://velog.io/@hono2030/JVM%EC%9D%98-%EA%B5%AC%EC%A1%B0)

 * JVM의 구성
1) Class Loader
     >class파일들을 모아서 JVM이 운영체제로 부터 할당 받은 메모리 영역인 <br>
      Runtime Data Area로 적재하는 역할
2) Execution Engine
     >Class Loder에 의해 메모리에 적재된 Class들을 기계어로 변경해 명령어 단위로 실행
3) Garbage Collector
     >Heap 메모리 영역에 생성된 객체들 중에 참조되지 않은 객체들을 제거하는 역할
4) Runtime Data Area
     >자바 Application 실행시 사용되는 데이터를 적재하는 영역
     
---
### 컴파일 하는 방법 및 실행하는 방법

* 컴파일의 목적은 원시 언어에서 목적언어로 바꾸는 작업이다.
    > 프로그래밍에서는 고급 언어를(사람이 작성한 code)를 기계어로 바꿔주는 것을 말한다.

* 컴파일을 하기 위해서는 JDK 자바 개발툴이 필요하다 (java.exe, javac.exe)

* 컴파일의 순서
    1. src code를 작성한다.
    2. src code가 생성이 되면 Java Compiler의 javac 라는 명령어를 사용해 <br>
    src를 기반으로 .class 파일을 생성한다. (아직 컴퓨터가 읽을 수 없는 자바 바이트코드)
        >javac.exe를 사용하여 src파일을 .class파일로 컴파일
    3. 생성된 .class파일은 class loder에 의해 JVM내로 로드 된다.
    4. 실행엔진에 의해 기계어로 해석되어 메모리 상(Runtime Data Area)에 배치됩니다.
        >실행 엔진이란 Interpreter와 JIT(Just-In-Time가 있습니다.<br>
        java.exe 실행


* code
    ```java
    javac.소스코드명.java;
    //src 파일을 class 파일로 컴파일 하는 작업.(javac.exe)

    java 클래스파일명;
    //컴파일 된 class 파일을 실행한다. (java.exe)

---
### 바이트 코드란 무엇인가

* BYTECODE
    >-바이트코드(Bytecode, portable code, p-code)는 특정 하드웨어가 아닌 가상 컴퓨터에서 돌아가는 실행 프로그램을 위한 이진 표현법이다. <br> 하드웨어가 아닌 -소프트웨어에 의해 처리되기 때문에, 보통 기계어보다 더 추상적이다.

* 자바 컴파일러로 변환되는 코드의 명령어 크기가 1바이트라서 바이트코드라고 불린다고 한다.

* JVM이 이해할 수 있는 언어로 변환된 코드를 의미한다.
    >src코드를 컴파일시 .class의 Bytecode 생성

* 실행을 할 때 src코드가 아닌 Bytecode를 java.exe로 실행시킨다.

* **os에 종속되지 않고 JVM만 있다면 실행할 수 있다.**

---

### JIT 컴파일러란 무엇이며 어떻게 동작하는지

* Just-In-Time compilation(JIT)란 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일 기법입니다. <br> 
(interpreter 방식의 단점을 보완하기 위해 도입된 방식)

* JIT Compiler에 의해 해석된 코드는 캐시에 보관하기 때문에 한 번 컴파일 된 후에는 빠르게 수행하는 장점이 있습니다. <br> 하지만 인터프리팅 방식보다는 훨씬 오래 걸리므로 한번만 실행하면 되는 코드는 인터프리팅 하는 것이 유리합니다.

---

### JDK와 JRE의 차이

1. JDk (ava Development kit)
    * 자바 프로그래밍시 필요한 컴파일러 등 포함
    * JDK는 개발을 위해 필요한 도구 (javac,java)등을 포함한다.
    * JDK를 설치하면 JRE도 같이 설치가 된다.
        >즉 JDK = JRE + @라고 생각하면 된다.

2. JRE (Java Runtime Enviroment)
    * 컴파일된 자바 프로그램을 실행시킬 수 있는 자바 환경
    * JRE는 JVM이 자바 프로그램을 동작시킬 때 필요한 라이브러리 파일들과 기타 파일들을 가지고 있다.
    * JRE는 JVM의 실행환경을 구현했다고 할 수 있다.
    * 자바 프로그램을 실행시키기 위해서는 JRE를 반드시 설치해야한다.

    * 자바 프로그래밍 도구를 포함하고 있지 않아 JDK를 설치해야한다.

* image

    <img src = "https://user-images.githubusercontent.com/74294325/99258862-83e72d00-285c-11eb-8579-c4e2a92bf4e9.JPG" width = "400" height = "300">

*   출처(https://goodgid.github.io/Java-JDK-JRE/)
