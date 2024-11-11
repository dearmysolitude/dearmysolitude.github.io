---
title: "JPA: Base Time Entity의 사용"
excerpt: "JPA로 데이터를 넣을 때 왠만하면 필요한 보일러플레이트"

categories:
  - Java
tags:
  - BaseTimeEntity
---
## BaseTimeEntity

모든 Entity의 상위 클래스에서 createdDate, updateDate를 자동으로 관리해 줌.

Date자료형보다 LocalDate, LocalDateTime을 사용할 것을 추천.

BaseTimeEntity 추상클래스를 구현하고 Entity 클래스들에게 상속시켜 사용한다.

### BaseTimeEntity 추상클래스

```java
@Getter
@MappedSuperclass
@EntityListeners(AuditingEntityListener.class)  // Auditing 기능 포함
public abstract class BaseTimeEntity {

    @CreatedDate
    @Column(updatable = false)
    private LocalDateTime createdDate;

    @LastModifiedDate
    private LocalDateTime modifiedDate;
}
```

`@MappedSuperclass`: JPA Entity 클래스들이 BaseTimeEntity를 상속할 경우 createdDate, modifiedDate 두 필드도 컬럼으로 인식하도록 설정

`@CreatedDate`: 생성시 날짜 자동 생성

`@LastModifiedDate`: 수정시 날짜 자동 갱신

### 다른 Entity에서 사용할 때에는 다음과 같이 상속하여 사용

```java
public class Members extends BaseTimeEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(length = 20, nullable = false)
    private String username;

    @Column(nullable = false)
    private String password;

    @Column(length = 10)
    private String name;

    @Enumerated(EnumType.STRING)
    private MembersRole role;
}
```

### 메인클래스에서 JPA Auditing을 활성화 해야한다

```java
@EnableJpaAuditing // JPA Auditing 활성화
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

## 참고 자료

[https://europani.github.io/spring/2021/10/05/027-baseTimeEntity.html](https://europani.github.io/spring/2021/10/05/027-baseTimeEntity.html)