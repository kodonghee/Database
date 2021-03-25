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

| 타입 변환 함수   |         |
| ---------------- | ------- |
| 그룹 합수        |         |
| 문자 데이터 함수 | UPPER   |
| 숫자 데이터 함수 |         |
| 날짜 데이터 함수 | SYSDATE |
|                  |         |
|                  |         |
|                  |         |





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



