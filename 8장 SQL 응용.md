# 8장 SQL 응용

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