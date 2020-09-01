# 👯‍♂️ Polymorphism 다형성

> 다양한 객체를 동일한 사용방법으로 접근할 수 있는 것
> 동일한 방식으로 다양한 실행 결과를 얻어낼 수 있다.

<br />

### 왜 다형성을 사용할까?

> 1. 부모클래스와 이를 상속받는 자식클래스는 필드와 메소드가 같으므로 사용방식이 동일
> 2. 또한 자식클래스에서 Override를 통해서 더 우수한 결과를 나타낼수도 있음
>    ==> **따라서**, 상위의 부모타입에서 다양한 특징을 가진 자식 객체들을 생성하면서 더 우수한 결과를 얻어낼 수 있기 때문

<br />
#### 자동 타입 변환

> 자식 타입이 부모타입을 그대로 상속받아 타입 변환이 일어나는 것

- 부모 클래스

```java
class Animal
```

- 자식 클래스

```java
class Cat extends Animal
```

- 부모 클래스 :

  상위 클래스에서 하위 클래스의 Cat 객체를 생성하고 Animal에 대입하면 자동 타입 변환이 일어남

```java
class Animal{
    public static void main(String[] args){

        Cat cat = new Cat();
        Animal animal = cat;
        // or Animal animal = new Cat();

        cat == animal // true
    }
}
```

<br />

#### 강제 타입 변환

> - 부모타입을 자식 타입으로 변환하는 것
> - 자식 -> 부모로 자동 타입 변환 시, 부모에 선언된 필드와 메소드만 사용 가능
> - 만약 자식에 선언된 필드와 메소드를 사용헤야한다면 자식타입으로 **강제 변환**해서 사용

>

- 강제 타입변환 예시

```java
자식타입 변수 = (자식타입)부모타입;
Parent parent = new Child(); // 자동 타입 변환
Child child = (Child)parent; // 강제 타입 변환

```

#### 🦊 객체 타입 확인 instanceof

> - 강제 타입 변환은 자식 -> 부모로 타입 변환되어있는 상태에서만 사용 가능
> - 처음부터 부모타입으로 생성된 객체는 자식타입으로 변환할 수 없음

```java
Parent parent = new Parent();
Child child = (Child)parent; // 강제 타입변환 불가
```

<br/>
따라서, 부모변수가 참조하는 객체가 부모 / 자식인지 확인 할 수 있도록

**instanceof** 를 사용하면 어떤 객체가 어떤 클래스의 인스턴스인지 확인이 가능하다.

```java
boolean result = Object1 instanceof Object2
```

<br>

**예시)**
Magazine, Novel, ReferenceBook을 자식클래스로 갖는 Publicaion을 생성한 후,
각 객체의 타입에 따라 가격을 다르게 지정해줌

```java
public void modifyPrice(Publication p) {

	if(p instanceof Magazine) {
		p.setPrice((int)(p.getPrice() * 0.6));
	}else if(p instanceof Novel) {
		p.setPrice((int)(p.getPrice() * 0.8));
	}else if(p instanceof ReferenceBook) {
		p.setPrice((int)(p.getPrice() * 0.9));
	}


}
```
