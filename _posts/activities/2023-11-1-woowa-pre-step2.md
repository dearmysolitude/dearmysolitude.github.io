---
title: "2023 우테코 프리코스 후기 - 2"
excerpt: "우테코 준비과정의 두 번째 단계에서 코드를 작성하며 정리한 내용들입니다."

categories:
  - 우테코 프리코스
tags:
  - 에세이
---
[https://github.com/dearmysolitude/java-racingcar-6](https://github.com/dearmysolitude/java-racingcar-6)

   
여러 차의 이름을 입력받고, 모든 차에 대해 각각 주사위를 굴려 4 이상이 나오면 전진하도록 하여 입력된 횟수대로 n번 수행하여 우승자를 출력하는 어플리케이션. 과제는 간단하지만, 추가적으로 **테스트 코드를 작성**하고, **메서드를 간략화하여 indentation을 2 이하로** 하는 요구사항이 추가되었다. 구현하고 보니 테스트 코드를 작성하면서 구현하는게 나았을지도 모르겠다는 생각을 했다(하지만 테스트 코드를 작성해 보는게 처음이다보니 먼저 손 대기가 쉽지 않았다😂).  
\*\* 구현 기능마다 커밋하라는 지시사항을 읽지 못했다😱 \*\*  
테스트 코드부터는 커밋을 나눠서 진행했다( 난 글렀어 ㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎㅎ ) 테스트 코드를 짜기 위해 우테코에서 제공한 외부 라이브러리를 뜯어보았다: 메서드들을 살펴보는데 많은 시간이 걸리지는 않았다.  
 

![library](https://1drv.ms/i/s!Ano-rmQ7e_nEwHwTUV-hlD2nsEUA?embed=1&width=609&height=333)

Console 클래스는 Java 에서 제공하는 Scanner 클래스의 다양한 메서드들 중 필요한 메서드만 사용하도록 유도하고 signleton으로 사용하여 객체를 무분별하게 생성하지 않고 운영체제에서 관리하는 입력 스트림에 대한 메모리를 제한하여 memory leakage가 나지 않도록 최적화한 wrapper 클래스이다.  
Randoms도, 뜯어보지는 않았지만 임의의 숫자를 생성하도록 하는 메서드의 wrapper 클래스일 것이다. 이는 test 코드를 구현하기 위해 특정 메서드를 사용하는 것으로 제한하고 적절한 가공을 하기 위한 것 같다.  
테스트 코드를 위한 라이브러리 클래스는 아래에서 더 살펴보자. 테스트 코드(단위 테스트)에 대한 학습을 위해 구현 어플리케이션의 구성은 간단했고, 덕분에 테스트 코드에 대해 조금 알게 되었다.  
 

## 구현

### input.split(",") 으로 입력된 내용을 분리하였으나 ,,, 와 같은 입력은 예외처리 하지 못함

```java
public Car(String name) {
    if (!isValid(name)) {
        throw new IllegalArgumentException("Car name invalid: " + name);
    }
    this.name = name;
    this.score = 0;
}

private boolean isValid(String input) {
    return input.length() <= 5 && !input.isEmpty();
}
```

```java
public List<Car> makeCars () {
    List<Car> cars = new ArrayList<>();
    
    try {
        String input = Console.readLine();
        String[] temps = input.split(",", -1); // 정규 표현식을 사용하여 문자 분리
        for (String temp : temps) {
            Car carTemp = new Car(temp);
            cars.add(carTemp);
        }
    } catch (NullPointerException e) {
        throw new IllegalArgumentException("Invalid input: 제대로 된 이름이 입력되지 않음", e);
    }
    
    return cars;
}
```

split(",")메서드가 ,,,가 입력된 경우 빈 문자열을 반환하지 않는다. 메서드는 길이가 0인 배열을 반환하여 for 루프가 실행되지 않는다. 따라서 Car 객체는 생성되지 않고, Car 생성자로 Car가 만들어 질 때 실행되도록 한 예외처리도 되지 않는 것이다.

split(",", -1)을 사용하여 빈 문자열도 포함하도록 할 수 있다. 이렇게 하면 입력이 ",,"인 경우에 \["", "", ""\]와 같이 배열이 만들어져서 for 루프를 통해 생성자에 접근하게 되고, 생성자에서 예외 처리가 가능해진다.

### 한 글자를 반복하여 여러번 출력하기

Java 8 이상에서는 String.join() 메서드를 사용하여 리스트의 모든 요소를 특정 구분자로 연결할 수 있다. 이 메서드는 첫 번째 인자로 구분자를, 두 번째 인자로 연결할 요소를 가진 컬렉션을 받는다.

```java
List<String> list = Arrays.asList("Apple", "Banana", "Cherry");
String result = String.join(", ", list);
System.out.println(result);  // 출력: Apple, Banana, Cherry
```

이 메서드는 리스트의 요소가 문자열이 아닌 경우에도 toString() 메서드를 통해 문자열로 변환한다.

### 람다 표현식에서 외부 변수의 참조: 외부 변수는 변경되면 안된다

Java의 람다 표현식에서는 외부 변수를 참조할 때 그 변수가 사실상 최종(final)이거나 실제로 최종인 경우에만 참조가 허용된다. 이는 람다가 실행될 때 외부 변수가 변경되어서는 안되기 때문. 따라서 maxScore를 람다 표현식 내에서 변경하려고 하면 컴파일 오류가 발생하는 것이다.

maxScore를 배열의 요소로 만드는 것으로 처리할 수 있다. 배열의 참조는 변경되지 않으므로 람다 표현식에서 사용할 수 있는 것이다.

```java
final int[] maxScore = {0};
cars.forEach(car -> {
    int score = car.getScore();
    
    if(score > maxScore[0]) {
        maxScore[0] = score;
        winner.clear();
        winner.add(car);
    } else if (score == maxScore[0]) {
        winner.add(car);
    }
});
```

### 추가: Application 클래스에 입력 관련 메서드를 추가하려다 Bing에 물어보았다

Application 클래스에도 메서드를 추가할 수 있나?

1.  **프로그램 초기화**: 프로그램이 시작될 때 필요한 초기화 작업을 수행하는 메서드를 추가할 수 있습니다. 예를 들어, 데이터베이스 연결을 설정하거나, 필요한 리소스를 로드하거나, 애플리케이션 설정을 구성하는 등의 작업을 수행할 수 있습니다.
2.  **유틸리티 메서드**: Application 클래스에는 프로그램 전반에서 사용되는 유틸리티 메서드를 추가할 수 있습니다. 이러한 메서드는 로깅, 에러 처리, 입력 검증 등과 같은 공통 작업을 수행합니다.
3.  **프로그램 종료 처리**: 프로그램이 종료될 때 수행해야 하는 작업을 처리하는 메서드를 추가할 수 있습니다. 예를 들어, 열려 있는 데이터베이스 연결을 닫거나, 사용 중인 리소스를 해제하거나, 임시 파일을 삭제하는 등의 작업을 수행할 수 있습니다.

하지만, Application 클래스가 너무 많은 책임을 가지게 되면 유지 보수가 어려워질 수 있으므로, 적절한 클래스 설계와 메서드 분리가 중요합니다. 이는 SOLID 원칙과 같은 객체 지향 설계 원칙을 따르는 것이 좋습니다.

Bing AI에 의해 생성됨

## 테스트 코드

외부 라이브러리로 제공되는 다음 코드들을 살펴보자.

#### ApplicationTest.java

NsTest를 구체화한 클래스로 구성하였는데, 해당 추상 클래스의 runMain() 메서드를 override하여 실행할 어플리케이션을 지정하였다(실행하고자 하는 application.main 메서드를 실행하여 테스트하도록 하였다).

```java
class ApplicationTest extends NsTest {
    private static final int MOVING_FORWARD = 4;
    private static final int STOP = 3;

    @Test
    void 전진_정지() {
        assertRandomNumberInRangeTest(
            () -> {
                run("pobi,woni", "1");
                assertThat(output()).contains("pobi : -", "woni : ", "최종 우승자 : pobi");
            },
            MOVING_FORWARD, STOP
        );
    }

    @Test
    void 이름에_대한_예외_처리() {
        assertSimpleTest(() ->
                assertThatThrownBy(() -> runException("pobi,javaji", "1"))
                        .isInstanceOf(IllegalArgumentException.class)
        );
    }

    @Override
    public void runMain() {
        Application.main(new String[]{});
    }
}
```

위 메서드들은 아래의 Assertion 클래스를 보면 잘 이해할 수 있을 것 같다.

#### Assertions.java

```java
public class Assertions {
    private static final Duration SIMPLE_TEST_TIMEOUT = Duration.ofSeconds(1L);
    private static final Duration RANDOM_TEST_TIMEOUT = Duration.ofSeconds(10L);

    private Assertions() {
    }

    public static void assertSimpleTest(final Executable executable) {
        assertTimeoutPreemptively(SIMPLE_TEST_TIMEOUT, executable);
    }

    public static void assertRandomNumberInListTest(
        final Executable executable,
        final Integer value,
        final Integer... values
    ) {
        assertRandomTest(
            () -> Randoms.pickNumberInList(anyList()),
            executable,
            value,
            values
        );
    }

    public static void assertRandomNumberInRangeTest(
        final Executable executable,
        final Integer value,
        final Integer... values
    ) {
        assertRandomTest(
            () -> Randoms.pickNumberInRange(anyInt(), anyInt()),
            executable,
            value,
            values
        );
    }

    public static void assertRandomUniqueNumbersInRangeTest(
        final Executable executable,
        final List<Integer> value,
        final List<Integer>... values
    ) {
        assertRandomTest(
            () -> Randoms.pickUniqueNumbersInRange(anyInt(), anyInt(), anyInt()),
            executable,
            value,
            values
        );
    }

    public static <T> void assertShuffleTest(
        final Executable executable,
        final List<T> value,
        final List<T>... values
    ) {
        assertRandomTest(
            () -> Randoms.shuffle(anyList()),
            executable,
            value,
            values
        );
    }

    private static <T> void assertRandomTest(
        final Verification verification,
        final Executable executable,
        final T value,
        final T... values
    ) {
        assertTimeoutPreemptively(RANDOM_TEST_TIMEOUT, () -> {
            try (final MockedStatic<Randoms> mock = mockStatic(Randoms.class)) {
                mock.when(verification).thenReturn(value, Arrays.stream(values).toArray());
                executable.execute();
            }
        });
    }
```

다른 메서드들은 모두 AssertionRandomTest을 사용해 Random클래스의 다양한 메서드들을 실행하도록 되어있다.

**A****ssertRandomTest()**

특정 Verification과 Executable을 받아서, 주어진 시간 내에(RANDOM\_TEST\_TIMEOUT) Executable이 실행되는 것을 보장하는 테스트를 수행한다. 제네릭 타입 T를 사용하여 다양한 타입의 값을 처리할 수 있다.  
   
인자들:

-   verification: MockedStatic<Randoms> 객체가 어떤 메서드 호출에 대해 특정 값을 반환하도록 설정
-   executable: 테스트하려는 코드를 포함하는 Executable 객체
-   value: verification에 의해 반환되는 첫 번째 값
-   values: verification에 의해 반환되는 추가 값들

메서드 동작:

1.  먼저, assertTimeoutPreemptively 메서드를 사용하여 주어진 시간 내에 코드가 완료되는지 확인.
2.  그런 다음, MockedStatic<Randoms> 객체를 생성. 이 객체는 Randoms 클래스의 정적 메서드를 가로채고, 그 메서드가 호출될 때마다 특정 값을 반환하도록 설정할 수 있다.
3.  mock.when(verification).thenReturn(value, Arrays.stream(values).toArray()); 코드를 사용하여, verification이 호출될 때마다 value와 values를 순차적으로 반환하도록 설정
4.  마지막으로, executable.execute(); 코드를 실행하여 테스트하려는 코드를 실행

잘 모르겠는 몇 가지 항목들을 다시 찾아보았다: Executable, Verification, MockedStatic  
 

### Executable

라이브러리에서 구현되어 있는 메서드들은 공통적으로 Executable 인수를 전달받았는데, wrapping한 메서드들에서는 여기에 람다식이 들어가 있어서 찾아보게 되었다.  
Executable은 java.util.concurrent 패키지에 있는 함수형 인터페이스로, 특정 코드 블록을 실행하는 역할을 한다. 이 인터페이스는 매개변수 없이 실행되며 반환값도 없는 void execute()라는 단일 추상 메서드를 정의한다. 따라서 Executable로 전달되는 것은 테스트하려는 코드 블록이다다. 이 코드 블록을 람다식으로 표현할 수 있는 것 뿐이다.

```java
Executable executable = () -> {
    // 실행할 코드
};
```

이렇게 작성된 코드는 executable 인자를 통해 메서드로 전달된다.

### MockedStatic

Mockito 라이브러리에서 제공하는 인터페이스로, 타입의 정적 메서드를 mock하는데 사용된다. MockedStatic은 활성화된 정적 모의를 나타내며, 범위가 정의되어 있어 해제되면 원래의 동작으로 돌아간다. 정적 메서드의 모의 동작을 정의하고, 정적 메서드 호출을 검증하기 위해 MockedStatic 참조를 사용한다. [출처](https://howtodoinjava.com/mockito/mock-static-methods/)

참고 자료: [MockedStatic (Mockito 3.4.3 API) (javadoc.io)](https://javadoc.io/static/org.mockito/mockito-core/3.4.3/org/mockito/MockedStatic.html)  
 

### Verification

Mockito에서 제공하는 기능으로, 특정 메서드 호출이 예상대로 이루어졌는지 검증하는 데 사용된다. 만약 해당 메서드가 호출되지 않았다면 테스트는 실패한다.

참고자료: [Verification in Java (JVM) - GeeksforGeeks](https://www.geeksforgeeks.org/verification-java-jvm/)  
 

### assertTimeoutPreemptively

assertTimeoutPreemptively에 대한 설명은 다음과 같다(JUnit에 있는 assertTimeoutPreemptively와 그에 대한 설명).

> Assert that execution of the supplied executable completes before the given timeout is exceeded.  
> Note: the executable will be executed in a different thread than that of the calling code.
> Furthermore, execution of the executable will be preemptively aborted if the timeout is exceeded.
> See the Preemptive Timeouts section of the class-level Javadoc for a discussion of possible undesirable side effects.  
>   
> 관련 주제:  
> assertTimeoutPreemptively(Duration, Executable, String)
> assertTimeoutPreemptively(Duration, Executable, Supplier)
> assertTimeoutPreemptively(Duration, ThrowingSupplier)
> assertTimeoutPreemptively(Duration, ThrowingSupplier, String)
> assertTimeoutPreemptively(Duration, ThrowingSupplier, Supplier)
> assertTimeout(Duration, Executable)

```java
public static void assertTimeoutPreemptively(Duration timeout, Executable executable) {
    AssertTimeout.assertTimeoutPreemptively(timeout, executable);
}
```

   
 

#### NsTest.java

```java
public abstract class NsTest {
    private PrintStream standardOut;
    private OutputStream captor;

    @BeforeEach
    protected final void init() {
        standardOut = System.out;
        captor = new ByteArrayOutputStream();
        System.setOut(new PrintStream(captor));
    }

    @AfterEach
    protected final void printOutput() {
        System.setOut(standardOut);
        System.out.println(output());
    }

    protected final String output() {
        return captor.toString().trim();
    }

    protected final void run(final String... args) {
        try {
            command(args);
            runMain();
        } finally {
            Console.close();
        }
    }

    protected final void runException(final String... args) {
        try {
            run(args);
        } catch (final NoSuchElementException ignore) {
        }
    }

    private void command(final String... args) {
        final byte[] buf = String.join("\n", args).getBytes();
        System.setIn(new ByteArrayInputStream(buf));
    }

    protected abstract void runMain();
}
```

run과 runExecption은 command와 runMain메서드의 예외 처리를 해 주는 wrapper 메서드이다. 앞서 보았듯이 runMain은 ApplicationTest 클래스에서 Application.main으로 오버라이드하여 작동할 어플리케이션을 설정한다.  
command메서드는 args를 가변 인자로 받아서 이를 하나의 문자열로 바꾼 후, 바이트 배열로 변환, 입력 스트림으로 설정한다. 즉, 전달받은 args들을 입력 스트림으로 올려놓아 runMain에 의해 어플리케이션이 실행되었을 때 입력값으로 전달되는 것이다.  
 

#### assertThat과 assertThrows의 차이점이 뭐야?

assertThat과 assertThrows는 둘 다 JUnit과 AssertJ와 같은 단위 테스트 라이브러리에서 제공하는 메서드들입니다. 이 두 메서드는 테스트의 목적에 따라 사용되며, 각각 다음과 같은 차이점이 있습니다:

-   assertThat: 이 메서드는 AssertJ 라이브러리에서 제공하며, 주어진 값이 특정 조건을 만족하는지 확인합니다. 예를 들어, assertThat(actual).isEqualTo(expected)는 actual 값이 expected 값과 같은지 확인합니다. 만약 조건을 만족하지 않으면, 테스트는 실패합니다.
-   assertThrows: 이 메서드는 JUnit 라이브러리에서 제공하며, 주어진 코드 블록이 특정 타입의 예외를 발생시키는지 확인합니다. 예를 들어, assertThrows(IllegalArgumentException.class, () -> { /\* 코드 블록 \*/ })는 코드 블록이 IllegalArgumentException을 발생시키는지 확인합니다. 만약 예외가 발생하지 않거나 다른 타입의 예외가 발생하면, 테스트는 실패합니다.

따라서 assertThat은 주로 값의 상태를 검증하는 데 사용되며, assertThrows는 예외가 발생하는지 검증하는 데 사용됩니다. 이 두 메서드를 적절히 사용하면 다양한 상황에서 코드의 동작을 테스트할 수 있습니다.

Bing AI에 의해 생성됨

### 코멘트

앞으로는 TDD를 시도해 보는 걸로...?  
개별 메서드를 대상으로 테스트코드를 작성하는 것과 기능별, 테스트 케이스를 고려하여 원하는 결과가 어플리케이션을 실행했을 때 제대로 얻어지는지 확인할 수 있었는데, 후자의 경우 외부 라이브러리로 주어진 테스트 코드를 사용할 수 있었으나 전자의 경우 처음 사용해보는 탓에 제대로 작성된 것인지 확신할 수 없었다.

## 피드백(공통 사항)

-   README.md은 상세히 작성한다.
-   기능 목록 재검토
    -   클래스 설계와 구현, 메서드 설계와 구현같이 너무 상세하게 작성하지 않는다.
    -   구현 기능을 중심으로 작성한다.
    -   에외 상황도 기능 목록에 추가하여 정리한다.
-   기능 목록 업데이트: 구현하면서 업데이트를 해나가도록 한다.
-   값은 하드코딩 하지 않는다: final static 사용 - "java 상수"와 같은 검색어를 통해 구현 방법을 학습할 수 있을 것.
-   구현 순서도 컨벤션

```java
class A {
    상수(static final) 또는 클래스 변수

    인스턴스 변수

    생성자

    메서드
}
```

-   변수 이름에 자료형 넣지 말 것.
-   한 함수가 한가지 기능만 담당하게 한다: 기준을 세워볼 것(15줄 이내로 작성)
-   테스트를 작성하는 이유는?
    1.  코드에 대한 빠른 피드백
    2.  [학습: 학습 테스트를 통해 JUnit 학습하기](https://techcourse-storage.s3.ap-northeast-2.amazonaws.com/9b82d8a360c548fcadd14c551dbcbe06)
-   테스트는 처음부터 큰 단위를 만들지 않는다.
    -   테스트의 중요한 목적은 작성 코드에 대한 빠른 피드백이다.
    -   문제를 작게 나누고, 핵심 기능에 가까운 부분부터 작게 테스트를 작성하도록 한다.

## 참고

[Java static 메소드를 mocking 하여 테스트하기](https://unluckyjung.github.io/testcode/2021/12/20/Mockito-StaticMethod-Mocking/)