# DDL과 DML

# 학습 목표

SQL에서 DDL과 DML의 차이점을 설명하고, 각각의 대표적인 명령어들의 용도를 설명하세요.

---

# 학습 내용

# SQL 이란?

SQL(Structured Query Language, 구조화된 질의 언어)은 데이터베이스에서 데이터를 저장, 조회, 수정, 삭제할 때 사용하고, 데이터베이스 자체의 성능 유지 관리, 최적화에 사용되는 언어이다

데이터베이스를 다루기 위해 쓰는 언어라고 할 수 있다.

SQL의 문법은 DDL, DML, DCL, TCL로 나눌 수 있다.

---

# DDL(Data Definition Language, 데이터 정의어)

> 데이터베이스 구조 정의에 사용하는 언어로, 테이블이나 컬럼 등을 생성, 수정, 삭제한다.
데이터베이스의 전체 골격을 구성하는 역할
> 

## 종류와 역할

### CREATE

- 새로운 데이터베이스 객체(테이블, 인덱스 등)을 생성

### ALTER

- 데이터베이스 객체 수정 (컬럼 추가, 변경 등)

### DROP

- 데이터베이스 객체 삭제

### TRUNCATE

- 기존 테이블 모든 데이터 삭제 (구조는 유지, 초기화)

## 예시 (MySQL & PostgreSQL)

```sql
-- 데이터베이스 생성
CREATE DATABASE mydb;

-- 테이블 생성
CREATE TABLE users (
    id SERIAL PRIMARY KEY,  -- PostgreSQL: SERIAL (자동 증가)
    name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18)  -- 나이가 18 이상만 가능
);

-- 테이블 구조 변경 (컬럼 추가)
ALTER TABLE users ADD COLUMN email VARCHAR(100);

-- 테이블 삭제
DROP TABLE users;

-- 테이블 데이터 초기화
TRUNCATE TABLE users;

-----------------------------------------------------------------
-- MySQL 과 PostgreSQL 차이점
-- MySQL
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18)  -- MySQL 8.0 이상에서만 CHECK 제약 조건 지원
);

-- PostgreSQL
CREATE TABLE users (
    id SERIAL PRIMARY KEY, -- PostgreSQL에서 AUTO_INCREMENT 역할
    name VARCHAR(50) NOT NULL,
    age INT CHECK (age >= 18) -- 모든 버전에서 CHECK 지원
);

-- MySQL 은 AUTO_INCREMENT, PostgreSQL 은 SERIAL 사용
-- MySQL 5.x 에서는 CHECK 제약 조건이 무시됨 (8.0 이상에서는 지원)
```

---

# DML(Data Manipulation Language, 데이터 조작 언어)

> 데이터 조작에 사용되는 언어로, 테이블의 데이터를 조회, 저장, 수정, 삭제 한다.
데이터베이스 내부에 실제로 저장된 데이터들을 다루는 역할
> 

## 종류와 역할

### SELECT

- 저장된 데이터를 조회
- SELECT는 DML에 포함하지 않고 DQL(Data Query Language)로 분류하는 경우도 있음

### INSERT

- 새로운 데이터를 저장

### UPDATE

- 저장된 데이터를 수정

### DELETE

- 저장된 데이터를 삭제

## 예시 (MySQL & PostgreSQL)

```sql
-- 데이터 삽입
INSERT INTO users (name, age, email) VALUES ('박태식', 30, 'taesik@example.com');

-- 데이터 조회
SELECT * FROM users WHERE age >= 18;

-- 데이터 수정
UPDATE users SET email = 'newemail@example.com' WHERE name = '박태식';

-- 데이터 삭제
DELETE FROM users WHERE name = '박태식';

-----------------------------------------------------------------
-- MySQL 과 PostgreSQL 차이점
-- MySQL
INSERT INTO users (name, age) VALUES ('박태식', 30);

-- PostgreSQL
INSERT INTO users (name, age) VALUES ('박태식', 30) RETURNING id;

-- PostgreSQL은 RETURNING을 사용해 자동 생성된 id 값을 반환할 수 있음
-- MySQL에서는 LAST_INSERT_ID()를 사용해야 함
```

---

# DCL(Data Control Language, 데이터 제어어)

> 데이터베이스에 대한 접근 제어로 사용하는 언어로, 각종 권한을 부여, 회수한다
권한 관리를 통해 시스템 보안을 유지하는 역할을 한다
> 

## 종류와 역할

### GRANT

- 특정 사용자에게 권한을 부여

### REVOKE

- 특정 사용자의 권한을 회수

## 예시 (MySQL & PostgreSQL)

