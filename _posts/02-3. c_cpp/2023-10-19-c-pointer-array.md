---
title: "포인터와 배열, 포인터 연산"
excerpt: "C언어의 포인터 살펴보기"

toc: true
toc_sticky: true

categories:
  - C&C++
tags:
  - pointer
  - array
  - 포인터
  - 배열
---

## 배열

배열(array)은 번호(인덱스)와 번호에 대응하는 데이터들로 이루어진 자료 구조를 나타낸다.

 <br> 

## 포인터

포인터(pointer)는 다른 변수, 혹은 그 변수의 메모리 공간주소를 가리키는 변수를 말한다. 포인터가 가리키는 값을 가져오는 것을 역참조라고 한다. 그 포인터가 가르키는 공간의 값을 가져오는 것을 역참조라고한다.
32 바이트 환경에서 C언어의 포인터는 4 바이트이며, 64 바이트 환경에서는 8 바이트이다.

![C 언어의 32 bit 환경에서 포인터](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216593&authkey=%21AIoQ2r1D1P3FMjE&width=740&height=330)

 <br> 

### 포인터 표기
- 포인터의 선언

```c
int* p; // int형 데이터를 가르키는 포인터 선언: 초기화하지 않음
```
 
- 포인터의 역참조

```c
int a = *p; // p의 위치에 있는 값을 a로 복사
```

- 주소 연산: &

```c
int s = 123;
int* b = &s; //s의 주소를 b포인터로 할당하여 저장
```
 
- 이중 포인터

```c
int number = 1004; // int형 자료
int*p = &number; // int형 자료를 가르키는 포인터형 자료
int** pp = &p; // 포인터 p를 가르키는 이중 포인터: 포인터형을 가르키는 포인터형 자료
```

 <br> 

### 포인터 연산

- 포인터는 연산자를 지원한다.
- 뺄셈도 같은 방식으로 지원한다.
![pointer](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216594&authkey=%21AAoue9CPiBi3vQ8&width=818&height=549)
- 포인터 변수의 자료형의 크기만큼 가상메모리의 주소를 이동하여 접근한다.
- 포인터에서 +, - 연산으로 포인터를 이동시켜 가르키는 곳의 메모리를 읽거나 참조할 경우, 이 메모리 주소의 데이터가 유효해야만 하니 항상 주의해야 한다!

 <br> 

## 상수 포인터

가리키고 있는 값이 상수이므로, 가리키는 주소를 더이상 바꾸지 못한다.

```c
#include <stdio.h>
int main() {
  int a;
  int b;
  int* const pa = &a; //pa가 가르키는 것은 int형의 a변수의 주소이고 변할 수 없음.

  *pa = 3;  // 올바른 문장: pa변수에 저장된 주소의 값을 3으로 초기화
  pa = &b;  // 올바르지 않은 문장: pa는 const로 선언하였으므로 더이상 바뀔 수 없다.

  return 0;
}
```

 <br> 

## 배열과 포인터

다음과 같이 배열과 포인터를 출력해보면 (포인터처럼)배열은 첫번째 요소의 주소를 출력하는 것을 확인할 수 있다(아래 메서드의 출력 값은 둘 다 동일하다).

```c
#include <stdio.h>
int main() {
  int arr[3] = {1, 2, 3};

  printf("arr 의 정체 : %p \n", arr);
  printf("arr[0] 의 주소값 : %p \n", &arr[0]);

  return 0;
}
```

하지만 둘의 크기를 비교해 보면 차이가 드러나는데,

```c
#include <stdio.h>
int main() {
  int arr[6] = {1, 2, 3, 4, 5, 6};
  int* parr = arr;

  printf("Sizeof(arr) : %d \n", sizeof(arr));
  printf("Sizeof(parr) : %d \n", sizeof(parr));
}
```

이 메서드의 결과 값은 각각 24, 8(32bit 시스템의 경우 4) 인 것을 확인할 수 있다.
배열이 포인터와 같은 주소값을 출력하는 이유는 , sizeof나 &와 사용될 때를 빼면 **배열 이름 사용시 암묵적으로 첫번째 원소를 가리키는 포인터로 타입 변환되기 때문**이다. 그렇다면 다음과 같이 배열의 요소를 접근하기 위한 [] 연산자는 어떤식으로 작동하는 걸까?

```c
/* [] 연산자 */
#include <stdio.h>
int main() {
  int arr[5] = {1, 2, 3, 4, 5};

  printf("a[3] : %d \n", arr[3]);
  printf("*(a+3) : %d \n", *(arr + 3));
  return 0;
}
```

arr[3] 이라 사용한 것은 사실 *(arr + 3) 으로 바뀌어서 처리가 된다. arr 은 + 연산자와 사용되기 때문에(포인터 자료형은 연산이 가능하므로) 첫 번째 원소를 가리키는 포인터 로 변환 되어서 arr + 3 이 포인터 덧셈을 수행하게 된다. 따라서 다음과 같은 사용도 가능하다.

```c
/* 신기한 [] 사용 */
#include <stdio.h>
int main() {
  int arr[5] = {1, 2, 3, 4, 5};

  printf("3[arr] : %d \n", 3 [arr]);
  printf("*(3+a) : %d \n", *(arr + 3));
  return 0;
}
```

3[arr]는 arr 포인터에서 3만큼 떨어진 위치의 값을 의미한다(arr[i] = *(arr + i) 이므로, 3[arr] = *(3 + arr) 이다). 둘은 같은 결과를 가르킨다: 4
 <br> 

## 2 차원 배열의 포인터

![2dpointer](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216607&authkey=%21AAFpFG4JfZJa9vE&width=716&height=601)

실제로는 이런식으로 저장된다. 출처: https://wonit.tistory.com/527

