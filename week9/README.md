Java Live Study
===
## 9주차 과제


### 학습 목록

1. 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)
2. 자바가 제공하는 예외 계층 구조
3. Exception과 Error의 차이는?
4. RuntimeException과 RE가 아닌 것의 차이는?
5. 커스텀한 예외 만드는 방법

---

## 예외 , 에러

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우를 에러 또는 오류라고 한다.<br>

에러는 컴파일 에러와 런타임 에러로 나뉘는데 컴파일 에러란 컴파일러가 컴파일을 할 때 발생하는 에러이고
 런타임 에러는 프로그램을 실행할 때 발생하는 에러랄 이야기한다. 이외 논리적 에러도 존재한다.<br>

* 컴파일 에러 : 컴파일 시 발생하는 에러

* 런타임 에러 : 프로그램을 실행 시 발생하는 에러

* 논리적 에러 : 실행은 됮만 의도와 다르게 동작하는 것을 이야기한다.

<br>

컴파일 에러는 자바의 컴파일러가 문법 검사를 통해서 오류를 잡아내 준다. 그렇기 때문에 컴파일러가 검사를 통해 알려준 부분을 수정하여 정상적으로 프로그램을 실행 시킬 수 있다. <br>

하지만 컴파일 에러만으로 모든 에러를 처리했다고 생각할 수 없다. 이미 자바 프로그래밍을 하면서 실행은 되지만 수없이 많은 런타임 에러를 봤으며 앞으로도 계속 볼 것이다.<br>

자바에서는 런타임 에러를 예외(Exception)과 에러(Error)로 구분한다. <br>

에러는 메모리 부족(OutOfMemory), 스택오버플로우처럼 JVM이나 하드웨어 등의 기반 시스템의 문제로 발생하는 것으로 심각한 오류이다. 그렇기에 사전에 방지를 하고 신경을 써야 한다. <br>

반면 예외는 에러보다는 조금 덜 심각한 편으로(이 또한 심각한 것이긴 하지만) 개발자가 미리 적절한 코드를 작성하여 프로그램이 비정상적인 종료가 되지 않도록 방지할 수 있다.

<br>

---

<br>

## 자바에서 예외 처리 방법 (try, catch, throw, throws, finally)

예외처리란 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이며, 목적은 예외의 발생으로 인한 실행 중인 프로그램의 갑작스런 비정상 종료를 막고 정상적인 실행 상태를 유지함 에 있다.<br> 

### try-catch

기본적인 예외 처리 구문이며 예외가 발생할 가능성이 있는 문장에 대해 catch문을 사용하여 예외가 발생 시 예외에 맞는 catch문이 실행 된다.

<br>

```java
try{
    //예외발생이 예상되는 code를 작성한다.
}catch(예외처리class 객체명){
    //예외가 발생했을 때 제공할 code
}catch(예외처리class 객체명){
    //예외가 발생했을 때 제공할 code
}finally{
    //예외가 발생하거나 발생하지 않더라도 실행되어야 할 code
}
```

<br>

예외가 발생하면 가장 처음 발생한 예외에 해당하는 catch문을 실행 후 종료된다. (여러개의 catch문이 실행되지 않는다.) <br>

**catch문을 작성할 때 제일 위에 있는 catch문 부터 마지막 catch문까지 최하단에 있는 자식부터 최상단 부모 순으로 작성한다.** (처음 catch문 부터 예외의 부모인 Exception으로 처리하면 세세한 예외를 처리할 수 없다.)

* 예제 코드

```java
try {
	num1 = Integer.parseInt(args[0]);//ArrayIndexOutOfBoundsException, NumberFormatException
	num2 = Integer.parseInt(args[1]);//ArrayIndexOutOfBoundsException, NumberFormatException

	result = num1/num2;//ArithmeticException
	System.out.printf("%d / %d = %d",num1, num2 , result);

}catch (ArrayIndexOutOfBoundsException aioobe) {
	
}catch(NumberFormatException nfe){
	
} catch (ArithmeticException e) {
	
} catch (Exception e){ //개발자가 인식하지 못하는 예외 처리를 하기 위해 최상위 예외class를 마지막에 정의해준다. 
}
```
<br>

### 멀티 catch 블럭

JDK1.7부터 여러 catch 블럭을 '|'를 이용하여 하나의 catch블럭으로 합칠 수 있게 되었다.

```java
try{
    ,,,
}catch( Exception A | Exception B){
    e.printstackTrace();
}
```
<br>

멀티 catch블럭을 사용할 때 주의할 점은 **자식 예외와 부모 예외를 같은 블럭으로 묶을 수 없다. 왜나하면 그 경우 부모 예외만 써주는 것과 똑같은 것이기 때문에 에러가 발생한다.** 

