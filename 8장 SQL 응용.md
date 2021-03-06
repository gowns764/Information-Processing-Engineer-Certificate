# 8장 SQL 응용 (11%)

## 1. SQL - DDL (Data Define Language, 데이터 정의어)
- DDL은 DB 구조, 데이터 형식, 접근 방식 등 DB를 구축하거나 수정할 목적으로 사용하는 언어이다.
- 번역한 결과가 데이터 사전이라는 특별한 파일에 여러 개의 테이블로 저장된다.
- DDL의 3가지 유형
  - **CREATE**
    - SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 정의한다.
  - **ALTER**
    - TABLE에 대한 정의를 변경하는 데 사용한다.
  - **DROP**
    - SCHEMA, DOMAIN, TABLE, VIEW, INDEX를 삭제한다.

### 1.1. CREATE SCHEMA
- 스키마를 정의하는 명령문이다.
- 표기 형식
  ```
  CREATE SCHEMA 스키마명 AUTHORIZATION 사용자_id;
  ```
### 1.2. CREATE DOMAIN 
- 도메인을 정의하는 명령문이다.
- 표기 형식
  ```
  CREATE DOMAIN 도메인명  [AS] 데이터_타입
        [DEFAULT 기본값]
        [CONSTRAINT 제약조건명 CHECK 범위값];
  ```
- 데이터 타입
  - SQL에서 지원하는 데이터 타입이다.
- 기본값
  - 데이터를 입력하지 않았을 때 자동으로 입력되는 값이다.

### 1.3. CREATE TABLE
- 테이블을 정의하는 명령문이다.
- 표기 형식
  ```
  CREATE TABLE 테이블명
         (속성명 데이터_타입 [DEFAULT 기본값] [NOT NULL], ...
         [, PRIMARY KEY(기본키_속성명, ...)]
         [, UNIQUE(대체키_속성명, ...)]
         [, FOREIGN KRY(외래키_속성명, ...)]
                  REFERENCES 참조테이블(기본키_속성명))
  ```
- 기본 테이블에 포함될 모든 속성에 대하여 속성명과 그 속성의 데이터 타입, 기본값, NOT NULL 여부를 지정한다.
- **PRIMARY KEY**
  - 기본키로 사용할 속성을 지정한다.
- **UNIQUE**
  - 대체키로 사용할 속성을 지정한다.
  - 중복된 값을 가질 수 없다.
- **FOREIGN KEY ~ REFERENCES ~**
  - 외래키로 사용할 속성을 지정한다.
  - ON DELETE 옵션
    - 참조 테이블의 튜플이 삭제되었을 때 기본 테이블에 취해야 할 사항을 지정한다.
  - ON UPDATE 옵션
    - 참조 테이블의 참조 속성 값이 변경되었을 때 기본 테이블에 취해야 할 사항을 지정한다.
- **CONSTRINT**
  - 제약 조건의 이름을 지정한다.
- **CHECK**
  - 속성 값에 대한 제약 조건을 정의한다.

### 1.4. CREATE VIEW
- 뷰를 정의하는 명령문이다.
- 표기 형식
  ```
  CREATE VIEW 뷰명[(속성명[, 속성명, ...])]
  AS SELECT문;
  ```

### 1.5. CREATE INDEX
- 인덱스를 정의하는 명령문이다.
- 표기 형식
  ```
  CREATE [UNIQUE] INDEX 인덱스명
  ON 테이블명(속성명 [ASC | DESC] [, 속성명 [ASC | DESC]])
  [CLUSTER];
  ```
- **UNIQUE**
  - 사용된 경우
    - 중복 값이 없는 속성으로 인덱스를 생성한다.
  - 생략된 경우
    - 중복 값을 허용하는 속성으로 인덱스를 생성한다.
- 정렬 여부 지정
  - ASC
    - 오름차순 정렬
  - DESC
    - 내림차순 정렬
  - 생략된 경우
    - 오름차순으로 정렬된다.
- **CLUSTER**
  - 사용하면 인덱스가 클러스터드 인덱스로 설정된다.

### 1.6. ALTER TABLE
- 테이블에 대한 정의를 변경하는 명령문이다.
- 표기 형식
  ```
  ALTER TABLE 테이블명 ADD 속성명 데이터_타입 [DEFAULT '기본값'];
  ALTER TABLE 테이블명 ALTER 속성명 [SET DEFAULT '기본값'];
  ALTER TABLE 테이블명 DROP COLUMN 속성명 [CASCADE];
  ```
- **ADD**
  - 새로운 속성(열)을 추가할 때 사용한다.
