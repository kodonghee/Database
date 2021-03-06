## JOIN

* 두 개 이상의 table의 열을 합칠 때 사용

```sql
#employees와 departments 두 개의 table 정보 모두 출력하고 싶을 때

SELECT employee_id, first_name, department_id FROM employees;
SELECT department_id, department_name FROM departments;
```





#### JOIN

* subquery로 해결이 안됨

* 사용할 수 있는 다른 구문

  * INNER JOIN

  ```sql
  # department_id가 null인 경우 조회되지 않음
  # 양쪽 테이블에 모두 존재하는 데이터만 조회
  
  SELECT employee_id, first_name, e.department_id, department_name
  FROM employees e, departments d
  WHERE e.department_id = d.department_id;
  ```

  * OUTER JOIN
    * 한쪽 테이블에만 존재하는 데이터도 조회 
    * 데이터 없는 쪽에 (+)

  ```sql
  # 부서에 속해 있지 않은 사원도 조회
  
  SELECT employee_id, first_name, e.department_id, department_name
  FROM employees e, departments d
  WHERE e.department_id = d.department_id(+);
  ```

  ```sql
  # 직원이 없는 부서도 조회
  
  SELECT employee_id, first_name, e.department_id, department_name
  FROM employees e, departments d
  WHERE e.department_id(+) = d.department_id;
  ```

* 3개의 table에서 정보 조회하기 

```sql
SELECT first_name, department_name, city 
FROM employees e, departments d, locations l 
WHERE e.department_id = d.department_id AND d.location_id = l.location_id;
```

```sql
SELECT first_name, department_name, city 
FROM employees e, departments d, locations l 
WHERE e.department_id = d.department_id AND d.location_id = l.location_id
AND UPPER(city) = 'LONDON';
```

* 같은 table 복사해서 조회하기

  * JOIN할 table이 자기 자신일 때
  * SELF JOIN
    * SELF INNER JOIN
    * SELF OUTER JOIN

  ```sql
  # 내 사번, 내 이름, 상사의 사번, 상사의 이름 조회하기
  
  SELECT me.employee_id as 내사번, me.first_name as 내이름, 
  me.manager_id as 상사사번, man.first_name as 상사이름
  FROM employees me, employees man
  # manager가 존재하지 않는 사원도 조회
  WHERE me.manager_id = man.employee_id(+);
  ```

  * 각 table의 alias를 반드시 설정해야 한다.

| 표준 JOIN - ansi                                             | Oracle JOIN                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| SELECT a1.name, b1.dept_name<br />FROM A a1 INNER JOIN B b1<br />ON a1.id = b1.id; | A B<br />SELECT a1.name, b1.dept_name<br />FROM A a1, B b1<br />WHERE a1.id = b1.id; |
| **INNER JOIN**<br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e INNER JOIN departments d ON e.department_id = d.department_id; | **INNER JOIN**<br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e, departments d WHERE e.department_id = d.department_id; |
| **OUTER JOIN**<br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e LEFT OUTER JOIN departments d ON e.department_id = d.department_id;<br /><br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e RIGHT OUTER JOIN departments d ON e.department_id = d.department_id; | **OUTER JOIN**<br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e, departments d WHERE e.department_id = d.department_id(+);<br /><br />SELECT employee_id, first_name, e.department_id, department_name FROM employees e, departments d WHERE e.department_id (+) = d.department_id; |
| **SELF JOIN**<br />SELECT me.first_name, me.salary, me.manager_id FROM employees me INNER JOIN employees man ON me.manager_id = man.employee_id; | **SELF JOIN**<br />SELECT me.first_name, me.salary, me.manager_id FROM employees me, employees man WHERE me.manager_id = man.employee_id; |





#### Table 설계

* 하나의 table에 데이터를 모두 넣으면 비효율적인 경우
  * 중복된 데이터가 많을 때
  * null 데이터가 많을 때 (L자형 table)
* 데이터를 분리해서 table을 여러 개 만드는 것이 좋음
* 필요 시에 JOIN을 사용하면 됨





## 집합 연산자





#### UNION | UNION ALL

* 합집합과 같은 개념

* 두 table의 행을 합칠 때 사용 

  * JOIN과 달리 data의 개수가 늘어남
  * 같은 구조를 가지고 있는 두 개의 **서로 다른** table이 있을 때 유용

  ```sql
  #각각 조회할 경우
  SELECT employee_id, first_name FROM employees; #행 100개
  SELECT id, name FROM 회원; #행 200개
  
  #UNION 사용해서 조회할 경우
  SELECT employee_id, first_name FROM employees
  UNION
  SELECT id, name FROM 회원; #행 300개
  
  #UNION: 두 가지 조건을 모두 만족하거나 하나의 조건만 만족하는 데이터 조회
  #부서가 50이고 급여가 5000 이하인 사원이라면 중복자 제외
  SELECT first_name, department_id, salary
  FROM employees
  WHERE department_id = 50
  UNION
  SELECT first_name, department_id, salary
  FROM employees
  WHERE salary <= 5000;
  
  #UNION ALL: 두 가지 조건을 모두 만족하거나 하나의 조건만 만족하는 데이터 조회
  #부서가 50이고 급여가 5000 이하인 사원이라면 2번 조회
  SELECT first_name, department_id, salary
  FROM employees
  WHERE department_id = 50
  UNION ALL 
  SELECT first_name, department_id, salary
  FROM employees
  WHERE salary <= 5000;
  ```





#### MINUS

* 차집합과 같은 개념
  * 첫 번째 조건을 만족하는 결과에서 두 번째 조건을 만족하는 결과를 제외

```sql
#부서가 50번인 사람 중에서 salary가 5000 초과인 사원 조회
SELECT first_name, department_id, salary
FROM employees
WHERE department_id = 50
MINUS 
SELECT first_name, department_id, salary
FROM employees
WHERE salary <= 5000;
```





#### INTERSECT

* 교집합과 같은 개념
  * 두 조건을 모두 만족하는 데이터만 조회 (중복자 제외)

```sql
#부서가 50번이고 salary가 5000 이하인 사원 조회
SELECT first_name, department_id, salary
FROM employees
WHERE department_id = 50
INTERSECT
SELECT first_name, department_id, salary
FROM employees
WHERE salary <= 5000;
```