<br>

또한 멀티 블럭은 **하나의 catch블럭으로 여러개의 예외를 처리하는 것이기 때문에 실제로 어떠한 예외가 발생했는지 알 수 없다. 어떠한 예외인지 알고 싶다면 instanceof로 어떤 예외가 발생했는지 확인하고 처리할 수 있다.**

<br>

### throw

throw는 예외를 강제로 발생 시킬 때 사용한다. 

1. 먼저 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든다.
```java
Exception e = new Exception("예외문구");
```

2. throw를 이용해 예외를 발생시킨다.
```java
throw e;
```

<br>

throw는 주로 사용자 정의 예외처리 클래스와 사용하게 되며 특정 상황에서 예외를 발생시켜 처리할 때 사용한다.

```java
public void test() throws Exception{
   throw new Exception(); 
}
```

<br>

주로 예외를 발생 시킨 method에서 try~catch로 처리하는 것이 아니라 사용하는 쪽에서 예외를 처리하도록 해준다.

<br>


### throws 

throws를 통해 메서드에 예외를 선언 할 수 있다. 여러개의 메서드를 쉼표로 구분해서 선언할 수 있다.

```java
void method() throws ExceptionA, ExceptionB,,,{
    //method 내용
}
```

<br>

메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때 이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어져야 하는지 쉽게 알 수 있다.<br>

throws로 예외가 선언된 메서드를 사용할 때는 메서드를 호출한 쪽에서 try~catch로 예외를 처리해줘야한다. 그래서 throws를 사용해 예외를 던진다는 말을 주로 사용하는 것 같다. <br>

**throws를 사용하여 예외를 처리할 때 장점은 일을 처리하는 코드와 예외를 처리하는 코드를 분리할 수 있다.**

<br>


### finally

finally 블럭은 예외의 발생여부와 상관 없이 실행되어야 할 코드를 포함 시킬 목적으로 사용된다. try~catch문 끝에 사용할 수 있으며 필수 조건은 아니다.

```java
try{
    //예외가 발생할 가능성이 있는 코드들
}catch( Exception e){
    //예외가 발생하였을 때 실행 될 코드들
}finally{
    //예외의 발생여부에 관계 없이 실행될 코드들
}
```

<br>

**finally문은 try~catch문에 return문이 있더라도 실행되며** finally문에서도 예외가 발생할 수 있으며 예외가 발생 시 처리해주어야 한다.

<br>

* 예제코드
```java

Connection con = null;
PreParedStatement pstmt = null;
ResultSet rs = null;

try{
    con = DriverManager.getConnection(url,id,pwd);
    String sql = "SELECT * FROM EMP";
    pstmt = con.PreParedStatement(sql);
    rs = pstmt.executeQuery();
}catch(SQLException e){
    e.printStacktrace();
}finally{
    if(rs!=null){
        rs.close();
    }
    if(pstmt!=null){
        pstmt.close();
    }
    if(con!=null){
        con.close();
    }
}
```

<br>

---

<br>

## 자바가 제공하는 예외 계층 구조


<img src = https://user-images.githubusercontent.com/74294325/100398518-5424fa00-3092-11eb-96da-cdbb3f17c25d.png>

<br>

* 모든 예외의 최고 조상은 Exception class이다.

* 런타임 예외 class들은 주로 프로그래머의 실수에 의해서 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다.

* 런타임 예외를 제외한 나머지 예외들은 주로 외부의 영향으로 발생할 수 있는 것들로서, 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많다.

<br>

정리하자면 런타임 예외는 프로그래머의 실수로 발생하는 예외들, 런타임 예외를 제외한 예외들은 사용자의 실수와 같은 외적인 요인들에 의해 발생하는 예외들이다.


<br>

---

<br>

## 런타임 예외와 그 외의 예외들의 차이


런타임 예외는 상속받는 자식 클래스들의 주로 치명적인 예외 상황을 발생시키지 않는 예외들로 구성되어 있다. <br>

그렇기 때문에 런타임 예외를 Unchecked Exception으로 런타임 예외가 아닌 것을 Checked Exception으로 분류 한다. <br>

둘의 가장 큰 차이는 Checked Exception의 경우 예외를 예측하고 복구할 수 있기 때문에 예외를 처리하는 Exception Handler가 강제 됩니다.<br>

간단하게 이아기 하자면 try~catch문이 강제된다는 말이다. 하지만 런타임 예외는 그렇지 않다. <br>

