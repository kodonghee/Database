## JOIN

* 두 개 이상의 table을 합칠 때 사용

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

  





