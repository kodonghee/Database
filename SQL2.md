## PL/SQL 고급

* Oracle 내장 함수
* Oracle 데이터 타입





#### CAST ()

* 특정 함수를 다른 데이터 타입으로 바꿔서 보여줄 경우
  * CAST (보여주고 싶은 데이터 AS 데이터 타입 )

```sql
SELECT CAST(sysdate AS timestamp) FROM dual;

#결과
CAST(SYSDATEASTIMESTAMP)
---------------------------------------------------------------------------
21/03/25 11:47:43.000000
```





#### TO_CHAR / TO_NUMBER / TO_DATE

* 데이터 형태를 표현하는 문자들

| ,                                                | , 기호               |
| ------------------------------------------------ | -------------------- |
| $                                                | $ 기호               |
| L                                                | LOCALE CURRENCY - ￦ |
| 9                                                | 1자리 숫자           |
| 0                                                | 1자리 숫자           |
| YY - 2000년대<br />YYYY<br />RR(0 - 49, 50 - 99) | 년도                 |
| MM                                               | 월                   |
| DD                                               | 일                   |
| HH<br />HH24                                     | 시간                 |
| MI                                               | 분                   |
| SS                                               | 초                   |
| DAY                                              | 요일                 |

* TO_CHAR

  * 원하는 문자 형태로 변환

  ```sql
  SELECT TO_CHAR(12345, '$999,999') FROM DUAL;
  ```

  ```sql
  SELECT TO_CHAR(123456.789, '$999,999') FROM DUAL;
  #소수점은 원하는 문자 형태에 포함되지 않기 때문에 반올림한 형태로 출력
  ```

  ``` sql
  SELECT TO_CHAR(123456.7, 'L999,999.99') FROM DUAL;
  #￦(원)으로 출력
  ```
  ```sql
  #연도만 네자리로 출력
  SELECT TO_CHAR(sysdate, 'yyyy') FROM DUAL;
  
  #월만 출력
  SELECT TO_CHAR(sysdate, 'mm') FROM DUAL;
  ```

* TO_NUMBER

  * 원하는 숫자 형태로 변환

  ```sql
  SELECT TO_NUMBER('123,456', '999999.99') FROM DUAL;
  ```

* TO_DATE

  * 문자를 날짜로 변환

  ```sql
  SELECT sysdate FROM DUAL;
  SELECT TO_DATE('21/03/25', 'yy/mm/dd') + 100 FROM DUAL;
  ```





#### DICTIONARY

* 현재 Oracle 설정 날짜 형태

```sql
#DICTIONARY 조회
SELECT * FROM DICT;

#NLS 포함 테이블 조회
SELECT TABLE_NAME FROM DICT WHERE TABLE_NAME LIKE '%NLS%';

#환경 설정 파라미터 조회
SELECT * FROM NLS_SESSION_PARAMETERS;

#날짜 포맷 조회
SELECT * FROM NLS_SESSION_PARAMETERS WHERE PARAMETER='NLS_DATE_FORMAT';
```

* 날짜 포맷 변경

```sql
SELECT sysdate FROM dual;  #21/03/25 --> 2021/03/25 목요일 13:39:11

SELECT TO_CHAR(SYSDATE, 'YYYY/MM/DD DAY HH24:MI:SS')
FROM DUAL;

#원하는 구분자 양쪽에 " "
SELECT TO_CHAR(SYSDATE, 'YYYY"년도"MM"월"DD"일" HH24"시"MI"분"SS"초"') 
FROM DUAL;
------------------------------------------------------------------------
2021년도03월25일 13시51분51초

#fm: 의미 없는 포맷은 버림 
SELECT TO_CHAR(SYSDATE, 'fmYYYY"년도"MM"월"DD"일" HH24"시"MI"분"SS"초"') 
FROM DUAL;
------------------------------------------------------------------------
2021년도3월25일 13시53분5초
```





#### 함수 정리

