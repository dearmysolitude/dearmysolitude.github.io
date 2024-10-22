---
title: "Spring 예제 코드 01"
excerpt: "Spring 애플리케이션 컨텍스트에서 bean 정의 이름 목록을 검색하고, 이 목록의 이름을 콘솔에 울력하는 예제"

toc: false

categories:
  - Spring
tags:
---

```java
package com.jy.learnspringframework.examples.a0;

import java.util.Arrays;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.Configuration;

@Configuration
@ComponentScan
public class SimpleSpringContextLauncherApplication {
	public static void main(String[] args) {
		try(var context = new AnnotationConfigApplicationContext(SimpleSpringContextLauncherApplication.class)) {
			Arrays.stream(context.getBeanDefinitionNames())
			.forEach(System.out::println);
		} catch () {
			
		}
	}
}
```

`Arrays.stream(context.getBeanDefinitionNames()).forEach(System.out::println);`

Spring 애플리케이션 컨텍스트에서  bean 정의 이름 목록을 검색하고, 이 목록의 이름을 콘솔에 출력한다.
    
- `context.getBeanDefinitionNames()`

    Spring 애플리케이션 컨텍스트에서 bean 정의 이름 목록을 검색한다. String 배열을 반환하며, 각 요소는 bean 정의의 이름을 나타낸다.

- `Arrays.stream()`

    배열을 스트림으로 변환.

- `forEach`

    스트림의 각 요소에 대해 지정된 작업을 수행한다. 이 예제에서는 `System.out::println`메서드 참조를 전달하여 스트림의 각 요소를 출력하는 것이다.

