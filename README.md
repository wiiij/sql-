# sql 정리

SQL(Structured Query Language)
SQL의 Structured Query Language에는 크게 DQL, DML, DDL, TCL, DCL이 있습니다.



DQL
DQL는 Data Query Language의 약자로, 테이블의 데이터를 조회 및 검색하는 명령어로 SELECT 명령어가 있습니다.

SELECT
DDL (데이터 정의어)
DDL은 Data Definition Language의 약자로, 테이블을 포함한 여러 객체를 생성, 수정, 삭제하는 명령어입니다. 

DDL으로는 CREATE, ALTER, DROP이 있습니다.
DDL 쿼리는 트랜잭션이 바로 적용됩니다.

트랜잭션(transaction)이란 "쪼갤 수 없는 업무 처리의 최소 단위"를 말한다.
DCL (데이터 제어어)
DCL은 Data Control Language의 약자로, 데이터 사용 권한 부여 및 취소를 하는 명령어입니다.
DCL으로는 GRANT, REVOKE가 있습니다.

DML (데이터 조작어)
DML은 Data Manipulation Language의 약자로, 테이블의 데이터를 저장, 수정, 삭제하는 명령어입니다. 

DML으로는 INSERT, UPDATE, DELETE가 있습니다.

TCL (트랜잭션 제어어)
TCL은 Transaction Control Language로 트랜잭션 데이터의 영구 저장, 취소 등의 명령어 입니다. 

TCL으로는 COMMIT, ROLLBACK, SAVEPOINT가 있습니다.

Transaction 데이터베이스 관리 시스템 또는 유사한 시스템에서 상호작용의 단위이다.
SAVEPOINT : 트랜잭션의 특정 지점에 이름을 지정하고, 그 지점 이전에 수행 한 작업에 영향을 주지 않고 그 지점 이후에 수행한 작업을 롤백(ROLLBACK)할 수 있다. 단일 트랜잭션에서 여러 SAVEPOINT를 만들 수도 있다.
DQL(Data Query Language)
select 기본 구조

SELECT : 테이블에서 데이터 질의하는 키워드 

FROM : 데이터를 조회하고 싶은 테이블의 이름을 정하는 키워드

WHERE : 데이터를 조회하는 조건을 적는 키워드

GROUP BY : 특정 속성을 기준으로 그룹화 하여 검색할 때 속성 키워드

HAVING : 그룹 함수를 포함한 조건 키워드

ORDER BY : 정렬시 사용하는 키워드
SELECT문의 실행 순서

SELECT문은 FROM-WHERE-GROUP BY-HAVING-SELECT-ORDER BY 순으로 실행됩니다.
1. FROM : 발췌 대상 테이블 참조
2. WHERE : 발췌 대상 데이터가 아닌 것은 제거
3. GROUP BY : 행들을 소그룹화 한다.
4. HAVING :  그룹핑된 값의 조건에 맞는 것만을 출력한다.
5. SELECT : 데이터 값을 출력/계산한다.
6. ORDER BY :  데이터를 정렬한다.
SQL 프로그램의 최소 단위는 테이블입니다. 테이블은 행(튜플, 레코드)과 열(컬럼, 속성)의 관계로 표현됩니다.

테이블 모두 찾기
SELECT * FROM 테이블 이름;
테이블 전체의 레코드 수 확인
SELECT COUNT(*) FROM 테이블 이름;


소수의 데이터가 아니라 대량의 데이터가 있는 테이블을 다뤄야 할 경우, 이는 비효율적입니다. 이를 더 효율적으로 계산하려면 테이블의 기본 키의 수를 조회하면 됩니다.

이유 : 기본키는 null 값이 입력될 수 없으므로, 테이블의 전체 레코드 수와 동일하게 나오기 때문입니다.
테이블 기본키의 레코드 수 확인
SELECT COUNT(기본키) FROM 테이블 이름;
aliase(별칭)
aliase(별칭)이란 AS 키워드를 사용하여 쿼리에서 반환된 열에 새 이름을 지정하는 것을 해당 열의 별칭 지정(aliasing)이라고 하며, 사용자가 지정한 새 이름을 별칭(aliase)라고 합니다.



Aliase 주의사항

