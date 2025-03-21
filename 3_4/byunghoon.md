

![image (1)](https://github.com/user-attachments/assets/a7c9711f-8633-46e6-ad0c-43ca77228626)


- 참고 자료
    - DCL(Data Control Language) : 데이터베이스 시스템의 접근 권한(Privilege)과 보안(Security)을 제어하기 위한 명령어
    - TCL (Transaction Control Language) : TCL은 트랜잭션(Transaction)을 제어하기 위한 명령어, 즉, 여러 개의 DML 명령어를 하나의 논리적인 작업 단위로 묶어 처리할 수 있도록 하며, 이 작업을 **원자적**(Atomic)으로 다루는 것을 가능하게 함.

---

## ❗DDL vs DML 비교표

| 구분 | DDL (Data Definition) | DML (Data Manipulation) |
| --- | --- | --- |
| **역할** | 데이터베이스 **구조** 정의/수정/삭제 | 데이터 **삽입/조회/수정/삭제** 등 실제 데이터 조작 |
| **명령어** | CREATE, DROP, ALTER, TRUNCATE 등 | SELECT, INSERT, UPDATE, DELETE |
| **트랜잭션** | 일반적으로 **자동 커밋** (롤백 불가) | **롤백, 커밋** 가능 |
| **영향 범위** | 테이블 스키마, 인덱스, 뷰 등 **객체 레벨** | **레코드(행) 레벨** |
| **사용 목적** | **스키마 설계/변경**에 집중 (데이터 구조 변경) | **데이터 조작**에 집중 (CRUD 작업) |

## 1. DDL (Data Definition Language)

- **개념**
    
    데이터베이스의 구조(스키마)를 정의하거나 수정할 때 사용하는 언어를 말함.  테이블, 인덱스, 뷰 등 객체의 생성, 삭제, 변경 등을 수행
    
    - 스키마
        - 스키마란 데이터베이스의 구조와 제약조건에 관해 전반적인 명세를 기술한 것
        - 개체의 특성을 나타내는 속성(Attribute) 과 속성들의 집합으로 이루어진 개체(Entity),개체 사이에 존재하는 관계(Relation)에 대한 정의와이들이 유지해야 할 제약조건들을 기술한 것
        - 쉽게 정리하여, DB내에 어떤 구조로 데이터가 저장되는가를 나타내는 데이터베이스 구조를 스키마라고 함.
        
        https://jwprogramming.tistory.com/47
        
- **대표 명령어**
    1. **`CREATE`**
        - **용도**: 데이터베이스 객체(테이블, 뷰, 인덱스 등)를 새롭게 생성할 때 사용
        - **예시**:
            
            ```sql
            CREATE TABLE Employees (
              id INT PRIMARY KEY,
              name VARCHAR(100),
              position VARCHAR(100)
            );
            
            ```
            
    2. **`DROP`**
        - **용도**: 이미 존재하는 데이터베이스 객체를 완전히 삭제할 때 사용
        - **예시**:
            
            ```sql
            DROP TABLE Employees;
            
            ```
            
    3. **`ALTER`**
        - **용도**: 기존에 생성된 테이블 구조 등의 스키마를 변경할 때 사용 (컬럼 추가/삭제, 컬럼 타입 변경 등
        - **예시**:
            
            ```sql
            ALTER TABLE Employees
            ADD COLUMN salary DECIMAL(10,2);
            
            ```
            
    4. `TURNCATE`
        - 용도: **테이블의 모든 데이터를 빠르게 삭제**할 때 사용하는 **DDL** 명령어. 일반적인 `DELETE` 문과 달리 **테이블 구조(스키마)는 유지**하면서, 해당 테이블 안의 모든 레코드를 즉시 제거
            
            ```sql
            TRUNCATE TABLE Employees;
            ```
            
            - 주의) 위 명령어를 실행하면 `Employees` 테이블의 **모든 행**이 삭제되며,  `DELETE`는 DML(Data Manipulation Language)에 속하지만, `TRUNCATE`는 DDL로 분류됨. 따라서 `TRUNCATE`를 실행하면 **자동으로 COMMIT**되어 롤백이 되지 않습니다.
            

---

## 2. DML (Data Manipulation Language)

- **개념**
    
    데이터베이스 내부의 실제 데이터(행, 튜플)를 조회하거나 삽입, 수정, 삭제할 때 사용하는 언어
    
- **대표 명령어**
    1. **`SELECT`**
        - **용도**: 테이블(또는 여러 테이블)에서 데이터를 조회할 때 사용
        - **예시**:
            
            ```sql
            SELECT * FROM Employees;
            
            ```
            
    2. **`INSERT`**
        - **용도**: 새로운 데이터를 테이블에 삽입할 때 사용
        - **예시**:
            
            ```sql
            INSERT INTO Employees (id, name, position, salary)
            VALUES (1, 'Alice', 'Developer', 3000.00);
            
            ```
            
    3. **`UPDATE`**
        - **용도**: 기존 데이터의 값을 수정할 때 사용
        - **예시**:
            
            ```sql
            UPDATE Employees
            SET salary = 3500.00
            WHERE id = 1;
            
            ```
            
    4. **`DELETE`**
        - **용도**: 기존 데이터를 테이블에서 삭제할 때 사용
        - **예시**:
            
            ```sql
            DELETE FROM Employees
            WHERE id = 1;
            
            ```
## 1. 역정규화(De-normalization)란?

- **정의**
역정규화는 **정규화(Normalization)를 통해 잘 분리된 테이블을 다시 통합하거나, 중복 데이터를 의도적으로 허용**하여 **데이터 조회 성능**을 높이는 기법
- **배경**
일반적으로 정규화된 데이터베이스는 **데이터 무결성**을 높이고 **중복**을 최소화하지만, **JOIN 연산**이 자주 발생하거나 **쿼리 속도**가 중요한 시점에서 성능 문제가 생길 수 있음. 이 문제를 해결하기 위해 역정규화를 적용하는 경우가 있음.

---

## 2. 간단하게 보는 정규화 비정규화 예시

![image](https://github.com/user-attachments/assets/387c4907-ceb1-4fd5-bd32-e2d7b9387e07)


→ 결론 부터 말하면 정규화 상태를 **비정규화 상태로 만드는 역정규화 프로세스**는 데이터베이스의 **완벽한 구조설계를 포기**하고 데이터의 **무결성을 떨어트리는** 대신 관계형 데이터베이스의 **읽기(Read)성능 향상을 위한 설계** 방법이다.

예를 들어, 1교시 선생님에 대한 정보(강의, 수강료 등)에 대해 조회 시 

 정규화 테이블에서는 3가지 테이블을 같은 키로 JOIN한 뒤 데이터 값을 찾는다. 만약 테이블 개수가 엄청 많거나 테이블 안에 데이터가 엄청 많으면 JOIN시간이 훨씬 늘어나 **데이터 읽기 효율성**이 떨어진다.

 비정규화 테이블에서는 OIN을 할 필요가 없어 데이터 **읽기(조회)시간이 단축**된다.

https://chankim.tistory.com/m/7

---

## 3. 역정규화가 필요한 상황

1. **JOIN이 과도하게 발생하는 경우**
    - 여러 테이블을 조인하는 과정에서 쿼리 속도가 크게 저하될 때
    - 실시간성이 중요한 서비스(예: 대규모 트래픽의 웹서비스)에서 JOIN으로 인한 지연이 발생할 때
2. **조회(Read) 횟수가 압도적으로 많은 경우**
    - 데이터 쓰기보다 읽기 요청이 훨씬 많아서, 조회 성능 최적화가 중요할 때
    - 예: 게시판, 통계 대시보드, 트래픽 로그 분석 등
3. **빠른 응답 시간(Performance)이 가장 우선일 때**
    - 일정 수준의 중복 데이터 및 무결성 저하를 감수하더라도 쿼리 결과를 빠르게 반환해야 할 때
    - 예: 캐시 테이블을 따로 만들어서 속도 향상
4. **데이터 중복이나 무결성보다 확실한 성능 최적화가 필요한 경우**
    - 비즈니스 요구사항상 어느 정도 데이터 불일치 가능성을 용인하고, 더 나은 사용자 경험을 원하는 경우

---

## 3. 역정규화 적용 시 고려해야 할 사항

1. **데이터 무결성(Consistency) 문제**
    - 데이터를 중복 저장하면, 일부만 업데이트되어 **데이터가 일관되지 않을 위험**이 증가
    - 무결성을 유지하기 위한 **트랜잭션 처리**나 **애플리케이션 로직 보완**이 필요
2. **변경(UPDATE/DELETE) 로직 복잡성 증가**
    - 한 곳에서 데이터가 변경될 때, **중복된 모든 위치**를 업데이트해야 할 수 있음
    - 업데이트 빈도가 잦다면, 역정규화로 인한 이점보다 관리 부담이 더 클 수 있음
3. **적절한 적용 범위 설정**
    - 서비스에서 가장 **병목이 되는 쿼리나 기능**에만 역정규화를 적용하는 것이 일반적임
    - 무분별한 역정규화는 오히려 유지보수를 어렵게 만듦

---

## 4. 역정규화의 장단점

### 장점

1. **조회 성능 향상**
    - JOIN 연산을 줄이고, 필요한 컬럼을 한 테이블에 모아두어 **조회 시간이 단축**
2. **복잡한 쿼리 단순화**
    - 여러 테이블을 합치는 로직을 줄이거나 없애 **쿼리가 단순**해짐
3. **대규모 트래픽에 대한 대응력 향상**
    - 실시간 처리가 중요한 부분에 적용하면, **사용자 체감 속도**가 크게 개선

### 단점

1. **데이터 무결성 유지가 어려움**
    - 중복된 데이터가 여러 위치에 존재하므로, **동기화**가 필요하고 관리가 복잡해짐
2. **저장공간 증가**
    - 중복 데이터로 인해 테이블(컬럼) 수와 데이터 양이 커져, **스토리지 비용**이 증가
3.  **읽기(조회)속도는 빨라지지만 쓰기(삽입, 수정, 삭제)속도는 느려진다.**
    - 중복된 데이터 사본때문임.  쓰기 작업보다 읽기 작업의 성능이 중요할 때 고려
4. **선택적 적용의 어려움**
    - 역정규화를 한번 적용하면 **설계 복잡도**가 커져, 향후 유지보수 및 요구사항 변경 시 부담이 생길 수 있음

---






            
