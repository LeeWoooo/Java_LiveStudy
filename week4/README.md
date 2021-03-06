Java Live Study
===
## 4주차 과제


### 학습 목록
1. 선택문

2. 반복문
---

### 조건문, 반복문

* 순차적을 위에서 아래로 한 문장씩 진행되었던 `코드들을 조건에 따라 흐름을 바꾸어 조작`하기 위해 사용합니다.

* 조건문은 `조건에 따라 다른 문장이 수행`되도록 하고, 반복문은 `조건에 따라 특정 문장들을 반복`해서 실행한다.

---

## 조건문

* 조건문은 조건식과 문장을 포함하는 블럭{}으로 구성되어 있다.

    * 조건식에는 일반적으로 비교연산자와 논리연산자로 이루어져있다.

        * `기본형 Datatype의 값은 전부 비교가 가능하다.`

        * `문자열의 주소를 비교할 때 ==를 사용, 문자열의 내용을 비교할 때는 .equals()를 사용한다.`

    * 블럭{}에는 여러문장들을 묶어놓은 것으로 조건식에 결과에 따라 수행될 코드들이 들어있다.

* 조건식의 연산 결과에 따라 문장안에 있는 code들의 수행여부가 결정된다. 연산결과의 반환형은 boolean

* 조건문에는 if문과 switch문이 존재한다.

### if

* if문의 생김새

    * 단일 if
        ```java
        if(조건문){
            //조건문의 연산결과가 true일때 수행될 문장
        }
        ```

        * 프로그램이 실행되는 도중 `특정 조건에만 실행되어야 할 작업이 있을 경우.`

        * if문이 수행되지 않아도 프로그램이 실행되는데 error가 발생하지 않는다.

        * 블럭안에 수행될 문장이 하나일 경우 {}기호를 생략할 수 있지만 가독성을 위해 사용해주는것이 좋다.

    * if else
        ```java
        if(조건문){
            //조건문의 연산결과가 true일때 수행될 문장
        }else{
            //조건문의 연산결과가 false일때 수행될 문장
        }
        ```
        * 프로그램이 실행되는 도중 `둘중 하나의 코드가 반드시 실행되어야 할때 사용한다.`

        * else절이 추가가 되었으며 조건문의 결과가 false일 때 사용한다.


    * else if
        ```java
        if(조건문1){
            //조건문1의 연산결과가 true일때 수행될 문장
        }else if(조건문2){
            //조건문1의 연산결과가 false일때 수행될 문장
        }else{
            //조건문1,조건문2의 연산결과가 false일때 수행될 문장
        }
        ```

        * if else는 `두가지 경우 중 하나가 수행되는 구조이면 eise if문은 셋 이상인 경우 사용한다.`

        * 마지막 `else절의 생략이 가능하다. 만약 else if 절에서 모든 조건문이 false이고 else절이 없다면 아무것도 실행되지 않는다.`

        * else if는 연관이 있는 여러 조건들을 비교할 때 사용된다.

    * 중첩 if
        ```java
        if(조건문1){
            //조건문1의 연산결과가 true일때 수행될 문장
            if(조건문2){
                //조건문1,조건문2의 연산결과가 true일때 수행될 문장    
            }else{
                //조건문1의 연산결과가 true,조건문2의 연산결과가 false일때 수행될 문장
            }
        }else{
            //조건문1의 연산결과가 false일때 수행될문장
        }
        ```

        * 여러 조건을 중첩시켜 사용할 때 사용한다.



### switch

* if문은 경우의 수가 많아지면 else if절이 계속해서 증가되어 조건식이 복잡해진다. 이 경우 switch문을 사용해 가독성을 높인다.

* switch문은 `단하나의 조건식으로 많은 경우의 수를 처리할 수 있다.`

* switch문의 생김세

    ```java
    switch(조건식){

        case 값1:
            //조건식의 결과 값이 값1과 같을 경우 수행될 문장
        
        case 값2:
            //조건식의 결과 값이 값2와 같을 경우 수행될 문장

        default :
            //조건식의 결과 값이 case문에 일치하는 값이 없을 경우 수행될 문장
    }
    ```

    * default절은 if문의 else절과 같은 역할을 한다. `default절 또한 생략이 가능하다.`

    * `break가 없을 경우 조건식에 결과 값이 해당되는 case절을 수행후 자신의 밑에 있는 case문을 수행 후 switch문을 탈출한다.`

    * 조건식의 결과 값에 해당하는 case문만 수행하고 나가고 싶을 경우 case절 안에 break를 사용한다.

    * switch의 조건식의 결과는 정수, 문자열이 될 수 있다.

    * `case문의 값은 정수 상수만 가능하며 중복은 포함하지 않는다. 가독성을 위해 값을 constant처리하여 사용할 수 있다.`

    * switch문 또한 중복해서 사용할 수 있다.