- **ALTER**
  - 특정 속성의 Default 값을 변경할 때 사용한다.
- **DROP COLUMN**
  - 특정 속성을 삭제할 때 사용한다.

### 1.7. DROP
- 스키마, 도메인, 기본 테이블, 뷰 테이블, 인덱스, 제약 조건 등을 제거하는 명령문이다.
- 표기 형식
  ```
  DROP SCHEMA 스키마명 [CASCADE | RESTRICT];
  DROP DOMAIN 도메인명 [CASCADE | RESTRICT];
  DROP TABLE 테이블명 [CASCADE | RESTRICT];
  DROP VIEW 뷰명 [CASCADE | RESTRICT];
  DROP INDEX 인덱스명 [CASCADE | RESTRICT];
  DROP CONSTRAINT 제약조건명;
  ```
- **CASCADE**
  - 제거할 요소를 참조하는 다른 모든 개체를 함께 제거한다.
- **RESTRICT**
  - 다른 개체가 제거할 요소를 참조중일 때는 제거를 취소한다.

## 2. SQL - DCL (Data Control Language, 데이터 제어어)
- DCL은 데이터의 보안, 무결성, 회복, 병행 제어 등을 정의하는 데 사용하는 언어이다.
- 데이터베이스 관리자(DBA)가 데이터 관리를 목적으로 사용한다.
- DCL의 종류
  - **COMMIT**
    - 명령에 의해 수행된 결과를 실제 물리적 디스크로 저장하고, 데이터베이스 조작 작업이 정상적으로 완료되었음을 관리자에게 알려준다.
  - **ROLLBACK**
    - 데이터베이스 조작 작업이 비정상적으로 종료되었을 때 원래의 상태로 복구한다.
  - **GRAINT**
    - 데이터베이스 사용자에게 사용 권한을 부여한다.
  - **REVOKE**
    - 데이터베이스 사용자의 사용 권한을 취소한다.

### 2.1. GRAINT / REVOKE
- 데이터베이스 관리자가 데이터베이스 사용자에게 권한을 부여하거나 취소하기 위한 명령어이다.
- **GRAINT**
  - 권한 부여를 위한 명령어이다.
- **REVOKE**
  - 권한 취소를 위한 명령어이다.
- 사용자 등급 지정 및 해제
  ```
  GRAINT 사용자등급 TO 사용자_ID_리스트 [IDENTIFIED BY 암호];
  REVOKE 사용자등급 FROM 사용자_ID_리스트;
  ```
- 테이블 및 속성에 대한 권한 부여 및 취소
  ```
  GRAINT 권한_리스트 ON 개체 TO 사용자 [WITH GRAINT OPTION];
  REVOKE [GRAINT OPTION FOR] 권한_리스트 ON 개체 FROM 사용자 [CASCADE];
  ```
  - 권한 종류
    - ALL, SELECT, INSERT, DELETE, UPDATE, ALTER 등
  - **WITH GRAINT OPTION**
    - 부여받은 권한을 다른 사용자에게 다시 부여할 수 있는 권한을 부여한다.
  - **GRAINT OPTION FOR**
    - 다른 사용자에게 권한을 부여할 수 있는 권한을 취소한다.
  - **CASCADE**
    - 권한 취소 시 권한을 부여받았던 사용자가 다른 사용자에게 부여한 권한도 연쇄적으로 취소한다.

### 2.2. COMMIT
- 트랜잭션 처리가 정상적으로 완료된 후 트랜잭션이 수행한 내용을 데이터베이스에 반영하는 명령이다.
- COMMIT 명령을 실행하지 않아도 DML문이 성공적으로 완료되면 자동으로 COMMIT되고, DML이 실패하면 자동으로 ROLLBACK이 되도록 Auto COMMIT 기능을 설정할 수 있다.

### 2.3. ROLLBACK
- 변경되었으나 아직 COMMIT되지 않은 모든 내용들을 취소하고 데이터베이스를 이전 상태로 되돌리는 명령어이다.
- 트랜잭션 전체가 성공적으로 끝나지 못하면 일부 변경된 내용만 데이터베이스에 반영되는 비일관성 상태가 될 수 있기 때문에 일부분만 완료된 트랜잭션은 롤백되어야 한다.

### 2.4. SAVEPOINT
- 트랜잭션 내에 ROLLBACK할 위치인 저장점을 지정하는 명령어이다.
- 저장점을 지정할 때는 이름을 부여한다.
- ROLLBACK할 때 지정된 저장점까지의 트랜잭션 처리 내용이 모두 취소된다.


