---
title: "Collection 프레임 워크(+ Generic 조금)"
excerpt: "자바에서 Collections 프레임워크 이해하기: Generic을 활용하여"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
## Generic
자바에서 사용하는 제내릭 기법과 collections프레임워크를 사용하는 방법

무엇이든 담을 수 있는 ObjectBox가 있다고 가정해보자. 

```java
public class ObejctBoxMain{
  public static void main(String[] args) {
    ObjectBox box = new ObjectBox();
    box.add("kim");
    String str = (String)box.get();
    System.out.println(str);

    box.add(new Integer(5));
    Integer i = (Integer)box.get();
    System.out.println(i.intValue());
  }
}
```

```java
public class ObjectBox{
  private Object object;
  public void add(Object obj) {
    this.object = obj;
  }
  public void get() {
    return this.object;
  }
}
```

여기에서 꺼낸 것은 모두 Object이므로 다시 형변환하여 사용해야한다: 번거로움 → Generics 기법

### GenericBox

```java
public class GenericBox<T> { //Generic: T라는 이름의 아직 정해지지 않은 자료형을 사용한다.
  private T t;
  public void add(T obj) {
    this.t = obj;
  }
  public T get(){
    return this.t;
  }
}
```

이렇게 클래스를 작성하게되면 이 클래스를 사용할 때 사용할 타입을 정하여 입력해주면 된다.

```java
public class GenericBoxMain{
  public static void main(String[] args) {
    GenericBox<String> strBox = new GenericBox<>(); // 뒤의 꺽쇄는 앞에서 나온 제니릭과 같은 타입을 쓰겠다는 것
    strBox.add("kim");
    String str = bstrBoxox.get();
    System.out.println(str);

    GenericBox<Ineteger> intBox = new GenericBox<>(); // 앞서 나온 제네릭 박스는 String을 위한 것이 되었으므로 여기에서는 사용할 수 없다. 
    intBox.add(new Integer(5)); //Integer 생성자는 deprecated되었음
    Integer i = intBox.get();
    System.out.println(i.intValue());

    GenericBox<Object> Box = new GenericBox<>(); // 앞서 나온 제네릭 박스는 String을 위한 것이 되었으므로 여기에서는 사용할 수 없다. 
    intBox.add(new Integer(5)); //Integer 생성자는 deprecated되었음
    Integer i = (Integer)intBox.get();
    System.out.println(i.intValue());
  }
}
```

GenericBox
- T 는 제네릭과 관련있다
- 제네릭은 클래스 이름 뒤나, 메서드의 리턴 타입 앞에 붙을 수 있다.
- `<T>`부분은 T 라는 제네릭 타입을 선언하는 것을 의미한다.
- 꼭 T 라고 쓸 필요는 없음

사용시 장점
- 정해진 타입만 사용하도록 강제할 수 있다.
- 타입을 강제함으로써 컴파일할 때 잘못된 타입의 값이 저장되는 것을 막을 수 있다.

## 컬렉션 프레임워크(Collection Framework)
- 앞서 살펴본 제네릭 문법은 일부분: 컬렉션 프레임워크를 이해하기 위함
- Java Collections Framework 라고 불리는 Collections API는 Java 2 부터 추가된 자료구조 클래스 패키지
- ✨ 자료(Data)를 다룰 때 꼭 필요한 클래스의 모음이다. ✨

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216708&authkey=%21AP_6naBLifmlrH0&width=787&height=663" alt="">
  <figcaption>Java의 Collections Framework</figcaption>
</figure>

### Collection 인터페이스(java.util.Collection)
- 객체 그룹, 요소를 나타내는 컬렉션을 나타낸다.
- java.util.Collection 인터페이스는 컬렉션 프레임워크에서 가장 기본적인 인터페이스이다.
- 순서를 기억하지 않고, 중복을 허용하여 자료를 다루는 목적으로 만들어졌다.
- 메서드
  - `add()`
  - `iterator()`
  - `size()`

### Iterator 인터페이스
- 컬렉션의 요소에 순차적으로 접근하는 방법을 제공한다.
- 컬렉션의 구조가 드러나지 않으면서도 각 요소에 대한 연산을 진행할 수 있는 표준 메커니즘을 제공하는 것이 목적이다.
- 컬렉션 인터페이스가 의존한다(`--->`).
- Iterator 패턴을 적용한다.
- 메서드
  - `hasNext()`:
  - `next()`: 
  - `remove()`: 

✨참고✨ Iterator 패턴: 모든 요소의 처리를 목적으로하는 디자인 패턴

### 컬렉션 인터페이스를 상속받는 자료구조

Collection 인터페이스를 상속(`———▻`)받는다.

하위의 클래스들은 대표적으로 이 인터페이스들을 구현(`---▻`)하는 자료구조들

#### List(java.util.List) 인터페이스

- 상속받은 모든 메서드를 가지고 있음
- **순서**를 기억하는 자료구조
- index로 요소에 접근할 수 있음.
- 순서를 알고 있으므로 특정 순서로 저장된 자료를 꺼낼 수 있는 get(int) 메서드를 가지고 있다.

#### Set(java.util.Set) 인터페이스

- 중복을 허용하지 않는 자료구조
- 같은 값이 들어오면 하나의 데이터로만 저장한다.
- Set 인터페이스에 저장되는 객체들은 Object가 가지고 있는 equals()메서드와 hashCode()메서드를 오버라이딩 해야한다.

#### Map(java.util.Map) 인터페이스

- Set 자료 구조에 의존(`---->`)한다.
- Key, Value 를 가지고 있는 자료구조
- keySet()메서드를 가지고 있는데, Set을 리턴한다: Set을 통해 유일한 Key를 보관하는 것
- Key에 접근하기 위해서는 Set의 부모인 Collection이 가지고 있는 iterator를 통해 순차로 접근하게 된다.

