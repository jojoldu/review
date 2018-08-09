# 오픈소스, 줘도 못 먹나 - 오픈소스로 팀의 개발 생산성 높이기 by kenu

오늘은 OKKY의 창시자이신! kenu 허광남님의 세미나가 있어 다녀왔습니다.  
주제는 **오픈소스, 줘도 못 먹나 - 오픈소스로 팀의 개발 생산성 높이기**입니다.  
  
세미나 주제를 보고 신청하신 분들도 많으시겠지만, kenu님의 발표라 믿고 신청하신 분들도 많지 않았을까 싶습니다.  
(뇌피셜..)  
  
여튼! [세미나 공지](https://okky.kr/article/486676)를 보시면 대략적으로 어떤 세미나였을지 아실수 있을것 같습니다.  
자 그럼 본격 발표 내용을 시작하겠습니다.

## 0. Intro

![발표자소개](./images/발표자소개.png)

(Kenu-허광남님)  

* 개발인생 34년 이야기
    * 14살부터 시작
* 많은 소프트웨어를 개발했지만 **현재까지 남아 있는건 OKKY**
* 현재 나이(48세)에도 개발팀을 이끌고 개발하고 있는, **살아 남기 위한 제 이야기**입니다.

* [발표자료](https://www.slideshare.net/kenu/ss-109189829)


## 1. 오픈소스에 대한 사실과 오해

* 인간 vs 컴퓨터
    * 컴퓨터가 인간을 앞지른 시기
    * 뛰어난 능력을 가진 **컴퓨터에 명령하는 것이 프로그래머**
* 개발자의 아이러니
    * 자동화는 내가 편하려고 하는 것
    * 근데 자동화하려고 드는 비용이 더 많을때가 있음
* **정말 어려운 것은 사람**
    * 저에게 일 부탁하는 사람들은 생각이 다름
    * 블랙박스처럼 개발자를 바라보며 무슨 일 하는지 이해하지 못함
* 컴퓨터 < 프로그래머 << 사람
    * **사람이 원하는 것을 만들어 컴퓨터에게 전달하는 역할**
* 절차
    * 요구사항 -> 분석 -> 설계 -> 구현 -> 테스트 -> 배포
    * 출시하면 그때부터 고생 시작
    * 나에게 일을 시킨 윗 사람의 생각은 고객 생각과 다름
* 어떻게 하면 좀 더 편하게 일 할 수 있을까
    * Log
        * 기록을 잘 남기는 것이 중요
        * 이슈트래커가 있어도 팀장님의 의지가 굉장히 중요함
        * 나같은 경우 메일/메신저로 업무 요청이 오면 그 내용 그대로 복사해서 이슈트래커 생성을 하고, 가이드 함
        * 이슈트래커는 소프트웨어 설치만 한다고 쓸수 있는것이 아님
        * **이슈트래커로 업무를 진행하는 문화 조성이 필요**
    * 테스트 자동화
        * 테스트 케이스가 만들어져 있으면, 다른 사람이 와도 **사이드 이펙트를 체크하기가 쉬움**
        * 새로운 코드, 새로운 기능, 새로운 사람이 왔을때 다른 코드와 기능은 문제가 없는지 보장해줄 수 있는 가장 쉽고 확실한 방법
    * 단계별 오픈소스
        * 요구사항: 이슈트래커
        * 분석: 마인드맵
        * 구현: IDE (or 편집기), 디버거
            * 현재는 VS Code에 푹빠짐
        * 테스트: 수동테스트/자동테스트
        * 배포: 빌드/CI도구
* 오픈 소스 사용하기 전
    * 오픈 소스를 대하는 자세
        * 처음부터 너무 깊게 가지 말기
        * **우선 사용**부터
* 오픈 소스 시작하기
    * 우선 사용부터
    * 다운로드 하고 실행하기
    * 장황한 메뉴얼?
        * 블로그나 책에 있는 오픈소스를 따라하려면 **버전을 꼭 맞출것**
        * 최신 버전으로 하면 엄청난 삽질을 하게 됨
    * Getting started 한번 돌려보기
    * **TV 리모컨 버튼 5개 법칙**
        * 채널 업다운 버튼 2개, 볼륨 업다운버튼 2개, 전원버튼 1개만 알면 원하는걸 사용할 수 있음
        * 오픈소스도 마찬가지인데, **필요한 것들, 디폴트 세팅으로 시작**하면 됨
* 오픈 소스 전문가?
    * 전문가는 교육 받아 만들어진다? No
    * 경험 = 시간 + 시행착오 + 성공/실패
    * **특정 주제로 3년간 블로그를 열심히 쓰면 주변에서 전문가**라고 부르게 됨
* 오픈 소스 트러블 슈팅
    * "에러메세지" 구글링
        * ```""``` 로 감싸 구글링하면 **검색어가 분리되지 않음**
    * 스택오버플로우에서 검색
        * ```-site:stackoverflow.com``` : 스택오버플로우 제외한 검색결과
        * ```site:stackoverflow.com``` : 스택오버플로우에서 검색결과
    * 한국어로 번역...
* 오픈 소스 함부로 수정하지 마라
    * 버전 업 주기가 매우 빠름
    * Pull Request 이용할 것
* 최신 버전을 대하는 자세
    * 마루타
    * Bloody Edge
        * 최전선에서 피가 난무하는 상황
        * 최신 버전 쓴다고하면 상당한 각오가 필요할 것
        * LTS를 사용할 것
* LTS로 대동단결
    * LTS라는 용어는 우분투로부터 시작
    * Long Term Support
    * 서비스에 사용할 버전
    * **최소 2년은 안심하고 써도 됨**
* 팀장 설득하기
    * 우리팀에 도입하고 싶은데, 팀장이 꼰대?
    * 회사를 바꾸세요 or 마음을 접으세요.
* 저도 기여하고 싶어요
    * 사용
    * 블로깅
    * 설정 튜닝
    * 이슈 등록
    * 문서 번역 [kenu.github.io/tomcat70/docs](kenu.github.io/tomcat70/docs)
    * Fork, Pull Request

## 2. 오픈소스와 함께 - 인과 연

* 제가 경험한 오픈 소스 제품들
    * 톰캣을 시작으로
    * Ant
    * Eclipse
    * JUnit
    * JMeter
* 현재 사용하는 오픈 소스 제품들
    * Yona, Git, VS Code
        * Yona: 이슈트래커이면서 Git 저장소
        * VS Code: **지금껏 사용한 에디터 중에서 가장 마음에 듬** (MS 만세!)
    * Jenkins, CentOS, VirtualBox
        * VirutalBox는 윈도우 테스트에서 사용
    * Nginx, Node.js, Express.js
        * 템플릿 엔진으로는 ejs를 선호함
    * MariaDB, ELK, Uptime
        * Nginx Access log를 ELK에 다 보내서 모니터링
        * Uptime: 1분마다 서버 체크해주는 모니터링 소프트웨어
    * Mocha, SonarQube, ZAP
        * Mocha: JS 테스트 프레임워크
        * ZAP: 웹 어플리케이션 취약점 테스트 툴 (SQL 인잭션, 파라미터변조 등등)
* Yona
    * 네이버 오픈 소스
    * 이슈트래커 + Git (SVN)
    * 이메일 설정해서 사용하면 정말 깊게 쓸 수 있음
* Git
    * [상황별 Git 커맨드 정리](okdevtv.com/mib/git/status)
    * SVN을 쓰고 있는 회사라면 입사는 고민해볼것 (Git이 너무 좋음)
* VS Code
    * [VS Code Tip And Tricks](https://github.com/Microsoft/vscode-tips-and-tricks) 참고
    * **GitLens Plugin 꼭 설치**해볼것
    * [Quokka Plugin](http://jojoldu.tistory.com/286) ($50) 은 유료라 추천만..
* Jenkins
    * Continuous Integration
    * Build & Monitor 자동화
    * [https://okdevtv.com/mib/jenkins](https://okdevtv.com/mib/jenkins)
* Centos 
    * 레드햇 계열
    * AWS EC2의 AMI도 레드햇 계열
    * [리눅스 커맨드](https://okdevtv.com/mib/linux)
* Virtual Box
    * 가상머신
    * 윈도우/우분투를 위해 사용
    * [https://okdevtv.com/mib/virtualbox](https://okdevtv.com/mib/virtualbox)
* Nginx
    * 무거운 아파치 httpd
    * 설정도 가벼움
    * [https://okdevtv.com/mib/nginx](https://okdevtv.com/mib/nginx)
    * 로드밸런싱 지원
* Node.js, Express.js
    * 클라우드 시대에 메모리와 CPU는 비용
    * Java는 보통 2G 이상 vs Node.js 0.5G 충분
    * VS Code에서 디버깅 가능
    * [https://okdevtv.com/mib/nodejs](https://okdevtv.com/mib/nodejs)
    * [nodejs 시작하기](http://bit.ly/oknodejs)
* MariaDB
    * [https://okdevtv.com/mib/mariadb](https://okdevtv.com/mib/mariadb)
* ELK
    * 엑세스 로그 모니터링
    * [https://okdevtv.com/mib/elk/elk6](https://okdevtv.com/mib/elk/elk6)
* Uptime
    * 서비스 모니터링
    * 서버에 Agent를 설치하는게 아니라서 1분마다 헬스체크 하는 방식 
    * [https://okdevtv.com/mib/uptime](https://okdevtv.com/mib/uptime)
* Mocha
    * JS Unit Test Framework
* SonarQube
    * Code Quality
    * 중복 코드 발견과 같이 코드 퀄리티 체크
    * [https://okdevtv.com/mib/sonar](https://okdevtv.com/mib/sonar)
* ZAP
    * 웹 어플리케이션 보안 취약점을 찾기 위한 침투테스트
    * [https://okdevtv.com/mib/zap](https://okdevtv.com/mib/zap)

* 오픈소스로 훈련하고 상용 소프트웨어 쓰는 것도 좋은 전략

## 후기

세미나의 내용도 너무 좋았지만, 허광남님 같은 고수분들도 여전히 공부하고 기록([https://okdevtv.com/](https://okdevtv.com/))하고 계신다는 것을 알게 되서 좋았습니다.  
저도 좀 더 열심히, 그리고 오래오래 기록해야겠습니다.  