## 3. SQL - DML (Data Manipulation Language, 데이터 조작어)
- DML은 데이터베이스 사용자가 저장된 데이터를 실질적으로 관리하는데 사용되는 언어이다.
- 데이터베이스 사용자와 데이터베이스 관리 시스템 간의 인터페이스를 제공한다.
- DML의 유형
  - **SELECT**
    - 테이블에서 튜플을 검색한다.
  - **INSERT**
    - 테이블에 새로운 튜플을 삽입한다.
  - **DELETE**
    - 테이블에서 튜플을 삭제한다.
  - **UPSATE**
    - 테이블에서 튜플의 내용을 갱신한다.

### 3.1. 삽입문(INSERT INTO~)
- 기본 테이블에 새로운 튜플을 삽입할 때 사용한다.
- 일반 형식
  ```
  INSERT INTO 테이블명([속성명1, 속성명2, ...])
  VALUES (데이터1, 데이터2, ...);
  ```
- 대응하는 속성과 데이터는 개수와 데이터 유형이 일치해야 한다.
- 기본 테이블의 모든 속성을 사용할 때는 속성명을 생략할 수 있다.
- SELECT문을 사용하여 다른 테이블의 검색 결과를 삽입할 수 있다.

### 3.2. 삭제문(DELETE FROM~)
- 기본 테이블에 있는 튜플들 중에서 특정 튜플(행)을 삭제할 때 사용한다.
- 일반 형식
  ```
  DELETE
  FROM 테이블명
  [WHERE 조건];
  ```
- 모든 레코드를 삭제할 때는 WHERE절을 생략한다.
- 모든 레코드를 삭제하더라도 테이블 구조는 남아 있기 때문에 디스크에서 테이블을 완전히 제거하는 DROP과는 다르다.

### 3.3. 갱신문(UPDATE~ SET~)
- 기본 테이블에 있는 튜플들 중에서 특정 튜플의 내용을 변경할 때 사용한다.
- 일반 형식
  ```
  UPDATE 테이블명
  SET 속성명 = 데이터[, 속성명=데이터, ...]
  [WHERE 조건];
  ```

## 4. DML - DELECT-1

### 4.1. 일반 형식
```
SELECT [PREDICATE [테이블명.]속성명 [AS 별칭][, [테이블명.]속성명, ...]
[, 그룹함수(속성명) [AS 별칭]]
[, Window함수 OVER (PARTITION BY 속성명1, 속성명2, ...
                ORDER BY 속성명3, 속성명4, ...)]
FROM 테이블명[, 테이블명, ...]
[WHERE 조건]
[GROUP BY 속성명, 속성명, ...]
[ORDER BY 속성명 [ASC | DESC]];
```
- **SELECT절**
  - PREDICATE
    - 검색할 튜플 수를 제한하는 명령어를 기술한다.
    - DISTINCT
      - 중복된 튜플이 있으면 그 중 첫 번째 한 개만 표시한다.
  - 속성명
    - 검색하여 불러올 속성(열) 또는 속성을 이용한 수식을 지정한다.
  - AS
    - 속성이나 연산의 이름을 다른 이름으로 표시하기 위해 사용한다.
- **FROM절**
  - 검색할 데이터가 들어있는 테이블 이름을 기술한다.
- **WHERE절**
  - 검색할 조건을 기술한다.
- **ORDER BY절**
  - 데이터를 정렬하여 검색할 때 사용한다.
  - 속성명
    - 정렬의 기준이 되는 속성명을 기술한다.
    - [ASC | DESC]
      - 정렬 방식으로, 'ASC'는 오름차순, 'DESC'는 내림차순이다.
      - 생략하면 오름차순으로 지정된다.

### 4.2. 조건 연산자
- **비교 연산자**
  - =, <>, \>, <, >=, <=
- **논리 연산자**
  - NOT, AND, OR
- **LIKE 연산자**
  - 대표 문자를 이용해 지정된 속성의 값이 문자 패턴과 일치하는 튜플을 검색하기 위해 사용된다.
  - 대표 문자
    - %
      - 모든 문자를 대표한다.
    - _
      - 문자 하나를 대표한다. 
    - \#
      - 숫자 하나를 대표한다.

### 4.3. 기본 검색
- SELECT절에 원하는 속성을 지정하여 검색한다.

### 4.4. 조건 지정 검색
- WHERE절에 조건을 지정하여 조건에 만족하는 튜플만 검색한다.

### 4.5. 정렬 검색
- ORDER BY절에 특정 속성을 지정하여 지정된 속성으로 자료를 정렬하여 검색한다.