## 예제 코드

### List 예제

```java
public class ListExample{
  public static void main(String[] args) {
    Collection<String> collections = new ArrayList<>();
    collectsions.add("sth1");
    collectsions.add("sth2");
    collectsions.add("sth3");

    Iterator<String> iter = collections.iterator();
    while(iter.hasNext()) {
      String str = iter.next();
      System.out.println(str);
    }
  }
}
```

- 우선, 참조타입이 Collection이므로, List의 get을 사용할 수 없다.
- 따라서 iterator()메서드를 사용하여 순차적으로 자료에 접근하였고
- List다보니 입력된 데이터의 순서를 기억하고 있으므로 출력되는 결과물은 sth1, sth2, sth3 순서대로 출력된다.
- HashSet을 사용한다면 어떤 순서로 나올지 알 수 없음.
- ✨✨✨Collection을 구현하고 있는 클래스는 모두 ArrayList자리에 올 수 있다.

**Collection을 사용할 때에는 인스턴스가 무엇이든, 사용하고자 하는 목적의 인터페이스로 참조하여 사용하도록 해야한다.**
- 필요에 따라서 적절하게 구현체를 바꾸어 적용할 수 있다.
- **참조 자료형은 인터페이스를 사용하고, 인스턴스 생성은 구현체를 사용하는 것을 연습하도록 한다.**
- 한 가지 더, 인터페이스를 구상하고 프로그램을 구현함으로써 자연스럽게 설계를 접할 수 있을 것: 항상 인터페이스를 만들고 구현하는 연습을 해야한다: 객체지향 프로그래밍은 인터페이스 간의 관계를 도출하는 것이 중요하다.


### Set 예제1

```java
public class setExample1{
  public static void main(String[] args) {
    Set<String> set = new HashSet<>();

    boolean flag1 = set.add("hello");
    boolean flag2 = set.add("hi");
    boolean flag3 = set.add("hong");
    boolean flag4 = set.add("hong");

    System.out.println(flag1);
    System.out.println(flag2);
    System.out.println(flag3);
    System.out.println(flag4); // false 출력됨: 중복된 값은 추가되지 않았다는 걸 나타냄

    Iterator<String> iter = set.iterator();
    while(iter.hasNext()) {
      String str = iter.next();
      System.out.println(str);
    }
  }
}
```

### Set 예제2

```java
public class setExample2{
  public static void main(String[] args) {
    Set<MyData> mySet = new HashSet<>();

    mySet.add(new MyData("kim", 200));
    mySet.add(new MyData("lee", 500));
    mySet.add(new MyData("park", 700));
    mySet.add(new MyData("park", 700));

    Iterator<MyData> iter = mySet.iterator();
    while(iter.hasNext()) {
      MyData myData = iter.next();
      System.out.println(myData);
    }
  }
}

class Mydata{
  private String name;
  private int value;

  public MyData(String name, int value) {
    this.name = name;
    this.value = value;
  }

  /* Getter 생략: 불변 객체 */
}
```

- 중복 저장되지 않는다고 했는데 이렇게 작성하면 두 번 출력된다: 왜?
  - Set 자료형이 데이터를 저장할 때에는 hashCode를 통해 해시값을 생성 후 이를 equals()를 통해 계속해서 비교한다.
  - MyData에 아무런 hashCode(), equals() 오버라이드가 없으므로 Object의 것을 사용한다.
  - 아무런 의미가 없으므로 결과적으로 아무런 작업도 수행하지 않는다.
  - 이렇게 특정 자료형을 클래스로 만들어 사용할 때에는 equals()와 hashCode()메서드를 반드시 목적에 맞게 override해 주어야 한다.
- IntelliJ에서 자동으로 어떤 필드값을 사용할지, 어떤 알고리즘으로 해시값을 생성할지 결정하여 자동으로 코드를 작성해주는 기능을 가지고 있다.

### Map 예제

```java
public class mapExample1{
  public static void main(String[] args) {
    Map<String, String> map = new HashMap<>(); // key, value의 데이터타입임

    map.push("k1", "hello");
    map.push("k2", "hi");
    map.push("k3", "안녕");
    map.push("k3", "안녕하세요,");

    System.out.println(map.get("k1"));
    System.out.println(map.get("k2"));
    System.out.println(map.get("k3"));

  }
}
```

- Map에 저장할 때에 키가 중복되어 push되면 value값이 덮어씌워진다.

```java
public class mapExample2{
  public static void main(String[] args) {
    Map<String, String> map = new HashMap<>(); // key, value의 데이터타입임

    map.push("k1", "hello");
    map.push("k2", "hi");
    map.push("k3", "안녕");
    map.push("k3", "안녕하세요,");

    Set<String> keySet = map.keySet();
    Iterator<String> iterator = keySet.iterator();
    while(iterator.hasNext()) {
      String key = iterator.next();
      String value = map.get(key);

      System.out.println(key + " : " + value);
    }
  }
}
```

- keySet은 Set자료구조를 가지고 있고
- 이를 모두 접근하기 위해서는 Iterator를 통해 해당 set에 접근해야 한다.
- 그리고 얻은 key를 통해 value을 꺼낸다.

### Sort 에제

```java
public class sortExample{
  public static void main(String[] args) {
    List<String> list = new ArrayList<>();
    list.add("sth1");
    list.add("sth2");
    list.add("sth3");

    Collections.sort(list);

    for(int i = 0; i < list.size(); i++) {
      System.out.println(list.get(i));
    }
  }
}
```

- 정렬이 되기 위해서는 비교할 클래스가 Comparable을 구현하고 있어야 한다. → compareTo메서드를 구현해 주어야함
- sort()말고도 주요한 메서드는 shuffle();