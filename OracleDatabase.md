## Oracle Database

* 자바 프로그래밍 언어-컴퓨터 다목적 프로그램 제작
* 데이터들 --> 영구적 파일/데이터베이스 저장





#### 파일 텍스트들

1. 데이터 분리 기준이 없다.
2. 데이터 type 기준이 없다.
3. 특정 위치의 데이터에 접근이 불가능하다.
4. 중복 데이터가 가능하다.
5. 데이터 현재 상태 잘못된 표현





#### 데이터 베이스

* 의미 있는 데이터 모음

  * 예시
    * 학생 = (학번 이름 전공 학교명 학년 성적)
    * 회사원 = (사번 이름 부서 회사명 직급 급여)

* 데이터 베이스 표현 방법

  1. 계층형

  2. 네트워크형

  3. 관계형 / 객체 관계형 데이터 베이스 제품

     Ex) oracle / mysql / ms sqlserver / db2 / ...

  4. 관계형 (relational DB-RDB)

     * 데이터 관계를 행과 열의 테이블 구조로 표현
     * 열(COLUMN) - 데이터의 구성요소로서 1개 표현 단위
     * 행(ROW) - 1개 데이터 RECORD

     * 학생 테이블

| 학번<br />정수 3자리<br />중복 x | 이름<br />문자 10자리 | 성적<br />실수 5자리(2자리) |
| -------------------------------- | --------------------- | --------------------------- |
| 100                              | 이학생                | 4.5                         |
| 100                              | xxxx                  | xxxx                        |

   * 데이터 테이블
        * 테이블 생성-삭제
        * 데이터 1개 ROW 저장-삭제
        * 데이터들 조회
        * 함수들 = 자바 메소드

* 관계형 데이터베이스 활용 문법 언어
  * SQL
    1. RDB 제품 종류에 관계없이 사용 "표준" SQL
    2. 각 RDB 독자적 SQL





#### Oracle

* 설치
  * oracle 11g express edition
    * 무료, 크기 제한, 1개만 사용
    * system schema = 계정 (super user)
    * hr(잠겨 있음) - 테이블들 조회 실습
  * enterprise / standard edition  
* 삭제
  * 제어판 - 프로그램 및 기능 - oracle 11g xe .. 제거





#### SQL 입력 실행 tool

1. RUN SQL Command Line 실행
2. SQL Developer / Orange / Toad / Eclipse data explorer 기능





#### SQL Command Line

##### oracle 독자적 sql

* SQL> connect system / 암호 
* SQL> alter user hr identified by hr account unlock;
* SQL> disconnect
  * 입력하지 않아도 connect 두번째 계정에 접속됨.
* SQL> connect hr/hr -> hr 테이블 실습 가능 
* SQL> select * from tab;
  * set pagesize 100; -> table 크기 설정
  * set linesize 150; -> table 크기 설정
* SQL> disconnect
* 4글자 축약 가능 (conn, disconn) / 대소문자 구분 x (단, 암호나 ''문자 데이터는 대소문자 구분)

##### SQL 종류

* Data Definition Language (DDL)
  * 테이블 생성 - 변경 - 삭제
  * 데이터 저장 구조를 정의하는 언어
* Data Manipulation Language (DML)
  * 테이블에 데이터 저장 - 수정 - 삭제
  * 데이터 조작 언어
* Data Controll Language (DCL)
  * 계정 생성 - DB 접속 허용
  * System 계정
* Transaction Controll Language (TCL)
  * 트랜잭션 제어 언어
* Data Query Language (DQL)
  * 데이터 조회

| DDL       | create table ...<br />alter table<br />drop table |
| --------- | ------------------------------------------------- |
| DML       | insert ...<br />update<br />delete                |
| DCL       | grant, revoke<br />새로운 계정 생성시 사용        |
| TCL       | commit<br />rollback                              |
| ***DQL*** | ***select***                                      |





#### hr 8개 테이블 조회 실습

* conn hr/hr;

* select * from tab; --> 테이블 목록 조회

* 문법

  * select 조회 column from 테이블 이름;
    * column명 = *  --> 모든 column 조회
    * select
    * from
    * where
    * group by
    * having
    * order by

  * desc 테이블 이름;
    * 테이블 column명 타입 갯수

```sql
SELECT first_name FROM Employees;
SELECT * FROM Employees;
```

* 사칙 연산 가능

```sql
SELECT first_name, salary, salary * 12 FROM Employees;
```

* 실제 column명을 조회 임시 변경 - alias 

```sql
SELECT first_name as 이름, salary as 월급, salary * 12 as 연봉 FROM Employees;
```

* 종류별로 1개만 조회

```sql
SELECT distinct job_id from Employees; 
```

* upper()
  * 소문자를 대문자로 변경

```sql
SELECT first_name, upper(first_name) FROM Employees;
```

