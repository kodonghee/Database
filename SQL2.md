## PL/SQL 고급

* Oracle 내장 함수
* Oracle 데이터 타입





#### CAST ()

* 특정 함수를 다른 데이터 타입으로 바꿔서 보여줄 경우
  * CAST (보여주고 싶은 데이터 AS 데이터 타입 )

```sql
SELECT CAST(sysdate AS timestamp) FROM dual;

# 결과
CAST(SYSDATEASTIMESTAMP)
---------------------------------------------------------------------------
21/03/25 11:47:43.000000
```





#### TO_CHAR / TO_NUMBER / TO_DATE

