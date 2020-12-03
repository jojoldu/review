# (2019.11.21) 우아한 레디스 - 강대명


## 1. 레디스 기본


### Redis Collection

* 컬렉션 선택에 따라 기능별로 속도 차이가 심하게 난다.
* 즉, 해당 기능에 적절한 컬렉션을 선택하는 것이 중요하다.
* ex)
  * key/values, SortedSet
  * SortedSet: score 필드를 사용해 랭킹을 정하여 정렬시킬 수 있다.
    * 단, score가 정수가 아니라 ```double```이기 때문에 **값이 정확하지 않을수 있다**.
  * 하나의 컬렉션에 너무 많은 아이템을 담으면 좋지 않음
    * 1만개 이하 (몇천개) 수준으로 유지하는게 좋다.

## 2. 레디스 운영

### 메모리 관리

* Maxmemory를 설정하더라도 이보다 더 사용할 가능성이 더 크다
* RSS값을 모니터링 해야함
* 큰 메모리를 사용하는 인스턴스 하나 보다는 적은 메모리를 사용하는 인스턴스 여러개가 안전하다.
* 메모리 파편화가 발생할 수 있다

### 메모리 줄이기 위한 설정

* Hash, Sorted Set, Set
  * 위 자료구조들은 메모리를 많이 사용한다.
* Ziplist를 이용하자
  * 속도는 조금 느리더라도, 메모리 절약이 많이 된다.
  * 20~30%이상 절약 가능

### Single Thread의 의미

* 오래 걸리는 작업은 하면 안된다
  * KEYS
    * scan으로 대체한다.
  * FLUSHALL, FLUSHDB
  * Delete Collections
  * Get All Collections
    * Collection의 일부만 가져오거나 (Sorted Set에서 지원)
    * 큰 Collection을 작은 여러개의 Collection으로 나눠서 저장
* List (O(N)) 컬렉션으로 저장/삭제 하기 보다는 Set (O(1)) 으로 해야 성능 저하가 없다

### Redis Replication

* Async Replication
  * 즉, Replication Lag이 발생할 수 있다.
* Replication 과정에서 fork가 발생하므로 메모리 부족이 발생할 수 있다.
* Redis-cli --rdb 명령은 현재 상태의 메모리 스냅샷을 가져오므로 fork 발생 가능함

### redis.conf 권장 설정 Tip

* Maxclient 설정 50,000
* RDB/AOF 설정 off
* 특정 커맨드 disable
  * keys
  * AWS는 이미 하고 있음
* 적절한 ziplist 설정

## 3. Redis 데이터 분산

* Redis는 Cache일때 유리하지만, Persistent로는 유리하지 않다.

### Redis CLuster

* 장점
  * 자체적인 Primary, Secondary Failover
  * Slot 단위의 데이터 관리
* 단점
  * 메모리 사용량이 더 많음
  * 마이그레이션 자체는 관리자가 시점을 결정해야 한다
  * 라이브러리에서 구현되어 있어야 한다.
    * 대부분의 라이브러리는 이미 구현되어 있긴 하다.

## 4. Redis Failover

* Coordinator 기반 Failover
  * Zookeeper, etcd, consul 등의 Coordinator 사용
  * Coordinator 기반으로 설정을 **이미 관리**하고 있다면 편하게 기능 추가하고 관리가 가능
* VIP / DNS 기반 Failover
  * 헬스체커가 Primary가 죽으면 VIP / DNS를 다른 서버에 할당을 시켜 페일오버를 발생시킨다. (MHA랑 똑같음)
  * **클라이언트에 추가적인 구현이 필요없다**
  * DNS 기반은 DNS Cache TTL을 관리해야 한다.

## 5. 모니터링

* 초당 처리 요청수는 CPU에 영향을 받는다
  * CPU 하나를 쓰기 때문에 더 좋은 사양을 사용하는게 해결책
* O(N) 계열의 특정 명령이 많은 경우


## 결론

* Client-output-buffer-limit 은 메모리가 높을수록 크게 잡아야 한다
  * 이 설정보다 높은