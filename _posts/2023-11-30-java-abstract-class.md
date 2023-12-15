---
title: "추상 클래스(Abstract class)"
excerpt: "추상 클래스와 탬플릿 메서드 패턴(Template Method Pattern)"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
## 추상클래스
- 여러가지 속성들을 추상화하여 일반적인 속성만을 모아놓은 추상 클래스로 작성할 수 있다.
- 추상클래스는 인스턴스가 될 수 없다.
- 추상 클래스를 상속받는 자손이 인스턴스가 된다.
- abstract 키워드를 사용하여 클래스를 정의한다.
- 1개 이상의 추상 메서드로 이루어진다.

<figure style="width: 85%" class="align-center">
  <img src="https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216719&authkey=%21AGBFfraFIewTlHQ&width=549&height=120" alt="">
  <figcaption>자동차 뿐만 아니라 자동차의 후손들도 참조할 수 있다</figcaption>
</figure>

추상 클래스는 템플릿 메서드 패턴에서 가장 잘 사용할 수 있다.
## 템플릿 메서드 패턴
다음 같은 상황이라고 생각해보자.

```java
    Controller 종류가 여러개 →
    {
    초기화 - 같은 코드
    실행 - 다른 코드
    마무리 - 같은 코드
    }
```

Controller의 종류를 여러개 만들어 여러 종류의 케이스에 대해서 각각 처리를 하도록 Cotroller(추상클래스)를 부모로 하여 모델을 구현해 보자. 각 클래스는 적절하게 패키지를 작성하는 것이 좋다: 패키지 도메인은 생략

```java
public abstract class Controller {
	protected final void init() {
    	System.out.println("작업 시작");
    }
    protected final void close() {
    	System.out.println("작업 종료");
    }
    protected abstract void run(); // 추상 메서드: 작업마다 다른 실행
    
    //가지고 있는 메서드를 호출
    //일정한 순서를 가지고 있음: 템플릿 메서드라고 한다.
    public void execute() {
    this.init();
    this.run();
    this.close();
    }
}
```

protected 접근 지정자의 경우, 같은 package이거나 상속 받았을 경우에만 사용 가능
final 제한자는 오버라이딩이 금지된다.

다음은 추상 메서드를 구현한 클래스

```java
public class FirstController extends Controller {
	@Override
    protected void run() {
    	System.out.println("별도로 작성된 구체화 클래스");
    }
}
```

메인에서는 다음과 같이 사용할 수 있다.

```java
public class ControllerMain {
	public static void main(String[] args) {
   		Controller c1 = new FirstController():
		c1.execute();
    }
}
```

c1 참조 객체는 구체화 된 FirstController를 통해 초기화되면 이 객체를 c1의 execute 메서드, 즉 템플릿 메서드를 사용하여 구현된 메서드를 템플릿에 맞게 실행한다.

 
## 정리

템플릿 메서드 패턴은 추상 클래스를 가장 많이 사용하는 패턴 중에 하나로 다음의 내용들을 쉽게 이해하는데 도움이 된다:
- protected 접근 지정자를 사용하여 같은 패키지나 상속받은 클래스만 사용할 수 있도록 구성하며
- final 제한자를 사용함으로써 의도에서 벗어난 override를 방지하도록 설정한 것
