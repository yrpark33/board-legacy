# 게시판 웹 애플리케이션 프로젝트

## 프로젝트 개요
Spring Legacy + MyBatis 기반으로 게시판 웹 애플리케이션을 구현한 프로젝트입니다.  
게시글 관리, 검색 및 페이징, 댓글(AJAX), 파일 업로드 기능을 구현하며 웹 애플리케이션의 기본적인 동작 구조와 데이터 처리 흐름을 이해하는 것을 목표로 진행했습니다.

---

## 기술 스택

### Backend
- Java
- Spring MVC
- MyBatis

### Frontend
- JSP
- HTML
- CSS
- JavaScript
- jQuery

### Database
- MariaDB

### Server
- Apache Tomcat

### Build Tool
- Maven

---

## 주요 기능

- 게시글 등록 / 수정 / 삭제 / 조회 (CRUD)
- 게시글 검색 및 페이징 처리
- 댓글 등록 및 조회 (jQuery AJAX 비동기 처리)
- 파일 업로드 (서버 저장 경로 관리, 예외 처리 구현)
- 상품 등록 / 수정 / 삭제 / 조회 및 상품 이미지 관리

---

## 프로젝트 구조

웹 요청 처리 흐름을 이해하기 위해 다음과 같은 계층 구조로 구현했습니다.

```
src/main/java
└── org/oolong
    ├── controller
    │   ├── BoardController.java
    │   ├── ProductController.java
    │   └── ReplyController.java
    ├── service
    │   ├── BoardService.java
    │   ├── FileService.java
    │   ├── ProductService.java
    │   └── ReplyService.java
    ├── mapper
    │   ├── BoardMapper.java
    │   ├── ProductMapper.java
    │   └── ReplyMapper.java
    └── dto
        ├── BoardDTO.java
        ├── ProductDTO.java
        ├── ReplyDTO.java
        └── ...
```
각 계층의 역할을 분리하여 유지보수성과 코드 구조를 고려하여 개발했습니다.

---

## 데이터베이스 구조 (ERD)

### Board (게시글)
- bno (PK)
- title
- content
- writer
- regdate
- updatedate
- delflag

### Reply (댓글)
- rno (PK)
- bno (FK)
- replytext
- replier
- regdate
- updatedate
- delflag

### Product (상품)
- pno (PK)
- pname
- pdesc
- price
- sale
- writer
- regdate
- moddate

### ProductImage (상품이미지)
- ino (PK)
- pno (FK)
- uuid
- filename
- ord
- regdate


### 테이블 관계
Board (1) ───── (N) Reply <br>
Product (1) ───── (N) ProductImage <br>

---

## 주요 구현 경험

- Controller–Service–Mapper 구조를 직접 설계하며 웹 애플리케이션의 요청 처리 흐름을 이해했습니다.
- MyBatis를 활용하여 SQL을 작성하고 데이터 CRUD 기능을 구현했습니다.
- 게시글 검색 및 페이징 기능을 구현하며 데이터 조회 로직을 설계했습니다.
- jQuery AJAX를 활용하여 비동기 방식으로 페이지 새로고침 없이 댓글 등록 및 조회가 가능하도록 처리했습니다.
- 파일 업로드 기능 구현 과정에서 서버 저장 경로 관리와 예외 처리 로직을 점검하며 오류를 해결했습니다.
- 로그를 통해 오류 원인을 추적하고 문제를 분석하는 경험을 했습니다.

---

## 문제 해결 경험

파일 업로드 기능을 구현하는 과정에서 데이터베이스와 물리적 파일 저장소 간 데이터 일관성 문제를 고려했습니다.

데이터베이스는 트랜잭션을 통해 작업 실패 시 롤백이 가능하지만, 물리적인 파일 저장소는 트랜잭션 관리 대상이 아니기 때문에 파일 저장과 데이터베이스 저장 과정 중 하나만 실패할 경우 데이터 불일치가 발생할 수 있습니다.

예를 들어 파일 저장은 성공했지만 데이터베이스 저장이 실패하여 트랜잭션이 롤백되는 경우 서버에는 불필요한 파일이 남을 수 있고, 반대로 삭제 과정에서 파일은 삭제되었지만 데이터베이스 작업이 롤백될 경우 데이터베이스에는 파일 정보가 존재하지만 실제 파일은 없는 상황이 발생할 수 있습니다.

이를 고려하여 파일 저장 이후 데이터베이스 저장을 수행하도록 처리하고, 데이터베이스 작업 실패 시 업로드된 파일을 삭제하는 예외 처리 로직을 구현하여 데이터 불일치 가능성을 최소화했습니다.

---