두 예외는 사용자 정의 예외를 만들 때도 차이가 나는데 **Checked Exception 종류의 사용자 정의 예외를 만들려고 하면 Exception을 상속 받으면 되고 Unchecked Exception 종류의 사용자 정의 예외를 만들려고 하면 RuntimeException을 상속받으면 된다.**

<br>

---

<br>


## 커스텀한 예외 만드는 방법.

사용자 예외를 만들어 사용하면 장점으로 자바 API에서 제공하는 예외 class외의 예외를 만들 수 있다. 또한 컴파일러가 체크하지 않는 실행 예외를 선언할 수 있다. <br>

**2가지 생성자를 선언하여 1개의 생성자는 기본생성자로 작성하고 또 다른 한개는 예외 오류문을 작성할 message를 작성할 수 있도록 해 작성할 수 있다.**

```java
public class Test {
    public static void main(String[] args){
        ExceptionTest exceptionTest = new ExceptionTest();
        try {
            exceptionTest.test();
        } catch (CustomException e) {
            e.printStackTrace();
        }
    }
}

public class ExceptionTest {
    public void test() throws CustomException {
        throw new CustomException("사용자 정의 예외입니다.");
    }
}

public class CustomException extends Exception {
    public CustomException() {
        super();
    }
    
    public CustomException(String message) {
        super(message);
    }   

}
```

## Java_Live Study를 듣고 새로 공부하게 된 내용.

1. 커스텀한 예외는 먼저 자바가 기본적으로 제공하는 예외를 사용해도 처리 할 수 없을 때 만드는 것이 기본적이며 Runtime Exception을 잘 알고 있어야 한다.

<br>

2. try-with-resources

try-with-resources는 하나 이상의 리소스를 정의하는 try와 관련된 문법이다. 리소스는 프로그램이 끝내기 전에 반드시 종료되어야 하는 객체를 의마한다. <br>

try-with-resources문은 마지막에 리소스를 닫도록 해준다. **try-with-resources를 사용할 수 있는 리소스는 java.lang.AutoCloseable, java.io.closeable을 구현해야 사용할 수 있다.**
<br>

JDBC로 DB와 연결할 때를 예로 들어보면 DB와 연결할 때 사용하는 Connection과 Statement의 종류들은 반드시 사용 후 종료해야 하는 리소스 들이다. <br>

이들 또한 자바 API를 확인해 보면 AutoCloseable을 구현하고 있다. 이전 try-with-resources를 사용하지 않았을 때의 내가 사용하던 코드이다.

<br>

```java
public int insert() throws SQLException{
    
    try{
    Class.forName("oracle.jdbc.OracleDriver");
    }catch(ClassNotFoundException e){
        e.printStackTrace();
    }

    String url = "jdbc:oracle:thin:@localhost:1521:orcl";
    String id = "scott";
    String pwd = "tiger";

    Connection con = null;
    PreparedStatement pstmt = null;

    try{
        con = DriverManager.getConnection(url,id,pwd);
        String sql = "INSERT INTO EMP (EMPNO, ENAME) VALUES (? , ?)";

        pstmt = con.PreparedStatement(sql);

        pstmt.setString(1,10);
        pstmt.setString(2,Lee);

        pstmt.executeUpdate();
    }finally{
        if(pstmt!=null){
            pstmt.close();
        }
        if(con!=null){
            con.close();
        }
    }
}   
```
<br>

JDK 1.7 이후 부터는 try-with-resources문을 적용하여 작성하면 finally에서 닫지 않아도 try문에서 사용한 리소스 들을 종료 시점에서 닫아준다. <br>

**주의해야할 점은 try-with-resources에서 선언한 순서 역순으로 리소스가 닫힌다.(선언을 con,pstmt 순으로 했다면 닫히는 순서는 pstmt,con순이다.)** <br>

한번 적용해보자.

<br>

```java
public int insert() throws SQLException{
    
    try{
    Class.forName("oracle.jdbc.OracleDriver");
    }catch(ClassNotFoundException e){
        e.printStackTrace();
    }

    String url = "jdbc:oracle:thin:@localhost:1521:orcl";
    String id = "scott";
    String pwd = "tiger";

    String sql = "INSERT INTO EMP (EMPNO, ENAME) VALUES (? , ?)";

    try(con = DriverManager.getConnection(url,id,pwd);
        pstmt = con.PreparedStatement(sql)){

        pstmt.setString(1,10);
        pstmt.setString(2,Lee);
        pstmt.executeUpdate();
    }
}
  
```

<br>

이 처럼 try()안에 AutoCloseable를 구현한 객체를 할당해 주면 try~catch()절이 종료될 때 객체의 close()가 자동으로 호출 된다.