| 타입 변환 함수   | CAST<br />TO_DATE, TO_CHAR, TO_NUMBER                        |
| ---------------- | ------------------------------------------------------------ |
| 그룹 합수        | SUM, AVG, MIN, MAX, COUNT, STDEV, VARIANCE                   |
| 문자 데이터 함수 | UPPER, LOWER, INITCAP<br />CONCAT, LENGTH, LENGTHB, INSTR, SUBSTR<br />REPLACE, TRANSLATE<br />LTRIM, RTRIM |
| 숫자 데이터 함수 | MOD - 나머지 구하는 함수<br />MOD(10, 3) = 1<br />ROUND - 반올림 해주는 함수<br />ROUND(10 / 3, 0); <br />(index: 백의 자리(-2) - 십의 자리(-1) - 일의 자리(0) <br />- 소수 첫째 자리(1) - 소수 둘째 자리(2) 형태)<br /> ex) ROUND(363.6789, -2) = 400<br />TRUNC - 버림 해주는 함수<br />TRUNC(3.953, 0) = 3; |
| 날짜 데이터 함수 | SYSDATE<br />SYSTIMESTAMP - 1/1000초<br />ADD_MONTHS()<br />MONTHS_BETWEEN() |
| 순위 함수        | ROWNUM<br />ROW_NUMBER()<br />RANK()<br />DENSE_RANK()<br /><br />ROW_NUMBER()<br />RANK()<br />DENSE_RANK()<br />순위함수 ()<br />OVER (PARTITION BY 소그룹 COLUMN<br />ORDER BY COLUMN ACS/DESC) |
| NULL 처리함수    | NVL(salary, 0)                                               |
|                  |                                                              |





#### LENGTH () / LENGTHB ()

* LENGTH ()

  * 문자의 길이 출력

  ```sql
  SELECT LENGTH('A') FROM DUAL;
  ```

* LENGTHB ()

  * 메모리를 차지하는 byte 수

  ```sql
  SELECT LENGTHB('가') FROM DUAL;
  ```





#### CONCAT () / ||

* 문자열 결합 

```sql
SELECT CONCAT('A', '가') FROM DUAL;
SELECT 'A' || '가' FROM DUAL;

# +는 문자열 결합을 해주지 않는다.
```





#### INSTR ()

* INSTR(a, b)
  * a에 b가 포함되어있다면 몇 번째 인덱스에 있는지 출력
    * index는 1부터 시작
  * a에 b가 포함되지 않는다면 0을 출력

```sql
SELECT INSTR('이것이 자바다', '자바') FROM DUAL;

INSTR('이것이자바다','자바')
----------------------------
                           5
```

* 특정 문자열이 포함된 데이터만 출력

```sql
SELECT first_name FROM employees WHERE INSTR(first_name, 'er') >= 1;
SELECT hire_date FROM employees WHERE INSTR(hire_date, '03') = 4;
```





#### UPPER () / LOWER () / INITCAP ()

* UPPER ()
  * 소문자를 대문자로 변경
* LOWER ()
  * 대문자를 소문자로 변경
* INITCAP ()
  * 첫 글자를 모두 대문자로 변경





#### REPLACE () / TRANSLATE ()

* REPLACE (문자열, 원래 문자열, 바꿀 문자열)
* TRANSLATE (문자열, 원래 문자열, 바꿀 문자열)

```sql
SELECT REPLACE ('이것이 자바다', '자바', '오라클') FROM DUAL;
```





#### SUBSTR ()

* SUBSTR(문자열, index, 문자열 길이)
  * 문자열에서 해당 index의 문자를 문자열 길이만큼 출력

```sql
SELECT SUBSTR('이것이 자바다', 1, 2) FROM DUAL;

SUBSTR('이것
------------
이것
```

```sql
SELECT hire_date FROM employees
WHERE SUBSTR(hire_date, 4, 2) = '03';
```





#### LTRIM () / RTRIM ()

