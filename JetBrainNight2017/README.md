# JetBrains Night 2017

JetBrains Night 2017에 다녀왔습니다.  
사전에 JetBrains 관련 커뮤니티 분들과 


## 1. IntelliJ IDEA의 Tip & Tricks

주제가 [2016년도](http://jojoldu.tistory.com/61)와 같아서 내용이 거의 비슷하겠구나 싶었는데, 생각보다 많이 달랐습니다.  

> 위 링크를 통해 한번 보시고 아래 내용을 보시면 더 좋습니다.

모든 단축키는 Mac OS X 기준으로 진행됩니다.

* Structure (```command+7```)

![structure](./images/structure.png)

> 해당 클래스의 전체 구조를 보여주는데, 이게 왜 장점인지 궁금해하실텐데요.  
lombok을 적용하고 실제로 잘 적용되었는지, 어떻게 구성되었는지를 이를 통해 바로 확인할 수 있습니다. 

* search에서 ```#``` 를 태그처럼 활용할 수 있음

![hashtag](./images/hashtag.png)

> ```command+shift+a```로 action 검색시, plugins 옵션만 바로 검색하고 싶다면 ```#plugins```로 하면 plugins 옵션들만 바로 출력도비니다.

* ```Test Restful```
  * ```tr```로 abrigation 으로 등록하여 쉽게 찾기
* autoscroll 선택하면 파일선택시마다 좌측 프로젝트 화면 변경

* ```ctrl+shift+space``` 스마트 자동완성

![basic_completion](./images/basic_completion.png)

(Basic : ```Ctrl+Space```)

![smart_completion](./images/smart_completion.png)

(Smart : ```Ctrl+Shift+Space```)

> 좀더 상세한 차이는 [가이드 문서](https://confluence.jetbrains.com/display/IDEADEV/Completion+features) 를 참고하세요.


* ```option+/```
* 문자열 안에서 ```option+enter```로 -> ```inject Language or Reference``` 
* ```ctrl + shift + j``` : String join line
* Refactor 중, 상속 -> 구성으로 변경하기
* Analaze Date Flow 로 현재 변수/필드의 데이터 변경 지점 찾을수 있음
* Duplicates Project로 중복된 코드 찾기
* Structural Search로 원하는 형태의 템플릿 코드들 찾기

```java
try{
    $code$;
}catch(Exception e){
    System.out.println(e.getessage());
}
```

* Replace Structure 로 배드 패턴은 교체할 수 있음
* Stream Debugger 를 통해서 각 스트림으로 데이터가 어떻게 변하는지 확인 가능 (디버거 버튼 가장 우측)
* JVM Debugger Memory View Plugin으로 메모리 확인 가능
* SQL 문자열로 쿼리 실제 바로 실행 가능
* .http 파일로 Rest 테스트 대상 기록할 수 있음
* plugins -> MetricsReloaded
* Advanced Java Folding Plugins : Scala 표현식으로 보여줌