* rownum
  * 조회하는 행 번호 생성 함수

```sql
SELECT rownum, hire_date from employees;
# 행 번호까지 출력
```





#### WHERE

* 특정 조건에 해당하는 데이터만 조회

```sql
SELECT first_name, salary FROM Employees WHERE salary >= 10000;
```

* in ()

```sql
SELECT employee_id, first_name FROM Employees 
WHERE employee_id in (50, 100, 150, 200, 250, 300);
```

* like '%' '___'
  * 유사한 패턴을 찾을 때

```sql
SELECT employee_id, first_name FROM employees 
WHERE first_name like 'J%'; # J로 시작하는 이름을 가진 직원 조회
SELECT job_id FROM employees 
WHERE job_id like '___MAN'; # MAN 앞에 세 자리 
SELECT job_id FROM employees 
WHERE job_id like '__\_MAN' escape '\' ;
```

* 연산자

| 산술 연산자                          | + - * /                                                      |
| ------------------------------------ | ------------------------------------------------------------ |
| 비교 연산자                          | > >= < <= != (<>) =                                          |
| 논리 연산자                          | not and or                                                   |
| 목록 연산자                          | in (...)                                                     |
| 유사 연산자<br />(문자데이터만 가능) | `LIKE` <br />% - 모든 문자, 문자 갯수 상관 없다<br />_ - 모든 문자 1개 |
| 범위 연산자                          | between ~ and                                                |
| null 처리 연산자                     | order by 1 asc --> null 마지막<br />order by 1 desc --> null 처음<br />is null<br />is not null <br />=null (x)<br />!=null (x) |

* 현재 시스템 날짜 시각 정보

```sql
SELECT sysdate FROM dual;
```

* rr/mm/dd
  * rr ---> 0 - 49 값 2000년대 / 50 - 99 값 1900년대
  * 날짜 비교 가능

```sql
SELECT first_name as 이름, hire_date as 입사일 
from employees where hire_date >= '05/01/01'and hire_date <= '05/12/31';
SELECT first_name as 이름, hire_date as 입사일 
from employees where hire_date between '05/01/01' and '05/12/31';
# 05년도 입사자 찾아서 조회
```





#### ORDER BY

```sql
SELECT first_name FROM employees ORDER BY first_name asc; # 알파벳 순서대로 a - z / 가 - 하
SELECT first_name FROM employees ORDER BY first_name desc; # 알파벳 역순으로
SELECT salary FROM employees ORDER BY salary desc; # 숫자가 큰 순으로
SELECT hire_date FROM employees ORDER BY hire_date asc; # 오래된 순서대로
```

* order 기준을 두 개 이상으로 잡을 경우

```sql
SELECT first_name, salary FROM employees ORDER BY salary desc, first_name asc;
# 같은 급여를 가지고 있는 사람들은 알파벳 순으로 조회
```

* asc는 생략 가능하지만 desc는 생략할 수 없다.
* 조회한 데이터가 여러 개라면 첫 번째가 1, 두 번째가 2 .... (index 개념)

```sql
SELECT hire_date, first_name FROM employees ORDER BY 1; # hire_date 오름차순으로 정렬
```

* ORDER BY 뒤에 column 이름 대신에 alias 입력 가능
* 최근 입사자 5명만 조회

```sql
SELECT hire_date from employees order by hire_date desc;
```





#### 작성순서 및 실행순서

* 작성순서
  * select
  * from
  * where
  * order by
* 실행순서
  * from 테이블 찾는다
  * where 조건에 맞는 레코드를 찾는다
  * select 컬럼 조회한다
  * order by 정렬 기준 컬럼 정렬한다 = 순서 뒤바뀐다





#### subquery

> 1. 정렬 이후 상위 몇 개를 가져올 경우 
>    * TOP-N QUERY

```sql
SELECT hire_date
FROM (SELECT * FROM employees ORDER BY hire_date desc) # subquery
WHERE rownum <= 5;
```

> 2. 다른 테이블의 정보를 조건으로 설정할 경우

```sql
SELECT first_name, department_id from employees
WHERE department_id = (SELECT department_id FROM departments
                      WHERE department_name = 'Sales');
```

> 3. William보다 더 급여를 많이 받거나 동일하게 받는 사원 조회
>
>    * 모든 William의 급여와 같거나 많을 때
>
>      ```sql
>      SELECT employee_id, first_name, salary FROM employees WHERE salary >= all(SELECT salary FROM employees WHERE first_name = 'William');
>      ```
>
>    * 1명의 William 급여와 같거나 많을 때
>
>      ```sql
>      SELECT employee_id, first_name, salary FROM employees WHERE salary >= any(SELECT salary FROM employees WHERE first_name = 'William');
>      ```

> 4. 단일행 리턴
> 5. 다중행 리턴 
>    * IN, NOT IN, all, any