Java Live Study
===
## 13주차 과제


### 학습 목록

1. 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O
2. InputStream과 OutputStream
3. Byte와 Character 스트림
4. 표준 스트림 (System.in, System.out, System.err)
5. 파일 읽고 쓰기

<Br>

---

<br>

## 스트림 (Stream) / 버퍼 (Buffer) / 채널 (Channel) 기반의 I/O

<Br>

### 스트림 (Stream)

자바에서 입출력을 수행하려면, 즉 어느 한쪽에서 다른 쪽으로 데이터를 전달하려면, 두 대상을 연결하고 데이터를 전송할 수 있는 무언가가 필요한데 이것을 stream이라고 부른다. (람다에서 사용되는 stream과는 같은 용어이지만 다른 개념.)

<br>

스트림의 **큰 특징은 단방향통신만 가능하다는 것이다.** 하나의 스트림으로 입력과 출력을 동시에 처리할 수 없다. 그렇기 때문에 입력과 출력을 동시에 수행하려면 입력스트림과 출력 스트림 2개가 필요하다. <br>

또 하나의 특징은 **먼저 보낸 데이터를 먼저 받게 되어 있으며** 중간에  건너뜀 없이 연속적으로 데이터를 주고 받는다. 큐(queue)와 같은 FIFO(First in Frist out)과 같은 구조이다.<br>

* 구조

    <img src = https://user-images.githubusercontent.com/74294325/108526517-91f19180-7314-11eb-85f8-f52cf15cff13.JPG>

<br>

### 버퍼 (Buffer)

버퍼는 CPU와 입출력장치의 속도 차이를 극복하기 위해 만들어진 것이다. 이 속도의 차이를 극복하기 위해 메모리 공간에 버퍼라는 공간을 만들어서 입력되거나 출력되는 데이터들을 버퍼공간에 쌓아두었다가 한번에 보내는 방식이다. <br>

* 구조 

    <img src = https://user-images.githubusercontent.com/74294325/108526550-9b7af980-7314-11eb-834e-97432739ab0f.JPG>

<br>

### 채널 (Channel) 기반의 I/O

채널은 서버와 클라이언트간의 통신수단을 나타낸다. ‘데이터가 다니는 통로’라는 점에서 스트림과 비슷하지만, 채널은 양방향이기 때문에 읽기와 쓰기를 동시에 하는 것이 가능하다.

<br>

---

<br>

## InputStream과 OutputStream

InputStream과 OutputStream은 모든 바이트기반 스트림의 조상이다(추상 클래스). 모든 바이트 기반 스트림은 이 둘의 클래스를 상속받아서 만들어진다.

입력 스트림 | 출력 스트림 | 입출력 대상의 종류 
:--- | :--- | :---
FileInputStream | FileOutputStream | File
ByteArrayInputStream | ByteArrayOutputStream | 메모리(byte배열)
PipedInputStream | PipedOutputStream | 프로세스(프로세스간의 통신)
AudioInputStream | AudioOutputStream | 오디오 장치

<br>

여러 종류의 입출력 스트림이 있으며 상황에 적합한 스트림을 가져다가 사용을 하면 된다. <br>


또한 InputStream과 OutputStream에서 제공하는 method들이 있다.

* InputStream (주요 method)

    method | 설명
    :--- | :---
    void close() | 스트림을 닫음으로써 사용하고 있던 자원을 반환한다.
    abstract int read() | 1byte를 읽어온다(0~255사이의 값). 더이상 읽어올 값이 없다면 -1을 반환한다. 추상 method라 자식class의 상황에 맞게 구현해서 사용
    int read(byte[] b) | 배열 b 크기만큼 읽어서 배열을 채우고 읽어온 데이터의 수를 반환한다.

<br>

* OutputStream (주요 method)

    method | 설명
    :--- | :---
    void close() | 스트림을 닫음으로써 사용하고 있던 자원을 반환한다.
    void flush() | 스트림 버퍼에 있는 모든 내용을 출력 소스에 쓴다.
    abstract void write(int b) | 주어진 값을 출력 소스에 쓴다.
    void write(byte[] b) | 주어진 배열 b에 저장된 모든 내용을 출력소스에 쓴다.

<br>

flush()와 같은 경우는 버퍼가 있는 OutputStream의 경우에만 의미가 있으며 버퍼가 없는 OutputStream에는 아무런 일도 일어나지 않는다. <br>

또한 프로그램이 종료될 때 사용하고 닫지 않은 스트림을 JVM이 자동적으로 닫아주기는 하지만 스트림을 사용해서 모든작업을 마치고 난 후에는 close()를 호출해서 반드시 닫아주어야 한다. (표준스트림은 닫아주지 않아도 된다.)

<Br>

---

<br>

## Byte와 Character 스트림

먼저 Stream의 객체도를 살펴보면 이러하다 

