# 🌿 Abstact 추상화

> 객체를 직접 생성할 수 있는 클래스 == 실체 Class
> 이 실제 클래스들의 공통적인 특성을 추출해서 선언한 클래스 == **추상 Class**

<br >

### 🌲 추상 클래스의 용도?

- 하위 클래스에 공통되는 필드와 메소드를 추출하여 통일
- 실체 클래스를 작성할 시간 절약
  => 상속받는 실체 클래스를 생성할 떄는 추상클래스의 속성을 모두 물려받아 실용적

<br>

### 🌾 추상 클래스

> - 추상 매소드가 포함된 class는 무조건 추상 class로 작성해야함
> - 자체적으로 객체 생성 불가능 반드시 상속 통하여 객체 생성
> - 일반적인 메소드, 멤버변수도 포함할 수 있다.

[작성 방식]

```java
//abstract를 넣어줘야 함
public abstract class AbstractClass{
    ...
}
```

<br>

### 추상 매소드

> - 추상 클래스를 상속받는 실체 클래스에서 매소드의 리턴값을 다르게 하고 싶을 경우, 추상 클래스 내의 매소드는 선언만 해주고 실체 클래스에서 구체화 시킨다.
> - 추상 클래스는 선언만 해놓고 실행 내용인 {}은 작성하지 않는다.
> - 실체 클래스는 추상 클래스로부터 받은 매소드를 override해서 사용한다.

[ 작성 방식 ]

```java

public abstract (return type) methodName(params);
 // or
protected abstract (return type) mothodName2(params);
```

<br >

[예시]

```java

//추상 클래스
public abstract class Animal {
	protected static int legs;

    //Constructor 생성자 반드시 포함
	protected Animal(int legs) {
		this.legs = legs;
	}

	public abstract void eat();

	public void walk() {
		System.out.println("the number of legs is  " + legs);
	}

}

```

```java

// 추상 클래스를 상속받은 실제클래스
public class Spider extends Animal  {
	public Spider() {
		super(8);
	}

    //추상 매소드 재정의
	@Override
	public void eat() {
		System.out.println("a spider eats ants");
	}


}
```
