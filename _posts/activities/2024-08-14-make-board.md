---
title: "간단한 게시판 만들어보기"
excerpt: "MVC패턴의 적용과 페이지네이션, 대댓글 기능"

toc: true
toc_sticky: true

image: "assets/images/teaser-image.png"
categories:
  - Activity
tags:
  - featured
---

## 구현 이슈

간단한 실습으로 웹페이지 구조를 체화하기: JSP로 간단한 홈페이지를 구현하며 웹 서비스의 구조를 살펴보자.
1. DB 설계 우선
2.  DAO의 구현: 앞서 살펴본 것 처럼, DB 단과의 연결은 코드가 정해져있다.
3. Service: 비즈니스 로직을 수행하는 코드로, DB에서 불러온 데이터를 원하는 자료구조로 받아 가공한다. 이 경우 주요 로직은 페이지네이션...  필요에 따라 구현 방법이 달라지나 지금은 문제해결에만 치중함.
4. 구현할 때에는 이 하위 두 부분에 대한 구현을 우선 진행한다. 이 때, 어느 상황에서든 코드가 동일한 DAO는 DB와 연결을 관리하고, 리소스를 처리, 발생한 예외에 대해 적절한 처리를 하도록 구현하기만 하면 된다. 발생한 예외에 대해서는 DAO단에서만 처리하고 반환값은 적절히 하여 예외가 다른 단계에 영향을 미치지 않도록 해야한다.
5. 서비스 단의 경우, 주로 관심있는 코드들이지만, 자칫 다른 단계와 결합성이 커질 수 있으므로, 단계의 모듈화를 위해 추상화된 인터페이스를 생성한 후, 이를 구현하는 구현체를 작성하는 방식으로 구현한다.
6. View를 담당할 정적인 페이지는 JSP로 생성하는데, JSP로 구현할 경우, controller와 view의 구분이 명확하지 않은 것은 Spring으로 이식하면서 명확히 할 수 있다: 이 때  IoC를 위한 Injection이 쉽게 하기 위해 JSTL로 모든 view의 Java 코드를 jstl 태그로 대체하고 로직을 상단으로 빼버리면 애매모호한 contoller 와 view를 구분하여 추후에 Controller를 분리하기 쉬워진다. 화면의 구현은 다음과 같은 단계로 실행하면 구조적으로 접근할 수 있다.
	1. 화면 설계를 위해서는 먼저 보여줄 html 페이지를 만들어 의도한대로 뿌려지는지 확인한다.
	2. 화면간 이동에 대한 로직을 구성한다.
	3. 정적인 화면이 완성된 상태로 JSP 코드를 추가하여 필요한 데이터를 뿌려주도록 한다.

- 입력에 대한 검증은 DB 단, 서버 단, 클라이언트 단 모두 구현한다. 서버단에서 확인하지 않으면 기본적인 sql inejction, xss같은 공격에 취약하다: preapred statement
- DB에 대한 연결은 커넥트풀을 생성하여 관리하는 것이 일반적이므로 참고하여 구현하자.
- JSTL로 변수들을 전달할 경우 변수의 양이 많아지는 문제는 추후에 DTO를 구현한다.
- 테스트 코드는 모듈 작성 후 바로: 완성되고 나면 mock을 만들어 두면 대체하여 상위 단계를 테스트하기 쉬워진다: 아무리 테스트를 했어도 필요한대로 데이터를 불러오게 할 수 잇으므로.
- DAO는 상태를 가지지 않는 유틸 클래스이므로 보통 singleton으로 생성하는 것이 보편적
### JSTL
- JSTL로 로직을 윗 부분으로 뽑아내 라이브러리가 객체를 전달하도록 하면 view만 (jsp의) body에서 뽑아낼 수 있다.
- 이 경우 JSP로 구현된 View + Controller를 Spring 적용 시 로직 부분을 빼내어 View와 Controller를 분리하기 쉬워진다.

## 다대다 관계
-  1대1 
- 다 대 다: 서로 _1 대 다_ 관계를 맺고있는 관계
- Java 에서는 자식 객체가 생성되어 있지만 db에서는 자식 테이블이 foreign key로 부모 테이블의 id 를 가지고 있다.
- 
게시물을 다루는 페이지에서, 개시글을 다루는 데이터에는 게시글 자체의 id 외에도, 댓글 기능을 구현하기 위한 메타데이터를 구성하여 댓 게시글 기능을 구현하였다. 

```SQL
CREATE TABLE board(
	id int NOT NULL PRIMARY KEY AUTO_INCREMENT,
	title varchar(70),
	date TIMESTAMP DEFAULT CURRENT_TIMESTAMP ,
	content text
) DEFAULT CHARSET=UTF8;
```
초기 게시판 DB 정의

