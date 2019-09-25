# 개발자를 위한 MySQL 쿼리 최적화

## 1. 조인

* 모든 조인은 1:N을 고려해서 진행

2015년 이후 생성된 부서의 부서명과 사원명을 조회하라

```sql
SELECT a.dept_cd, a.dept_nm, b.emp_no, b.emp_nm
FROM dept a INNER JOIN emp b
ON a.dept_cd = b.dept_cd
AND a.rgst_ymd >='2015-01-01';
```

2016년 이후 입사한 사원의 부서명과 사원명을 조회하라

```sql
SELECT a.dept_nm, b.emp_nm
FROM dept a INNER JOIN emp b
ON a.dept_cd = b.dept_cd
AND b.enter_ymd >= '2016-01-01';
```

2016년 이후 입사한 사원이 있는 부서리스트를 조회하라?

* 1:N 관계에서 결과는 1만 필요한 경우엔?

```sql
SELECT a.dept_cd, a.dept_nm
FROM dept a INNER JOIN emp b
ON a.dept_cd = b.dept_cd
AND b.enter_ymd >= '2016-01-01';
```

* 중복 결과는 어떻게 하지?
* distinct, group by 등을 사용

### Semi Join

* 조인 되는 행의 개수와 상관 없이 Outer 테이블의 결과를 추출하는 Join 방법

사원이 있는 부서 리스트를 조회하라

```sql
SELECT distinct a.dept_nm
FROM dept a, emp b
WHERE a.dept_cd = b.dept_cd
AND b.enter_ymd >= '2016.01.01'
```

* ```in (sub-query)```, ```exists (sub-query)``` 등을 사용하면 distinct와 같은 비효율 제거 가능하다


### Anti Join

* Semi Join의 부정형
* 조인에 참여하지 않은 애들만 찾겠다
  * ```not in(sub-query)```, ```not exists```, ```left join ... on ... where b.key is null```

### Nested Loop Join

* 순차적인 처리
* MySQL에서 지원하는 유일한 조인 방식
  * 최신 버전에서는 Hash Join 지원
* 먼저 처리되는 테이블의 처리 범위에 따라 전체 쿼리의 비용이 결정
* 조인에 참여하는 레코드 건수가 많아질수록 전체적인 응답 속도 저하가 발생
* 조인 컬럼에 인덱스 구성이 되어있지 않은 경우 Lookup 회수만큼 Full Table Scan을 해야 한다.

#### Nested Loop의 이해

Q. Access순서는? (두 테이블은 pk만 존재)

* 부서 A는 부서코드가 PK

```sql
SELECT 부서코드, 부서명, 사원번호, 사원명
FROM 부서 A, 사원 B
where a.부서코드 = b.부서코드
```

* N -> 1로 조회하도록 드라이빙 테이블을 구성하는게 좋다
* 즉, 사원 B를 드라이빙으로 두고, 부서 A를 찌르도록 하는게 좋다.

### 조인 순서의 결정

* 기본적으로 MySQL 옵티마이저는 모든 테이블 조인 순서에 대해서 Cost를 계산
* Cost 측정 방법은 단일 쿼리의 측정 방법과 동일
* 최소 Cost가 계산된 조인 순서를 기반으로 실행 계획 생성

### 1-7. 1:N Join 시 튜닝

* 1:N 테이블이 어딘지 확인
* select 절에 속한 컬럼 확인
  * 1만 필요하다면 그래도 튜닝할거리가 많음
  * N도 필요하면 거의 튜닝할게 없을 수도 있음
* SEMI JOIN 형태로 변경

#### 1:N 관계에서의 쿼리 패턴

* 게시글 1 : 댓글 N
* 게시글 목록에서 각 게시글마다 달린 댓글 건수 노출이 필요하다면?
* 1:N 관계의 쿼리가 필요함
* 쿼리 튜닝 요소가 거의 없음
  * 이럴 경우 2가지 방법이 있음
  * 댓글 건수 노출 기능을 제거 하거나
  * 댓글 건수 필드 or 테이블을 별도로 두어 N 쿼리 없이 1쿼리만 수행되도록 구성한다.
    * 단, 이럴 경우 애플리케이션에서는 댓글이 등록/삭제 될때마다 별도의 count 로직 수행이 필요하다.

### 1장 연습 문제

1.1) 상품(50건)테이블과 주문상품(500건)테이블이 있을 때 아래 각 쿼리의 결과는?
( 주문상품의 모든 상품번호는 상품테이블에 존재, 주문상품 테이블에 없는 상품번호는 총 5개 )

```sql
A. SELECT count(*) FROM 상품 A, 주문상품 B ON A.상품번호 = B.상품번호
B. SELECT count(*) FROM 상품 A LEFT OUTER JOIN 주문상품 B ON A.상품번호 = B.상품번호
C. SELECT count(*) FROM 주문상품 A LEFT OUTER JOIN 상품 B ON A.상품번호 = B.상품번호
```