---

## 반복문

* 반복문은 어떤 작업이 반복적으로 수행되도록 할 때 사용되며 종류로는 for, while문이 있다.

### for

* `for문은 초기화, 조건식, 증감식, 블럭{}으로 이루어져있다.`

* 초기화

    * 반복문에서 `사용될 변수를 초기화` 하는 부분이며 초기화시 1개 이상의 변수초기화 가능

* 조건식

    * 반복문이 실행될 때 `변수가 값이 변경되면 변경된 변수가 조건식에 연산되어 결과를 boolean으로 반환`

* 증감식

    * `반복문을 제어하는 변수의 값을 증가시키거나 감소`시키는 식이다.

* 블럭{}

    * 조건문의 연산된 값이 true일때 반복수행할 문장
    
* 작업 순서는 `초기화 -> 조건식 -> 블럭{} -> 증감식 -> 조건식` 순서이다.

* `for문은 반복문의 시작과 끝을 알 때` 적합하다 (반복문의 반복횟수를 알 때)

* for문 안에 변수를 선언하고 생성하면 코드를 실행하는 속도에 영향을 준다. (연산 속도 감소)

* 초기화, 조건식, 증감식은 필요에 의해 생략이 가능하다.

* 초기화, 조건식, 증감식이 모두 생략되면 true값으로 무한반복문이 된다.

* for문의 생김세

    * for

        ```java
        for(변수초기화; 조건식 ; 증감식){
        //반복될 수행 될 문장
        }
        ```



    * 중첩for문

        ```java
        for(초기화; 조건문; 증감식){
            for(초기화; 조건문; 증감식){

            }
        }
        ```

        * for문 안에 for문을 하나 더 중첩 시켜 사용하는 형태로 대부분 2차원배열과 같이 사용된다.

    * for each

        ```java
        for( 타입 변수명 : 배열 또는 컬렉션){
            //반복될 문장
        }
        ```

        * JDK 1.5부터 배열과 컬렉션에 저장된 요소에 접근할 때 기존 방법보다 편리한 for each문 추가

        * 배열 또는 컬렉션의 index를 사용할 수 없다.

        * 배열 또는 컬렉션의 첫 방부터 끝 방 까지 순서대로 읽혀 변수에 저장된다.


### While

* for문과 다르게 `while문은 반복의 시작과 끝을 알 수 없을때(반복 횟수를 모를때) 적합하다.`

* for문과 while문은 항상 서로 변환이 가능하다.

* while의 조건식은 생략이 불가능 하다.

* while의 생김세

    * while

        ```java
        while(조건문){
            //조건문의 연산결과가 true일때 반복수행 될 문장들            
        }

    * do while

        ```java
        do{
            //조건문의 연산결과가 true일때 반복수행 될 문장들
        }while(조건문);
        ```

        * while문과 다르게 블럭{}안에 있는 문장들이 무조건 한번 실행 된 후 조건문 연산

        * 조건문 뒤에는 ; 잊지말기.

---

## break, continue

## break

* `반복문이나 조건문에서 프로그램을 진행하던 도중 break를 만나면 가장 가까운 반복문 혹은 조건문을 탈출.`

* `대부분 조건문과 함께 사용`되며 조건에 맞을때 반복문 혹은 조건문을 탈출하는 용도로 사용된다.

## continue

* `반복문 내에서만 사용할 수 있으며`, 반복이 진행되는 도중 continue문을 만나면 반복문 끝으로 이동 후 다음 반복으로 넘어간다.

* for문같은경우 증감식으로 넘어가 조건식으로 진행되고 while문은 조건식으로 이동한다.

---

## 이름있는 조건문

* break는 근접한 단 하나의 반복문만 벗어날 수 있기 때문에 `여러개의 반복문이 중첩된 경우 이름을 붙여 하나 이상의 반복문을 탈출 가능케 한다.`

* 예제code

    ```java
    Loop1 : for(int i = 0; i < 11; i ++){
        for(int j = 0; j < 11; j ++){
            if(j == 5){
                break Loop1;
                break;
            } //end if
        } // end for
    } // end for
    ```

    * break Loop1;은 가장 외각에 있는 for문을 탈출한다.

    * break는 Loop1 for문 안에 중첩 for만 탈출한다.




