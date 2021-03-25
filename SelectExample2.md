1. 이름이 'adam' 인 직원의 급여와 입사일을 조회하시오.

```sql
SELECT salary, hire_date FROM employees WHERE LOWER(first_name) = 'adam';
```

2. 나라 명이 'united states of america' 인 나라의 국가 코드를 조회하시오.

```sql
SELECT country_id FROM countries 
WHERE LOWER(country_name) = 'united states of america';
```

3. 'Adam의 입사일은 05/11/2 이고, 급여는 7,000￦ 입니다.' 의 형식으로 직원 정보를 조회하시오.

```sql
SELECT first_name || '의 입사일은 ' || TO_CHAR(hire_date, 'YY/MM/fmDD')
|| ' 이고, 급여는 ' || TO_CHAR(salary, '99,999L') || ' 입니다.' FROM employees;
```

4. 이름이 5글자 이하인 직원들의 이름, 급여, 입사일을 조회하시오.

```sql
SELECT first_name, salary, hire_date FROM employees WHERE LENGTH(first_name) <= 5;
```

5. 06년도에 입사한 직원의 이름, 입사일을 조회하시오.

```sql
SELECT first_name, hire_date FROM employees 
WHERE SUBSTR(hire_date, 1, 2) = '06';
```

6. 10년 이상 장기 근속한 직원들의 이름, 입사일, 급여, 근무년차를 조회하시오.

```sql
SELECT first_name, hire_date, salary, (sysdate - hire_date) / 365 as 근무년차
FROM employees WHERE (sysdate - hire_date) / 365 >= 10;
```

7. employees 테이블에서 직종이(job_id) 'st_clerk'인 사원 중에서 급여가 1500 이상인 사원의
   first_name, job_id, salary 를 조회하시오. 단 이름은 모두 대문자로 출력하시오.

```sql
SELECT UPPER(first_name), job_id, salary FROM employees 
WHERE LOWER(job_id) = 'st_clerk' and salary >= 1500;
```

8. 급여가 20000 이상인 직종(job_id)의 job_id, 급여 합계 조회하시오.
   단, 급여 합계는 10자리로 출력하되 공백은 '0'으로 표시하시오.

```sql
SELECT job_id, TO_CHAR(sum(salary), '0000000000') as 급여합계 
FROM employees GROUP BY job_id HAVING sum(salary) >= 20000; 
```

9. 직원의 이름, 급여, 직원의 관리자 사번을 조회하시오. 단, 관리자가 없는 직원은
      '<관리자 없음>'이 출력되도록 합니다.

```sql
SELECT first_name, salary, NVL(TO_CHAR(manager_id), '<관리자 없음>') as manager_id
FROM employees;
```