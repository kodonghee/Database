## SQL 기본

* 데이터 조회
* SELECT 문





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

* WHERE 절은 GROUP BY 절보다 먼저 작성

```sql
SELECT department_id, sum(salary) FROM employees 
WHERE department_id is not null GROUP BY department_id;
```





#### HAVING

