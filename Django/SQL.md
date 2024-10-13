# SQL Structure Query Language

: 데이터베이스에 정보를 저장하고 처리하기 위한 프로그래밍 언어

- 테이블의 형태로 구조화된 관계형 데이터베이스에게 요청을 질의(요청)

- SQL Syntax
  
  - SQL 키워드는 대소문자를 구분하지 않음 (단, 명시적 구분을 위해 대문자로 작성하는 것 권장)
  
  - 각 SQL Statements 끝에는 세미콜론`;`이 필요

- SQL Statements
  
  : SQL을 구성하는 가장 기본적인 코드 블록
  
  - ex) `SELECT column_name FROM table_name;`
    
    - 이 SELECT Statement는 `SELECT`, `FROM` 2개의 keyword로 구성됨
  
  - 4가지 유형
    
    | 유형                               | 역할                     | SQL 키워드                               |
    | -------------------------------- | ---------------------- | ------------------------------------- |
    | DDL (Data Defination Language)   | 데이터의 기본 구조 및 형식 변경     | `CREATE`, `DROP`, `ALTER`             |
    | DQL (Data Query Language)        | 데이터 검색                 | `SELECT`                              |
    | DML (Data Manipulation Language) | 데이터 조작 (추가, 수정, 삭제)    | `INSERT`, `UPDATE`, `DELETE`          |
    | DCL (Data Control Language)      | 데이터 및 작업에 대한 사용자 권한 제어 | `COMMIT`,`ROLLBACK`,`GRANT`, `REVOKE` |

- Query
  
  : 데이터베이스로부터 정보를 요청하는 것
  
  - 일반적으로 SQL로 작성하는 코드를 쿼리문(SQL문)이라 함

---

## DDL - `CREATE`

: 테이블 생성

```sql
CREATE TABLE table_name (
    column_1 data_type constraints,
    column_2 data_type constraints,
    ...,
);
```

- 각 필드에 적용할 데이터 타입 작성

- 테이블 및 필드에 대한 제약조건(constraints) 작성
  
  > ```sql
  > CREATE TABLE examples (
  >     ExamID INTEGER PRIMARY KEY AUTOINCREMENT,
  >     LastName VARCHAR(50) NOT NULL,
  >     FirstName VARCHAR(50) NOT NULL
  > );
  > ```

- `PRAGMA`
  
  : 테이블 schema(구조) 확인
  
  > ex) `PRAGMA table_info('examples');`

- `cid`
  
  : Column ID를 의미하며, 각 컬럼의 고유한 식별자를 나타내는 정수 값
  
  - 직접 사용하지 않으며, PRAGMA 명령과 같은 메타데이터 조회에서 출력 값으로 활용됨

- SQLite 데이터 타입
  
  - `NULL` : 아무런 값도 포함하지 않음을 나타냄
  
  - `INTEGER` : 정수
  
  - `REAL` : 부동 소수점
  
  - `TEXT` : 문자열
  
  - `BLOB` : 이미지, 동영상, 문서 등의 바이너리 데이터

- Constraints 제약 조건
  
  : 테이블의 필드에 적용되는 규칙 또는 제한 사항
  
  - 데이터의 무결성을 유지하고 데이터베이스의 일관성을 보장
  
  - 대표 제약 조건 3가지
    
    - `PRIMARY KEY` : 해당 필드를 기본 키로 지정
      
      - `INTEGER` 타입에만 적용되며, `INT`, `BIGINT` 등과 같은 다른 정수 유형은 적용되지 않음
      
      - `NOT NULL` : 해당 필드에 `NULL` 값을 허용하지 않도록 지정
      
      - `FOREIGN KEY` : 다른 테이블과의 외래 키 관계를 정의

- `AUTOINCREMENT` keyword
  
  : 자동으로 고유한 정수 값을 생성하고 할당하는 필드 속성
  
  - 필드의 자동 증가를 나타내는 특수한 키워드
  
  - 주로 primary key 필드에 적용
  
  - `INTEGER PRIMARY KEY AUTOINCREMENT` 가 작성도니 필드는 항상 새로운 레코드에 대해 이전 최대 값보다 큰 값을 할당
  
  - 삭제된 값은 무시되며 재사용할 수 없게 됨

---

## DDL - `ALTER`

: 테이블 및 필드 조작  

| 명령어                                                 | 역할        |
| --------------------------------------------------- | --------- |
| `ALTER TABLE (테이블 이름) ADD COLUMN (필드명)`             | 필드 추가     |
| `ALTER TABLE(테이블 이름) RENAME COLUMN (필드명) TO (새 이름)` | 필드 이름 변경  |
| `ALTER TABLE(테이블 이름) DROP COLUMN (삭제할 필드명)`         | 필드 삭제     |
| `ALTER TABLE(테이블 이름) RENAME TO (새 이름)`              | 테이블 이름 변경 |