### 4.6. 하위 질의
- 조건절에 주어진 질의를 먼저 수행하여 그 검색 결과를 조건절의 피연산자로 사용한다.

### 4.7. 복수 테이블 검색
- 여러 테이블을 대상으로 검색을 수행한다.

## 5. DML - SELECT-2

### 5.1. 일반 형식
```
SELECT [PREDICATE] [테이블명.]속성명 [AS 별칭][, [테이블명.]속성명, ...]
[, 그룹함수(속성명) [AS 별칭]]
[, WINDOW함수 OVER (PARTITION BY 속성명1, 속성명2, ...
              ORDER BY 속성명3, 속성명4, ...) [AS 별칭]]
FROM 테이블명[, 테이블명, ...]
[WHERE 조건]
[GROUP BY 속성명, 속성명, ...]
[HAVING 조건]
[ORDER BY 속성명 [ASC | DESC]];
```
- **그룹 함수**
  - GROUP BY절에 지정된 그룹별로 속성의 값을 집계할 함수를 기술한다.
- **WINDOW 함수**
  - GROUP BY절을 이용하지 않고 속성의 값을 집계할 함수를 기술한다.
  - PARTITION BY
    - WINDOW 함수의 적용 범위가 될 속성을 지정한다.
  - ORDER BY
    - PARTITION 안에서 정렬 기준으로 사용할 속성을 지정한다.
- **GROUP BY절**
  - 특정 속성을 기준으로 그룹화하여 검색할 때 사용한다.
  - 일반적으로  GROUP BY절은 그룹 함수와 함께 사용된다.
- **HAVING절**
  - GROUP BY와 함께 사용되며, 그룹에 대한 조건을 지정한다.

### 5.2. 그룹 함수
- GROUP BY절에 지정된 그룹별로 속성의 값을 집계할 때 사용된다.
- 함수
  - **COUNT(속성명)**
    - 그룹별 튜플 수를 구하는 함수이다.
  - **SUM(속성명)**
    - 그룹별 합계를 구하는 함수이다.
  - **AVG(속성명)**
    - 그룹별 평균을 구하는 함수이다.
  - **MAX(속성명)**
    - 그룹별 최대값을 구하는 함수이다.
  - **MIN(속성명)**
    - 그룹별 최소값을 구하는 함수이다.
  - **STDDEV(속성명)**
    - 그룹별 표준편차를 구하는 함수이다.
  - **VARIANCE(속성명)**
    - 그룹별 분산을 구하는 함수이다.
  - **ROLLUP(속성명, 속성명, ...)**
    - 인수로 주어진 속성을 대상으로 그룹별 소계를 구하는 함수이다.
    - 속성의 개수가 n개이면, n+1 레벨까지, 하위 레벨에서 상위 레벨 순으로 데이터가 집계된다.
  - **CUBE(속성명, 속성명, ...)**
    - ROLLUP과 유사한 형태지만 CUBE는 인수로 주어진 속성을 대상으로 모든 조합의 그룹별 소계를 구한다.
    - 속성의 개수가 n개면, 2ⁿ 레벨까지, 상위 레벨에서 하위 레벨 순으로 데이터가 집계된다.

### 5.3. WINDOW 함수
- GROUP BY절을 이용하지 않고 함수의 인수로 지정한 속성의 값을 집계한다.
- 함수의 인수로 지정한 속성이 집계할 범위가 되는데, 이를 왼도우(WINDOW)라고 부른다.
- WINDOW 함수
  - **ROW_NUMBER()**
    - 윈도우별로 각 레코드에 대한 일련번호를 반환한다.
  - **RANK()**
    - 윈도우별로 군위를 반환하며, 공동 순위를 반영한다.
  - **DENSE_RANK()**
    - 윈도우별로 순위를 반환하며, 공동 순위를 무시하고 순위를 부여한다.

#### 5.3.1. WINDOW 함수 이용 검색
- GROUP BY절을 이용하지 않고 함수의 인수로 지정한 속성을 범위로 하여 속성의 값을 집계한다.

### 5.4. 그룹 지정 검색
- GROUP BY절에 지정한 속성을 기준으로 자료를 그룹화하여 검색한다.

