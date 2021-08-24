<br/>

## 데이터 모델링

<br/><hr><br/>

## SQL

<br/>

### SQL 언어
**1) 데이터 조작어 (DML: Data Manipulation Language)** 
- `SELECT`: 데이터 조회
~~~sql
-- *: 컬럼 전체 조회
-- DISTINCT: 중복 데이터 제외
SELECT [* | DISTINCT] FROM 테이블명;
~~~
<br/>

- `INSERT`: 데이터 삽입    
~~~sql
-- 컬럼리스트를 생략할 경우 전체컬럼을 입력해야 함.
-- 컬럼리스트와 컬럼값을 1:1 매핑하여 작성
INSERT INTO 테이블명 ('컬럼리스트')
VALUES ('컬럼값');
~~~
<br/>

- `UPDATE`: 데이터 수정
~~~sql
UPDATE 테이블명
SET 컬럼명=값;
~~~
<br/>


- `DELETE`: 데이터 삭제
~~~sql
DELETE FROM 테이블명;
~~~
<br/> <br/>


**2) 데이터 정의어 (DDL: Data Definition Language)**
> ORACLE에서는 DDL 수행 후 Auto Commit을 수행한다. <br/> 
SQL Server에서는 DDL 수행 후에도 Auto Commit 되지 않는다.

<br/>

- `CREATE`: 테이블 생성
>❗️ 테이블 생성 시 주의사항 <br/>
> - 테이블명은 중복될 수 없다.
> - 한 테이블 내에서는 컬럼명이 중복될 수 없다.
> - 테이블 이름을 지정하고 각 컬럼들은 괄호 "()" 로 묶어 지정한다.
> - 각 컬럼들은 콤마 "," 로 구분하고, 테이블 생성문의 끝은 항상 세미콜론 ";" 으로 묶어 지정한다.
> - 컬럼 뒤에 데이터 유형은 꼭 지정되어야 한다.
> - 테이블명과 컬럼명은 반드시 문자로 시작해야한다.
> - 허용되는 문자: A-Z, a-z, 0-9, _, $, #
> - 테이블명으로 예약어를 사용할 수 없다.

~~~sql
CREATE TABLE 테이블명 (
    컬럼명1 DataType,
    컬럼명2 DataType,
    ...
);
~~~

<br/>

- `ALTER`: 테이블 수정
~~~sql
ALTER TABLE 테이블명 ADD 추가컬럼명 데이터유형;
ALTER TABLE 테이블명 DROP COLUMN 삭제할컬럼명;

/* 테이블 칼럼에 대한 정의 변경 */
-- ORACLE
ALTER TABLE 테이블명 MODIFY (컬럼명 데이터유형 [NOT NULL]);
-- SQL Server
ALTER TABLE 테이블명 ALTER COLUMN 컬럼명 데이터유형 [NOT NULL];

~~~
<br/>

- `DROP`: 테이블 삭제
~~~sql
-- CASCADE: 외래키로 물린 하위테이블까지 삭제
DROP TABLE 테이블명 [CASCADE CONSTRAINT];
~~~
<br/>

- `TRUNCATE`: 테이블 안의 데이터 삭제
~~~sql
-- 테이블 자체가 삭제되는 것이 아님
-- 해당 테이블의 모든 행들이 제거되지만, 해당 테이블의 스키마 정의는 유지되어있음.
TRUNCATE TABLE 테이블명;
~~~
<br/>

- `RENAME`: 테이블 이름 변경
~~~sql
RENAME 기존테이블명 TO 바꿀테이블명;
~~~
<br/> <br/>

> ❓DROP / TURNCATE / DELETE의 차이

|DROP|TURNCATE|DELETE
|-|-|-|
|DDL|DDL<br>일부 DML의 성격|DML
|Auto Commit|Auto Commit|사용자 Commit
|테이블이 사용했던 Storage를 모두 Release|테이블이 사용했던 Storage중 최초 테이블 생성시 할당된 Storage만 남기고 Release|데이터를 모두 Delete해도 사용했던 Storage는 Release되지 않음
|테이블 정의 자체를 삭제|테이블을 최초 생성된 초기상태로|데이터만 삭제

<br/><br/>

**3) 데이터 제어어 (DCL: Data Control Language)**
- `GRANT`: 권한 부여
~~~sql
-- Ojbect에 대한 권한 부여

-- ROLE을 이용한 권한 부여
~~~
<br/>

- `REVOKE`: 권한 삭제
~~~sql

~~~
<br/> <br/>


**4) 트랜잭션 제어어 (TCL: Transation Control Language)**
- `COMMIT`: 트랜잭션을 DB에 반영시키는 명령어
- `ROLLBACK`: 트랜잭션 수행 이전의 상태로 되돌리는 명령어

> ❓트랜잭션의 4가지 특성
- `원자성(atomicity)`: 트랜잭션에서 정의된 연산들은 모두 성공적으로 실행 or 전혀 실행되지 않은 상태로 남아있어야 함. (All or Nothing)
- `일관성(consistency)`: 트랜잭션이 실행되기 전의 데이터베이스 내용이 잘못 되어있지 않다면, 트랜잭션이 실행된 이후에도 데이터베이스의 내용에 잘못이 있으면 안된다.
- `고립성(isolation)`: 트랜잭션이 실행되는 도중에 다른 트랜잭션의 영향을 받아 잘못된 결과를 만들어선 안된다.
- `지속성(durability)`: 트랜잭션이 성공적으로 수행되면, 그 트랜잭션이 갱신한 데이터베이스의 내용은 영구적으로 저장된다.

<br/> <hr> <br/>

### 제약조건
- `PRIMARY KEY` (기본키): NOT NULL + UNIQUE
    + 하나의 테이블에 하나만 지정가능하다.
    + 자동으로 Unique index 생성됨
- `UNIQUE` (고유키)
    + 중복 불가
    + NULL은 허용
- `NOT NULL`
    + NULL 허용 X
- `CHECK`
    + 입력 값 범위 제한
    + TRUE or FALSE 논리식 지정
- `FOREIGN KEY` (외래키)
    + 다른 테이블의 기본키 or 고유키를 참조
    + 외래키 값은 NULL이거나 부모테이블 키의 값과 동일해야 함.
    + **외래키 옵션**
        - on delete(ou update) cascade: 부모 데이터 삭제(업데이트) 시 자식 데이터도 삭제(업데이트)
        - on delete(on update) set null: 부모 데이터 삭제(업데이트) 시 자식 데이터도 null값으로 변경
        - on delete(on update) set default: 부모 데이터 삭제(업데이트) 시 자식 데이터를 default값으로 변경
        - on delete(on update) restrict: 참조하고 있을 경우, 데이터 삭제(업데이트) 불가능
        - on delete(on update) no action : default, 아무 일도 하지 않음


<br/><hr><br/>