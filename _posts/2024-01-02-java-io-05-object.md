---
title: "자바 I/O - 5"
excerpt: "ObjectInputStream, ObjectOutputStream 그리고 객체 직렬화"

toc: true
toc_sticky: true

categories:
  - Java
tags:
  - Java I/O
  - Serializable
---
## ObjectInputStream, ObjectOutputStream

- **직렬화 가능한 대상만**을 읽고 쓸 수 있다.
- 직렬화 가능한 대상은 _기본형 타입_ or _java.io.Serializable 인터페이스를 구현하고 있는 객체_ 이다.

> 마크 인터페이스
> 메서드가 하나도 없는 것이 특징.
> 표시해주는 역할. Serializable 인터페이스는 이러한 마크 인터페이스의 한 종류다.

## 직렬화

고전 영화: The Fly, 물체 전송기

객체Object를 쓴다. -> Byte의 흐름으로 바꾼다(직렬화). -> 전송 -> 객체Object로 변환(역직렬화).

```java
public class User implements Serializable{
    private String email;
    private String name;
    private int birthYear;

    public User(String email, String name, int birthYear) {
        this.email = email;
        this.name = name;
        this.birthYear = birthYear;
    }

    // Getter / toString 메서드 생략
    // ...
}
```

- User Class의 인스턴스를 직렬화하려고 한다.
- User의 필드인 String과 int 모두 직렬화가 가능함: String 클래스를 확인해보자.

### 객체 쓰기

```java
public class ObjectOutputExample1 {
    public static void main (String[] args) throws Execption{
        User user = new User("a@example.com", "a", 1994);
        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("/tmp/user.dat"));
        out.wirteObject(user);
        out.close();
    }
}
```

### 객체 읽기

```java
public class ObjectInputExam1 {
    public static void main(String[] args) throws Execption{
        ObjectInputStream in = new ObjectInputStream(new FileInputStream("tmp/user.dat"));
        User user = (User)in.readObject(); // <-
        in.close(); 
        System.out.println(user);
    }
}
```

- 원래 형태대로 형변환
- 파일을 Object로 읽어 User로 형변환

### 

```java
public class ObjectOutputExample2 {
    public static void main(String[] args) throws Execption{
        User user1 = new User("a@example.com", "a", 1994);
        User user2 = new User("b@example.com", "b", 1992);
        User user3 = new User("c@example.com", "c", 1996);

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        list.add(user3);

        ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("/tmp/userlist.dat"));
        out.wirteObject(list);
        out.close();
    }
}
```

- ArrayList도 Serializable을 구현한 것
- 역시 직렬화가 가능하여 이같이 파일을 쓰는 것이 가능하다.

```java
public class ObjectInputExam2 {
    public static void main(String[] args) throws Execption{
        ObjectInputStream in = new ObjectInputStream(new FileInputStream("tmp/userlist.dat"));
        ArrayList<User> userlist = (ArrayList)in.readObject(); // <-
        in.close();
        for(int i = 0; i < list.size(); i++) {
            System.out.println(userlist.get(i));
        }
    }
}
```

## 깊은 복사를 위한 객체직렬화

```java
public class ObjectInputOutExam {
    public static void main(String[] args) throws Execption{
        User user1 = new User("a@example.com", "a", 1994);
        User user2 = new User("b@example.com", "b", 1992);
        User user3 = new User("c@example.com", "c", 1996);

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        list.add(user3);

        ArrayList<User> list2 = list;

        for(int i = 0; i < list2.size(); i++) {
            System.out.println(list2.get(i));
        }  
    }
}
```

- User 객체 3개가 생성되고 list의 각 요소는 각 User 객체를 참조한다.
- list2는 list와 **동일한 객체**를 참조한다.
- 즉 `ArrayList list2 = list;`는 복사가 아니다.
- 다음 코드를 살펴보자.

```java
public class ObjectInputOutExam {
    public static void main(String[] args) throws Execption{
        User user1 = new User("a@example.com", "a", 1994);
        User user2 = new User("b@example.com", "b", 1992);
        User user3 = new User("c@example.com", "c", 1996);

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        list.add(user3);

        ArrayList<User> list2 = new ArrayList<>();

        for(int i = 0; i < list2.size(); i++) {
            list2.add(list.get(i));
        }
    }
}
```

- list2는 list의 각 User를 참조하는 별개의 객체가 된다.
- list의 User 정보를 변경하게 되면 list2를 통하여 사용하는 정보들도 변경된다. → 얕은 복사
- 깊은 복사를 하기 위해서는 어떻게 해야할까?

```java
public class ObjectInputOutExam {
    public static void main(String[] args) throws Execption{
        User user1 = new User("a@example.com", "a", 1994);
        User user2 = new User("b@example.com", "b", 1992);
        User user3 = new User("c@example.com", "c", 1996);

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        list.add(user3);

        // =================== 직렬화 시작
        ByteArrayOutputStream bout = new ByteArrayoutputStream();
        ObjectOutputStream out = new ObjectOutputStream(bout);

        out.writeObject(list);

        out.close();
        bout.close();

        byte[] array = bout.toByteArray();

        ObjectInputStream in = new ObjectInputStream(new ByteArrayInputStream(array));
        ArrayList<User> list2 = (ArrayList)in.readOjbect();
        in.close();
        // ==================== 직렬화 끝

        for(int i = 0; i < list2.size(); i++) {
            System.out.println(list2.get(i));
        }
    }
}
```

그럼 다음과 같이 메서드 추출할 수 있다.
```java
public class ObjectInputOutExam {
    public static void main(String[] args) throws Execption{
        User user1 = new User("a@example.com", "a", 1994);
        User user2 = new User("b@example.com", "b", 1992);
        User user3 = new User("c@example.com", "c", 1996);

        ArrayList<User> list = new ArrayList<>();
        list.add(user1);
        list.add(user2);
        list.add(user3);

        ArrayList<User> list2 = copy(list);

        for(int i = 0; i < list2.size(); i++) {
            System.out.println(list2.get(i));
        }
    }
    private static Arraylist<User> copu(ArrayList<User> list) throws IOException, ClassNotFoundException {
        ByteArrayOutputStream bout = new ByteArrayoutputStream();
        ObjectOutputStream out = new ObjectOutputStream(bout);

        out.writeObject(list);

        out.close();
        bout.close();

        byte[] array = bout.toByteArray();

        ObjectInputStream in = new ObjectInputStream(new ByteArrayInputStream(array));
        ArrayList<User> list2 = (ArrayList)in.readOjbect();
        in.close();

        return list2;
    }
}
```

- `writeObject()`를 통해 list의 Serializable한 객체 모두 byte[]로 변환하여 저장
- ByteArrayInputStream이 array로부터 이 byte[]를 읽는다.
- 결과적으로 새롭게 복제된 list2가 생성된다.
- 깊은 복사가 발생