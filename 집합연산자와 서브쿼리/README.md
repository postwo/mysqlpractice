-- union all : 위 아래를 붙여주기만 한다

-- 부서번호와 부서번호별 토탈 우러클을 출력
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno

UNION ALL  

SELECT NULL, SUM(sal) -- 위 하고 아래 컬럼 개수를 같게 해줘야 한다 , 데이터타입을 위아래로 맞춰줘야 성능이 좋다 (to_number()는 오라클에서 사용가능)
FROM emp;

-- 직업과 직업별 토탈월급을 출력하는데 맨 아래에 전체 토탈월급도 출력

select job,sum(sal)
from emp
group by job
union all
select null,sum(sal)
from emp
order by job asc;


-- union : union all과 차이점은 union은 데이터를 정렬해서 출력한다 그리고 중복을 제거 한다 

-- 부서번호와 부서번호별 토탈 우러급을 출력하고 다음과 같이 맨 아래에 전체 토탈 우러급도 출력하는데 부서번호가 오름차순으로 정렬되어서 출력 하시오
SELECT deptno, SUM(sal)
FROM emp
GROUP BY deptno

UNION ALL

SELECT NULL, SUM(sal)
FROM emp

ORDER BY deptno;

-- 입사한 년도, 입사한 도별 토탈월급을 출력하는데 맨 아래에 전체 토탈월급이 출력되게 하세요.(입사한 년도는 정렬이 되어서 출력되게 하세요)

SELECT hiredate, SUM(sal) AS total_salary
FROM emp
GROUP BY hiredate

UNION

SELECT NULL, SUM(sal) AS total_salary
FROM emp

ORDER BY hiredate;



# 단일행 서브쿼리: 서브쿼리에서 메인쿼리로 하나의 값이 리턴되는 경우  연산자: =,!=,^=,<>,>,<,>=,<=
# 다중행 서브쿼리: 서브쿼리에서 메인쿼리로 여러개의 값이 리턴되는 경우  연산자: in,not in, >all,<all,>any,<any

-- 단일행 서브쿼리 : = 를 사용하면 된다 
-- jones 보다 더 많은 월급을 받는 사원들의 이름과 월급을 출력

-- 이 두개의 쿼리를 한 번에 실행하는게 서브쿼리이다
select ename,sal
from emp
where sal > 2975;

select sal
from emp
where ename = 'jones';

-- 서브쿼리 버전
select ename,sal
from emp
where sal > (select sal
from emp
where ename = 'jones');


-- allen 보다 더 늑제 입사한 사원들의 이름과 월급을 출력하세요
select ename,sal
from emp
where hiredate > (select hiredate from emp
where ename = "allen");



-- 다중 행 서브쿼리 : in을 사용해야한다 

-- 직업이 salesman인 사원들과 같은 월급을 받는 사원들의 이름과 월급을 출력 (= -> 이거는 하나만 출력할때 in은 여러개 출력할떄 사용 )
select ename,sal
from emp
where sal in (select sal from emp where job = "salesman");

-- 부서번호가 20번인 사원들과 같은 직업을 갖는 사원들의 이름과 직업을 출력
select ename,job
from emp
where job in (select job from emp where deptno =20);


13강부터 듣기 