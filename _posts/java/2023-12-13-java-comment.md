---
title: "자바의 주석문"
excerpt: "자바에서 주석문을 살펴보자 | 토막글"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
| 주석 기호 | 설명 | 
|---|---|
| // | // 부터 시작해서 줄 끝까지 주석 처리 된다. |
| /* ... */ | /*와 */사이의 내용이 모두 주석처리 된다. |
| /** ... */ | /**와 */ 사이의 내용이 모두 주석처리 된다. JavaDoc 주석문 |

## JavaDoc 주석문

클래스 상단에 붙이는 JavaDoc 주석문

```java
/** 
 * 책 한권의 정보를 담기 위한 클래스
 * @author lucid(<a href="mailto:optimisticnihilism2007@gmail.com">박주영</a>)
 * @since 2022.03
 * @version 0.1.2
 * 
 */
```

메서드 상단에 붙이는 JavaDoc 주석문

```java
/** 
 * 책의 제목을 반환한다.
 * @return 책의 제목
 */
```

이러한 JavaDoc 주석문을 활용하여 JavaDoc을 생성할 수 있다.

### IntelliJ에서 JavaDoc 생성하기

- shift 키를 2번 연속으로 누른다.
- generate javaDoc을 입력한다.
- custon scope 를 선택한 후 JavaDoc을 생성할 때 패키지, 클래스, 인터페이스 등을 선택한다.
- output directory에서 JavaDoc이 생성할 경로를 지정한다.
- other command line arguments에는 다음을 입력한다: `-encoding UTF-8 -charset UTF-8 -docencoding UTF-8` 
- OK 버튼을 누른다.

### Command Line으로 JavaDoc 실행하기

`javadoc -protected -splitindex -encoding UTF-8 -charset UTF-8 -doencoding UTF-8 -d /Users/urstory/Documents/MyJavaDoc@/private/var/folders/kp/gbjcdk810dv1tyv7_dmm1vh0000gn/T/javadoc_args`

## 주석문 잘 작성하기
- 주석문이 없어도 이해할 수 있도록 클래스, 메서드, 변수 이름을 작성한다.
- 주석문은 최소한으로 작성한다.
- JavaDoc 주석문을 잘 작성한다.