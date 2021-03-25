## SQL 기본

* 데이터 조회
* SELECT 문
* SQL --> 1개 문장 실행
* PL/SQL --> 반복, 조건, 변수 선언 x





#### 그룹 함수

| sum      | 총합 - 숫자                                                  |
| -------- | ------------------------------------------------------------ |
| avg      | 평균 - 숫자                                                  |
| count    | 갯수 - 숫자, 문자, 날짜<br />null 값 제외<br />count(*)  -> null 값 포함 |
| max      | 최대값 - 숫자, 문자, 날짜                                    |
| min      | 최소값 - 숫자, 문자, 날짜                                    |
| stdev    | 표준편차 - 숫자                                              |
| variance | 분산 - 숫자                                                  |

* 그룹 함수 조회하는 select 절에 다른 column 기술 불가능 
  * 단, GROUP BY 절의 column 제외





#### GROUP BY

* 어떤 항목별로 나눠서 조회

```sql
SELECT department_id, sum(salary) FROM employees GROUP BY department_id;
```

* 여러 개의 항목으로 나눠서 조회

```sql
SELECT department_id, job_id, sum(salary) FROM employees
GROUP BY department_id, job_id;
```

* WHERE 절은 GROUP BY 절보다 먼저 작성

```sql
SELECT department_id, sum(salary) FROM employees 
WHERE department_id is not null GROUP BY department_id;
# 부서별 salary 총합을 조회
```





#### HAVING

* 그룹 함수 조건식을 나타낼 때 사용
* 부서별로 급여 총합을 조회하되 부서별 급여 총합이 10000 미만인 부서의 결과만 조회

```sql
SELECT department_id, sum(salary) FROM employees
WHERE sum(salary) < 10000 GROUP BY department_id;

# 1. FROM -> 2. WHERE -> 3. GROUP BY - 4. SELECT 순으로 실행
# 3, 4번이 실행되어야 2번이 실행될 수 있는 잘못된 코드
```

```sql
SELECT department_id, sum(salary) FROM employees
GROUP BY department_id HAVING sum(salary) < 10000; 

# GROUP BY 절에 대한 조건을 제시해 주는 HAVING 사용
# 1. FROM -> 2. WHERE -> 3. GROUP BY - 4. HAVING -> 5. SELECT 순으로 실행
# WHERE (일반 조건식)
# HAVING (그룹 함수 조건식)
```

* 부서별로 급여 총합을 조회하되 사원의 급여가 5000 미만은 제외하고 부서별 급여 총합이 10000 미만인 부서의 결과만 조회

```sql
SELECT department_id, sum(salary) FROM employees WHERE salary >= 5000 
GROUP BY department_id HAVING sum(salary) >= 50000;
```





#### ROLLUP ()

* ROLLUP (a, b)
  * a와 b에 동시에 해당하는 값들의 합을 조회 + a에 해당하는 값들의 합 조회

```sql
SELECT department_id, job_id, sum(salary) FROM employees
GROUP BY ROLLUP(department_id, job_id);

# 같은 부서에 있는 job_id 별로 salary 합을 보여주고 department_id 별 salary 합도 조회
```





#### CUBE ()

* CUBE (a, b)
  * b에 해당하는 값들의 합 조회 + a에 해당하는 값들의 합 조회

```sql
SELECT department_id, job_id, sum(salary) FROM employees
GROUP BY CUBE(department_id, job_id);

# job_id 별로 salary 합을 보여주고 department_id 별 salary 합도 조회
```





#### 데이터 형식

| 문자<br />(VARCHAR2 사용)     | CHAR, VARCHAR2 -> 영문: 1byte<br />                                       한글: 3byte<br />CHAR(50) --> 'ABC'<br />--> [ABC + 47byte 고정]<br />VARCHAR2(50) --> 'ABC'<br />--> [ABC]<br />NCHAR, NVARCHAR2 -> 유니코드 2byte 한글<br />'데이터' --> 대소문자 구분 |
| ----------------------------- | ------------------------------------------------------------ |
| 정수<br />(NUMBER(8) 사용)    | BINARY_INT, INT, NUMBER(8), NUMBER(8, 0)                     |
| 실수<br />(NUMBER(8, 2) 사용) | BINARY_FLOAT, FLOAT, NUMBER(8, 2) --> 정수 6, 소수 2         |
| 날짜<br />(DATE 사용)         | 초 표현 -> DATE<br />1/1000초 표현 -> TIMESTAMP, TIMESTAMPXXXXX |
| 대용량                        | CLOB - 1TB 문자열 대용량 데이터<br />웹서버(java) - 네트워크 -> DB<br />BLOB - 1TB binary 대용량 데이터<br />BFILE<br />BIN |





#### DUAL

* dual table --> 가상 임시 테이블
* 함수 결과 조회 시 사용, 1행 구성

```sql
SELECT sysdate FROM dual;

# ==> 날짜(RR/MM/DD)
```