### 5.5. 집합 연산자를 이용한 통합 질의
- 집합 연산자를 사용하여 2개 이상의 테이블의 데이터를 하나로 통합한다.
- 두 개의 SELECT문에 기술한 속성들은 개수와 데이터 유형이 서로 동일해야 한다.
- 집합 연산자의 종류(통합 질의의 종류)
  - **UNION**
    - 두 SELECT문의 조회 결과를 통합하여 모두 출력한다.
    - 중복된 행은 한 번만 출력한다.
    - 합집합
  - **UNION ALL**
    - 두 SELECT문의 조회 결과를 통합하여 모두 출력한다.
    - 중복된 행도 그대로 출력한다.
    - 합집합
  - **INTERSECT**
    - 두 SELECT문의 조회 결과 중 공통된 행만 출력한다.
    - 교집합
  - **EXCEPT**
    - 첫 번째 SELECT문의 조회 결과에서 두 번째 SELECT문의 조회 결과를 제외한 행을 출력한다.
    - 차집합

## 6. 프로시저(Procedure)
- SQL을 사용하여 작성한 일련의 작업을 저장해두고 호출을 통해 원할 때마다 저장한 작업을 수행하도록 하는 정차형 SQL이다.
- 데이터베이스에 저장되어 수행되기 때문에 스토어드(Stored) 프로시저라고도 불린다.
- 시스템의 일일 마감 작업, 일괄(Batch) 작업 등에 주로 사용된다.

### 6.1. 프로시저의 구성도
- **DECLARE**
  - 프로시저의 명칭, 변수, 인수, 데이터 타입을 정의하는 선언부이다.
- **BEGIN / END**
  - 프로시저의 시작과 종료를 의미한다.
- **CONTROL**
  - 조건문 또는 반복문이 삽입되어 순차적으로 처리된다.
- **SQL**
  - DML, DCL이 삽입되어 데이터 관리를 위한 조회, 추가, 수정, 삭제 작업을 수행한다.
- **EXCEPTION**
  - BEGIN ~ END 안의 구문 실행 시 예외가 발생하면 이를 처리하는 방법을 정의한다.
- **TRANSACTION**
  - 수핸된 데이터 작업들을 DB에 적용할지 취소할지를 결정하는 처리부이다.

### 6.2. 프로시저 생성
- 프로시저를 생성하기 위해서는 **CREATE PROCEDURE** 명령어를 사용한다.
- 표기 형식
  ```
  CREATE [OR REPLACE] PROCEDURE 프로시저명(파라미터)
  [지역변수 선언]
  BEGIN
    프로시저 BODY;
  END;
  ```
  - **OR REPLACE**
    - 선택적인 예약어이다.
    - 이 예약어를 사용하면 동일한 프로시저 이름이 이미 존재하는 경우, 기존의 프로시저를 대체할 수 있다.
  - **프로시저명**
    - 생성하려는 프로시저의 이름을 지정한다.
  - **파라미터**
    - IN
      - 호출 프로그램이 프로시저에게 값을 전달할 때 지정한다.
    - OUT
      - 프로시저가 호출 프로그램에 값을 반환할 때 지정한다.
    - INOUT
      - 호출 프로그램이 프로시저에게 값을 전달하고, 프로시저 실행 후 호출 프로그램에 값을 반환할 때 지정한다.
    - 매개변수명
      - 호출 프로그램으로부터 전달받은 값을 저장할 변수의 이름을 지정한다.
    - 자료형
      - 변수의 자료형을 지정한다.
  - **프로시저 BODY**
    - 프로시저의 코드를 기록하는 부분이다.
    - BEGIN에서 시작하여 END로 끝나며, BEGIN과 END 사이에는 적어도 하나의 SQL문이 있어야 한다.

### 6.3 프로시저 실행
- 프로시저를 실행하기 위해서는 **EXECUTE** 명령어 또는 **CALL** 명령어를 사용하며, EXECUTE 명령어를 줄여서 **EXEC**로 사용하기도 한다.
- 표기 형식
  ```
  EXECUTE 프로시저명;
  EXEC 프로시저명;
  CALL 프로시저명;
  ```

### 6.4. 프로시저 제거
- 프로시저를 제거하기 위해서는 **DROP PROCEDURE** 명령어를 사용한다.
- 표기 형식
  ```
  DROP PROCEDURE 프로시저명;
  ```

## 7. 트리거(Trigger)
- 데이터베이스 시스템에서 데이터의 삽입, 갱신, 삭제 등의 이벤트가 발생할 때 관련 작업들이 자동으로 수행되게 하는 절차형 SQL이다.
- 데이터베이스에 저장되며, 데이터 변경 및 무경성 유지, 로그 메시지 출력 등의 목적으로 사용된다.
- 트리거의 구문에는 DCL을 사용할 수 없으며, DCL이 포함된 프로시저나 함수를 호툴하는 경우에 오류가 발생한다.

