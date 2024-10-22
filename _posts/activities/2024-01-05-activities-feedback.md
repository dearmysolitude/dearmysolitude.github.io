---
title: "우테코 프리코스 백엔드 피드백"
excerpt: "프로그래밍 간단 지침: 코딩 컨벤션에서 문제 풀이 전략까지"

toc: true
toc_sticky: true

categories:
  - Activity
tags:
  - 피드백
  - 우아한 테크코스
---

## Week 01. 숫자 야구

### 0. 일반

- 요구 사항을 정확히 준수한다: 각 항목을 모두 잘 지켰는지 다시 한 번 점검한다. 
- 커밋 메세지를 의미 있게 작성한다: 작업 내용에 대한 이해가 가능해야 한다.
- Pull Request를 보내기 전 브랜치를 확인한다.
- 공백도 코딩 컨벤션이다.
- 공백 라인을 의미있게 사용한다: 문맥을 분리하는 부분에 사용하는 것이 좋지만, 과도한 공백은 다른 개발자에게 의문을 줄 수 있다.
- space와 tab을 혼용하지 않는다.
- 의미 없는 주석을 달지 않는다: 변수 이름, 메서드 이름을 통해 의도가 드러난다면 굳이 주석을 달지 않는다. 가능하면 이름을 통해 의도를 드러내고, 이름을 통해 의도가 드러나기 힘든 경우에만 주석을 달도록 한다.


### 1. git을 통해 관리할 자원에 대해서 고려한다.

`.class` 파일은 java 코드로 생성되고, IntelliJ IDEA의 `.idea`, Eclipse의 `.metadata`폴더도 개발 도구가 자동으로 생성하는 폴더이므로 git으로 관리하지 않아도 된다.

git 에 코드를 추가할 때에는 git을 통해 관리할 필요가 있는지 고려해 보는 것이 좋다. `.gitignore`는 git의 root 디렉터리에 저장하여 git repository나 staging area에 추가되지 않는 폴더/파일을 정의하는 파일이므로 문법에 맞게 작성함으로써 관리할 수 있다.

