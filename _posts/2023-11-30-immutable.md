---
title: "Immutable, 자바 생성자와 super()"
excerpt: "Immutable 자료형을 만드는 법과 자바의 생성자, super()메서드"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Immutable
---
## Java 생성자

기본 생성자는 생성자 오버로딩이 하나라도 되어있으면 자동으로 생성되지 않는다.

기본 생성자나 아무것도 초기화하지 않는 생성자의 경우 super() 생성자를 자동으로 포함하여 컴파일한다. 사용자가 super()생성자를 호출하는 코드를 작성하지 않으면 자동으로 부모의 기본 생성자가 호출된다. 만약 부모 클래스에 기본 생성자가 없을 경우에는 필요한 인자를 super()의 인자를 통해 전달하는 코드를 작성해야만 한다. 부모 클래스의 생성자로 초기화 후 자식 클래스 생성자가 초기화를 진행해야 한다는 것.

자신의 생성자를 호출할 때는 this()를 사용한다. this() 생성자는 생성자 내에서만 사용할 수 있다. 생성자 안에서 super()생성자를 호출하는 코드 다음이나, 첫번째 줄에 위치해야 한다.
 
## super 키워드

부모 클래스의 메서드나 필드를 참조하기 위해, 혹은 부모 클래스의 생성자를 호출하기 위해 사용된다.

## 불변 객체 Immutable
생성 후 그 상태를 바꿀 수 없는 객체. 힙 영역에서 그 객체가 가리키고 있는 데이터 자체의 변화가 불가능하다.

예: String 클래스

## 원시 타입의 불변 객체화
primitive 타입은 참조 객체를 포함하지 않기 때문에 값을 외부로 내보내는 경우에도 내부 객체의 값은 불변이다. 따라서 Setter를 사용하지 않는 것만으로도 primitive 타입은 불변 객체로 만들 수 있다.

## 참조 타입의 불변 객체화
참조 타입의 객체를 불변 객체로 만들기 위해서는 원시 타입보다는 조금 더 복잡한 구조를 가져야 한다. 초기화할 때만 값을 입력해주고 벡터 메서드만 가지고 있어야 한다.

### Step 1. 생성자를 통해 자료를 전달받았을 때 새로운 객체를 생성하여 새로운 값을 참조하도록 복사하기.
만약에 array의 객체를 불변객체로 만들고 싶다면, this.cars = new ArrayList<>(cars);와 같이 생성자를 설정해주는 것이다.

```java
public class Cars {
    private final List<Car> cars;

    public Cars(List<Car> cars) {
	    this.cars = new ArrayList<>(cars);
    }

    public List<Car> getCars() {
    	return cars;
    }
}
```

이렇게 되면 외부에서 넘겨주는 List와 내부에서 사용하는 인스턴스 변수가 참조하는 값이 달라 외부에서 제어가 불가능하다.

하지만 Getter를 통해 객체를 가져오는 경우 역시 원본 데이터를 수정할 수 있다.

```java
public static void main(String[] args) {
    List<Car> carNames = new ArrayList<>();
    carNames.add(new Car("hodol"));
    Cars cars = new Cars(carNames);

    for(Car car : cars.getCars()) {
        System.out.println(car.getName());
    }

    cars.getCars().add(new Car("pobi"));

    for(Car car : cars.getCars()) {
        System.out.println(car.getName());
    }
}
```

### 2. Collections가 제공하는 api를 활용하여 Getter를 통해 객체를 불러와 수정하는 것을 막을 수 있다.

```java
public class Cars {
    private final List<Car> cars;

    public Cars(List<Car> cars) {
    	this.cars = new ArrayList<>(cars);
    }

    public List<Car> getCars() {
        return Collections.unmodifiableList(cars);
    }
}
```

다만, 클래스 내부에서 객체를 조작하지 않고 Getter를 통해 새로운 객체로 복사하는 경우에는 이런 조작이 먹히지 않는다는 것을 주의

## 추가 정보
final 키워드는 참조 타입에서 변수의 재할당만을 금지하는 키워드이다.
객체가 다른 객체를 참조하는 경우 참조하고 있는 객체에 대해서도 불변이 성립해야 이 객체도 불변이 성립한다.

불변 객체를 사용하는 경우: 외부에서 값을 임의로 제어할 수 없기 때문에
  1, 객체의 자율성이 보장되고
  2. 프로그램 내에서 변화되지 않고 고정된 부분이 많아져 프로그램의 안정성이 강화된다.
 
## 참고 자료
[불변객체 만들기, 테코블](https://tecoble.techcourse.co.kr/post/2020-05-18-immutable-object/)

 