### 7.1. 트리거의 구성도
- **DECLARE**
  - 트리거의 명칭, 변수 및 상수, 데이터 타입을 정의하는 선언부이다.
- **EVENT**
  - 트리거가 실행되는 조건을 명시한다.
- **BEGIN / END**
  - 트리거의 시작돠 종료를 의미한다.
- **CONTROL**
  - 조건문 또는 반복문이 삽입되어 순차적으로 처리된다.
- **SQL**
  - DML문이 삽입되어 데이터 관리를 위한 조회, 추가, 수정, 삭제 작업을 수행한다.
- **EXCEPTION**
  - BEGIN ~ END 안의 구문 실행 시 예외가 발생하면 이를 처리하는 방법을 정의한다.

### 7.2. 트리거의 생성
- 트리거를 생성하기 위해서는 **CREATE TRIGGER** 명령어를 사용한다.
- 표기 형식
  ```
  CREATE [OP REPLACE] TRIGGER 트리거명 [동작 시기 옵션][동작 옵션] ON 테이블명
  REFERENCING [NEW | OLD] AS 테이블명
  FOR EACH ROW
  [WHEN 조건식]
  BEGIN
    트리거 BODY;
  END;
  ```
  - **OR REPLACE**
    - 선택적인 예약어이다.
    - 이 예약어를 사용하면 동일한 트리거 이름이 이미 존재하는 경우, 기존의 트리거를 대체할 수 있다.
  - **동작 시기 옵션**
    - 트리거가 실행될 때를 지정한다.
      - AFTER
        - 테이블이 변경된 후에 트리거가 실행된다.
      - BEFORE
        - 테이블이 변경되기 전에 트리거가 실행된다.
  - **동작 옵션**
    - 트리거가 실행되게 할 작업의 종류를 지정한다.
      - INSERT
        - 테이블에 새로운 튜플을 삽입할 때 트리거가 실행된다.
      - DELETE
        - 테이블의 튜플을 삭제할 때 트리거가 실행된다.
      - UPDATE
        - 테이블의 튜플을 수정할 때 트리거가 실행된다.
  - **NEW | OLD**
    - 트리거가 적용될 테이블의 별칭을 지정한다.
      - NEW
        - 추가되거나 수정에 창여할 튜플들의 집합(테이블)을 의미한다.
      - OLD
        - 수정되거나 삭제 전 대상이 되는 튜플들의 집합(테이블)을 의미한다.
  - **FOR EACH ROW**
    - 각 튜플마다 트리거를 적용한다는 의미이다.
  - **WHEN 조건식**
    - 선택적인 예약어이다.
    - 트리거를 적용할 튜플의 조건을 지정한다.
  - **트리거 BODY**
    - 트리거의 본문 코드를 입력하는 부분이다.
    - BEGIN으로 시작해서 END로 끝나는데, 적어도 하나 이상의 SQL문이 있어야 한다.
    - 그렇지 않으면 오류가 발생한다.

### 7.3. 트리거의 제거
- 트리거를 제거하기 위해서는 **DROP TRIGGER** 명령어를 사용한다.
- 표기 형식
  ```
  DROP TRIGGER 트리거명;
  ```

## 8. 사용자 정의 함수
- 프로시저와 유사하게 SQL을 사용하여 일련의 작업을 연속적으로 처리하지만, 종료 시 처리 결과로 단일 값만을 반환하는 절차형 SQL이다.
- 데이터베이스에 저장되어 SELECT, INSERT, DELETE, UPDATE 등 DML문의 호출에 의해 실행된다.
- 예약어 RETURN을 통해 단일 값만을 반환하며, 출력 파라미터가 없다.

### 8.1. 사용자 정의 함수의 구성도
- **DECLARE**
  - 사용자 정의 함수의 명칭, 변수, 인수, 데이터 타입을 정의하는 선언부이다.
- **BEGIN / END**
  - 사용자 정의 함수의 시작과 종료를 의미한다.
- **CONTROL**
  - 조건문 또는 반복문이 삽입되어 순차적으로 처리된다.
- **SQL**
  - SELECT문이 삽입되어 데이터 조회 작업을 수행한다.
- **EXCEPTION**
  - BEGIN ~ END 안의 구문 실행 시 예외가 발생하면 이를 처리하는 방법을 정의한다.
- **RETURN**
  - 호출 프로그램에 반환할 값이나 변수를 정의한다.

