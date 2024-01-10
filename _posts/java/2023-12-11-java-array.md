---
title: "배열"
excerpt: "자바의 배열, 깊은 복사/얕은 복사에 대해 살펴본다."

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - 배열
---
## 기본형 배열
### 초깃값 없이 선언하기
``기본형 타입[] = 변수명;`` 혹은 ``기본형 타입 = 변수명[];``

```java
public class Array01 {
    public static void main(String[] args) {
        // 선언을 통해 array를 참조한다는 것을 알린다.
        int[] array1;
        int array2[];
        int array3[];

        // 선언하였다면 반드시 초기화 해 주어야 한다.
        array1 = new int[5];
        array2 = new int[5];
        array3 = new int[0]; // 배열 인스턴스는 만들어지지만 아무것도 가질 수 없다.

    }
}
```

### 초깃값을 주어 선언과 동시에 초기화하기
- ``기본형타입[] 변수명 = new 기본형 타입[배열의 크기];``
- ``변수명 [index값] = 값;``
- ``기본형 타입[] 변수명 = new 기본형타입[] {값1, 값2, ...};``
- ``기본형 타입[] 변수명 = {값1, 값2, 값3, ...}``

## 참조형 배열
- 배열의 타입이 기본형이 아닌 경우
- 배열 변수가 참조하는 배열의 공간이 값을 저장하는 것이 아니라 값을 참조한다는 의미이다.
- 참조형 배열이 만들어지게되면 배열은 만들어지지만 그 배열이 참조하는 것은 null이므로 적절하게 초기화해 주어야한다.

## 이차원 배열
- ``타입[][] 변수명 = new 타입[행의 수][열의 수];``
- ``변수명[행 인덱스][열 인덱스] = 값;``
- 배열이 배열을 참조하는 형태

### 자바의 배열은 가변 배열: 이차원 가변 배열
자바의 배열은 가변 배열로, 다음과 같은 이차원 배열의 선언과 초기화가 가능하다.
- ``타입[][] 변수명 = new 타입[행의 수][];``
- ``변수명[행의 인덱스] = new 타입[열의 수];``

```java
int[][] koreanScoreArray = new int[3][];
koreanScoreArray[0] = new int[20];
koreanScoreArray[1] = new int[19];
koreanScoreArray[2] = new int[21];
```

## for each문
배열 안에 있는 요소를 하나씩 꺼내서 조작하는데 사용한다.

```java
public class Array03 {
  public static void main(String[] args) {
    int[] array = {1, 2, 3, 4, 5};

    for(int i : array) {
      System.out.println(i);
    }
  }
}
```

## Arrays
배열을 다룰 때 사용하는 유틸리티 클래스. java.util 패키지에 속해 있음

### copyOf 메서드

인자로 받은 배열의 인자 값들을 각각 복사하여 새로운 배열로 만든다.

```java
import java.util.Array;

public class Array04 {
  public static void main(String[] args) {
    int[] copyFrom = {1, 2, 3};

    int[] copyTo = Arrays.copyOf(copyFrom, copyFrom.length);

    for(int i : copyTo) {
      System.out.println(i);
    }

    System.out.println("================================");

    int[] copyTo2 = Arrays.copyOf(copyFrom, 5); // 5개 배열이 만들어지는데, 거기에 copyFrom의 값이 복사되는 것. 나머지는 0
    for(int i : copyTo2) {
      System.out.println(i);
    }

    int[] copyTo3 = copyFrom;
  }
}
```

- ``copyTo1``과 ``copyTo3``의 차이점은: ``copyTo1``은 인자를 복사한 새로운 인스턴스를 가지지만 ``copyTo3``는 ``copyFrom``과 동일한 객체를 참조한다는 것이다.

👉 깊은 복사와 얕은 복사에 대해서는 별도의 문서에서 살펴보자

### copyOfRange 메서드

인자로 받은 배열의 일정 범위(일부분)를 복사하는 메서드

### compare 메서드
```java
import java.util.Array;

public class Array05 {
  public static void main(String[] args) {
    int[] array1 = {1, 2, 3, 4, 5};
    int[] array2 = {1, 2, 3, 4, 5};
    
    int comapre = Arrays.compare(array1, array2);
    
    System.out.println(compare);
  }
}
```
- 자바에서는 비교하는 메서드가 자주 사용된다.
- 양수, 0, 음수를 기억할 것: 왼쪽이 크면 양수, 오른쪽이 크면 음수, 같으면 0이 반환된다.
- compare 메서드는 배열의 값과 배열의 크기를 비교한다.
참고: String 클래스는 따로 import 하지 않아도 사용할 수 있음.

### sort 메서드와 binarySearch 메서드

- sort메서드: 주어진 배열을 인자의 크기별로 정렬한다. 인자로 받은 배열 자체가 정렬되어 저장된다.
- binarySearch 메서드: 
- 주의: binarySearch는 정렬된 경우 사용할 수 있음

```java
import java.util.Array;

public class Array06 {
  public static void main(String[] args) {
    int[] array1 = {3, 4, 5, 2, 1};
    
    int comapre = Arrays.sort(array1);
    
    int i = Arrays.binarySearch(array1, 4);
    System.out.println(i);
  }
}
```

