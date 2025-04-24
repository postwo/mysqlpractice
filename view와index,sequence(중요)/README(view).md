-- view = 다른 데이터를 못 보게 하기 위해 사용 = 테이블을 바라만 보는거다 그러므로 데이터를 수정하면 수정이 된다
-- 1. view 사용이유 보안(월급같은 부분)을 위해서 2. 복잡한 쿼리문을 간단하게 검색 하고자 할때 사용

-- 뷰  삭제 drop view [뷰이름]


-- 1. 사원테이블에서 직업이 salesman인 사원들의 사원번호, 사원이름, 직업,관리자 번호,부서번호만 바라볼 수 있는 view를 만들어라
create view emp_view
as
select empno,ename,sal,job,deptno
from emp
where job ="salesman";

select * from emp_view;

-- 수정 하면 데이터가 변한
update emp_view
set sal= 0
where ename = "MARTIN";


-- 2. 사원 테이블에서 부서번호가 20번인 사원들의 사원번호와 사원이름 직업,월급을 볼 수 있는 뷰를 생성
create view emp_view2
as
select empno,ename,sal,job
from emp
where deptno =20;

select * from emp_view2;


-- 3. 사원 테이블에서 부서번호와 부서번호별 평균월급만 바라볼수 있는 view를 만들어라
-- round(avg(sal)) 같은 계산식이나 함수 결과는 자동으로 컬럼 이름이 지정되지 않습니다. 따라서 as avgsal과 같이 별칭(alias)을 지정해야한다
-- ROUND는 SQL에서 숫자를 반올림하는 데 사용
create view emp_view3
as
select deptno,round(avg(sal)) as avgsal
from emp
group by deptno;

select * from emp_view3;


-- 4. 직업,직업별 토탈월급을 출력하는 view를 emp_view96으로 생성
create view emp_view96
as
select job,sum(sal) as 토탈월급
from emp
group by job;

select * from emp_view96;
