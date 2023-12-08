---
title: "for문의 라벨링 / java"
excerpt: "자바에서는 for문 라벨링이 가능하다: 토막글"

toc: true
toc_sticky: true

categories:
  - Programming
tags:
  - Java
---
for 문에 라벨링을하여 다중 반복문을 조절할 수 있다.

## Example 1

```java
public class LabelExam {
	public static void main(String[] args) {
    	outter:
    	for(int i = 0; i < 3; i ++) {
    		for(int k = 0; k < 3; k++) {
        		if(i == 0 && k == 2)
            		continue break;
            	System.out.println(i+", "+k);
         }
     }
 }
```

## Example 2

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
```