.gitignore를 작성하는 방법은 구글링으로 찾아보면 많은 자료를 찾아볼 수 있다. 추가로, [gitignore.io](https://www.toptal.com/developers/gitignore) 에서는 원하는 개발 환경에 맞는 .gitignore를 작성해 주므로 잘 활용하자.

### 2. 이름을 통해 의도를 드러낸다.

개발자와 소통을 위해 가장 중요한 활동 중 하나가 좋은 이름 짓기이다. 변수 이름, 함수(메서드) 이름, 클래스 이름을 짓는 데 시간을 투자하라.

변수, 함수, 클래스의 역할에 대한 의도를 드러내야 한다. 연속된 숫자를 붙이거나 불용어(Info, Data, a, an, the)를 추가하는 방식은 적절하지 못하다.

### 3. 축약하지 않는다.

의도를 드러낼 수 있다면 이름이 길어져도 괜찮다.

>누구나 실은 클래스, 메서드, 또는 변수의 이름을 줄이려는 유혹에 곧잘 빠지곤 한다. 그런 유혹을 뿌리쳐라. 축약은 혼란을 야기하며, 더 큰 문제를 숨기는 경향이 있다. 클래스와 메서드 이름을 한 두 단어로 유지하려고 노력하고 문맥을 중복하는 이름을 자제하자. 클래스 이름이 Order라면 shipOrder라고 메서드 이름을 지을 필요가 없다. 짧게 ship()이라고 하면 클라이언트에서는 order.ship()라고 호출하며, 간결한 호출의 표현이 된다. 
>
>- 객체 지향 생활 체조 원칙 5: 줄여쓰지 않는다 (축약 금지)

### 4. IDE의 코드 자동 정렬 기능을 활용한다.

- IntelliJ IDEA: ⌥⌘L, Ctrl+Alt+L
- Eclipse: ⇧⌘F, Ctrl+Shift+F

### 5. Java에서 제공하는 API를 적극 활용한다.

메서드를 직접 구현하기 전에 Java API에서 제공하는 기능인지 검색을 먼저 해본다. 제공하지 않을 경우 직접 구현한다.

예시: 사용자 출력시 2명 이상일 때 다음과 같은 구현이 가능하다.

```java
List<String> members = Arrays.asList("pobi", "jason");
String result = String.join(",", members); // "pobi,jason"
```

### 6. 배열 대신 Java Collection을 사용한다.

Java Collection(List, Set, Map 등)을 사용하면 데이터를 조작할 때 다양한 API를 사용할 수 있다.

### 7. 추가 학습 자료

- [[10분 테코톡] 오리&코린의 Merge, Rebase, Cherry pick](https://www.youtube.com/watch?v=b72mDco4g78)
- [[10분 테코톡] 🎲 와일더의 Git Commands](https://www.youtube.com/watch?v=JsRD2AWxxFg)
- [git - 간편 안내서](https://rogerdudler.github.io/git-guide/index.ko.html)
- [git과 github](https://www.inflearn.com/course/git-and-github)

## Week 02. 자동차 경주

### 0. 일반

- README.md를 상세히 작성한다: 소스코드에 앞서 프로젝트에 대한 설명을 마크다운으로 작성하여 소개하는 문서이다. 어떤 프로젝트인지, 어떤 기능을 담는지 기술하는 문서.
- 기능 목록을 업데이트 한다: README.md 파일에 작성하는 기능 목록은 기능 구현하면서 변경될 수 있다. 시작할 때 모든 기능 목록을 완벽하게 정리해야 한다는 부담을 가지기보다, 기능을 구현하면서 문서를 계속 업데이트한다. 죽은 문서가 아니라 살아있는 문서를 만들기 위해 노력한다.
- 값을 하드 코딩 하지 않는다: 상수(`static final`)를 만들고 이름을 부여해 이 변수의 역할이 무엇인지 의도를 드러내라. 구글에서 "java 상수"와 같은 키워드로 검색하여 상수 구현 방법을 학습해보라.
- 구현 순서도 코딩 컨벤션이다: 클래스는 상수, 멤버 변수, 생성자, 메서드 순으로 작성한다.
  ```java
  class A {
    상수(static final) 또는 클래스 변수

    인스턴스 변수

    생성자

    메서드

    }
  ```
- 변수 이름에 자료형, 자료 구조는 사용하지 않는다.

### 1. 기능 목록을 재검토한다.

- 기능 목록을 클래스 설계와 구현, 함수(메서드) 설게와 구현과 같이 너무 상세하게 작성하지 않는다. 클래스 이름, 함수(메서드) 시그니처와 반환값은 언제든지 변경될 수 있기 때문이다.
- 세세한 부분까지 정리하기보다, **구현해야할 기능 목록을 정리하는 데 집중**한다. - 정상적인 경우도 중요하지만, **예외적인 상황도 기능 목록에 정리한다. 예외 상황은 시작 단계에서 모두 찾기 힘들기 때문에 기능을 구현하면서 계속해서 추가해 나간다.**

### 2. 한 함수가 한 가지 기능만 담당하게 한다.

함수 길이가 길어진다면 한 함수에서 여러 일을 하려고하는 경우일 가능성이 높다. 아래와 같은 경우 이를 적절하게 분리하는 것이 좋다.

```java
public List<String> userInput() {
    System.out.println("경주할 자동차 이름을 입력하세요(이름은 쉼표(,)를 기준으로 구분).");
    String userInput = Console.readLine().trim();
    String[] splittedName = userInput.split(",");
    for (int index = 0; index < splittedName.length; index++) {
        if (splittedName.length < 1 || splittedName.length > 5) {
            throw new IllegalArgumentException("[ERROR] 자동차 이름은 1자 이상 5자 이하만 가능합니다.");
        }
    }
    return Arrays.asList(splittedName);
}
```

이를 판단하기 위해, 함수가 한가지 기능만 하는지 확인하는 기준을 세운다: 여러 함수에서 **중복되어 사용되는 코드가 있다면 함수 분리를 고민**한다. 또한, 15 라인을 넘어가지 않도록 구현하며 함수 분리하는 것을 **의식적으로 연습해야** 한다.

### 3. 테스트를 작성하는 이유에 대해 본인의 경험을 토대로 정리한다.

기능을 점검하기 위한 목적으로 테스트를 작성하는 것은 아니다. 작성 과정을 통해서 나의 코드에 대해 빠르게 피드백을 받을 수 있을 뿐만 아니라, 학습 도구로도 활용할 수 있다.

### 4. 처음부터 큰 단위의 테스트를 만들지 않는다.

테스트의 중요한 목적 중 하나는 내가 작성하는 코드에 대해 빠르게 피드백을 받는 것이다. 시작부터 큰 단위의 테스트를 만들게 된다면 작성한 코드에 대한 피드백을 받기까지 많은 시간이 걸린다. 문제를 작게 나누고, 핵심 기능에 가까운 부분부터 작게 테스트를 만들어 나간다.

## Week 03. 로또 게임

### 0. 일반

- 발생할 수 있는 예외 상황에 대해 고민한다: 정상적인 경우를 구현하는 것보다 예외 상황을 모두 고려해 프로그래밍 하는 것이 더 어렵다. 예외 상황을 고려해 프로그래밍 하는 습관을 들인다.
- `final` 키워드를 활용해 값의 변경을 막는다: 다만, 참조 변수가 `final`일 경우, 참조하는 메모리 위치만 고정이다.
- 객체의 상태 접근을 제한한다: 인스턴스 변수의 접근 제어자는 `private`로 구현한다.
- 테스트 코드도 코드다: 리팩터링을 통해 개선해 나가야 한다. 특히 반복적으로 하는 부분을 중복되지 않게 만들어야 한다.
- 테스트를 위한 코드는 구현 코드에서 분리되어야 한다: 테스트를 위한 편의 메서드를 구현코드에 구현하면 안된다. 테스트를 위해 접근제어자를 바꾸는 경우나 테스트 코드에서만 사용되는 메서드가 그 예이다.

### 1. 비즈니스 로직과 UI 로직을 분리한다.

한 클래스가 비즈니스 로직과 UI로직을 담당하지 않도록 한다(단일 책임의 원칙).

```java
public class Lotto {
    private List<Integer> numbers;

    // 로또 숫자가 포함되어 있는지 확인하는 비즈니스 로직
    public boolean contains(int number) {
        ...
    }

    // UI 로직
    private void print() {
        ...
    }
}
```

→ 현재 객체의 상태를 보기 위한 로그 메세지 성격이 강하다면 `toString()`을 통해 구현한다. View에서 사용할 데이터라면 getter 메서드를 통해 데이터를 전달한다.

### 2. 연관성이 있는 상수는 static final 대신 enum을 활용한다.

```java
public enum Rank {
    FIRST(6, 2_000_000_000),
    SECOND(5, 30_000_000),
    THIRD(5, 1_500_000),
    FOURTH(4, 50_000),
    FIFTH(3, 5_000),
    MISS(0, 0);

    private int countOfMatch;
    private int winningMoney;

    private Rank(int countOfMatch, int winningMoney) {
        this.countOfMatch = countOfMatch;
        this.winningMoney = winningMoney;
    }
}
```

### 3. 객체는 객체스럽게 사용한다.

getter를 사용해 본인의 상태 값을 전달하는게 아니라, 객체 내에서 처리 후, 상태 메세지를 던지도록 구조를 바꾸어 객체 스스로가 일하도록 한다.

예시

```java
public class Lotto {
    private final List<Integer> numbers;
    
    public Lotto(List<Integer> numbers) {
        this.numbers = numbers;
    }

    public int getNumbers() {
        return numbers;
    }
}

public class LottoGame {
    public void play() {
        Lotto lotto = new Lotto(...);

        // 숫자가 포함되어 있는지 확인한다.
        lotto.getNumbers().contains(number);
        
        // 당첨 번호와 몇 개가 일치하는지 확인한다.
        lotto.getNumbers().stream()...
    }
}
```

```java
public class Lotto {
    private final List<Integer> numbers;

    public boolean contains(int number) {
        // 숫자가 포함되어 있는지 확인한다.
        ...
    }
    
    public int matchCount(Lotto other) {
        // 당첨 번호와 몇 개가 일치하는지 확인한다.
        ...
    }
}

public class LottoGame {
    public void play() {
        Lotto lotto = new Lotto(...);
        lotto.contains(number);
        lotto.matchCount(...); 
    }
}
```

[getter를 사용하는 대신 객체에 메시지를 보내자](https://tecoble.techcourse.co.kr/post/2020-04-28-ask-instead-of-getter/)

### 4. 필드(인스턴스 변수)의 수를 줄이기 위해 노력한다.

필드(인스턴스 변수)의 수가 많으면 객체의 복잡도를 높이고, 버그 발생 가능성을 높인다.

필드에 중복이 있거나, 불필요한 필드가 없는지 확인해 필드의 수를 최소화한다. 특히, 아래와 같이 reault만 있어도 산출할 수 있는 필드는 구현하지 않는다.

```java
public class LottoResult {
    private Map<Rank, Integer> result = new HashMap<>();
    private double profitRate;
    private int totalPrize;
}
```

```java
public class LottoResult {
    private Map<Rank, Integer> result = new HashMap<>();

    public double calculateProfitRate() { ... }
    
    public int calculateTotalPrize() { ... }
}
```

### 5. 단위 테스트하기 어려운 코드를 단위 테스트하기

아래 코드는 `Random`때문에 `Lotto`에 대한 단위 테스트를 하기 힘들다. 어떻게 고치는 것이 좋을까?

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

올바른 로또 번호가 생성되는 것을 테스트하기 위해 클래스 외부로 분리하는 시도를 한다.

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

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216787&authkey=%21AL56OqAizGR1FVo&width=1012&height=573" alt="">
  <caption>위 코드를 그림으로 나타낸 것이다.</caption>
</figure>

[메서드 시그니처를 수정하여 테스트하기 좋은 메서드로 만들기](https://tecoble.techcourse.co.kr/post/2020-05-07-appropriate_method_for_test_by_parameter/)

단위 테스트를 할 때 테스트하기 어려운 부분은 분리하고 테스트 가능한 부분을 단위 테스트한다. 테스트하기 어려운 부분은 단위테스트 하지 않아도 된다. 남은 `LottoMachine`은 어떻게 테스트하기 쉽게 바꿀 수 있을지 고민해 볼 것.

### 6. private 메서드를 테스트하고 싶다면 클래스(객체) 분리를 고려한다.

가독성의 이유만으로 분리한 private 함수의 경우 public으로도 검증 가능하다고 여겨질 수 있다. public 함수가 private 한수를 사용하고 있기 때문에 테스트 범위에 자연스럽게 포함되는 것이다.

하지만 가독성 이상의 역할을 하는 경우, 테스트하기 쉽게 구현하기 위해서는 해당 역할을 수행하는 다른 객체를 만들 때가 아닌지 고민해보아야 한다. 다음 단계를 진행할 때에는 너무 많은 역할을 하고 있는 함수나 객체를 어떻게 의미 있는 단위로 분할할지에 초점을 맞추어 진행한다.