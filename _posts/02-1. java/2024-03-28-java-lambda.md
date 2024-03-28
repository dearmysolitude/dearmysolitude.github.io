---
title: Score Card 예제
excerpt: 간단한 문제로부터 - Comparable/Comparator & 람다식

toc: true
toc_sticky: true

categories:
    - Java
tags:
    - 람다식
    - 정렬
    - Sorting
    - Comparable
    - Comparator
---
## 문제
Score Card는 배열로 입력받은 이름과 점수에 대하여, score card를 이름, 점수에 따라 오름차순/내림차순으로 sorting하는 간단한 문제이다. 

## 해결
객체를 생성하여 각 객체를 자료 구조에 넣고 속성 값을 비교하여 먼저 와야할 객체를 ArrayList에 삽입하는 식으로 정렬하여 저장하였다.

- ArrayList에 있는 요소를 순차적으로 돌다가며 sorting된 배열에 넣으려 하나씩 크기를 비교한다.
- 모든 ArrayList의 요소에 접근해야 하므로 O(n)의 시간 복잡도를 가진다. 요소가 많지 않으므로 이런 방식으로 푸는 것이 가능하다.

```java
// ScoreCard 클래스
// scoreCard의 것이 더 앞선다면 양수를 반환
public int compareWithName(ScoreCard scoreCard) { 
	return name.compareTo(scoreCard.getName());
}

// scoreCard의 점수보다 낮다면 true 반환
public boolean compareWithScore(ScoreCard scoreCard) {
	return score < scoreCard.getScore();
}
```
이전에 보았던 compareTo 메서드가 기억에 남았던 모양이다. 이번 기회에 Comparable, Comparator에 대해 확실히 이해하자(더해서 람다식과 sorting 알고리즘도).

## 풀이 개선
1. 자료 구조에 객체를 배치할 때, 더 나은 성능을 가지는 sorting 알고리즘을 사용하는 것이 좋다.
2. 클래스 내부에 비교하는 메서드를 넣을 수도 있지만, 클래스가 Comparable 인터페이스를 implement하도록 하여 객체가 Java Collections Framework가 제공하는 Arrays의 sorting 메서드를 사용 가능하도록 할 수 있다.
3. 다른 풀이: 객체를 만드는 게 아니라 문자열 배열 자체를 비교 - Java Collections Framework에서 제공하는 Arrays.sort() 메서드 중 Comparator를 인자로 받는 메서드에 람다식으로 어떻게 비교할지 전달해줄 수 있다.

## Arrays

Arrays는 다양한 배열들을 다루는 메서드들을 포함하고 있다. 또한 배열들이 리스트처럼 보이도록 지원하는 역할을 한다. 2차원 배열도 지원하므로 배열 간 비교도 오버로드된 compare() 메서드로 가능하다(말인 즉슨 뒤에서 Comparator와 예제로 볼 수 있듯 sorting이 가능하다.)

## Comparator/Comparable

Comparable과 Comparator에 대한 내용은 Collections 인터페이스를 공부할 때 잠깐 살펴본 적이 있다.

### Comparable

어떤 클래스의 객체가 비교 가능하도록 구현하고자 할 때, 이 인터페이스를 구현하도록 한다. compareTo 메서드만을 가지고 있으며, compareTo를 구현하면 Arrays.sort()메서드는 이 CompareTo 메서드를 사용하여 sorting이 가능해진다(equals()메서드는 Object가 가지고 있으므로 sorting이 필요 없다면 equals()메서드를 override하는것만으로 충분하다. 또한, Arrays.sort()는 기본적으로 Merge Sort를 한다). Java Collections Framework가 제대로 이 클래스 객체를 활용하기 위해서는 compareTo메서드는 int를 리턴하도록, 음수/0/양수를 리턴하도록 제대로 정의해야한다(구현을 위해서는 Java 공식 문서를 참고!).

### Comparator

Comparator는 함수적 인터페이스로, 람다식이나 메서드 참조에 사용될 수 있다. Comparable이 클래스에 비교 기준을 정해주는 속성을 부여하는 것이라면, Comparator는 함수로서 어떻게 행동할지를 전달 가능하게하는 인터페이스이다. Comparator 인터페이스는 내부에 compare 메서드들로 가득 차 있으며, Comparator 인터페이스의 역할은 compare 메서드를 함수로 정의해 전달하는 것이다.

```java
import java.util.Comparator;

public class StringLengthComparator implements Comparator<String> {

    @Override
    public int compare(String s1, String s2) {
        return s1.length() - s2.length();
    }

    public static void main(String[] args) {
        String[] strings = {"apple", "banana", "kiwi", "orange"};
        
        // 문자열 길이 오름차순 정렬
        Arrays.sort(strings, new StringLengthComparator());
        System.out.println(Arrays.toString(strings)); // [kiwi, apple, banana, orange]
        
        // 문자열 길이 내림차순 정렬
        Arrays.sort(strings, new StringLengthComparator().reversed());
        System.out.println(Arrays.toString(strings)); // [orange, banana, apple, kiwi]
    }
}
```

Comparator 인터페이스를 구현한 클래스.