- `ADD`하고자 하는 필드에 `NOT NULL` 제약 조건이 있을 경우 `NULL`이 아닌 기본 값 설정 필요

- 테이블 생성 시 정의한 필드는 기본 값이 없어도 `NOT NULL` 제약 조건으로 생성되며, 내부적으로 Default value는 `NULL`로 설정됨

---

## DDL - `DROP`

: 테이블 삭제

```sql
DROP TABLE (테이블 이름);
```

---

## DQL - `SELECT`

: 테이블에서 데이터를 조회

- `SELECT` 키워드 이후 데이터를 선택하려는 필드를 하나 이상 지정

- `FROM` 키워드 이후 데이터를 선택하려는 테이블의 이름을 지정
  
  > ```sql
  > SELECT DISTINCT
  >     select_list
  > FROM
  >     table_name
  > WHERE
  >     search_condition;
  > ORDER BY
  >     column1 [ASC|DESC],
  >     column2 [ASC|DESC],
  >     ...;
  > ```

- `*` : 전체 선택

- `AS` : 속성이나 테이블의 이름을 새로 지정해 사용

- `ORDER BY` : 데이터를 정렬해 조회
  
  - `ORDER BY (정렬할 속성) (정렬 방식. ASC(오름차순) or DESC(내림차순))`
  - `NULL` 값이 존재할 경우, 오름차순 정렬 시 결과에 `NULL`이 먼저 출력

- SELECT statement 실행 순서
  
  1. 테이블에서 (`FROM`)
  2. 특정 조건에 맞추어 (`WHERE`)
  3. 그룹화 하고 (`GROUP BY`)
  4. 만약 그룹 중에서 조건이 있다면 맞추고 (`HAVING`)
  5. 조회하여 (`SELECT`)
  6. 정렬하고 (`ORDER BY`)
  7. 특정 위치의 값을 가져옴 (`LIMIT`)

- Filtering data 관련 Keywords
  
  - Clause
    
    - `DISTINCT` : 조회 결과에서 중복도니 레코드 제거
      
      - `SELECT` 키워드 바로 뒤에 작성
    
    - `WHERE` : 조회 시 특정 검색 조건을 지정
      
      - `FROM` clause 뒤에 위치
      
      - 비교연산자(`IS`, `!=` 등) 및 논리연산자(`AND`, `OR`, `NOT` 등) 사용 가능
      
      - `LIKE` : `WHERE` 조건문에서 attribute 값과 문자열을 비교할 때 사용
    
    - `LIMIT` : 조회하는 레코드 수를 제한
      
      - 하나 또는 2개의 인자를 사용 (0 또는 양의 정수)
      
      - `row_count`는 조회하는 최대 레코드 수를 지정
      
      - `LIMIT [offset, ] row_count;`
        
        - ex) `LIMIT 2, 5;` : 3번째 줄부터 7번째 줄까지
    
    - `GROUT BY` : 레코드를 그룹화하여 요약본 생성(집계 함수와 함께 사용)
      
      - `FROM` 및 `WHERE` 절 뒤에 배치
      
      - 집계 함수(Aggregation Functions)
        
        : 값에 대한 계산을 수행하고 단일한 값을 반환하는 함수
        
        - `SUM`, `AVG`, `MAX`, `MIN`, `COUNT`
    
    - `HAVING` : 집계 항목에 대한 세부 조건을 지정
      
      - 주로 `GROUP BY`와 함께 사용되며, `GROUP BY`가 없다면 `WHERE`처럼 동작
  
  - Operator
    
    - `BETWEEN`
    
    - `IN` : 값이 특정 목록 안에 있는지 확인
    
    - `LIKE` : 값이 특정 패턴에 일치하는지 확인 (Wildcards와 함께 사용)
      
      - Wildcard Characters
        
        - `%` : 0개 이상의 문자열과 일치하는지 확인
        
        - `_` : 단일 문자와 일치하는지 확인
    
    - Comparison
    
    - 논리(Logical) 연산자
      
      - `AND`(`&&`), `OR`(`||`), `NOT`(`!`)

---

## DML - `INSERT`

: 테이블 레코드 삽입

```sql
INSERT INTO table_name (c1, c2, ...)
VALUES (v1, v2, ...);
```

- `INSERT INTO`절 다음에 테이블 이름과 괄호 안에 필드 목록 작성

- `VALUES` 키워드 다음 괄호 안에 해당 필드에 삽입할 값 목록 작성

---

## DML - `UPDATE`

: 테이블 레코드 수정