```c
int (*parr)[3];
```

- parr은 포인터
- (*parr)는 parr가 가리키는 것을 의미한다.
- (*parr)[3]은 parr가 가르키는 것이 3개의 int를 가진 배열임을 의미한다.
- 이는 이차원 배열을 함수로 전달하려 할 때 사용될 수 있다.
 
```c
#include <stdio.h>
int main() {
  int arr[2][3] = /*초기화 했다고 치자 중괄호로 초기화 했지만 liquid에서 이걸 자꾸 컴파일 한다.*/;
  int **parr;

  parr = arr;

  printf("parr[1][1] : %d \n", parr[1][1]);  // 버그!

  return 0;
}
```

parr[1][1] = *((parr + 1 ) + 1)과 동일하다. parr은 int**를 가르키는 포인터이고, int*의 크기는 8바이트이므로, 실제 주소값은 8이 증가한다. 다음 그림들을 보면 쉽게 이해할 수 있다.

![2dpointer1](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216609&authkey=%21AGMon2Fh3Y0gzgc&width=1280&height=1063)
![2dpointer2](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216608&authkey=%21AAD63Uk2uS6tFS4&width=325&height=514)

그림으로 보니 이해가 잘 된다.
 
<br> 

## 추가: void pointer

void*

Generics programming은 다양한 자료형을 하나의 메서드로 조작할 수 있게 해주는 프로그래밍이다. C에는 이러한 기능이 없는데, 비슷한 기능으로, void pointer를 사용해 구현할 수 있다: Generics programming은 다른 기회에 자세하게 알아보도록하자.

하지만 void 포인터를 사용하기 위해서는 받은 자료의 포인터를 사용할 자료형에 맞추어 형변환을 해 주어야 하며(즉, 타입 안정성을 보장하지 않는다.) , void 포인터는 포인터 연산을 할 수 없다는 점에서 한계는 명확하다.
 
>Generics programming
>
>C++
>template 프로그래밍을 통해 지원한다. Compile시간에 타입을 결정하며, 런타임 오버헤드가 없다. 하지만 코드가 복잡하고, 컴파일 오류 메세지를 이해하기 어렵다.
>
>Java
>Generics를 지원하며, 컴파일 타임에 타입 안정성을 보장한다. 타입 소거(type erasure) 방식을 사용하므로, 런타임에는 제네릭 타입 정보가 없다: 제한 사항이 있음.
>
>Python
>동적 타이핑 언어로, 모든 변수가 기본적으로 모든 타입의 객체를 가리킬 수 있다. 따라서 아무것도 안해도 자연스럽게 generics 프로그래밍을 가능하게 한다........하하하.... Python3.5이상에서는 선택적 타입 힌트를 제공하여 타입을 명시할 수 있따.
 
<br> 

## 생각해보자

```c
void** ptr = 0x00000000FFFF8392; // 예시로 든 값 입니다.
char* name = (char*) *ptr;
printf("%s", name);
void** ptr2 = 0x00000000FFFF3821; // 마찬가지
int64_t* number = (int64_t*) *ptr2;
printf("%llx", *number);
```

→ *ptr과  *ptr2는 모두 void*형(void 포인터)로, 보이드 포인터를 가르키는 이중 포인터를 역참조하고 있다. 따라서 이를 각각 char 와 int를 가르키는 포인터로 형변환하여 자료를 저장하였다: printf를 적절하게 사용하기 위함.

 <br>

## 연습 문제
![example](https://onedrive.live.com/embed?resid=C4F97B3B64AE3E7A%216606&authkey=%21AO2qkUU-kOu48ao&width=1280&height=897)
이걸 이해할 정도면 충분합니다

- 32 비트 OS: 포인터와 char, int의 크기는 4 바이트.

- a는 int형 데이터를 가르키는 이중 포인터로,  이 변수의 주소는 0X1986abed에 위치하고 있다.

- int  i = 0X1986abed이므로, a의 주소값을 가르키는 16진수 숫자를 가르킨다.

- char* b = 0X0000191a 이므로, char 형 데이터를 가르키는 포인터로 주소값으로 정의되어 있다.

**a =

오른쪽 그림을 통해 a가 가르키는 값은 16임을 확인할 수 있다(이 포인터가 위치한 주소가0X1986abed 이므로, 여기에 위치한 포인터가 가르키는 걸 2번 확인하면 16이 써있는 것을 확인할 수 있다).

a ++ =

여기에서 a에 1을 더하게 되면, 4 바이트(int* 크기는 4바이트 이므로) 4가 증가된 주소값을 가르키며, 이 주소가 가르키는 주소가 무엇을 가르키거나 무슨 데이터가 있는지 모르므로 여기에서 그 값을 알 수는 없다(주소만을 가르킴).

**(int **) i =

int 의 이중 포인터로 형변환된 i의 주소에 있는 값을 확인하고자 하고 있으므로, 16.

printf("%s", b);

b의 값을 string으로 출력하고자 하므로  b에 위치한 글자(4 바이트)들이 /n을 만날때까지 출력할 것이다.

++(++b);

b 에서부터 4 바이트씩 2 번 이동하므로 d에 위치한 주소를 가르키도록 조작한 것이다.

printf("%d", b);

여기에 있는 주소를 10진수 형태로 출력할 것이다.

printf("%s", b);

b가 가르키는 문자열을 출력하므로, d 위치부터 시작하여 4바이트 단위로 ASCII 문자를 출력한다. \n이 나올때까지 문자열 출력을 진행한다.
 
<br> 

피드백은 언제든 감사합니다!
 
## 출처

[https://wonit.tistory.com/527](https://wonit.tistory.com/527)

[모두의 코드님 블로그1](https://modoocode.com/18), [2](https://modoocode.com/23)