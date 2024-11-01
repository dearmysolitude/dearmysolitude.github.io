---
title: "2023 우테코 프리코스 후기 - 1"
excerpt: "우테코 준비과정의 첫 단계에서 코드를 작성하며 정리한 내용들입니다."

categories:
  - Activity
tags:
  - 에세이
  - 우테코 프리코스
---
링크: [https://github.com/dearmysolitude/java-baseball-6](https://github.com/dearmysolitude/java-baseball-6)

## 구현

### 0\. Application

```java
public class Application {
    public static void main(String[] args) {
        // TODO: 프로그램 구현

        int button = 1;
        AnswerBox answerBox = new AnswerBox();
        List<Integer> numGenerated;

        System.out.println("숫자 야구 게임을 시작합니다.");

        while (button == 1) { // 게임: 종료 버튼(2) 안눌리면 계속 진행됨 
            numGenerated = NumGen.generate();
            answerBox.setScore(0, 0)
                    .setMessage("낫싱");

            Processor.doGame(answerBox, numGenerated);

            System.out.println("3개의 숫자를 모두 맞히셨습니다! 게임 종료");
            System.out.println("게임을 새로 시작하려면 1, 종료하려면 2를 입력하세요.");

            try {
                button = Integer.parseInt(Console.readLine());
            } catch (NumberFormatException e) {
                throw new IllegalArgumentException("Invalid input: Cannot enter anything other than 1 or 2");
            }

            if (button != 1 && button != 2) {
                throw new IllegalArgumentException("Invalid input: Cannot enter anything other than 1 or 2");
            }

        }
        
        Console.close();
    }
}
```

-   Button 변수와 while 문으로 사용자가 게임 끝낼 때까지 반복
-   임의의 숫자 생성 및 게임 시작 문구 출력
-   **게임 진행 메서드** 호출
-   게임 회차가 종료되면 안내 메세지 출력
-   값을 입력받아 button 변수를 업데이트, button 변수가 2로 업데이트 되었다면 while문을 탈출하여 게임 종료
-   싱글톤으로 사용하였던 Scanner 클래스의 메서드를 (Console.nexLine()로 호출) Console.close()로 종료

### 1\. AnswerBox

```java
public class AnswerBox {
    private Integer strike;
    private Integer ball;
    private String message;
    
    public AnswerBox() {
        this.strike = 0;
        this.ball = 0;
        this.message = "낫싱";
    }
    
    public AnswerBox setScore(Integer strike, Integer ball) {
        this.strike = strike;
        this.ball = ball;
        return this;
    }

    public void setMessage(String message) {
        this.message = message;
    }
    
    public String getMessage() {
        return this.message;
    }

    public void updateMessage() {

        if (this.strike.equals(0) && this.ball.equals(0)) {
            this.message = "낫싱";
        } else if (this.strike.equals(0)) {
            this.message = this.ball + "볼";
        } else if (this.ball.equals(0)) {
            this.message = this.strike + "스트라이크";
        } else {
            this.message = this.ball + "볼 " + this.strike + "스트라이크";    
        }
    }
    
}
```

-   Ball, strike, 출력할 message를 담고있는 클래스
-   Ball, strike 수에 따라 message를 업데이트하는 메서드 내장
-   생성된 객체는 게임이 종료할 때까지 계속 사용된다: 싱글톤

처음에는 updateMessage()메서드를 구현할 때 복잡하게 하였으나 수정하며 복잡도를 줄일 수 있었다: IntelliJ Code Metrics 플러그인 -> 나중에 알아보자

### 2\. NumGen

```java
public class NumGen {
    
    public static List<Integer> generate() {
        ArrayList<Integer> list = new ArrayList<>();

        while(list.size() < 3) {
            int temp = Randoms.pickNumberInRange(1, 9);
            if(!list.contains(temp)) {
                list.add(temp);
            }
        }
        
        return list;
    }
    
}
```

-   게임에서 사용할 숫자를 생성하여 반환하는 클래스: 싱글톤
-   랜덤 숫자를 생성하는 클래스, 예시로 제공된 코드를 그대로 사용하였다.

### 3\. Processor

```java
public class Processor {
    
    private Processor() {}
    
    public static void doGame(AnswerBox answerBox, List<Integer> numGenerated) {
        while (!answerBox.getMessage().equals("3스트라이크")) { // 사용자에게서 맞출때까지 계속해서 입력 받음
            System.out.print("숫자를 입력해 주세요: ");
           
            String input = Console.readLine();
            if(input.isEmpty()) {
                throw new IllegalArgumentException("Invalid Input: Empty");
            }
            // 입력받은 String 의 input 을 List<Integer>형태로 변환
            List<Integer> numGet;
            
            try {
                numGet = Arrays.stream(input.split(""))
                        .map(Integer::parseInt)
                        .toList();
            } catch (NumberFormatException e) {
                throw new IllegalArgumentException("Invalid Input: Not a number", e);
            }
            
            Set<Integer> compare = new HashSet<>(numGet);
            //입력 받은 수의 유효성 검사
            if(numGet.size() != 3 || compare.size() != 3 || numGet.contains(0)) {
                throw new IllegalArgumentException("Invalid Input: Input numbers are not suitable");
            }
            
            Processor.compareNum(numGet, numGenerated, answerBox);
            
            System.out.println(answerBox.getMessage());
        }
    }
    
    private static void compareNum(List<Integer> numGet, List<Integer> numGenerated, AnswerBox answerBox) {
        int strike = 0;
        int ball = 0;

        for(int i = 0; i < numGenerated.size(); i++) {
            Integer a = numGenerated.get(i);
            if (a.equals(numGet.get(i))) {
                strike += 1;
            } else if (numGet.contains(a)) {
                ball += 1;
            }
        }

        answerBox.setScore(strike, ball);
        answerBox.updateMessage();
    }
    
}
```

사용자가 게임에서 승리(3 스트라이크)할 때까지 반복 진행

-   사용자에게서 숫자 3개를 입력받음
-   List<Integer>형태로 형변환 및 예외 처리
-   숫자 비교 메서드 실행, AnswerBox에 적용된 메세지를 출력한다.

#### Processor 클래스: 숫자 비교 메서드

-   Strike와 ball을 구별하여 AnswerBox에 반영한다.

### 기타

-   이 게임은 하나의 스레드로만 진행하므로 모든 클래스는 static과 싱글톤 패턴을 사용
-   주로 다루는 자료형은 List<Integer>로 하였다.
-   charAt(int index)을 사용하면 String을 통해서도 구현하는 것이 가능할 것 같다.

## 알게 된 내용

### Scanner 클래스

-   입력 스트림을 처리하며, 운영체제의 리소스를 사용한다.
-   자바 유틸리티 클래스 중 하나. 간단한 텍스트 스캐닝 및 파싱을 위해 사용한다.
-   InputStream, File, String 등의 다양한 입력 소스로부터 텍스트를 읽어들이고, 이를 원하는 데이터 타입으로 변환하는 기능을 제공한다.
-   Token(토큰)과 delimeter(구분자)를 사용하여 데이터를 가공하며, 입력 소스를 token으로 분리하여 원하는 형태로 처리할 때 delimeter를 기준으로 한다.
-   Scanner 객체가 생성되면 이 객체는 입력 스트림(Stream)을 읽기 시작한다. 내장 메서드들을 호출함으로써 Scanner가 토큰을 읽는 것을 제어한다.

Stream은 데이터를 순차적으로 처리하는 연속적인 데이터 흐름의 추상화 개념이다.

[The Java 8 Stream API Tutorial | Baeldung](https://www.baeldung.com/java-8-streams)

**Stream에 대해서는 나중에 개별 주제로 보는 걸로**

이 프로젝트에서는 Wrapper Class(Console)로 감싸 **API 메서드를 단순화**하고, **시스템 리소스에서 leakage가 발생하지 않도록 명시적으로 해제할 수 있도록** 하며, **하나의 인스턴스만 생성하여 재사용하는 싱글톤으로 패턴을 사용(메모리 사용량 줄임)**하도록 하였다: 교육용!! 아래가 Scanner 의 wrapper 클래스인 Console 클래스이다.

```java
public class Console {
    private static Scanner scanner;

    private Console() {
    }

    public static String readLine() {
        return getInstance().nextLine();
    }

    public static void close() {
        if (scanner != null) {
            scanner.close();
            scanner = null;
        }
    }

    private static Scanner getInstance() {
        if (scanner == null) {
            scanner = new Scanner(System.in);
        }
        return scanner;
    }
}
```

### GC(Garbage Collector)

**Java의 GC는 스레드의 heap 메모리 영역만 관리한다(모든 애플리케이션 스레드가 공유).** JVM(Java Virtual Machine)당 하나만 실행된다. 별도의 스레드로 실행되며, 이 스레드는 모든 애플리케이션 스레드와 독립적으로 작동한다. JVM은 GC를 여러 스레드로 병렬처리 하도록 설계되어 있다. 여러 스레드에 GC 작업이 분산되지만, GC 스레드들은 여전히 JVM 전체 힙 메모리를 공유하기 때문이다.

시스템 리소스를 사용하는 Scanner 클래스는 입력 스트림은 GC로 처리되지 않으므로 이를 관리하기 위해 Console 클래스에는 Scanner 객체를 해제하는 close()메서드를 내장한 것이다.

**Java의 GC에 대해서도 나중에 개별 주제로 자세하게 살펴보자  
**

### 예외 처리

Java에서는 NullPointerException이 가장 중요한 예외 중 하나이다. Java는 객체를 관리하는 것이 중요한 언어로, 참조로 전달된 객체가 null pointer인 경우를 관리하기 위해 Null PointerExcepotion을 발생시킨다. 다만 이번 과제에서는 NullPointerException보다는 예외를 처리하는 코드 작성에 대해 익숙해진 기회가 된 것 같다.\\

#### 예외 처리를 할 때 빠트리지 않고 어떻게 다 처리해?

1.  **문서화**: 대부분의 프로그래밍 언어와 라이브러리는 함수나 메소드가 던질 수 있는 예외에 대해 문서화하고 있습니다. 이러한 문서를 참조하면 어떤 예외가 발생할 수 있는지 알 수 있습니다.
2.  **코드 분석**: 코드를 직접 분석하여 어떤 예외가 발생할 수 있는지 파악할 수 있습니다. 예를 들어, Integer.parseInt() 메소드는 입력 문자열이 유효한 정수가 아닐 경우 NumberFormatException을 던집니다.
3.  **테스트**: 코드를 실행하고 다양한 입력 값을 제공하여 어떤 예외가 발생하는지 확인할 수 있습니다. 이 방법은 특히 문서화가 부족하거나 코드 분석이 어려운 경우 유용합니다.
4.  **IDE 및 도구 사용**: 많은 통합 개발 환경(IDE) 및 코드 분석 도구는 코드에서 발생할 수 있는 예외를 자동으로 감지하고 알려줍니다.

Bing AI가 작성한 글입니다.

### Static 메서드와 Singleton 패턴의 비교

이번 과제를 수행하면서 반복 사용하게되는 메서드에 대해서는 static 제한자로 설정하여 객체를 생성하지 않고 메서드를 사용할 수 있도록 하였다. 반면 Scanner 클래스를 래핑한 Console 클래스의 경우 singleton 패턴을 활용하여 하나의 객체를 재사용 하도록 하였다. 둘의 차이점은 다음과 같다.

**Static 메서드**:

-   클래스의 인스턴스를 생성하지 않고도 호출할 수 있다.
-   주로 유틸리티나 헬퍼 함수를 작성할 때 사용한다. (수학 계산, 날짜 변환)
-   객체의 상태를 변경하지 않는다. 즉, static 메서드 내에서는 static이 아닌 필드나 메서드에 접근할 수 없다.

**Singleton 패턴**:

-   특정 클래스의 인스턴스가 하나만 생성되도록 보장하는 디자인 패턴.
-   전역 변수를 사용하지 않고도 객체를 여러 곳에서 공유해야 할 때 유용합니다. (설정, 로그, 드라이버 객체)
-   상태를 가질 수 있으며, 이 상태는 모든 클라이언트에게 공유된다.

둘 다 잘못 사용하면 코드의 유지 보수성을 해칠 수 있다. 예를 들어, 과도한 static 메서드 사용은 객체 지향 프로그래밍의 이점을 상실하게 만들 수 있고, Singleton 패턴은 전역 상태를 만들어 디버깅을 어렵게 만들 수 있다. 또한 **멀티스레드 환경에서는 동기화 문제를 고려해야 한다.** 객체의 상태가 필요하고, 이를 여러 곳에서 공유해야 하는 경우에는 signgleton 패턴을 사용하는 것이 좋다.