```sql
-- 사용자에게 테이블 접근 권한 부여
GRANT SELECT, INSERT ON users TO 'user1'@'localhost';

-- 사용자 권한 회수
REVOKE INSERT ON users FROM 'user1'@'localhost';

-----------------------------------------------------------------
-- MySQL 과 PostgreSQL 차이점
-- MySQL
GRANT SELECT, INSERT ON users TO 'user1'@'localhost';

-- PostgreSQL
GRANT SELECT, INSERT ON users TO user1;

-- MySQL에서는 'user1'@'localhost' 형태로 사용자 지정
-- PostgreSQL은 user1처럼 간단히 사용
```

---

# TCL(Transaction Control Language, 트랜잭션 제어어)

> DCL에서 트랜잭션을 컨트롤하는 명령어를 TCL로 분류한다.
TCL  개념을 사용하지 않고 DCL 로 분류하는 경우도 있다.
> 

## 종류와 역할

### COMMIT

- 변경사항 저장
- 올바르게 완료한 작업으로 인한 데이터를 데이터베이스에 영구적으로 반영

### ROLLBACK

- 변경사항 취소
- 작업 시작 이전의 상태로 되돌림

### SAVEPOINT

- 특정 시점 저장 후 롤백 가능
- 저장점을 지정, 이후 ROLLBACK과 함께 사용하여 특정 지점까지 ROLLBACK이 가능

## 예시(MySQL & PostgreSQL)

```sql
-- 트랜잭션 시작
START TRANSACTION;

-- 데이터 삽입
INSERT INTO users (name, age, email) VALUES ('김철수', 25, 'chulsoo@example.com');

-- 트랜잭션 저장 (확정)
COMMIT;

-- 트랜잭션 시작
START TRANSACTION;

-- 데이터 수정
UPDATE users SET age = 40 WHERE name = '김철수';

-- 변경사항 취소
ROLLBACK;

-----------------------------------------------------------------
-- MySQL 과 PostgreSQL 차이점
START TRANSACTION;
UPDATE users SET age = 40 WHERE name = '김철수';
ROLLBACK;  -- 변경사항 취소

-- MySQL에서는 START TRANSACTION;을 사용해야 명시적으로 트랜잭션 시작
-- PostgreSQL은 BEGIN;으로도 시작 가능
```

---

# DDL 과 DML 차이점