SELECT 
        A.EMPNO     AS EMPNO    -- 사원번호
       ,A.ENAME     AS ENAME    -- 사원이름
       ,A.JOB       AS JOB      -- 사원직책
       ,A.MGR       AS MCR      -- 상관사원번호
       ,A.HIREDATE  AS HIREDATE -- 입사일
       ,A.SAL       AS SAL      -- 급여    
       ,A.COMM      AS COMM     -- 수당
       ,A.DEPTNO    AS DN   -- 부서번호 
FROM 
        SCOTT.EMP A
WHERE DN=10;


해당 문을 실행할 경우 "부서번호" : 부적합한 식별자 오류가 나오게 됩니다. 이유는 SELECT문은 FROM-WHERE-GROUP BY-HAVING-SELECT-ORDER BY 순으로 실행되기 때문입니다.

따라서, 순서를 잘 인지하고 WHERE절에 A.DEPTNO=10으로 다시 입력하면 해결 됩니다.

SELECT 
        A.EMPNO     AS EMPNO    -- 사원번호
       ,A.ENAME     AS ENAME    -- 사원이름
       ,A.JOB       AS JOB      -- 사원직책
       ,A.MGR       AS MCR      -- 상관사원번호
       ,A.HIREDATE  AS HIREDATE -- 입사일
       ,A.SAL       AS SAL      -- 급여    
       ,A.COMM      AS COMM     -- 수당
       ,A.DEPTNO    AS DN   -- 부서번호 
FROM 
        SCOTT.EMP A
WHERE A.DEPTNO=10;


만약, WHERE절에 꼭 DN으로 작성하여 출력하고 싶다면, FROM절에 서브쿼리문을 사용해서 해결할 수 있습니다.

SELECT 
         A.EMPNO    -- 사원번호
        , A.ENAME    -- 사원이름
       , A.JOB      -- 사원직책
       , A.MCR      -- 상관사원번호
       ,A.HIREDATE -- 입사일
       , A.SAL      -- 급여    
       , A.COMM     -- 수당
       , A.DN   -- 부서번호 
FROM 
        (SELECT 
         EMPNO     AS EMPNO    -- 사원번호
        ,ENAME     AS ENAME    -- 사원이름
        ,JOB       AS JOB      -- 사원직책
        ,MGR       AS MCR      -- 상관사원번호
        ,HIREDATE  AS HIREDATE -- 입사일
        ,SAL       AS SAL      -- 급여    
        ,COMM      AS COMM     -- 수당
        ,DEPTNO    AS DN   -- 부서번호 
       FROM  EMP) A
WHERE  A.DN=10;


FROM 절은 WHERE절보다 더 먼저 실행되기 때문입니다.

where절 활용
WHERE절은 조건절로 할용할 수 있습니다.
WHERE 절은 반드시 해당 내용이 참일 때만 수행 됩니다.
예시로 임의에 EMP테이블이 있다고 쳤을 경우 특정 컬럼에 데이터를 출력하고 싶을 때 사용합니다.

select *
from EMP
where pname = '황기자';


라고 하면 pname이 황기자인 컬럼들의 데이터를 출럭합니다.

and = 모든조건을 만족하는 값만 참으로 반환
or = 조건중 하나라도 만족하는 경우 참으로 반환
결측치 대체하기
결측치는 COUNT가 되지 않기 때문에 연산과 카운팅이 불가합니다. 따라서, 결측치를 0으로 바꿔줘야 합니다.

SELECT 
      A.ENAME                      AS ENAME  -- 사원이름
        ,A.SAL                       AS SAL    -- 급여
       ,A.SAL * 12                  AS TSAL    -- 연봉
       ,NVL(A.COMM, 0)              AS COMM    -- 수당 
       ,A. SAL * 12 + NVL(COMM, 0)  AS CSAL   -- 수당을포함한 연봉
FROM 
        EMP A
ORDER BY TSAL DESC;


ORDER BY는 정렬 키워드로 DESC는 내림차순 정렬, ASC는 오름차순 정렬을 의미합니다. (DEFAULT값은 ASC입니다.)

LIKE
특정 글자가 들어가는 지 확인하는 키워드에는 LIKE 키워드가 있습니다.

SELECT ENAME FROM EMP WHERE ENAME LIKE '%S';
SELECT ENAME FROM EMP WHERE ENAME LIKE '_A%';
 '%'는 모든 문자를 의미 || '_'는 한 글자를 의미
