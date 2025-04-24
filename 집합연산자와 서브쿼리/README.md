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

-- 관리자인 사원들의 이름과 월급과 직업을 출력
select ename
from emp
where empno in (select mgr from emp);

-- 관리자가 아닌 사원들의 이름과 월급과 직업을 출력 = not in은 null값이 들어가면 조회가 안된다
select ename
from emp
where empno not in (select mgr from emp where mgr is not null);



-- 서브 쿼리 사용하기 (exists(서브쿼리의 결과가 존재하는지 확인)와 not exists 를 사용하면 메인쿼리가 서브쿼리보다 먼저 실행된다(중요))

-- 부서 테이블에 있는 부서번호중에서 사원 테이블에 존재하는 부서번호에 대한 모든 컬럼을 출력
select deptno
from dept d
where exists (select *
from emp e where e.deptno = d.deptno);

-- 부서 테이블에 있는 부서번호중에서 사원 테이블에 존재하지 않는 부서번호에 대한 모든 컬럼을 출력
select deptno
from dept d
where not exists (select *
from emp e where e.deptno = d.deptno);




-- group by 검색조건 having절 사용방법

-- 직업과 직업별 토탈월급을 출력하는데 직업이 salesman인 사원들의 토탈월급보다 더 큰 것만 출력
SELECT job, SUM(sal)
FROM emp
GROUP BY job
HAVING SUM(sal) > (SELECT SUM(sal) FROM emp WHERE job = 'salesman');

-- 부서번호, 부서번호별 인원수를 출력하는데 10번 부서번호의 인원수보다 더 큰것만 출력
select deptno, count(*)
from emp
group by deptno
having count(*)> (select count(*) from emp
where deptno =10);



-- from 절의 서브 쿼리

-- 사원 테이블에서 월급들 가장 많이 받는 사원의 이름과 월급과 궐급의 순위를 출력
-- 분석함수는 where 절에 사용을 못한다 = rank() 등
-- rank() 데이터의 순위를 계산, over 창 함수가 작동할 범위(Window)를 정의하는 역할
SELECT *
FROM (
SELECT ename, sal, RANK() OVER (ORDER BY sal DESC) AS rnk
FROM emp
) AS emp_rank
where rnk =1;

-- 직업이 salesman인 사원들중에서 가장 먼저 입사한 사우너의 이름과 입사일을 출력
SELECT *
FROM (
SELECT ename, hiredate, job, RANK() OVER (ORDER BY hiredate ASC) AS rnk
FROM emp
WHERE job = 'salesman'
) AS emp_hiredate
where rnk =1;



-- select 절의 서브 쿼리

-- 직업이 salesman인 사원들의 이름과 월급을 출력하면서 그 옆에 직업이 salesman인 사원들의 최대우러급과 최소월급을 출력
select ename,sal ,(select max(sal) from emp where job ='salesman') as max1,
(select min(sal) from emp where job= 'salesman') as min1
from emp where job='salesman';

-- 부서 번호가 20번인 사원들의 이름과 월급을 출력하고 그 옆에 20번 부서번호인 사원들의 평균월급이 출력
select ename,sal ,(select avg(sal) from emp where deptno =20) 평균
from emp
where deptno =20;