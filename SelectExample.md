1. 2002년 이후에 입사한 직원들 중에서 급여가 13000 이상 20000 이하인 직원들의 이름, 급여, 부서id, 입사일을 조회하시오.

```sql
SELECT first_name, salary, department_id, hire_date FROM employees 
WHERE (salary between 13000 and 20000) and (hire_date >= '02/01/01');
```

2. 근무한지 10년이 넘은 사원의 이름과 근무년수를 조회하시오.

```sql
SELECT first_name, ((SELECT sysdate FROM dual) - hire_date)/365 as 근무년수 
FROM employees WHERE ((SELECT sysdate FROM dual) - hire_date)/365 > 10;
```

3. 직원 중에서 상관이 없는 직원의 이름과 급여를 조회하시오. 상관의 정보는 manager_id 컬럼에 있습니다.

```sql
SELECT first_name, salary FROM employees WHERE manager_id is null;
```

4. 10, 50 번 부서에 속해있으면서 급여가 13000 이상인 직원의 이름, 급여, 부서id를 조회하시오.

```sql
SELECT first_name, salary, department_id FROM employees 
WHERE salary >= 13000 and department_id in (10, 50);
```

5. 직종이 clerk 직종인 직원의 이름, 급여, 직종코드를 조회하시오. (clerk 직종은 job_id에 CLERK을 포함하거나 CLERK으로 끝난다.)

``` sql
SELECT first_name, salary, job_id FROM employees WHERE job_id LIKE '%CLERK%';
```

6. 12월에 입사한 직원의 이름, 급여, 입사일을 조회하시오.

```sql
SELECT first_name, salary, hire_date FROM employees 
WHERE hire_date LIKE '__/12/__';
```

7. 이름이 m으로 끝나는 직원의 이름, 급여, 입사일을 조회하시오.

```sql
SELECT first_name, salary, hire_date FROM employees 
WHERE first_name LIKE '%m';
```

8. 이름의 세번째 글자가 d인 이름, 급여, 입사일을 조회하시오.

```sql
SELECT first_name, salary, hire_date FROM employees 
WHERE first_name LIKE '__d%';
```

9. 커미션을 받는 직원의 이름, 커미션, 총급여를 조회하시오. (총급여는 커미션*급여로 계산합니다.)

```sql
SELECT first_name, commission_pct, commission_pct*salary as totalsalary 
FROM employees WHERE commission_pct is not null;
```

10. 커미션을 받지 않는 직원의 이름, 급여를 조회하시오.

```sql
SELECT first_name, salary FROM employees WHERE commission_pct is null;
```

11. 10월에 입사해서 30, 50, 80 번 부서에 속해있으면서, 
    급여를 5000 이상 17000 이하를 받는 직원을 조회하시오. 
    단, 커미션을 받지 않는 직원들은 검색 대상에서 제외시키며, 먼저 입사한 직원이 
    먼저 출력되어야 하며 입사일이 같은 경우 급여가 많은 직원이 먼저 출력되록 하시오.

```sql
SELECT first_name, salary, hire_date, department_id FROM employees 
WHERE department_id in (30, 50, 80) and hire_date LIKE '__/10/__' and 
salary between 5000 and 17000 and commission_pct is not null 
ORDER BY hire_date, salary desc;
```

12. jobs 테이블
    job_id : 직종코드
    job_title : 직종명
    max_salary : 해당직종 최대급여
    min_salary : 해당직종 최소급여

    jobs 테이블에서 회장과 부회장의 직종명, 최소급여,최대급여를 조회하시오.
     job_title은 직종명이고 회장은 president, 부회장은 vise president를 포함합니다.

```sql
SELECT job_title, min_salary, max_salary FROM jobs 
WHERE job_title LIKE '%President%' or job_title LIKE '%Vise President%';
```

13. countries 테이블
    country_id : 국가코드
    country_name : 국가이름

    countries 테이블에서 국가이름이 United Kingdom 인 국가의
    국가코드를 조회하시오.

```sql
SELECT country_id FROM countries WHERE country_name = 'United Kingdom';
```

14. locations 테이블
    city : 도시이름
    country_id : 도시가 위치한 국가코드

    13번에서 조회한 결과를 이용하여 United Kingdom에 위치한
    도시이름을 조회하시오.

```sql
SELECT city FROM locations WHERE country_id = 
(SELECT country_id FROM countries WHERE country_name = 'United Kingdom');
```