```sql
UPDATE table_name
SET column_name = expression,
[WHERE
    condition];
```

- `SET`절 다음에 수정할 필드와 새 값을 지정

- `WHERE`절에서 수정할 레코드를 지정하는 조건 작성

- `WHERE`절을 작성하지 않으면 모든 레코드를 수정

---

## DML - `DELETE`

: 테이블 레코드 삭제

```sql
DELETE FROM table_name
[WHERE
    condition];
```

- `DELETE FROM`절 다음에 테이블 이름 작성

- `WHERE`절 에서 삭제할 레코드를 지정하는 조건 작성

- `WHERE`절을 작성하지 않으면 모든 레코드를 삭제

- ex) articles 테이블에서 작성일이 오래된 순으로 레코드 2개 삭제
  
  > ```sql
  > DELETE FROM
  >     articles
  > WHERE id IN (
  >     SELECT id FROM articles
  >     ORDER BY createdAt
  >     LIMIT 2
  > );
  > ```

---

## 관계 - `JOIN`

: 여러 테이블 간의 (논리적) 연결. 둘 이상의 테이블에서 데이터를 검색하는 방법

- `JOIN`이 필요한 순간
  
  - 테이블을 분리하면 데이터 관리는 용이해질 수 있으나 출력시에는 문제가 있음
  
  - 테이블 한 개만을 출력할 수 밖에 없어 다른 테이블과 결합하여 출력하는 것이 필요해짐

- 종류
  
  - `INNER JOIN`
    
    : 두 테이블에서 값이 일치하는 레코드에 대해서만 결과를 반환
    
    > ```sql
    > SELECT
    >     select_list
    > FROM
    >     table_a
    > INNER JOIN table_b
    >     ON table_b.fk = table_a.pk;
    > ```
    
    - `FROM`절 이후 메인 테이블 지정
    
    - `INNER JOIN`절 이후 메인 테이블과 조인할 테이블을 지정
    
    - `ON` 키워드 이후 조인 조건(테이블간의 레코드를 일치시키는 규칙)을 작성
  
  - `LEFT JOIN`
    
    : 오른쪽 테이블의 일치하는 레코드와 함께 왼쪽 테이블의 모든 레코드를 반환
    
    > ```sql
    > SELECT
    >     select_list
    > FROM
    >     table_a
    > LEFT JOIN table_
    >     ON table_b.fk = table_a.pk;
    > ```
    
    - `FROM` 절 이후 왼쪽 테이블 지정
    
    - `LEFT JOIN`절 이후 오른쪽 테이블 지정
    
    - `ON` 키워드 이후 조인 조건(왼쪽 테이블의 각 레코드를 오른쪽 테이블의 모든 레코드와 일치시킴)을 작성
    
    - 오른쪽 테이블과 매칭되는 레코드가 없으면 `NULL`을 표시



---

### cf. SQL 표준

- SQL은 미국 국립 표준 협회(ANSI)와 국제 표준화 기구(ISO)에 의해 표준이 채택됨

- 모든 RDMBS에서 SQL 표준을 지원

- 다만 각 RDBMS마다 독자적인 기능에 따라 표준을 벗어나는 문법이 존재하니 주의

### cf. 타입 선호도 (Type Affinity)

: 컬럼에 데이터 타입이 명시적으로 지정되지 않았거나 지원하지 않을 때 SQLite가 자동으로 데이터 타입을 추론하는 것

- SQLite 타입 선호도의 목적
  
  - 유연한 데이터 타입 지원
    
    - 데이터 타입을 명시적으로 지정하지 않고도 데이터를 저장하고 조회할 수 있음
    
    - 컬럼에 저장되는 값의 특성을 기반으로 데이터 타입을 유추
  
  - 간편한 데이터 처리
    
    - `INTEGER` Type Affinity를 가진 열에 문자열 데이터를 저장해도 SQLite는 자동으로 숫자로 변환하여 처리
  
  - SQL 호환성
    
    - 다른 데이터베이스 시스템과 호환성을 유지

### cf. 반드시 `NOT NULL` 제약을 사용해야 할까?

- 아니오. 하지만, 데이터베이스를 사용하는 프로그램에 따라 `NULL`을 저장할 필요가 없는 경우가 많으므로 대부분 `NOT NULL`을 정의

- '값이 없다'라는 표현을 테이블에 기록하는 것을 `0`이나 `빈 문자열` 등을 사용하는 것으로 대체하는 것을 권장

### cf. SQLite의 날짜와 시간

- SQLite에는 날짜 및 시간을 저장하기 위한 별도 데이터 타입이 없음

- 대신 날짜 및 시간에 대한 함수를 사용해 표기 형식에 따라 `TEXT`, `REAL`, `INTEGER` 값으로 저장
