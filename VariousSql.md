| DDL - 자동 commit<br />DROP TABLE emp;                       | CREATE<br />ALTER<br />DROP    |
| ------------------------------------------------------------ | ------------------------------ |
| DML - commin \| rollback 결정<br />transaction 처리 언어<br />1. commit 하지 않은 상태 \| rollback 상태 이면<br />다른 session에 처리 결과 미반영<br />2. commit 하면 다른 session에 처리 결과 반영 | INSERT<br />UPDATE<br />DELETE |





## DDL

* table 정의 | table 구조 변경 | 삭제





#### Table 정의

* CREATE TABLE  이름 (
  	COLUMN명 타입(길이) 제약조건

  ​	COLUMN명 타입(길이) 제약조건

  ​	...

  ​	COLUMN명 타입(길이) 제약조건

  );

* Table/Column

  * 숫자 시작 불가능
  * Oracle keyword 불가능
  * 길이 제한

* emp Table

| id 정수 5자리                         | 사번      |
| ------------------------------------- | --------- |
| name 문자열 20자리                    | 이름      |
| title 문자열 20자리                   | 직급      |
| dept_id 정수 5자리                    | 부서 코드 |
| salary 실수 정수 10자리\|소수점 2자리 | 급여      |

```sql
CREATE TABLE emp(
id number(5),
name varchar2(20),
title varchar2(20),
dept_id number(5),
salary number(12, 2));
```

* 다른 table의 데이터 가져와서 table 생성
  * Table 생성 + 데이터 복사

```sql
CREATE TABLE emp_copy AS SELECT employee_id, first_name FROM employees;
```





#### Table 변경

* ALTER TABLE 이름(ADD COLUMN명 타입(길이) 제약조건);

  * emp table에 입사일 저장 column 추가

  ```sql
  ALTER TABLE emp ADD indate date;
  ```

* ALTER TABLE 이름(MODIFY COLUMN명 타입(길이) 제약조건);

  * emp table에 title column 길이를 20자리에서 10자리로 변경

  ```sql
  ALTER TABLE emp modify title varchar2(10);
  ```

  * 길이 축소가 불가능한 경우
    * 이미 원하는 자릿수가 넘는 데이터를 저장한 상태일 경우
  * 길이 확장은 항상 가능

* ALTER TABLE 이름(DROP column COLUMN명);

  * emp table에서 입사일 column 삭제

  ```sql
  ALTER TABLE emp drop column indate;
  ```

  



#### Table 삭제

* DROP TABLE 이름;

  * 복구 불가능

  ```sql
  DROP TABLE emp;
  ```

  



#### 사용자 만들기

* DB Table 소유주 = schema = 사용자 = 계정
* 사용자 생성 권한 - system 계정

```sql
# jdbc 계정 생성

conn system/system
create user jdbc identified by jdbc  # DDL
grant resource, connect to jdbc;  # Data Controll Language (DCL)
(revoke resource, connect from jdbc)  # DB 사용 권한 빼앗기
grant select on employees to jdbc; # DB 조회 권한만 부여
conn jdbc/jdbc
```





## DML

* 데이터 저장 | 수정 | 삭제





#### 데이터 저장

* emp table에 데이터 저장

```sql
INSERT INTO emp values(100, '이사원', '사원', 10, 99000.5);
INSERT INTO emp values(200, '김대리', null, null, null);

# 데이터를 넣을 column 이름을 () 안에 설정
# 나머지 column의 값은 null
INSERT INTO emp(id, name) values(300, '박과장');

INSERT INTO emp values(400, '최부장', '부장', 20, 99000.5);
INSERT INTO emp values(500, '박대리', '대리', 20, 99000.5);
```

* INSERT 수행 후 DB 영구 저장하는 sql 입력 필요 --> commit;
  * DB에 반영
  * 다른 session에서도 insert 결과가 보임
* INSERT 수행 후 DB 취소하는 sql --> rollback;
  * 메모리 삭제
  * 다른 session에서 insert 결과 미반영

* INSERT 수행 직후는 메모리에 데이터가 **임시** 저장된 상태
  * 다른 session에서는 결과가 보이지 않음
* subquery 사용

```sql
INSERT INTO emp (id, name, title, dept_id, salary) 
SELECT employee_id, first_name, job_id, department_id, salary 
FROM employees;  
```





#### 데이터 수정

* UPDATE

  * 조건에 맞는 column만 데이터 수정

    ```sql
    UPDATE emp SET title = '대리' WHERE id = 200;
    UPDATE emp SET salary = 1000 WHERE salary is null;
    
    UPDATE emp SET dept_id = (SELECT dept_id FROM emp WHERE name = '이사원') WHERE name = '박대리';
    ```

  * UPDATE  table명  SET  변경column명 = 변경값 --> table 모든 행 변경

  * 프로그램 두 개가 동시에 같은 데이터의 update 시도할 경우

    * 먼저 update 완료한 session에서 commit | rollback 하지 않은 상태로 

      다른 session이 update를 시도한다면 lock (정지)이 걸리게 됨





#### 데이터 삭제

* DELETE

  * DELETE  table명  WHERE  삭제조건식 --> 조건에 맞는 데이터행 삭제 | table 구조 남김
  * DELETE  table명 --> table 모든 데이터행 삭제 | table 구조 남김

  * DROP TABLE과 다르게 rollback 가능





## Sequence

* 자동으로 증가하는 sequence = 객체
  * CREATE TABLE
  * CREATE USER
  * CREATE SEQUENCE

* 숫자 데이터 값을 자동 증가시키는 객체 = sequence





#### sequence 생성

* CREATE SEQUENCE 시퀀스이름 --> 1부터 시작하고 1씩 증가

* CREATE SEQUENCE 시퀀스이름(

  start with 10,

  increment by 5,

  maxvalue 100

  )  --> 10부터 시작하고 5씩 증가해서 100까지





#### sequence 활용

* 시퀀스이름.currval 

  * 시퀀스가 증가되서 어디까지 증가했는지 보여줌

* 시퀀스이름.nextval 

  * 호출할 때마다 증가되고 증가된 값을 보여줌
  * 최초에 호출하면 시작 값을 보여줌

  ```sql
  # id가 중복되지 않게 정해진 sequence의 다음 숫자를 id로 하여 데이터 저장
  
  INSERT INTO emp values(emp_seq.nextval, '이자바', '사원', 30, 45000.55);
  ```

  





#### sequence 수정 | 삭제

* 수정
  * ALTER SEQUENCE 시퀀스이름 start with 10
  * ALTER SEQUENCE 시퀀스이름 increment by 5
* 삭제
  * DROP SEQUENCE 시퀀스이름