A. 500개
B. 505개
C. 500개 

1.2) 아래 쿼리는 2016년 상반기에 등록된 상품 중 판매이력이 있는 상품을 조회하는 쿼리입니다.  
다른 JOIN 방식을 사용하여 더 빠른 결과를 얻을 수 없는지 생각해 보고, 개선된 쿼리와 실행 플랜을 비교해 보세요.

```sql
SELECT a.prod_cd, a.prod_nm
FROM prod a
INNER JOIN ordr_prod b ON a.prod_cd = b.prod_cd
WHERE a.prod_rgst_ymdt < ‘2016-07-01 00:00:00’
GROUP BY a.prod_cd, a.prod_nm ;
```

* 집계 함수를 사용하지 않는다 + a 테이블의 필드만 select한다
* Semi Join을 쓰는게 낫다.

실행 플랜을 비교해 보았다면, 기존 쿼리에서 어떤 부분이 비효율적인지 생각해 보세요. 

## 2. 인덱스

* MySQL에서는 기본적으로 B-Tree 형태의 인덱스 구조를 사용
* Leaf 노드는 Double Linked List로 연결 (인덱스를 통한 정렬)
* Memory 엔진의 경우 HASH 인덱스를 지원
* InnoDB 엔진은 B-Tree 구조를 기본으로 Clustered 인덱스 구조를 가짐
* Clustered 인덱스의 Leaf Node에 모든 Row 데이터를 저장하여, Primary Key 혹은 Unique Key (PK가 없을 경우)가 Clustered
Key 역할을 수행하며 **Clustered Key는 한 테이블에 1개만** 존재
  * 이 때문에 MySQL에서 PK로 조회하는 쿼리는 성능이 엄청 빠름
  * 모든 Row 데이터를 갖고 있기 때문
* Primary/Unique Key 모두 선언되지 않은 테이블의 경우에는 6 byte의 Hidden Key를 생성 (rowid)
  * 개발자가 별도로 이용할 수는 없음
  * 그래서 MySQL에서는 PK는 꼭 생성 해야함
* Non-Clustered Key는 데이터의 물리적인 위치 대신 PK 값을 참조함
  
### 복합 인덱스

아래 쿼리가 필요할때 적절한 인덱스는?

```sql
select * 
from ordr 
where ordr_status_cd = 'PD'
and ordr_ymdt >= '2016-07-01';
```

* 정답은 ```ordr_status_cd, ordr_ymdt```
* =(Equal) 조건은 좌측으로 배치 <, >, Between등의 Range 조건 혹은 order by, group by 조건은 우측으로 배치
* 설령 남/녀와 같이 **50%를 차지하는 Key라도** eq라면 **Range보다는 앞에 선언하는게** 좋다
* 역순인 ```ordr_ymdt, ordr_status_cd``` 의 경우엔 ```ordr_status_cd```가 **거의 효과가 없어서** 단일 컬럼 인덱스가 더 나을수도 있다.
  
### 조인연결고리 이상

### 형변환

* 형변환 우선 순위
  * 숫자와 문자가 만나면 **문자가 형변환 된다**
  * 즉, index 컬럼이 문자인데, where 조건이 숫자라면
    * index를 타지 않는다
  * 반대로, index 컬럼이 숫자인데, where 조건이 문자라면
    * where 조건문이 문자로 숫자로 변경되어 index 수행된다.
  * 문자형과 날짜형은 양방향 형변환이 일어나 인덱스를 탄다.

### Covered Index

* SELECT 구문의 요청 칼럼과 WHERE 필터 등이 특정 인덱스의 구성 칼럼인 경우
* 데이터에 대한 접근 없이 인덱스 만으로 쿼리의 결과 생성 가능
* EXPLAIN 결과의 Extra 필드에 **Using index** 표시

### Null은 인덱싱 될까?

* **MySQL은 null값도 인덱싱** 처리 된다.
  * 오라클은 null값은 인덱싱되지 않는다.
* ```is null``` 조건으로 검색시 index range scan 발동

### 동적 쿼리를 위한 인덱스 구성

* 검색 조건이 동적인 경우
  * 컬럼 4개가 있다면 => 2^4 = 16 종의 쿼리 패턴이 발생 가능
* 모든 조합에 인덱스를 추가할 수는 없다!!!
* 독립적으로 검색되는 컬럼은 경우의 수에서 제외
  * 문서 번호, 연관CI와 같이 **unique**하거나 **선택도가 높은 조건**은 항상 단일 조건만 들어옴
* 필수조건을 중심으로 데이터량과 Read/Write 빈도를 고려하여 인덱스 구성
  * 4개 검색조건중 2개가 필수항목이라면 쿼리조합은 2^(4-2) = 4 개로 줄어듬. 4개 쿼리만 살펴보면 됨