| 구분 | DDL (Data Definition Language) | DML (Data Manipulation Language) |
| --- | --- | --- |
| 설명 | 데이터베이스의 구조(스키마, 테이블, 인덱스 등)를 정의하고 변경하는 명령어 | 테이블의 데이터를 삽입, 수정, 삭제, 조회하는 명령어 |
| 주요 명령어 | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` | `INSERT`, `UPDATE`, `DELETE`, `SELECT` |
| 영향 범위 | 테이블, 데이터베이스 자체를 변경 | 테이블의 개별 데이터 변경 |
| 트랜잭션 영향 | 일반적으로 트랜잭션을 지원하지 않음 (자동 반영됨) | 트랜잭션을 지원 (`COMMIT`, `ROLLBACK` 가능) |
| ROLLBACK 가능 여부 | 불가능 (일반적으로 즉시 적용) | 가능 (`ROLLBACK`을 통해 취소 가능) |

## 주요 차이점 요약

1. DDL은 데이터 구조를 변경하지만, DML은 데이터를 조작한다.
2. ROLLBACK
    1. DDL은 즉시 적용(자동 커밋)되며 ROLLBACK 불가
    2. DML은 트랜잭션 지원으로 ROLLBACK 가능
3. 단위 변경
    1. DDL은 테이블 단위의 변경
    2. DML은 개별 행(데이터) 단위의 변경

  ---
  ---

# 역정규화

# 학습 목표

역정규화가 필요한 상황과 적용 시 고려해야 할 사항, 그리고 역정규화를 적용할 때의 장단점을 설명해주세요.

---

# 학습 내용

# 역정규화(De-normalization)란?

역정규화는 정규화된 데이터베이스의 성능을 향상시키기 위해 중복을 허용하면서 일부 정규화를 해제하는 과정이다.

JOIN 연산을 줄이고 읽기 성능을 개선하기 위해 데이터를 중복 저장하거나 테이블을 합치는 작업을 의미

---

# 역정규화가 필요한 상황

역정규화는 주로 읽기(Read) 성능이 중요한 경우 또는 JOIN 연산이 과도한 경우에 사용

## 1) JOIN이 너무 많아 성능 저하가 발생하는 경우

- 정규화된 데이터베이스는 JOIN 연산이 많아질 수 밖에 없고, 이는 성능을 저하시킬 수 있음
- 특히 대량의 데이터를 처리하는 시스템에서는 JOIN이 많아지면 쿼리 실행 속도가 느려짐

### 예시

- 사용자 테이블과 주문 테이블이 분리되어 있을 때, 사용자 정보와 주문 내역을 조회하려면 JOIN이 필요
- 이를 해결하기 위해 주문 테이블에 사용자 정보를 추가(중복 저장) 하면 JOIN 없이 조회 가능

## 2) 데이터 읽기(Read) 성능이 중요한 경우

- 읽기 요청이 많고 쓰기 요청이 적은 경우
- JOIN을 줄이고 조회 성능을 높이기 위해 일부 데이터를 중복 저장할 수 있음

### 예시

- 게시판 시스템에서 글 목록을 조회할 때, 게시글 테이블과 사용자 테이블을 JOIN하면 느려짐
- 게시글 테이블에 작성자 이름과 프로필 정보를 중복 저장하여 조회 속도를 향상시킬 수 있음

## 3) 데이터 변경이 자주 발생하지 않는 경우

- 중복된 데이터가 있으면 데이터 변경(UPDATE)이 어렵지만, 변경이 거의 없는 경우에는 큰 문제가 되지 않음
- 조회 속도를 위해 역정규화를 적용할 수 있음

### 예시

- 제품 테이블과 카테고리 테이블이 분리되어 있을 때, 제품 조회 시 카테고리 정보를 JOIN해야 함
- 카테고리 변경이 자주 발생하지 않는다면 제품 테이블에 카테고리 정보를 중복 저장하여 조회 성능을 높일 수 있음

## 4) 실시간 분석 및 대시보드 시스템에서 사용

- 분석 시스템은 대량의 데이터를 빠르게 조회해야 하므로 역정규화를 적극 활용
- 사전 계산된 데이터를 포함하는 테이블을 생성하여 빠른 조회를 가능하게 함

### 예시

- 매출 내역과 제품 정보를 JOIN해서 분석해야 하는 경우
- 매출 분석 테이블을 만들어 `매출 금액, 제품명, 카테고리` 등의 정보를 포함할 수 있음

---

# 역정규화 적용 시 고려해야할 사항

역정규화를 적용할 때는 데이터 중복으로 인한 문제와 유지보수 비용을 고려해야 함

## 1) 데이터 일관성 유지 문제

- 동일한 데이터가 여러 테이블에 존재하면 데이터 변경 시 모든 테이블을 수정해야 함
- 데이터 동기화가 잘못되면 데이터 불일치 문제가 발생할 수 있음

### 해결책

- `트리거`, `배치 작업`, `캐시 사용` 등으로 동기화 관리

## 2) 저장 공간 증가

- 데이터를 중복 저장하면 디스크 사용량이 증가하여 저장 비용이 늘어날 수 있음

### 해결책

- 정말 필요한 데이터만 중복 저장하고, 인덱스를 적절히 활용

## 3) 데이터 수정(쓰기) 비용 증가

- 중복된 데이터를 여러 곳에서 업데이트해야 하므로 쓰기(INSERT, UPDATE, DELETE) 성능이 저하됨

### 해결책

- 읽기 성능이 중요한 경우에만 적용
- 데이터 변경이 적은 테이블을 대상으로 역정규화 수행

## 4) 유지보수 어려움

- 정규화된 테이블보다 구조가 복잡해질 수 있음
- 역정규화된 테이블이 많아지면 개발 및 유지보수가 어려워질 수 있음

### 해결책

- 주기적으로 데이터 정리 및 문서화를 통해 관리

---

# 역정규화의 장단점

## 장점

### 1. 조회 성능 향상(JOIN 감소)

- JOIN 없이 데이터를 조회할 수 있어 쿼리 속도가 빨라짐

### 2. CPU와 메모리 사용량 감소

- JOIN 연산이 줄어들어 서버 부하가 줄어듬

### 3. 데이터 분석 및 리포트 속도 개선

- 미리 계산된 데이터를 저장하면 실시간 분석이 쉬워짐

### 4. 캐싱 효과 증가

- 조회할 데이터를 한 테이블에서 가져올 수 있어 캐시 활용도가 높아짐

## 단점

### 1. 데이터 중복으로 인한 일관성 문제

- 변경 시 여러 테이블을 수정해야 함 (데이터 불일치 가능성 증가)

### 2. 쓰기(INSERT, UPDATE, DELETE) 성능 저하

- 중복된 데이터를 관리해야 하므로 쓰기 작업이 많아질 수 있음

### 3. 저장 공간 낭비

- 데이터 중복으로 인해 디스크 사용량 증가

### 4. 유지보수 어려움

- 데이터 구조가 복잡해져 관리 및 수정이 어려워질 수 있음

---

# 결론

## 언제 역정규화를 적용?

- 읽기 성능이 중요한 경우 (JOIN 이 많이 속도가 느려질 때)
- 데이터 변경이 적은 경우 (자주 변경되는 데이터에는 신중하게 적용)
- 실시간 분석 시스템 (사전 계산된 데이터를 활용)
- 트래픽이 높은 서비스 (읽기 성능 최적화 필요)

## 고려사항

- 데이터 일관성 문제와 유지보수 비용을 고려해야 함
- 역정규화는 신중하게 적용하고, 꼭 필요한 경우에만 사용!