첫 번째 쿼리문은 마지막 글자가 S로 끝나는 사람을 출력시키라는 의미이고,
두 번째 쿼리문은 두 번째 글자가 A인 사람을 출력하라는 의미입니다.

DDL(데이터 정의어){Data Definition Language}
테이블과 같은 데이터 구조를 정의하는데 사용되는 명령어들로 (생성, 변경, 삭제, 이름변경) 데이터 구조와 관련된 명령어들을 말함.

create table
create table 명령어는 데이터 베이스 엔진에게 테이블 공간을 만들어달라는 명령어입니다.

CREATE TABLE 계정이름.테이블이름(
칼럼명1  데이터타입1(사이즈)
    ,칼럼명2 데이터타입2
    ,칼럼명3 데이터타입3(사이즈)
    , ...
    , 칼럼명N 데이터타입N(사이즈)
)
계정이름은 생략이 가능하며, 계정이름이 없는 경우 현재의 계정에 테이블을 생성합니다.

create table EMP(
empno varchar2(10) not null,
    empname varchar2(10),
    ..
    ..
    ..
    primary key(empno)
);


이런 형식으로 만들수 있습니다.

table 복사
다음과 같은 명령어를 입력할 경우, 다른 테이블에 있는 값을 그대로 복사하여 가져올 수 있습니다.

단, Primary Key, Index 등등 Object는 복사가 되지 않습니다.
CREATE TABLE EMP_2 AS
SELECT*FROM EMP;


만약, 칼럼만 가져오고 싶다면 WHERE절을 통해, 칼럼만 가져오는 방법도 있습니다.

CREATE TABLE EMP_T1 AS
SELECT*FROM EMP WHERE 1=2;
WHERE 절은 참일 경우에만 실행 되므로, 이럴 경우, 칼럼만 가져오게 됩니다.
테이블 이름 바꾸기
RENAME 명령어를 통해 테이블의 이름을 바꿀 수도 있습니다.

RENAME TEST_T1 TO TEST_1;


해당 명령어를 통해 TEST_T1 이라는 테이블이름을 TEST_1이라는 테이블 이름으로 바꿀 수 있습니다.

테이블 변경
ALTER와 ADD 명령어를 통해 테이블에 칼럼을 추가할 수도 있습니다.

ALTER TABLE TEST_1
ADD TT VARCHAR2(100);


데이터 타입이 VARCHAR2이면서 사이즈가 100이고, 칼럼이름이 TT인 칼럼을 TEST_1 테이블에 추가합니다.

이 때, 주의해야 할 점은 새로 생성되는 칼럼은 마지막 칼럼으로 생성됩니다.
테이블 컬럼 이름 변경
ALTER와 RENAME 명령어를 통해 칼럼이름을 바꿀 수도 있습니다.

ALTER TABLE TEST_1
RENAME COLUMN TT TO TT_RENAME;


TEST_1 테이블의 TT 칼럼의 이름을 TT_RENAME 칼럼으로 바꿀 수 있습니다.

테이블 컬럼 사이즈 변경
ALTER와 MODIFY 명령어를 통해 테이블의 칼럼 사이즈를 변경할 수 있습니다.

ALTER TABLE TEST_1
MODIFY TT_RENAME VARCHAR2(200);


VARCHAR2(100)인 TT_RENAME 칼럼을 VARCHAR2(200)으로 사이즈를 변경할 수 있습니다.

테이블 컬럼 삭제
ALTER와 DROP 명령어를 통해서 테이블의 칼럼을 삭제 할 수 있습니다.

ALTER TABLE TEST_1
DROP COLUN TT_RENAME;
테이블 지우기
DROP 명령어를 통해 테이블을 DROP 할 수도 있습니다.

DROP TABLE TEST_1;
DCL(데이터 제어어){Data Control Language}
데이터베이스에 접근하고 객체들을 사용하도록 권한을 주고 회수하는 명령어들을 말함.

GRANT
GRANT 명령어는 사용자에게 권한을 부여하기 위한 명령어입니다.

GRANT CREATE TABLE, CREATE USER TO SCOTT WITH ADMIN OPTION;
REVOKE
REVOKE 명령어는 GRANT 명령어로 적용한 권한을 회수하는 명령어입니다.