* Stream

    <img src = https://user-images.githubusercontent.com/74294325/100566033-4e384e80-3308-11eb-846f-9dec997dc880.png>

    출처(https://m.blog.naver.com/PostView.nhn?blogId=dg_667&logNo=220953203959&proxyReferer=https:%2F%2Fwww.google.com%2F)

<br>

위의 객체도를 살펴보면 InputStream/OutputStream 과 Reader/Writer로 나누어 지는것을 볼 수 있다. 이처럼 Stream에서는 8bit Stream(InputStream/OutputStream)과 16bitStream(Reader/Writer)를 지원해준다. <br>

아래의 그림을 보면서 조금 더 자세하게 이야기해보자면 8bit Stream을 byteStream이라고 하고 16bit Stream을 String Stream이라 한다.

* 구조

    <img src = https://user-images.githubusercontent.com/74294325/100746458-1292a700-3424-11eb-84a4-ec779a4cb463.png>

<br>

8bit stream과 16bit stream의 차이점을 볼 수 있는데 8bit Stream은 bit가 16bit Stream에 비해 작기 때문에 한번에 이동하는 데이터의 양은 적지만 속도가 빠르다. 반면에 16bit Stream은 8bit Stream에 비해 한번에 이동하는 데이터의 양은 많지만 속도가 느리다. <br>

데이터의 양과 속도 측면 말고도 차이점을 가지고 있는데 <br>

16bit Stream은 문자열에 한하여 읽고 쓰기가 둘다 가능하다.(유니코드로 된 문자를 입출력) <br>

8bit Stream은 모든 종류의 Data에 대한 입출력이 가능하다. <br>

만약 한글을 8bit Stream을 가지고만 읽어들인다면 한글이 깨지는 현상이 발생한다. 그렇기 때문에 16bit Stream과 같이 사용하게 된다. 자바에서는 8bit Stream과 16bit Stream을 연결해주는 InputStreamReader과 같은 class를 지원해준다. 아래의 그림을 통해 확인해보자면<br>

* 구조

    <img src = https://user-images.githubusercontent.com/74294325/100746822-9056b280-3424-11eb-8632-11d752ae31f1.png>

<br>

이와 같이 InputStreamReader가 중간에서 encoding을 해주는 역할을 수행하며 한글을 온전히 읽어올 수 있게 된다.

<br>

---

<br>


## 표준 스트림 (System.in, System.out, System.err)

표준 스트림은 콘솔(console,도스창)을 통한 데이터 입력과 콘솔로의 데이터 출력을 의미한다. 자바에서는 표준 스트림을 3가지 제공을 하는데(System.in, System.out, System.err) 이 들은 자바 어플리케이션의 **실행과 동시에 사용할 수 있도록 자동적으로 생성이 된다.** 그렇기 때문에 개발자가 별도로 스트림을 생성하는 코드를 작성하지 않고도 사용이 가능하다. <br>

* System.in : 콘솔로부터 데이터를 입력받는데 사용 
* System.out : 콘솔로 데이터를 출력하는데 사용
* System.err : 콘솔로 데이터를 출력하는데 사용

<br>

---

<br>

## 파일 읽고 쓰기

### 파일 읽기

* Reader사용
```java
public Filetest() throws /* FileNotFoundException, */ IOException {
    File file = new File("D:/dev/jsp/prj/fileRead.txt");
    // 1. File이 존재한다면 File의 Stream을 연결하겠습니다.
    if (file.exists()) {// file class를 사용하여 file의 유무를 확인하기 때문에 예외를 날리지 않아도 된다.
        FileReader fr = new FileReader(file);
        //2. 줄 단위로 읽어들이는 기능이 있는 Stream을 연결
        BufferedReader bfr = new BufferedReader(fr);

        //파일에 있는 내용을 전부 읽기
        String str = "";
        while ((str=bfr.readLine()) != null) { //파일의 끝 (EOF(end of file)에는 null이 나온다.
            System.out.println(str);
        } // end while
        
        //3. Stream의 사용이 종료되었다면 연결 종료
        bfr.close();
    } else {
        System.err.println(file + "이 존재하지 않습니다.");
    } // end else

}// UseFileReader

public static void main(String[] args) {

    try { new Filetest();
    } catch (IOException ie) {
        System.err.println("파일의 내용을 읽어들일 수 없습니다");
        ie.printStackTrace();
    } // end catch

}// main
```    

<Br>

* 8bit Stream 및 16Bit Stream 같이 사용

```java
File file = new File("D:/dev/jsp/prj/fileRead.txt");
		
String temp ="";

try(BufferedReader bufferedReader = new BufferedReader(new InputStreamReader(new FileInputStream(file)))){
    
    while((temp =bufferedReader.readLine())!=null) {
        System.out.println(temp.trim());
    }
}
```

<br>

### 파일 쓰기

* writer 사용
```java
public UseFileWrite() throws IOException {
    FileWriter fw = null;
    try {

        // 1.파일에 스트림 연결
        fw = new FileWriter(new File("c:/dev/temp/String_data.txt"));
        // 2.데이터를 스트림 기록.
        String msg = "코로나야 물러가라";
        fw.write(msg);
        // 3.스트림의 내용을 목적지로 분출
        fw.flush();
        System.out.println("정상적인 기록");
    } finally {
        // 4.연결끊기
        if (fw != null) {
            fw.close();
        } // end if
    } // end finally
}// UseFileWrite

public static void main(String[] args) {
    try {
        new UseFileWrite();
    } catch (IOException e) {
        e.printStackTrace();
    } // end catch
}// main
```

* 8bit Stream 및 16Bit Stream 같이 사용

```java
public UseFileWrite2() throws IOException {
    BufferedWriter bw = null;
    try {
        // 1.파일에 스트림 연결
        bw = new BufferedWriter(new FileWriter(new File("c:/dev/temp/String_data.txt")));
        // 2.데이터를 스트림 기록.
        String msg = "코로나야 물러가라 이겨보자";
        bw.write(msg);
        // 3.스트림의 내용을 목적지로 분출
        bw.flush();
        System.out.println("정상적인 기록");
    } finally {
        // 4.연결끊기
        if (bw != null) {
            bw.close();
        } // end if
    } // end finally
}// UseFileWrite

public static void main(String[] args) {
    try {
        new UseFileWrite2();
    } catch (IOException e) {
        e.printStackTrace();
    } // end catch

}// main
```