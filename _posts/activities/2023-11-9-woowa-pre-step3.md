---
title: "2023 우테코 프리코스 후기 - 3"
excerpt: "우테코 준비과정의 세 번째 단계에서 코드를 작성하며 정리한 내용들입니다."

categories:
  - 우테코 프리코스
tags:
  - 에세이
---
> 우테코 프리코스 3주차 미션을 진행하면서 학습한 내용을 정리한 문서입니다.

[https://github.com/dearmysolitude/java-lotto-6/tree/dearmysolitude](https://github.com/dearmysolitude/java-lotto-6/tree/dearmysolitude)

## 게임 기능

1. 로또 번호의 숫자 범위는 1~45 까지이다.  
2. 1개의 로또를 발행할 때 중복되지 않는 6개의 숫자를 뽑는다.  
3. 당첨 번호 추첨 시 중복되지 않는 숫자 6개와 보너스 번호 1개를 뽑는다.  
4. 당첨은 1등부터 5등까지 있다. 당첨 기준과 금액은 아래와 같다.  
   - 1등: 6개 번호 일치 / 2,000,000,000원  
   - 2등: 5개 번호 + 보너스 번호 일치 / 30,000,000원  
   - 3등: 5개 번호 일치 / 1,500,000원  
   - 4등: 4개 번호 일치 / 50,000원  
   - 5등: 3개 번호 일치 / 5,000원

게임이 시작하면 1000원 단위로 구매할 로또의 금액을 입력받는다. 로또를 임의 번호대로 뽑고 출력하고 나면, 사용자가 당첨 번호 6자리를 쉼표로 구분하여 입력한다. 보너스 번호 1개까지 입력 받은 후, 작성된 로또 번호들을 당첨 번호, 보너스 번호와 비교하여 당첨 결과를 만들고 수익률을 계산하여 출력함으로써 프로그램을 종료시킨다.

## 프로그램 작성시 추가 요청 사항

-   함수(또는 메서드)의 길이가 15라인을 넘어가지 않도록 구현한다.
-   **함수(또는 메서드)가 한 가지 일만 잘 하도록** 구현한다.
-   else 예약어를 쓰지 않는다.
    -   힌트: if 조건절에서 값을 return하는 방식으로 구현하면 else를 사용하지 않아도 된다.
    -   else를 쓰지 말라고 하니 switch/case로 구현하는 경우가 있는데 switch/case도 허용하지 않는다.
-   Java Enum을 적용한다.
-   도메인 로직에 단위 테스트를 구현해야 한다. 단, UI(System.out, System.in, Scanner) 로직은 제외한다.
    -   핵심 로직을 구현하는 코드와 UI를 담당하는 로직을 분리해 구현한다.
    -   단위 테스트 작성이 익숙하지 않다면 test/java/lotto/LottoTest를 참고하여 학습한 후 테스트를 구현한다.

## 코멘트

### 어플리케이션 설계

-   이번에는 구현할 기능부터 **목록으로 작성하고 그에 따라 구현을 진행**하였다.
-   UI 기능과 주요 로직 기능을 분리하여 클래스로 구현하고자 하였다.
-   처음에는 입력 / 출력 / Lotto 클래스 / 결과 산출 으로 기능을 나누어 구현하려 계획을 세웠다.
-   구현하면서 자연스럽게 **GameManager 같이 모든 객체를 관리하면서 주요 로직을 실행하는 클래스**를 추가하게 되었다.
-   Enum 자료형에 대한 이해가 부족해 처음에는 계획을 세우지 않았으나, 등수와 상금 등 결과를 위한 데이터를 저장하는 용도로 사용하고자 Result 구현 중에 추가하였다.
-   InputHandler와 printHandler의 경우 처음에는 싱글톤으로 만들었으나, 객체가 상태를 저장하지 않는다는 점에서 퍼포먼스를 조금이라도 늘리기 위해 범용적으로 사용할 수 있는 입력 메서드들은 아니지만 static으로 변경하였다.
-   반복되는 상수들을 Constants 클래스로 분리하여 static으로 관리하도록 변경하였다.

#### GameManager 클래스

GameManager의 필드는 다음과 같다(로또 게임 진행을 위한 모든 요소를 필드로 가지고 있다).

-   int moneyYouPut
-   List<Integer> winningNumbers
-   Integer bonusNumber
-   List<Lotto>
    -   Lotto 클래스
-   Result 클래스  
    -   List<Score>

#### Lotto 클래스

-   Lotto 번호 객체
-   게임 당 가능한 로또 게임 갯수대로 임의로 생성된다

#### Result 클래스: 로또 성적 기록 메서드

-   게임 결과에 대한 점수들(Score enum 클래스)을 객체로 가지고 있음
-   게임 당 한 개
-   숫자가 몇개나 일치하는지 확인하여 로또 당 등수를 확인하여 List<Score\\>로 관리함
-   profit 을 계산하는 메서드와 profitRate를 필드로 가지고 있음

#### Score 클래스

-   Enum 자료형
-   Result 클래스를 보조하는 클래스
-   맞춘 갯수와 등수, 상금을 가지고 있는 Enum 자료형의 클래스이다.

#### InputHanlder 클래스

각 입력이 제대로 입력되었는지 확인하는 기능도 가지고 있다.

-   금액 입력: 1000원 단위 / valid한 숫자 입력
-   당첨 번호 입력: 6 개의 숫자 입력 하였는지 / 1~45 사이 숫자인지 / 유효한 입력인지 / 중복은 없는지
-   보너스 번호 입력: 먼저 입력한 6개와 중복되지는 않는지 / 유효한 입력인지 / 1~45 사이인지

#### PrintHandler 클래스, Constants 클래스, 테스트 메서드

### 구현 중 이슈

#### 1\. System.out.println()의 메세지를 테스트 코드로 확인하기

```java
import org.junit.jupiter.api.AfterEach;
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.Test;
import java.io.ByteArrayOutputStream;
import java.io.PrintStream;
import static org.assertj.core.api.Assertions.assertThat;

public class MyTest {
    private final PrintStream standardOut = System.out;
    private final ByteArrayOutputStream outputStreamCaptor = new ByteArrayOutputStream();

    @BeforeEach
    public void setUp() {
        System.setOut(new PrintStream(outputStreamCaptor));
    }

    @Test
    void whenInvalidInput_thenErrorMessageIsPrinted() {
        String input = "2145g";
        inputHandler.readCost(input);
        assertThat(outputStreamCaptor.toString())
            .contains("[ERROR] 숫자만을 입력해야 합니다.");
    }

    @AfterEach
    public void tearDown() {
        System.setOut(standardOut);
    }
}
```

[System.out.println() 메시지를 테스트하려면, 표준 출력 스트림(System.out)을 임시로 다른 OutputStream으로 변경하고, 그 스트림에 쓰여진 내용을 검사할 수 있다](http://daplus.net/java-system-out-println-%ec%97%90-%eb%8c%80%ed%95%9c-junit-%ed%85%8c%ec%8a%a4%ed%8a%b8/).

#### 2\. Enum 클래스 사용하기

Java에서 Enum 클래스를 처음 사용해 보았다.

```java
public enum Score {
    FIRST(6, 200000000, "6개 일치 (2,000,000,000원)"),
    SECOND(5, 30000000, "5개 일치, 보너스 볼 일치 (30,000,000원)"), // 보너스 번호 일치하는 경우
    THIRD(5, 1500000, "5개 일치 (1,500,000원)"),
    FOURTH(4, 50000, "4개 일치 (50,000원)"),
    FIFTH(3, 5000, "3개 일치 (5,000원)");

    private final int correctCount;
    private final int prize;
    private final String message;

    Score(int correctCount, int prize, String message) {
        this.correctCount = correctCount;
        this.prize = prize;
        this.message = message;
    }

    public int getCorrectCount() {
        return correctCount;
    }
    public int getPrize() {
        return prize;
    }
    public String getMessage() {
        return message;
    }
    public static Score valueOf(int correctCount) {
        for(Score score : values()) {
            if(score.getCorrectCount() == correctCount) {
                return score;
            }
        }
        return null;
    }
}
```

#### 3\. HashSet을 사용한 리스트 조작하기

HashSet을 사용하면 불변 속성이어야 하는 리스트로부터 새롭게 분리하여 중복이나 비교 등의 메서드로 간편하게 자료를 조작할 수 있다.

```java
    private int calculatePoint(List<Integer> winningNumbers, Integer bonusNumber, List<Integer> numbers) {
        Set<Integer> setOfWinning = new HashSet<>(winningNumbers);
        Set<Integer> setOfNumbers = new HashSet<>(numbers);
        setOfWinning.retainAll(setOfNumbers);
        return ifBonusIncluded(bonusNumber, numbers, setOfWinning);
    }
```

### 코멘트

1.  **단위 테스트를 진행하면서 구현**: 국소 메서드에 대한 신뢰성을 높이며 여러 메서드를 유기적으로 결합하였을 때 문제가 발생하면 문제의 범위를 쉽게 좁힐 수 있었다.
2.  **Enum 클래스**로 다양한 방법으로 데이터를 저장하고 사용하는 방법에 대해 숙지할 수 있었다.
3.  **객체 상태를 전달하고 관리하는 과정에서 발생할 수 있는 오류**에 대해 인지하면서 구현해야 한다.
    1.  이번 프로젝트에서는 GameManager에서 객체를 초기화하고
    2.  이 초기화 상태에서 벗어나지 않는경우, 즉 입력이 제대로 발생하지 않는 경우 예외를 발생시키고
    3.  이 예외 발생시 다시 반복문 처음으로 돌아가 입력을 받도록 구현하였다.
    4.  제대로 입력이 되면 상태가 변경된 객체를 리턴하여 반복문을 탈출하도록 하였다.
    5.  따라서 객체의 상태에 대한 반영이 제대로 되었는지, 입력 결과가 의도한 바와 다르지 않게 반영이 되었는지, 오류 발생시 어떻게 처리되는지 인지하여야 구현 과정에서 혼선이 없이 의도한 대로 구현할 수 있었다.
4.  **4주차 들어가기 앞서: 가장 작은 핵심 기능을 수행하는 프로그램을 작성하는 것이 최우선이다.**

### 피드백(공통 사항)

-   단일 메서드 라인은 15줄 이하
-   발생할 예외 상황에 대한 고민을 해볼 것
-   비즈니스 로직과 UI 로직을 분리한다.
-   연관성이 있는 상수는  static final 대신에 enum을 사용한다.
-   final 키워드를 사용하여 값의 변경을 막는다.
    -   최신 언어들 같은 경우 기본적으로 불변값으로 변수를 생성한다. 자바에서는 final을 활용하자.
-   객체의 상태 접근을 제한하자.
-   **객체는 객체스럽게 사용하자. ✨✨✨✨✨**
    -   getter를 인수로 전달하는 것은 객체의 상태를 변경하지도 않고, 객체가 일하는 것이 아닌 전달자로 사용하는 것이므로 객체지향적이라고 보기 힘들다(**모든 멤버변수에 getter를 생성해 놓고 상태값을 꺼내 그 값으로 객체 외부에서 로직을 수행한다면, 객체가 로직(행동)을 갖고 있는 형태가 아니고 메시지를 주고 받는 형태도 아니게 된다. 또한, 객체 스스로 상태값을 변경하는 것이 아니고, 외부에서 상태값을 변경할 수 있는 위험성도 생길 수 있다.**).
    -   이런 메서드의 구성은 getter로 속성을 불러온 후 또 다른 객체에서 메서드를 실행하므로 다시 별개의 객체가 필요하다.
    -   대신, 객체 내부에 메서드를 내장하면 객체가 스스로 해당 메서드를 실행하도록 할 수 있다.
    -   예를 들어, racing car에서 자동차의 거리를 비교하는 메서드를 외부에 작성하기보다 car내부에 내장하면, maximum position값을 받게되면 그 값을 자신의 필드값과 비교함으로써 객체지향적으로 일을 수행하게 할 수 있다.
    -   Lotto게임의 경우에는 Lotto 객체 내부에 contains()와 같은 메서드를 갖추어 winningNumber를 인자로 주면 Lotto 객체가 판단할 수 있도록 구현하는 것이다. 즉,

> 상태를 가지는 객체를 추가했다면 객체가 제대로 된 역할을 하도록 구현해야 한다.  
> 객체가 로직을 구현하도록 해야한다.  
> 상태 데이터를 꺼내 로직을 처리하도록 구현하지 말고 객체에 메시지를 보내 일을 하도록 리팩토링한다.

[getter를 사용하는 대신 객체에 메시지를 보내자 (techcourse.co.kr)](https://tecoble.techcourse.co.kr/post/2020-04-28-ask-instead-of-getter/)

-   필드의 수를 줄이기 위해 노력한다.

```java
public class LottoResult {
    private Map<Rank, Integer> result = new HashMap<>();
    private double profitRate;
    private int totalPrize;
}
```

이 경우 result만 있어도 모두 구할 수 있으므로 하나의 필드만으로 구현할 수 있다.

```java
public class LottoResult {
    private Map<Rank, Integer> result = new HashMap<>();

    public double calculateProfitRate() { ... }
    
    public int calculateTotalPrize() { ... }
}
```

-   성공하는 케이스 뿐만 아니라 예외에 대한 케이스도 테스트한다: **특히 경계값**
-   테스트 코드도 코드다: **리팩터링을 통해 개선해야 한다. 반복을 줄여 중복되지 않도록.** 파라미터의 값이 바뀌는 경우라면 다음과 같이 작성할 수 있다.

```java
@DisplayName("천원 미만의 금액에 대한 예외 처리")
@ValueSource(strings = {"999", "0", "-123"})
@ParameterizedTest
void underLottoPrice(Integer input) {
    assertThatThrownBy(() -> new Money(input))
            .isInstanceOf(IllegalArgumentException.class);
}
```

-   테스트를 위한 코드는 구현 코드에서 분리한다  
    -   테스트를 위해 접근제어자를 변경하는 경우,
    -   테스트 코드에서만 사용되는 메서드의 작성과 같은 경우는 없어야 한다.
-   **단위 테스트하기 어려운 코드를 단위 테스트 하기**

**A 상태**

```java
import camp.nextstep.edu.missionutils.Randoms;

public class Lotto {
    private List<Integer> numbers;

    public Lotto() {
        this.numbers = Randoms.pickUniqueNumbersInRange(1, 45, 6);
    }
}

——————

public class LottoMachine {
    public void execute() {
        Lotto lotto = new Lotto();
    }
}
```

**B 상태**

```java
public class Lotto {
    private List<Integer> numbers;

    public Lotto(List<Integer> numbers) {
        this.numbers = numbers;
    }
}

——————

import camp.nextstep.edu.missionutils.Randoms;

public class LottoMachine {
    public void execute() {
        List<Integer> numbers = Randoms
            .pickUniqueNumbersInRange(1, 45, 6);
        Lotto lotto = new Lotto(numbers);
    }
}
```

![for-test](https://1drv.ms/i/s!Ano-rmQ7e_nEwH7FZ_fKraxd2pun?embed=1&width=1012&height=573)
상태A에서 상태B로 리팩터링하여 Lotto 클래스에 대한 테스트가 쉬워졌지만, LottoMachine에 대한 테스트 방법은 생각을 해 보아야 한다.

[메서드 시그니처를 수정하여 테스트하기 좋은 메서드로 만들기 (techcourse.co.kr)](https://tecoble.techcourse.co.kr/post/2020-05-07-appropriate_method_for_test_by_parameter/)

-   Private 메서드를 테스트하고 싶을 때에는 클래스(객체)분리를 고려한다.
    -   가독성 때문에 private로 분리한 메서드는 public 메서드를 테스트함으로써 간접적으로 테스트할 수 있다.
    -   하지만 이 이유가 분리한 이유가 아니라면(그 이상의 역할을 한다면), 테스트하기 쉽게 구현하기 위해 해당 역할을 수행하는 다른 객체를 만들 타이밍인지 고려해 본다.
    -   다음 단계를 진행할 때에는 너무 많은 역할을 하고 있는 메서드나 객체를 어떻게 의미있는 단위로 분할 할지에 초점을 맞추어 진행한다.