* 선행조건이 선택도가 좋다면 중간조건은 생략가능
  * ex) 이름이 ‘김태호’이고 직급이 전임인 직원의 2016년 급여내역을 조회하시오
    * (이름, 직급, 급여지급일시) 인덱스를 구성하는 것이 정확하지만 동명이인이 많아야 3~4명이므로 직급은 인덱스에
서 제외시키고 필터처리 (이름, 급여지급일시) 만으로 인덱스를 구성해도 성능에 큰 영향이 없음
* 자주 사용되는 조합을 최우선으로 인덱스 구성
  * 모든 조합을 고려할 수는 없고 모든 조합에 인덱스를 생성할 수 없으므로 사용자를 인터뷰하거나 사용자의 검색패턴
을 고려하여 자주 사용되는 조합을 최우선으로 인덱스 구성

### in절 처리

* MySQL에서 인덱싱된 칼럼에 대한 IN 검색 쿼리는 내부적으로 **UNION** 방식으로 처리
* 쿼리 플랜을 통해서는 어떤 방식으로 스캔했는지 확인하기 어려움
  * ```show status like 'handler_read_next'``` 확인 가능

### y/n 으로 구성된 컬럼의 인덱스 구성

* 5:5 에서는 차라리 인덱스 안거는게 낫다
* 99:1 에서는 인덱스 거는게 좋다.

### order by a ASC, b DESC 정렬 패턴

* MySQL은 descending 인덱스를 지원하지 않음
* asc로 생성되지만, 인덱스의 스캔방향은 양방향이므로 **단일 컬럼의 desc는 문제 없음**
  * 데이터 입력시 **음수 처리를 통해** desc 인덱스가 되도록 하면 처리 가능

## 3. 기타

### 3-1. 부정형 비교

* 인덱스가 존재하더라도 부정형 비교가 발생하는 경우 인덱스를 사용할 수 없음
    * SELECT * FROM cust WHERE cust_id <> 10;
    * SELECT * FROM cust WHERE cust_id not in (10,11,15,20);
    * ```is not null``` 조건은 **인덱스를 활용함**

### 3.4 LIMIT n

* LIMIT 절은 full scan 이나 index scan 시 조건에 맞는 개수(N)을 만족하면 스캔을 중단
* ORDER BY col1 LIMIT n 절의 경우 order by절이 인덱스를 이용할 수 없는 경우 where 조건을 만족하는 모든 데이터를 물리 정렬 수행 후 LIMIT 적용하므로 좋지 않은 응답속도를 보임
* 실제 몇건을 스캔하는지는 runtime에 알 수 있으므로 실행계획에는 limit와 무관하게 표시됨
* 실행계획에서 처리범위를 확인할 수 없지만 handler_read_xxx status를 확인하여 알 수 있음

### 3.5 Union [ALL] ~ order by

* Union [ALL] ~ order by 는 order by시 인덱스를 사용할 수 없으므로 언제나 filesort를 유발
* 테이블이 분할되어 SQL튜닝으로 해결되지 않음


### 3.6 OR

* OR 연산자는 논리합을 만들어내는 연산이므로 각 조건을 모두 확인해야함
* Expansion 방식으로 처리됨
  * 인덱스를 여러개 쓸 수 없으니 union 방식으로 하거나, 쿼리를 별도로 수행하는 등의 방식이 필요하다
    * 인덱스를 같이 쓸수 없기 때문

### 3-7. GROUP BY ~ ORDER BY NULL

* group by 는 정렬을 수반하므로 정렬이 필요치 않은 경우 ```order by null``` 구문을 활용

### 3-8. DISTINCT VS GROUP BY

### 3-9. Loose Index Scan 유도

* 인덱스를 활용할 수 없는 조건절에 조건을 추가하여 Loose Index Scan을 유도
* ex)
  * 인덱스 : ( vip_yn, age )
  * ``` select * from cust where age = 25;```
  * age에 인덱스가 없으므로 full table scan
  * vip_yn in (‘Y’,’N’) 조건을 명시적으로 넣어 index scan을 유도

## 4. 실행 계획

* type
  * index
    * index 전체 페이지를 읽는다
    * 풀 스캔 하는거라 
  * all

### 정리

* type은 Access type을 의미한다
    * 좋다 : Const, system, eq_ref
    * 나쁘다 : ALL(테이블풀스캔), index(인덱스풀스캔)
    * 처리범위에 다르다 : range, ref
* extra는 실행계획을 생성할 때 적용하는 최적화 기법을 표현하거나 부가정보를 담는다
    * 좋다 : using index(커버링인덱스), using where(메모리에서 필터링)
    * 나쁘다 : using filesort, using join buffer
* query_cache는 5.6부터 메모리 병합을 일으켜 off하는게 낫다
  * 8.x에서는 제거됨
* MySQL은 하드 파싱이라 실행 계획을 계속 갖고 있지 않음


