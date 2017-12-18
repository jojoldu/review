# Java And Spring Forward

## 1. Java9 New Feature - 양수열

* 아무래도 2020년까지는 자바9 도입에 눈치를 볼것 같음
* 자바가 그동안 지켜왔던것중 하나가 하위호환성
  * 닷넷 출시후 VB 개발자들의 상당수가 자바로 넘어 왔는데, 그 이유는 VB와 닷넷이 차이가 너무 심했기 때문
  * 자바5 제네릭, 자바8 람다 등 이 도입되었음에도 하위호환성은 계속 유지 되었음
* 자바 9의 경우 가장 큰 변화가 이루어짐
  * 자바원 발표를 들었던 많은 자바 챔피언분들이 큰 혼란을 느꼈을 정도로 변화가 큰 버전

### Standards

* javadocs가 HTML5 지원 
* 유니코드 7, 8 추가
* SHA3 해시 알고리즘 추가
* 자바 공식 문서 검색 기능 추가와 같은 개편

작은 기능

* UTF-8 Property file
* XML Catalog API
* DRBG 기반의 암호키 알고리즘 생성 구현체 추가

### Major Features

> 자바9의 메인 기능이 모듈관리 기능  
기존 오픈소스 프로젝트들은 Modular 기반으로 변경이 필요함  



* 클라우드에선 CPU, 메모리, 디스크 등등 쓰는 모든 것은 다돈인 상황에서 과연 자바가 적합한가?
  * 자바가 클라우드 친화적이진 않다고 생각했었음
  * Modular 기반에선 가능하다고 봄
* 현재 안드로이드 VM에서 자바는 여러 쓸 수 있는 언어 중 하나로 전환
  * 더 작은 환경 (IoT) 에서 돌릴수 있길 바람
* 호환성이 큰 이슈
  * 기존 자바는 Modular 베이스가 아님 (라이브러리, 프레임워크 등 베이스)

Project Jigsaw

* JDK를 기준으로 모듈 단위로 쪼개고 로딩할 수 있도록 변경된 것이 직소
  * module java.base (Object 클래스와 같은 구조)
  * module java.logging
  * module java.sql
  * module java.xml

* 자바9 이전에선 디렉토리/패키지/클래스 구조
  * 자바9에선 디렉토리/모듈/패키지/클래스 구조로 이루어짐
  * 꼭 필요한 것들 아니면 같이 로딩할 필요 없도록

```java
moduel com.abc {
    require 모듈명
    require 모듈명
}
```

jlink

* Java Linker 
  * Java Version Numbering 이 가능
  * ```java.base@9```, ```com.onface.app@1.0``` 등으로 모듈별 버전을 다르게 지정해서 받을 수 있음

Multi-Release Jar Files (JEP 238)  
  

jshell 

* Java Shell
* Read-Evaluate Print Loop (REPL)을 위한 자바 플랫폼
* 개인적인 유추
  * 클라우드에서 서버리스와 관련된 지원이 강화되고 있음 (AWS 람다, AZURE 펑션 등)
  * jshell 서버리스를 지원하는 좋은 기능이라 생각함

More Concurrency Updates

* 리액티브 프레임워크 들에 대한 지원이 강화

강화된 Deprecation

* 기존엔 메세지만 보여줬음
* 기간을 정해서 그때까지만 쓸 수 있다던지 등등의 강화된 어노테이션 추가

G1GC

* 자바8에서 많은 성능 향상이 있었는데, 자바9에선 GC가 변경됨
  * 가비지 컬렉터가 G1GC로 변경
* GC가 발생하면 모든 쓰레드가 멈추게 되니 GC를 최대한 줄이는게 이슈
  * Low Pause Collector

Better String Performance

* String 관련 성능 이슈가 굉장히 얘기가 많았음
* Compact String

JVM logging

* 로그를 어떻게 수집하고 보여줄지에 대한 고민이 많았음
  * JVM상에서 로그 레이어를 전부 땡겨와 처리할수 있도록 지원
  * GC logging도 Unified됨

### 삭제된 기능

