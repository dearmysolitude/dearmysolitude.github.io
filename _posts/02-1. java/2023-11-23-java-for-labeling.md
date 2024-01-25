---
title: "for문의 라벨링 & 향상된 for 문"
excerpt: "자바에서의 이중 for문과 향상된 for문을 살펴보자"

toc: true
toc_sticky: true

categories:
  - Java
tags:
---
## 다중 for 문 라벨링 
for 문에 라벨링을하여 다중 반복문을 조절할 수 있다.

### Example 1

```java
public class LabelExam {
	public static void main(String[] args) {
    	outter:
    	for(int i = 0; i < 3; i ++) {
    		for(int k = 0; k < 3; k++) {
        		if(i == 0 && k == 2)
            		break outter;
            	System.out.println(i+", "+k);
         }
     }
 }
}
```

### Example 2

```java
public class LabelExam {
	public static void main(String[] args) {
    	outter:
    	for(int i = 0; i < 3; i ++) {
    		for(int k = 0; k < 3; k++) {
        		if(i == 0 && k == 2)
            		continue outter;
            	System.out.println(i+", "+k);
         }
     }
 }
}
```

## 컬렉션에 사용하는 향상된 for 문

```java
int sum = 0;
int i = 0;
for(int i score : scores) {
		if(i % 2 == 0) {
				sum = sum + scores[i];
		}
		i++;
}
```

이 경우, 홀수 번의 요소의 경우에, scores에서 순차적으로 요소가 score에 복사되므로 비효율적인 작업이 발생한다. 이렇게 **컬렉션에 사용하는 향상된 for문의 경우, 컬렉션의 모든 요소에 작업하는 경우에 사용한다.**

추가/권장: 원본 데이터를 건드리지 말고 사용하자 👉 필요하다면 복사하여 사용

```java
int sum = 0;
int i = 0;
for(final int i score : scores) { // << 이렇게 final로 해 놓으면,
		score = score * 100; // << 이 부분이 구문 오류로 잡힌다!
		if(i % 2 == 0) {
				sum = sum + scores[i];
		}
		i++;
}
```