### 8.2.사용자 정의 함수 생성
- 사용자 정의 함수를 생성하기 위해서는 **CREATE FUNCTION** 명령어를 사용한다.
- 표기 형식
  ```
  CREATE [OR REPLACE] FUNCTION 사용자 정의 함수명(파라미터)
  [지역변수 선언]
  BEGIN
    사용자 정의 함수 BODY;
    REYURN 반환값;
  END;
  ```
  - **OR REPLACE**
    - 선택적인 예약어이다.
    - 이 예약어를 사용하면 동일한 사용자 정의 함수의 이름이 이미 존재하는 경우, 기존의 사용자 정의 함수를 대체할 수 있다.
  - **파라미터**
    - 사용자 정의 함수의 파라미터로는 다음과 같은 것들이 올 수 있다.
      - IN
        - 호출 프로그램이 사용자 정의 함수에게 값을 전달할 때 지정한다.
      - 매개변수명
        - 호출 프로그램으로부터 전달받은 값을 저장할 변수의 이름을 지정한다.
      - 자료형
        - 변수의 자료형을 지정한다.
  - **사용자 정의 함수 BODY**
    - 사용자 정의 함수의 코드를 기록하는 부분이다.
    - BEGIN에서 시작하여 END로 끝나며, BEGIN과 END 사이에는 적어도 하나의 SQL문이 있어야 한다.
  - **RETURN 반환값**
    - 반환할 값이나 반환할 값이 저장된 변수를 호출 프로그램으로 돌려준다.

### 8.3. 사용자 정의 함수 실행
- DML에서 속성명이나 값이 놓일 자리를 대체하여 사용된다.
- 표기 형식
  ```
  SELECT 사용자 정의 함수명 FROM 테이블명;
  INSERT INTO 테이블명(속성명) VALUES (사용자 정의 함수명);
  DELETE FROM 테이블명 WHERE 속성명 = 사용자 정의 함수명;
  UPDATE 테이블명 SET 속성명 = 사용자 정의 함수명;
  ```

### 8.4. 사용자 정의 함수 제거
- 사용자 정의 함수를 제거하기 위해서는 **DROP FUNCTION** 명령어를 사용한다.
- 표기 형식
  ```
  DROP FUNCTION 사용자 정의 함수명;
  ```

## 9. 제어문
- 위에서 아래로 차례대로 실행되는 절차형 SQL의 진행 순서를 변경하기 위해 사용하는 명령문이다.

### 9.1. IF문
- 조건에 따라 실행할 문장을 달리하는 제어문이다.
- 형식1
  - 조건이 참일 때만 실행한다.
- 형식2
  - 조건이 참일 때와 가짓일 때 실행할 문장이 다르다.

### 9.2. LOOP문
- 조건에 따라 실행할 문장을 반복 수행하는 제어문이다.

## 10. 커서(Cursor)
- 쿼리문의 처라 결과가 저장되어 있는 메모리 공간을 가리키는 포인터이다.
- 커서의 수행은 열기, 패치, 닫기의 세 단계로 진행된다.

### 10.1. 묵시적 커서(Implicit Cursor)
- DBMS에 의해 내부에서 자동으로 생성되어 사용되는 커서이다.
- 커서의 속성을 조회하여 사용된 쿼리 정보를 열람하는 것이 가능하다.
- 수행된 쿼리문의 정상적인 수행 여부를 확인하기 위해 사용된다.
- 속성의 종류
  - **SQL%FOUND**
    - 쿼리 수행의 결과로 패치된 튜플 수가 1개 이상이면 TRUE이다.
  - **SQL%NOTFOUND**
    - 쿼리 수행의 결과로 패치된 튜플 수가 0개이면 TRUE이다. 
  - **SQL%ROWCOUNT**
    - 쿼리 수행의 결과로 패치된 튜플 수를 반환한다.
  - **SQL%ISOPEN**
    - 커서가 열린 상태이면 TRUE이다.
    - 묵시적 커서는 자동으로 생성된 후 자동으로 닫히기 때문에 항상 FALSE이다.

### 10.2. 명시적 커서(Explicit Cursor)
- 사용자가 직접 정의해서 사용하는 커서이다.
- 뭐리문의 경과를 저장하여 사용함으로써 동일한 쿼리가 반복 수행되어 데이터베이스 자원이 낭비되는 것을 방지한다.
- 커서는 기본적으로 '열기 - 패치 - 닫기' 순으로 이루어지며, 명시적 커서를 사용하기 위해서는 열기 단계 전에 선언해야 한다.

