-- index = 검색 속도를 높이기 위해 사용 

-- 오라클 방식
EXPLAIN
SELECT ename, sal
FROM emp
WHERE sal = 3000;

-- 실행 계획 확인
select * from table(dbms_xplan.dlsplay);


-- mysql 방식

EXPLAIN EXTENDED(8.0이전버전에서만 사용가능) 사용
더 자세한 실행 계획 정보가 필요하면 EXPLAIN EXTENDED를 사용할 수 있습니다:

-- 8.0 이상 버전
EXPLAIN ANALYZE ,EXPLAIN 사용하기

EXPLAIN ANALYZE는 일반적인 실행 계획(EXPLAIN)과는 조금 다르게 동작합니다. 
EXPLAIN 결과에서 볼 수 있는 type(e.g., fullscan, index, ref 등)은 EXPLAIN에 
표시되지만, EXPLAIN ANALYZE에서는 실제 실행된 작업의 통계와 성능에 초점이 맞춰져 있습니다.

EXPLAIN
SELECT ename, sal
FROM emp
WHERE sal = 3000;


-- 1. 월급이 3000인 사원의 이름과 월급을 인덱스를 통해서 빠르게 검색 하세요
-- 인덱스 명 작서할때는 테이블명뒤에는컬럼
create index emp_sal
on emp(sal);

EXPLAIN ANALYZE
SELECT ename, sal
FROM emp
WHERE sal = 3000;