REVOKE CREATE TABLE, CREATE USER FROM SCOTT;
DML(데이터 조작어){Data Manipulation Language}
DML은 테이블의 데이터를 저장, 수정, 삭제하는 명령어로 INSERT, UPDATE, DELETE 명령어가 있습니다. 



DML 특징

1. DML 쿼리의 경우, 트랜잭션은 처리해야 한다.
2. DML은 메모리에 적재된다. 이 때, TCL 명령어인 COMMIT을 하지 않을 경우, 외부 응용프로그램에서는 테이블 내용 중 조회가 불가능하다.
3. TCL 명령어인 COMMIT을 할 경우, 메모리에 적재된 내용을 파일에 적재하므로, 외부 응용프로그램에서 테이블 내용중 파일에 적재된 내용만 조회가 가능하다.
INSERT
CREATE TABLE SCOTT.TEST_2(
    TC2_1 NUMBER(7, 2)
    ,TC2_2 VARCHAR2(30)
    ,TC2_3 DATE
);


임의로 TEST_2하는 테이블을 생성해준다.
생성한 테이블에 INSERT 명령어를 통해 데이터를 입력할 수 있다.

INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.3, '바차 2 문자열', SYSDATE);


이 때, 유의사항은 칼럼 순서와 데이터 타입에 맞게 데이터를 입력해야 합니다.

DELETE
DELETE 명령어로 특정 데이터를 삭제할 수 있습니다.

INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.33, '바차 2 문자열1',SYSDATE);

DELETE FROM TEST_2
WHERE TC2_1 >= 1.32;
TEST_2 테이블에 아래와 같이 데이터가 생성되었다가

TC2_1 : 1.33
TC2_2 : '바차 2 문자열1'
TC2_3 : SYSDATE 
DELETE 명령어에서 TC2_1가 1.32 이상이므로, 삭제 되었습니다.

UPDATE
UPDATE 명령어로 특정 데이터를 업데이트 할 수 있습니다.

INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.33, '바차 2 문자열1',SYSDATE);

UPDATE TEST_2 
SET TC2_1=1.30
WHERE TC2_1>=1.32;


TEST_2 테이블에 아래와 같이 데이터가 생성되었다가

TC2_1 : 1.33
TC2_2 : '바차 2 문자열1'
TC2_3 : SYSDATE 
UPDATE 명령어에서 TC2_1가 1.32 이상이므로, 1.30으로 수정되었습니다.

TCL (트랜젝션 제어어){Transaction Control Language}
앞서, 언급한 DML의 특징의 경우, 트랜잭션 제어어 없이 외부 응용프로그램에서 테이블 내용 중 파일에 적재된 내용을 조회할 수 없습니다.

또한, DML 명령어로 작업하다가 이전으로 돌아가고 싶을 때에도 TCL 명령어를 통해 돌아갈 수 있습니다.

COMMIT
INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.33, '바차 2 문자열1',SYSDATE);

UPDATE TEST_2 
SET TC2_1=1.30
WHERE TC2_1>=1.32;

COMMIT;


이전의 명령어를 작성한 뒤, COMMIT을 하게 될 경우, 메모리에 적재된 내용을 파일에 적재합니다.

ROLLBACK
COMMIT;

INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.35, '바차 2 문자열5',SYSDATE);

ROLLBACK;


TEST_2 테이블에 아래와 같이 데이터가 생성했는데, 

TC2_1 : 1.35
TC2_2 : '바차 2 문자열5'
TC2_3 : SYSDATE
이를 다시 데이터를 생성하기 전으로 돌리고 싶으면, ROLLBACK 명령어를 사용할 수 있습니다.
ROLLBACK 명령어는 가장 최근 COMMIT했을 때의 상태로 돌아가게 해주는 명령어 입니다.

주의사항
COMMIT 명령어를 사용하면, 그 이전상태로 ROLLBACK이 되지 않습니다.

INSERT INTO TEST_2(TC2_1, TC2_2, TC2_3)
VALUES (1.35, '바차 2 문자열5',SYSDATE);

COMMIT;

ROLLBACK;


예시로 위와 같이 쿼리문을 작성하게 된다면, 

TC2_1 : 1.35
TC2_2 : '바차 2 문자열5'
TC2_3 : SYSDATE
데이터가 생성하기 전으로 돌아갈 수 없습니다.

