# 📺 Java 왕기초 (feat. 이클립스)

### 📕 책 ) 혼자 공부하는 자바 참고

<br />

## Java Project 시작하기

자바 소스파일 생성 순서

> 1.  자바 프로젝트 생성
> 2.  package 생성
> 3.  자바 파일 (.java) 생성

<br/>

## 객체지향 프로그래밍

> 하나의 프로그램을 만들기 위한 부품(기능, 속성)들을 객체로 쪼개서 프로그래밍 하는 것

<br />

## 자바의 꽃 클래스 💐

> Class 클래스 : 객체를 만들기 위한 설계도. 필드와 메소드가 정의되어 있음

<br/>

### 클래스의 구성

- **Field 필드**

  > - 이름, 값 등의 객체의 속성을 나타내는 것.
  > - 생성자와 메소드 전체에서 사용되며, 객체가 소멸되지 않는 한 존재
  > - <=> 변수: 생성자와 메소드 내에서만 사용되고 생성자와 메소드 실행이 종료되면 사라짐

- **Constructor 생성자**

  > - 객체 생성시 초기화 역할 담당
  > - new 연산자로 호출되는 특별한 중괄호 블록 {}
  > - 필드를 초기화하거나 메소드를 호출해서 객체를 사용할 준비를 함
  > - 이름은 클래스와 똑같이 설정해야 하며, 리턴타입이 없음
  > - 명시적으로 개발자가 정의한 Constructor가 없으면 컴파일러가 자동으로 기본 생성

- **Method 메소드**

  > - 객체가 수행하는 실질적인 기능, 동작 (function)
  > - 객체간의 데이터를 전달하는 수단
  > - 필드를 읽고 수정하는 역할 / 다른 객체를 생성해서 다양한 기능을 수행

- **Instance 인스턴스**

  > - 클래스로부터 만들어진 객체

```java

public class ClassName{

    //Field 필드
    int fieldName ;

    //Constructor 생성자
    ClassName(){
        contents
    }

    //Method 메소드
    void methodName(){

    }
}
```

### 클래스의 두 가지 용도

1. 라이브러리 : 다른 클래스에서 이용할 목적으로 설계됨
2. 실행용: main() 메소드를 갖고 라이브러리 여러 클래스들을 참조하여 결과를 리턴함

<br />

Greeting.java
<라이브러리 용도>

```java
 public class Greeting {
	String message = "message";


	 //greet == local변수
    public void greet() {
       System.out.println("hi");
    }

```

TestGreeting.java <실행 용도>

```java
public class TestGreeting {

	// greeting을 참조한 reference타입
   public static void main (String[] args) {
      Greeting hello = new Greeting();

      //main이 있어야 실행 가능한 함수
      //Greeting() 호출
      //객체 생성시,힙영역에 memory allocation

      System.out.println(hello.toString());
      System.out.println(hello);

      hello.greet();

    }
 }

  <실행 결과>
 //chap01.intro.Greeting@1e81f4dc
 //hi

```
