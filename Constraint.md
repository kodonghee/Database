## Constraint (제약조건)

* 제약 조건
* 현실 세계 모델링
* 테이블





#### 제약 조건 타입

1. 중복 x --> UNIQUE
2. null 값 허용 x --> not null
3. 중복 x + null x --> primary key
4. 다른 테이블 포함 값 사용 가능 --> foreign key
5. 사용자 조건 --> CHECK

| c_dept 모든 부서 정보                                        | c_emp 모든 사원 정보                                     |
| ------------------------------------------------------------ | -------------------------------------------------------- |
| dept_id         10          20<br />dept_name 인재개발부  교육부<br />city               제주        서울 | emp_id<br />emp_name<br />title<br />salary<br />dept_id |

* DDL인 CREATE TABLE에서 제약조건 정의
* DML인 INSERT, UPDATE, DELETE 사용 시 제약 조건 효력 발생 

```sql
CREATE TABLE c_dept(
dept_id number(5) constraint c_dept_id_pk primary key,
dept_name varchar2(20) constraint c_dept_name_uk unique,
city varchar2(20) constraint c_dept_city_nn not null);
```

```sql
CREATE TABLE c_emp(
emp_id number(5) constraint c_emp_emp_id_pk primary key,
emp_name varchar2(20) constraint c_emp_name_nn not null,
title varchar2(10) constraint c_emp_title_ck check (title in ('사원', '대리', '과장', '부장', '임원')),
salary number(12, 2) constraint c_emp_salary_ck check(salary >= 1000), 
dept_id number(5) constraint c_emp_dept_id_fk references c_dept(dept_id));
```

* CASCADE constraints
  * 제약조건 무시
* RENAME 이전table이름 to 새로운table이름