```sql
# 왼쪽 공백 제거
SELECT LTRIM('     aaa     ') FROM DUAL;

# 오른쪽 공백 제거
SELECT RTRIM('     aaa     ') FROM DUAL;

# 왼쪽 특정 문자 제거
SELECT LTRIM('##aaa##','#') FROM DUAL;

# 오른쪽 특정 문자 제거
SELECT RTRIM('##aaa###', '#') FROM DUAL;
```





#### MOD, ROUND, TRUNC

* employees table에서 홀수 사번 조회

```sql
SELECT employee_id FROM employees WHERE MOD(employee_id, 2) = 1;
```

* employees table에서 입사년도별 급여 평균 조회하되 평균은 정수로 출력 (소수점 이하 버림)

```sql
SELECT TO_CHAR(hire_date, 'yyyy') as hire_year, TRUNC(avg(salary), 0) 
FROM employees group by TO_CHAR(hire_date, 'yyyy');
```

```sql
SELECT SUBSTR(hire_date, 1, 2) as hire_year, TRUNC(avg(salary), 0) 
FROM employees group by SUBSTR(hire_date, 1, 2);
```





#### ADD_MONTHS

* 날짜를 기준으로 개월을 더한다.

```sql
SELECT ADD_MONTHS(SYSDATE, 1) FROM DUAL;
```





#### MONTHS_BETWEEN

* 두 날짜 사이에 몇 개월이 지났는지 출력

  * 몇 년이 지났는지, 몇 주가 지났는지는 나누기 연산으로 구하기 쉽지만 경과 개월 수는 구하기 어렵다. (매월 포함되는 일 수가 달라지기 때문) --> MONTHS_BETWEEN 사용
  * 입사한지 경과 개월 수 조회

  ```sql
  SELECT MONTHS_BETWEEN(sysdate, hire_date) FROM employees;
  ```





#### ROW_NUMBER () / RANK () / DENSE_RANK ()

* ROW_NUMBER ()

  * 같은 값을 가지는 항목의 순위도 다 다르게 매겨짐

  ```sql
  SELECT first_name as 이름, salary as 급여, 
  ROW_NUMBER() OVER (ORDER BY salary desc) as 급여순위 FROM employees; 
  ```

* RANK ()

  * 같은 값을 가지는 항목의 순위는 같게 매겨짐
  * 같은 값을 가지는 항목 수만큼 중간에 순위가 빠짐

  ```sql
  SELECT first_name as 이름, salary as 급여, 
  RANK() OVER (ORDER BY salary desc) as 급여순위 FROM employees; 
  ```

* DENSE_RANK ()

  * 같은 값을 가지는 항목의 순위는 같게 매겨짐
  * 중간에 빠지는 순위가 없도록 함

  ```sql
  SELECT first_name as 이름, salary as 급여, 
  DENSE_RANK() OVER (ORDER BY salary desc) as 급여순위 FROM employees; 
  ```





#### PARTITION BY

* 특정 항목 내에서 순위 매길 경우

```sql
SELECT first_name as 이름, salary as 급여, department_id as 부서코드,
ROW_NUMBER() OVER (PARTITION BY department_id ORDER BY salary desc) as 급여순위 FROM employees; 

#부서별 salary 순위 조회 --> 전체가 아닌 한 부서 내에서 순위 매김
```





#### NVL ()

* 값이 null일 경우 다른 값을 출력하고 싶을 때 사용
* 사용 예시
  * NVL (COLUMN, null 대체값)
  * NVL (COLUMN 정수, null 대체값 정수)
  * NVL (COLUMN 문자열, null 대체값 정수 + 문자열)

```sql
#commission_pct가 null일 경우 0을 출력하도록 함
SELECT first_name, NVL(commission_pct, 0) FROM employees;

#commission_pct가 null일 경우 '보너스 없음'을 출력하도록 함
#commission_pct가 숫자 타입이므로 문자로 변환 후에 NVL 적용
SELECT first_name, NVL(TO_CHAR(commission_pct), '보너스 없음') FROM employees;
```