```java
import java.util.Arrays;
import java.util.Comparator;

public class StringLengthComparator {
    public static void main(String[] args) {
        String[] strings = {"apple", "banana", "kiwi", "orange"};

        // 문자열 길이 오름차순 정렬
        Arrays.sort(strings, (s1, s2) -> s1.length() - s2.length());
        System.out.println(Arrays.toString(strings)); // [kiwi, apple, banana, orange]

        // 문자열 길이 내림차순 정렬
        Arrays.sort(strings, (s1, s2) -> s2.length() - s1.length());
        System.out.println(Arrays.toString(strings)); // [orange, banana, apple, kiwi]
    }
}
```

이렇게 람다식으로도 전달할 수 있다.

## 추가: Comparator 매개 변수에 람다식 전달하기

람다식이란, 익명 함수이고 인라인으로 쓰여지는 함수이며, 객체지향 언어인 자바에서 함수형 프로그래밍을 할 수 있도록 JDK 8부터 API를 제공하는 기능이다. 자세한 람다식에 대해서는 함수형 프로그래밍을 공부하며 Java Stream API와 함께 살펴볼 것이다.

위의 예제 코드에서 본 것과 같이, Arrays의 sort 메서드는 comparator를 인자로 받는 것이 오버로드 되어있다. 따라서 Arrays.sort()메서드에 첫 인자는 대상 배열을, 두 번째 인자에는 인라인으로 compare() 메서드를 정의하면 된다.

### 예제: 여러 속성을 가지는 배열을 비교하기

배열을 여러 차원에서 비교하기 위한 예제 코드. 람다식을 사용하였다.

```java
public static void main(String[] args) {
	String[][] arr = { ... }; // 이름, 점수 형태의 String 배열들
	
	Arrays.sort(arr, (a, b) -> Arrays.compare(a, b));
	for(String[] row : arr) { // 배열을 출력
		System.out.println(Arrays.toString(row));
	}
	
	Arrays.sort(arr, Comparator.comparingInt(a -> Integer.parseInt(a[1])));
	for(String[] row : arr) { // 배열을 출력
		System.out.println(Arrays.toString(row));
	}
}
```

#### 첫 번째 코드 블록

Arrays는 배열의 sorting을 돕는 API이므로, 람다식으로 전달된 `(a, b) -> Arrays.compare(a, b)`는 배열 내부의 원소를 하나씩 비교하는 것이라는 걸 추측할 수 있다. (Arrays의 sort메서드를 살펴보면 알 수 있겠지만,) 이 람다식은 Comparator 인터페이스를 구현한 것으로 간주된다. [Arrays의 문서를 살펴보면 String[]을 인자로 받는 compare 메서드는 없다. T[]를 받는 메서드가 호출된 것.](https://docs.oracle.com/javase%2F9%2Fdocs%2Fapi%2F%2F/java/util/Arrays.html#compare-T:A-T:A-) 설명에 따르면, Compares two Object arrays, within comparable elements, lexicographically. 라고 하고 서술하고 있다.

또한, If the two arrays share a common prefix then the lexicographic comparison is the result of comparing two elements of type T at an index i within the respective arrays that is the prefix length, as if by:

     Comparator.nullsFirst(Comparator.<T>naturalOrder()).compare(a[i], b[i])

라고 하므로, 배열의 공통 요소(prefix)를 제한 후 순서대로 비교하는 것을 알 수 있다.

이렇게 비교하는 방법을 람다식으로 전달하였으니, Arrays.sort()는 비로소 arr의 정렬을 수행할 수 있는 것이다.

#### 두 번째 코드 블록

여기에서는 Comparator에서 제공하는 comparingInt를 사용하였다. Comparator는 객체의 특정 필드나 메서드 리턴 값을 기준으로 정렬하는 Comparator를 쉽게 생성할 수 있도록 도와준다.

이 메서드에 대한 설명은 지금 이해 하기에는 함수형 프로그래밍에 대한 공부가 부족한 것 같으니 나중에 더 살펴보자.

`comparingInt()`

- Comparator.comparingInt(ToIntFunction)\<T\> keyExtractor)
- 객체에서 int 값을 추출하는 함수를 전달받아, 그 값을 기준으로 객체를 정렬하는 Comparator를 생성한다.

`comparingDouble()`

- Comparator.comparingDouble(ToDoubleFunction)\<T\> keyExtractor)
- 객체에서 double 값을 추출하는 함수를 전달받아, 그 값을 기준으로 객체를 정렬하는 Comparator를 생성합니다.

이들은 모두 정적 메서드이며, 인자로 객체에서 값을 추출하는 함수형 인터페이스(ToIntFunction, ToDoubleFunction)를 받는다. 이 함수들은 주로 메서드 참조를 사용해 전달한다.

## 참고자료
- [Java docs: Arrays](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html)
- [Java docs: sort(T, Comparator)](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Arrays.html#sort(T%5B%5D,java.util.Comparator))
- [Java docs: Comparable](https://docs.oracle.com/javase/8/docs/api/java/lang/Comparable.html)
- [Java docs: Comparator](https://docs.oracle.com/javase/8/docs/api/java/util/Comparator.html)