## 11. DBMS 접속
- 사용자가 데이터를 사용하기 위해 응용 시스템을 이용하여 DBMS에 접근하는 것을 의미한다.
- 응용 시스템은 사용자로부터 매개 변수를 전달받아 SQL을 실행하고 DBMS로부터 전달받은 결과를 사용자에게 전달하는 매개체 역할을 수행한다.
- 인터넷을 통해 구동되는 웹 응용 프로그램은 웹 응용 시스템을 통해 DBMS에 접근한다.
- 웹 응용 시스템은 웹 서버와 WAS로 구성된다.

### 11.1. DBMS 접속 기술
- 접속 기술은 DBMS에 접근하기 위해 사용하는 API 또는 API의 사용을 편리하게 도와주는 프레임워크 등을 의미한다.
- DBMS 접속 기술의 종류
  - **JDBC**
  - **ODBC**
  - **MyBatis**

### 11.2. 동적 SQL
- 다양한 조건에 따라 SQL 구문을 동적으로 변경하여 처리할 수 있는 SQL 처리 방식이다.
- 사용자로부터 SQL문의 일부 또는 전부를 입력받아 실행할 수 있다.
- 응용 프로그램 수행 시 SQL이 변형될 수 있으므로 프리컴파일할 때 구문 분석, 접근 권한 확인 등을 할 수 없다.
- 정적 SQL에 비해 속도가 느리지만, 상황에 따라 다양한 조건을 첨가하는 등 유연한 개발이 가능하다.

## 12. SQL 테스트
- SQL이 작성 의도에 맞게 원하는 기능을 수행하는지 검증하는 과정이다.
- 단문 SQL은 SQL 코드를 직접 실행한 후 결과를 확인하는 것으로, 간단히 테스트가 가능하다.
- 절차형 SQL은 테스트 전에 생성을 통해 구문 오류나 참조 오류의 존재 여부를 확인한다.
- 정상적으로 생성된 절차형 SQL은 디버깅을 통해 로직을 검증하고, 결과를 통해 최종적으로 확인한다.

### 12.1. 단문 SQL 테스트
- DDL, DML, DCL이 포함되어 있는 SQL과 TCL을 테스트하는 것으로, 직접 실행하여 결과물을 확인한다.
- DDL로 작성된 개체는 DESCRIBE 명령어를 이용하여 속성, 자료형, 옵션들을 확인할 수 있다.
  - DESC [개체명];
- DML로 변경한 데이터는 SELECT문으로 데이터의 정상적인 변경 여부를 확인할 수 있다.
- DCL로 설정된 사용자 권한은 사용자 권한 정보가 저장된 테이블을 조회하여 확인할 수 있다.

### 12.2. 절차형 SQL 테스트
- 프로시저, 사용자 정의 함수, 트리거 등의 절차형 SQL은 디버깅을 통해 기능의 적합성 여부를 검증하고, 실행을 통해 결과를 확인하는 테스트를 수행한다.
- 많은 코드로 구성된 절차형 SQL의 특성상 오류 및 경고 메시지가 상세히 출력되지 않으므로 SHOW 명령어를 통해 오류 내용을 확인하고 문제를 수정한다.
  - 형식 : SHOW ERRORS;
- 데이터베이스에 변화를 줄 수 있는 SQL문은 주석으로 처리하고, 출력문을 이용하여 화면에 출력하여 확인한다.
- 디버깅이 완료되면 출력문을 삭제하고, 주석 기호를 삭제한 후 절차형 SQL을 실행하여 결과를 검토한다.

## 13. ORM(Object-Relational Mapping)
- 객체지향 프로그래밍의 객체와 관계형 데이터베이스의 데이터를 연결하는 기술을 의미한다.
- 객체지향 프로그래밍에서 사용할 수 있는 가상의 객체지향 데이터베이스를 만들어 프로그래밍 코드와 데이터를 연결한다.
- ORM으로 생성된 가상의 객체지향 데이터베이스는 프로그래밍 코드 또는 데이터베이스와 독립적이므로 재사용 및 유지보수가 용지하다.

### 13.1. ORM 프레임워크
- ORM을 구현하기 위한 구조와 구현을 위해 필요한 여러 기능들을 제공하는 소프트웨어를 의미한다.

### 13.2. ORM의 한계
- 프레임워크가 자동으로 SQL을 작성하기 때문에 의도대로 SQL이 작성되었는지 확인해야 한다.
- 객체지향적인 사용을 고려하고 설계된 데이터베이스가 아닌 경우 프로젝트가 크고 복잡해질 수록 ORM 기술을 적용하기 어렵다.
- 기존의 기업들은 ORM을 고려하지 않은 데이터베이스를 사용하고 있기 때문에 ORM에 적합하게 변환하려면 많은 시간과 노력이 필요하다.