### Object를 받아서 정렬하는 경우
sort메서드는 여러가지 요소를 인자로 받을 수 있지만, Object[]를 인자로 받을 수 있다: 모든 객체의 조상이므로, 어떤 객체의 배열이든 받을 수 있다.
```java
import java.util.Array;

public class Array07 {
  public static void main(String[] args) {
    Item[] items = new Item[5];
    items[0] = new Item("Java", 3000);
    items[1] = new Item("Python", 4000);
    items[2] = new Item("Rust", 2000);
    items[3] = new Item("C++", 5000);
    items[4] = new Item("Dart", 1000);

    Arrays.sort(items); // → ?

    for(Item item: items) {
      System.out.println(item);
    }
  }

  class Item {
    private String name;
    private int price;

    /* Getter와 Setter, toString 오버라이드*/
  }
}
```

- 이 경우 ClassCastException이 발생함: java.lang.Comparable 인터페이스로 casting할 수 없음 
- Item 클래스가 Comparable을 implement해야 한다: 어떤 Item이 큰지 작은지 결정하는 인터페이스, 즉 어떤 기준으로 비교할 것인지 구현해주어야 한다.

```java
  class Item implements Comparable{
    private String name;
    private int price;

    /* Getter와 Setter, toString 오버라이드*/

    // 파라미터로 들어온 Object와 나 자신을 비교하는 메서드
    // Object를 받아들이도록 했지만 실제로는 Item이 들어온다.
    @Override
    public int compareTo(Object o) {
      Item d = (Item)o;
      return this.name.compareTo(d.name); // String에 들어가보면 Comparable을 implement하는 것을 확인할 수 있다.
    }
  }

```

- 대문자, 소문자, 한글 순으로 출력되는 것을 확인할 수 있다.
- 가격으로 정렬하고 싶다면, 

```java
    @Override
    public int compareTo(Object o) {
      Item d = (Item)o;
      return this.price - d.price;
    }
```

### sort의 두번째 인자: Comparator
Comaprator을 구현하여 객체간 정렬방식을 정의할 수 있다.
```java
import java.util.Array;

public class Array08 {
  public static void main(String[] args) {
    Item[] items = new Item[5];
    items[0] = new Item("Java", 3000);
    items[1] = new Item("Python", 4000);
    items[2] = new Item("Rust", 2000);
    items[3] = new Item("C++", 5000);
    items[4] = new Item("Dart", 1000);

    Arrays.sort(items, 정렬방식); // → Comparator

    for(Item item: items) {
      System.out.println(item);
    }
  }

  class Item {
    private String name;
    private int price;

    /* Getter와 Setter, toString 오버라이드*/

    @Override
    public int compareTo(Object o) {
      Item d = (Item)o;
      return this.name.compareTo(d.name);
    }
  }

  class ItemSorter implements Comparator {
    @Override
    public int compare(Object o1, Object o2) {
      Item item1 = (Item)o1;
      Item item2 = (Item)o2;

      return o1.getName.compareTo(o2.getName());
    }
  }
}
```

- 이렇게 외부에서 정렬할 수도 있음
- 람다 표현식을 사용하여 코드 내부에서 사용할 수도 있음.

```java
public class Array09 {
  public static void main(String[] args) {
    Item[] items = new Item[5];
    items[0] = new Item("Java", 3000);
    items[1] = new Item("Python", 4000);
    items[2] = new Item("Rust", 2000);
    items[3] = new Item("C++", 5000);
    items[4] = new Item("Dart", 1000);

    Arrays.sort(items, (items1, items2) -> o1.getName.compareTo(o2.getName())); // 초간단!

    for(Item item: items) {
      System.out.println(item);
    }
  }

  ...

}
```

람다식이 내부적으로 Item으로 유추한다 + return 생략 가능: 코드가 간단해진다.

## 명령행 아규먼트(Command-Line Arguments)
- main 메서드는 JVM이 실행하는 메서드이다.
- JVM이 main 메서드를 실행할 때 String[] 아큐먼트를 넘겨주는 것을 명령행 아규먼트라고 한다.
- 빈 칸을 기준으로 나누어 구분하게 된다.
- IntelliJ의 경우 실행 옵션에 Command-Line Argument를 설정할 수 있는 것을 확인할 수 있다.
- 커맨드 라인으로 실행할 경우에는 다음과 같다

  - ``javac EmptyCommandLineArgumentExam.java``: 컴파일 명령

  - ``java EmptyCommandLineArgumentExam a b c d hello``: a, b, c, d, hello를 String[]으로 전달.

  - ``java -javaagent:~JVM 실행 설정~ EmptyCommandLineArguemntExam a b c``: JVM 실행설정은 java와 진입점 사이에 작성하여 수행하게 한다.

- 큰 따옴표로 묶은 것은 하나의 String으로 전달된다.  

### 제한 없는 아규먼트(Unlimited Arguments)
- 경우에 따라서 메서드 아규먼트를 가변적으로 전달하고 싶은 경우가 있다.
- 메서드에 정수값을 경우에 따라 3개, 어떤 경우엔 5개를 넘기고 싶다면 어떻게 해야할까?

```java
public static int sum(int... args) { // 가변적인 메서드 아규먼트를 전달할 수 있게 된다: int형을 지니는 배열로 취급된다.
  System.out.println("print1 메서드 - agrs 길이: " + args.length);
  int sum  = 0;
  for(int i = 0; i < args.length; i++) {
    sum += args[i];
  }
  return sum;
}
```