```sql
-- 댓 게시글 기능 추가: 테이블 수정 후 기존 샘플 데이터 업데이트
ALTER TABLE board ADD rootid int;
ALTER TABLE board ADD relevel int;
ALTER TABLE board ADD recnt int;
ALTER TABLE board ADD viewcnt int DEFAULT 0;
```

여기에서 rootId는 원 게시글, relevel은 댓글 수준, recnt는 rootid내에서 댓글의 순서, viewcnt는 조회수를 속성이다.
이 경우, 댓글을 달 때마다 새로운 recnt를 부여하고 삭제시 다시 업데이트 하는 것이 주요 로직이 된다. 따라서 recnt를 순서대로 정렬하여 조회하게 되면 1 ~ (댓글의 수) 가 오름차순으로 조회되는 것이다.
주어진 삽입을 원하는 recnt를 구하기 위해, 
1. 현재 댓글을 달 recnt보다는 큰 것들 중에서,
2. 내 reLevel보다 크거나 같은 recnt 중,
3. 최소의 recnt가 새 댓글이 자리할 recnt이다.
4. 만약 조회를 실패한 경우: 가장 마지막에 댓글을 달면 된다.
이 알고리즘에 대한 코드는 블로그에 업로드 한다.

## 코멘트를 불러오는 DAO

### 원본

```java
List<CommentItem> list = new ArrayList<>();
try { return executeDatabaseOperation(conn -> {
	String query = String.format("SELECT id, date, content, relevel, recnt FROM comment WHERE rootid=? ORDER BY recnt asc;");
	try (PreparedStatement stmt = conn.prepareStatement(query)) 
		{ 
			stmt.setInt(1, rootId); System.out.println(query); 
			ResultSet rset = stmt.executeQuery(query);
			while(rset.next()) { 
				CommentItem newItem = new CommentItem.Builder()
				 .id(rset.getInt("id")) 
				 .content(rset.getString("content")) 
				 .date(rset.getDate("date")) 
				 .reLevel(rset.getInt("relevel")) 
				 .reCnt(rset.getInt("recnt"))
				 .build();
				 list.add(newItem);
				}
			return list;
			} 
		}); 
	} catch (SQLException e) {
	System.out.println(e.getMessage()); 
	return list;
}
```

### 개선
```java
public List<CommentItem> selectComments(int rootId) {
	List<CommentItem> list = new ArrayList<>();
	try {
	    return executeDatabaseOperation(conn -> {
	        String query = "SELECT id, date, content, relevel, recnt FROM comment WHERE rootid = ? ORDER BY recnt ASC";
	        try (PreparedStatement stmt = conn.prepareStatement(query)) {
	            stmt.setInt(1, rootId);
	            try (ResultSet rset = stmt.executeQuery()) {
	                while (rset.next()) {
	                    CommentItem newItem = new CommentItem.Builder()
	                            .id(rset.getInt("id"))
	                            .content(rset.getString("content"))
	                            .date(rset.getDate("date"))
	                            .reLevel(rset.getInt("relevel"))
	                            .reCnt(rset.getInt("recnt"))
	                            .build();
	                    list.add(newItem);
	                }
	            }
	            return list;
	        }
	    });
	} catch (SQLException e) {
	    System.err.println("데이터베이스 쿼리 실행 중 오류 발생: " + e.getMessage());
	    e.printStackTrace();
	    return list; // 예외 발생 시 빈 리스트 반환
	}
}
```

### 설명
- SQL Injection 취약점 해결:
    - 원래 코드에서는 `stmt.executeQuery(query)`를 사용하고 있었는데, 이는 이미 준비된 명령문에 쿼리를 다시 전달하는 것입니다. 대신 `stmt.executeQuery()`를 사용하여 이미 설정된 매개변수로 쿼리를 실행해야 합니다.
- 리소스 관리 개선:
    - `ResultSet`도 `try-with-resources` 구문으로 감싸 자동으로 닫히도록 했습니다.
- 쿼리 문자열 개선:
    - `String.format()`을 사용할 필요가 없어 제거했습니다. 준비된 명령문을 사용하므로 쿼리 문자열에 직접 값을 삽입할 필요가 없습니다.
- 예외 처리 개선:
    - 예외 메시지를 `System.err`로 출력하도록 변경했습니다.
    - 스택 트레이스를 출력하여 디버깅에 도움이 되도록 했습니다.
- 불필요한 출력 제거:
    - 디버깅용으로 보이는 `System.out.println(query);`를 제거했습니다.

