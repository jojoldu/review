# 하루 30분 36강으로 배우는 완전 초보의 SQL 따라잡기

![표지](./images/표지.png)

거의 대부분이 아는 내용들이라 아마 많은 분들은 크게 보면서 오오 하는건 없을것 같다.  
하지만 나는 최근 2년간 ORM만 사용하다보니 실제 쿼리 작성할 일이 거의 없어서 (기획자분들께 데이터를 CSV로 뽑아드릴때나, ISMS 심사할때 심사관들 앞에서 쿼리날릴때 정도?) 그래도 기본은 알아야하지 않나 싶어서 지하철 출퇴근 시간에 읽으면서 정리중! <br/>

블로그에 바로 쓸까하다가 마크다운 에디터를 Visual Studio Code에서 Atom으로 환승한 기념으로!! 마크다운으로 작성한다. (정말 좋다. 나중에 시간이 되면 sublime text/ visual studio code / atom 비교글을 써야겠다.)   

## 정리

### 기본
* 기본 예약어 명령어 처리 순서
  - where -> select -> order by
  - where 처리가 select보다 먼저 처리 되기 때문에 select에서 지정한 as는 where에서 사용할 수는 없다.
<br/>

### 그룹화
* COUNT(열 이름)
  - count()는 인자에 추가한 열의 개수를 구할때 null은 제외하고 추출한다.
  - 즉, 5개의 행이 있는데 1개의 행이 name : null인 경우 count(name) 할 경우 4개 출력
  - 이외에도 SUM, AVG, MIN, MAX 등의 집계함수가 있다.
  - 집계함수들은 where에서 사용할 수 없다.

* DISTINCT 열 이름
  - select -> distinct
  - ex) SELECT DISTINCT name FROM sample;
  - name의 중복 제거 및 null을 제외한 상태에서의 행 개수를 구하면?
  - SELECT COUNT(DISTINCT name) FROM sample;
  - count처럼 다른 집계함수들도 동일하게 DISTINCT와 조합하여 사용 가능하다.
<br/>

* GROUP BY
  - group by나 distinct는 집계함수를 사용하지 않는 한 큰 차이가 없다.
  - 내부처리 순서 : where -> group by -> select -> order by
  - SELECT name, COUNT(name) FROM sample WHERE **COUNT(name)**=1 GROUP BY name;
  - 위 쿼리는 실행되지 않는다. 내부처리 순서로 인해 GROUP BY가 WHERE보다 이후에 실행되기 때문에 그룹화 된 결과에서 조건을 실행시킬 수가 없다.
  - 이렇게 그룹화 된 결과물에서 조건절 실행을 하려면 **HAVING** 응 사용한다.
<br/>

* HAVING
  - SELECT name, COUNT(name) FROM sample GROUP BY name **HAVING COUNT(name) = 1**;
  - 내부처리 순서 : where -> group by -> having -> select -> order by
<br/>

* 복수열 그룹화
  - SELCT no, name, quantity FROM sample GROUP BY name;
  - 위 쿼리는 오류가 발생한다.
  - name으로 그룹화 하였지만, name이 A인 그룹의 quantity는 1,2로 2행이 있으면 어느 값을 반환할지 몰라 에러가 발생한다.
  - 이럴 경우 다른 열들도 집계함수에 추가하면 에러를 피할 수 있다.
  - SELECT **MIN(no)**, name, **SUM(quantity)** FROM sample GROUP BY name;

* 그룹화의 결과물 정렬
  - SELECT MIN(no), name, SUM(quantity) FROM sample GROUP BY name **ORDER BY SUM(quantity) DESC**;

### 서브쿼리
* sample 테이블에서 가장 작은 a 값을 가지고 있는 행 삭제하기
  - 대부분의 DB : DELETE FROM sample WHERE a = **(SELECT MIN(a) FROM sample)**;
  - MySQL : DELETE FROM sample WHERE a = **(SELECT a FROM (SELECT MIN(a) AS a FROM sample) AS x)**;
  - MySQL은 데이터를 추가/갱신 할 경우 동일 테이블을 서브쿼리에 사용할 수 없도록 되어있어, **인라인 뷰로 임시 테이블을 만들어 처리**해야만 한다.
