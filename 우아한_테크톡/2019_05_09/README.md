# 

## 0. 소개

* 2000년부터 시작
    * 삼성 SDS -> 네이버 -> SKP -> NHN (구 NHN Entertainment)
* 저서
  * 자바 성능 튜닝 이야기
  * 자바 개발자와 시스템 운영자를 위한 트러블 슈팅 이야기
  * Scouter를 이용한 자바 트러블 슈팅 (예정)
  * 자바의신 1,2

## 1. 자바 성능

* 왜 성능은 중요한가?
  * 성능 개선시
    * Pinterest, COOK 등 서비스스에서 성능 향상으로 인한 사용자/이탈률 이득 확인
* 성능과 관련 있는 항목들
  * User
  * Time
  * TPS
* 사용자 (User)
  * 시스템 관리자의 관점
    * 등록된 사용자 / 등록되지 않은 사용자
  * 서버 관점
    * 로그인 사용자 / 로그인되지 않은 사용자
  * 성능 테스터의 관점
    * Concurrent User / Active User
      * Concurrent User 는 **언제든 서버에 부하를 줄 수 있는 사람**
        * 웹 페이지를 띄어놓은 사용자 등
      * Active User는 서버에 요청을 주고, 응답을 기다리는 사람 (**서버에 부하를 주는 사람**)
    * Concurrent User에 비해 Active User의 비중
      * 뉴스와 같은 컨텐츠 서비스는 5~10%
      * 콜센터 같은 경우 높은 비율
    * ActiveUser는 **성능 테스트시 VUser와 거의 동일**
      * VUser는 가상 사용자를 의미
* 시간 (Time)
  * 시스템의 관점
    * RequestTime/ResponseTime/**ThinkTime**/RequestTime/ResponseTime
      * 뉴스를 보고 다음 클릭전까지의 시간을 **ThinkTime**
      * ID/PW를 입력하는 시간등등
    * 사용자의 관점
      * 가장 성능에 영향을 많이 주는 구간은?
        * 상황에 따라 다름
        * 클라이언트 렌더링 시간/데이터베이스에서 데이터를 읽어오는 시간 등등
    * 성능 테스트시
      * **클라이언트의 연산 시간은 제외**하고 측정
      * 유명한 성능 테스트툴
        * [gatling](https://gatling.io/)
        * [ngrinder](https://naver.github.io/ngrinder/)
* TPS
  * 초당 처리량
* TPS & Time
  * TPS는 scale up / out을 통해서 증가시킬 수 있음
  * **Time은 Scale으로 불가능**
  * 둘다 튜닝을 통해서 개선시킬 수 있음
* TPS & User
  * User가 증가함에 따라 TPS는 어느정도 증가하다가 더이상 증가하지 않음
* Time & User
  * User가 증가함에 따라 Time은 일정 속도를 유지하다가, 점차 증가함
  * 지만... 실제로는 **어느순간 급격히 증가함**
* Bottleneck
  * 병목지점

## 2. 자바

## 3. 모니터링

