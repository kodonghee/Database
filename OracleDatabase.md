### Oracle Database

* 자바 프로그래밍 언어-컴퓨터 다목적 프로그램 제작
* 데이터들 --> 영구적 파일/데이터베이스 저장



##### 파일 텍스트들

1. 데이터 분리 기준이 없다.
2. 데이터 type 기준이 없다.
3. 특정 위치의 데이터에 접근이 불가능하다.
4. 중복 데이터가 가능하다.
5. 데이터 현재 상태 잘못된 표현



##### 데이터 베이스

* 의미 있는 데이터 모음

  * 예시
    * 학생 = (학번 이름 전공 학교명 학년 성적)
    * 회사원 = (사번 이름 부서 회사명 직급 급여)

* 데이터 베이스 표현 방법

  1. 계층형

  2. 네트워크형

  3. 관계형 / 객체 관계형 데이터 베이스 제품

     Ex) oracle / mysql / ms sqlserver / db2 / ...

  4. 관계형 (relational DB-RDB)

     * 데이터 관계를 행과 열의 테이블 구조로 표현
     * 열(COLUMN) - 데이터의 구성요소로서 1개 표현 단위
     * 행(ROW) - 1개 데이터 RECORD

     * 학생 테이블

| 학번<br />정수 3자리<br />중복 x | 이름<br />문자 10자리 | 성적<br />실수 5자리(2자리) |
| -------------------------------- | --------------------- | --------------------------- |
| 100                              | 이학생                | 4.5                         |
| 100                              | xxxx                  | xxxx                        |

   * 데이터 테이블
        * 테이블 생성-삭제
        * 데이터 1개 ROW 저장-삭제
        * 데이터들 조회
        * 함수들 = 자바 메소드

* 관계형 데이터베이스 활용 문법 언어
  * SQL
    1. RDB 제품 종류에 관계없이 사용 "표준" SQL
    2. 각 RDB 독자적 SQL



##### Oracle

* 설치
  * oracle 11g express edition
    * 무료, 크기 제한, 1개만 사용
    * system schema = 계정 (super user)
    * hr(잠겨 있음) - 테이블들 조회 실습
  * enterprise / standard edition  
* 삭제
  * 제어판 - 프로그램 및 기능 - oracle 11g xe .. 제거



##### SQL 입력 실행 tool

1. RUN SQL Command Line 실행
2. SQL Developer / Orange / Toad / Eclipse data explorer 기능



##### SQL Command Line

###### oracle 독자적 sql

* SQL> connect system / 암호 
* SQL> alter user hr identified by hr account unlock;
* SQL> disconnect
  * 입력하지 않아도 connect 두번째 계정에 접속됨.
* SQL> connect hr/hr -> hr 테이블 실습 가능 
* SQL> select * from tab;
  * set pagesize 100; -> table 크기 설정
  * set linesize 150; -> table 크기 설정
* SQL> disconnect
* 4글자 축약 가능 (conn, disconn) / 대소문자 구분 x (단, 암호는 대소문자 구분)

###### SQL 종류

* Data Definition Language (DDL)
  * 테이블 생성 - 변경 - 삭제
  * 데이터 저장 구조를 정의하는 언어
* Data Manipulation Language (DML)
  * 테이블에 데이터 저장 - 수정 - 삭제
  * 데이터 조작 언어
* Data Controll Language (DCL)
  * 계정 생성 - DB 접속 허용
  * System 계정
* Transaction Controll Language (TCL)
  * 트랜잭션 제어 언어
* Data Query Language (DQL)
  * 데이터 조회

| DDL       | create table ...<br />alter table<br />drop table |
| --------- | ------------------------------------------------- |
| DML       | insert ...<br />update<br />delete                |
| DCL       | grant, revoke<br />새로운 계정 생성시 사용        |
| TCL       | commit<br />rollback                              |
| ***DQL*** | ***select***                                      |



##### hr 8개 테이블 조회 실습

* conn hr/hr;
* select * from tab; --> 테이블 목록 조회
* 문법
  * select 조회 column from 테이블 이름
    * select
    * from
    * where
    * group by
    * having
    * order by