* SHA-1 certificates는 삭제
  * 기존에 SHA-1 certificates 쓰는 프로젝트는 자바9에서 안될 수 있음
* Applet API Deprecate
* JRE vserion selection CLI 옵션 삭제
* JVM TI hprof agent 삭제
* jhat tool 삭제

### Summary

* 제일 큰 변화는 모듈화
  * 아직까지 많은 챔피언들 사이에서 이야기 되고 있음
  * 이 변화를 수용하고 같이 따라올꺼냐, 다른 생태계로 넘어갈것이냐의 과도기가 될것 같음
* 인터렉티브 개발자 기능
  * REPL, jshell
* 짜잘하지만 많은 성능/표준 기능들
  * 모듈화 기능이 워낙 비중이 커서 각광받지 못했음
  * GC 등의 변경은 크게 관심을 가졌을것 같음
* 사견이지만, 아직까진 관망할 시기

## 2. HTTP/2 세대의 Java - 정상혁

> 네이버 메인의 네트워크를 확인해보면 Http2 요청을 확인해볼 수 있음  
이미 세상이 Http2로 변화되고 있음  
내가 개발을 하지 않는다고 오지 않은것이 아님

* 헤더/바디 구조가 달라지진 않았지만, 전송방식이 변경됨
  * 기존의 텍스트 베이스가 바이너리 기반으로 변경됨
* 하나의 Http2 연결은 여러개르이 Stream을 동시에 연결하여 전달
* 기존 Keep-Alive 옵션은 무시됨
* Pseudo-header 필드
  * HTTP 1.1 메세지 시작줄에 표시하던 정보
  * ```:```로 시작
* HTTP1.1을 계승했지만 다른 프로토콜
  * 1.1로 요청이 오면 Response Code로 101을 내릴수 있음

발표에 사용할 용어

* h2: HTTP2 통신
  * 브라우저 to 서버 통신
* h2c: TLS없는 HTTP2 통신
  * 서버 to 서버 통신

### 기대

* 스태틱 파일 안합쳐도 된다
  * 더 효율적인 파일별 캐시
  * 빨라지는 FE 빌드
* Server push한 페이지에 필요한 자원은 미리 전송
  * 더이상의 계단 그래프는 없음

### Java 생태계의 HTTP/2 지원

서블릿 4.0: Server push

* Tomcat 9, Jetty 10에서 지원

자바9: HttpClient

* 신규 클라이언트 클래스

### 이상적인 서버 구성

레거시 구성

* 브라우저 -> L4 -> 웹 서버 -> WAS
  * 브라우저 -> L4 : 1.1
  * L4 -> 웹서버 : 1.1
  * 웹 서버 -> WAS : 1.1

http2 이상적인 구성

* 브라우저 -> ? -> ? -> WAS
  * 브라우저 -> ? : 2
  * Nginx에서 TLS를 벗겨내서 전달
  * ? -> WAS : 2


### 현실의 난관

* Ningx: 1.9.5 이상에서 HTTP2 지원
  * 1.9.9 미만에서는 upstream으로의 HTTP 1.1 연결에 버그가 있었음
  * 서버 push 지원 안함
* upstream 연결에 h2c 연결 지원 안함


어플리케이션 서버의 제약

* 톰캣9는 아직 베타
* 제티10은 아직 스냅샷
* AJP로는 서버 Push가 지원되지 않음
* 자바단에서 TLS off loading 하면 느림
* Java 9 서드파티 라이브러리 미지원
  * lombok 불안정

### 정리

* HTTPS 전환, Nginx 사용
* Nginx 에 설정 추가
  * 앞단에서만 2.0 쓰도록
* 서버 푸시를 포기하고 친숙한 구성
  * 앞단에 Nginx
  * Upstream 서버는 HTTP 1.1 시대와 동일하게 하면 됨

* 브라우저 -> L4 -> 웹 서버 -> WAS
  * 브라우저 -> L4: 2.0
  * L4 -> 웹 서버: 2.0
  * 웹 서버 -> WAS: 1.1
* 고민대상
  * 헬스체크 방식
  * 배포시 서버를 제외하는 방식
  

## 3. 