# 윈도우에서 맥북으로 넘어오기
이직한 회사에서는 맥북을 사용하게 되었는데, 윈도우에서 맥북으로 넘어오면서 특수키부터 시작해서 단축키까지 너무 달라서 적응하기가 힘들었다.<br/>
그래서 나같은 개발자가 또 있겠지 싶은 마음에 하나하나씩 정리할 예정이다.

## 단축키
맥은 특수키들이 외우기가 어려웠다. 이를 쉽게 외우는 [링크](http://macnews.tistory.com/564)를 참고하자. <br/>

![특수키](./images/특수키.png)

* 같은 어플리케이션들간 이동하기 : Command + ~
* 화면 넘기기 : Control + 좌우 방향키
* 화면 전체로 보기 : Control + 위 방향키
* 뒤로가기/잘라내기/복사/붙여넣기 : Command + z/x/c/v
* 작업취소 : Control + c
* 현재 열린 윈도우 새로 열기 : Command + n
* 파일 삭제 : Command + delete
* Dock 숨기기 : Command + Option + d
* 크롬 검사창 열기 (Windows에서 F12) : Command + Shift + c

* [Alfred](http://macworld.hjsong.net/62) 
  - 전체 어플리케이션 검색
  - 실행 : Option + Space
* [Monosnap](http://macnews.tistory.com/1064)
  - 기본 캡쳐 기능은 **캡쳐 후 바로 편집이 힘들다**. Monosnap 사용을 추천한다.
  - **영역지정** 캡쳐 : Option + Command + 5
  - **전체영역** 캡쳐 : Option + Command + 6
* [iterm2](https://gist.github.com/helger/3070258)
  - [iterm2 꾸미기](https://beomi.github.io/2017/07/07/Beautify-ZSH/)
  - 탭 이동 : Command + 좌우 방향키
  - 윈도우 세로 분할 : Command + shift + d
  - 윈도우 가로 분할 : Command + d
  - 윈도우 이동 : Command + Option + 방향키
  - [단축키 모음](https://gist.github.com/squarism/ae3613daf5c01a98ba3a)
* [Spectacle](http://macnews.tistory.com/3198)
  - 윈도우 화면 분할 : Option + Command + 방향키
  - 윈도우 풀 스크린 : Option + Command + f      

* [Scroll Reverser](http://macnews.tistory.com/1158)
  * 일반 휠 마우스를 쓸때 사용성을 그대로 가져갈 수 있는 앱
  * 터치패드와 휠 마우스의 휠이 사용성이 반대라서 발생하는 문제를 해결
* [Itsycal](http://macnews.tistory.com/3023)
  * 맥용 달력
  * 기본달력이 워낙 구려서 이걸 추천

## 개발환경

### Java8

```bash
brew tap caskroom/versions
brew cask install java8
java -version
```

### git

```bash
brew install git
```

### node

```bash
brew install node
npm install -g n
sudo n 본인이 원하는 버전
node --version
```

### Gradle

```bash
brew install gradle
gradle -v
```

### IntelliJ

* [ToolBox](https://www.jetbrains.com/products.html?fromMenu)
* File -> export settings으로 세팅 복사

## IntelliJ 단축키

IntelliJ의 경우 검색하면 다 나오는거지만 그때 그때 찾는 것도 귀찮고, 빨리 외우고 싶어서 작성한다. <br/>
Keymap 버전은 Default (Mac OSX 10.5+) 이다. <br/>
(여담이지만 Atom의 경우 플러그인으로 intelliJ용 Kaymap을 제공하고 있어 해당 플러그인 설치시에 아래 단축키들을 Atom에서도 그대로 사용 가능하다.) <br/>

* 전체 검색 : shift + shift
* Action 검색 : Command + shift + a
* Generate (생성자,getter,setter등) : Command + n
* 파일 저장 : Command + s
* 라인 삭제 : Control + delete
* 자동 완성 : Control + Space
* 라인 자동 정렬 : Control + Option + i
* 라인 추가 : Shift + return (enter)
* 라인 복사 : Command + d
* 라인 삭제 : Command + delete
* 라인 이동 : Option + shift + 위아래 방향키
* 선택 영역 좌우 이동 : Option + Shift + Command + 좌우 방향키
* 다음 단어로 이동 : Option + 좌우 방향키
* 최근 파일 리스트보기 : Command + e
* 현재 Run 실행/디버깅 : Control + r / d
* 현재 테스트 코드 실행/디버깅 : Control + Shift + r / d
* 다음 break point : Option + F9
* 현재 코드에서 수행 가능한 기능 출력 : Option + return (Enter)
* 구현 클래스 혹은 메소드로 이동 : Option + Command + b

### 키보드 매핑 (Realforce)

타버전은 모르겠는데, Sierra 버전에서는 **Karabiner 설치시 기본 키보드 설정이 안먹힌다.** <br/>
Realforce와 같은 윈도우 계열의 키보드 연결/비연결시 option키와 command키 변경을 자동으로 전환되도록 하기 위해 기본 키보드 설정만 사용하고, **그외 키보드 설정 어플리케이션은 사용하지 않는다.** <br/>

![키보드](./images/키보드.png)

(Realforce일 경우엔 option과 command를 변경) <br/>

![키보드 한영전환](./images/키보드_한영전환.png)

(맥의 한영전환키는 기본적으로 Control+Space로 되어있다. <br/>
때문에 IntelliJ에서의 자동완성키와 겹치기 때문에 **command+space**로 변경한다.) <br/>
<br/>

### 스크롤바

맥에서 IntelliJ를 사용할때 당혹스러운 것이 스크롤바가 보이지 않는 다는 것이다. <br/>

![스크롤바1](./images/스크롤바1.png)

(이렇게 가로/세로로 나타나는 스크롤바) <br/>
이 옵션은 IntelliJ 설정으로는 나타나지 않아서 검색해보니 **맥의 설정**을 수정하는 것이였다. <br/>
시스템 환경설정 -> 일반 -> 스크롤 막대보기 옵션 -> 항상 체크

![스크롤바2](./images/스크롤바2.png)

(위 이미지 참고)<br/>

### 키보드 속도

키 입력 반응 속도를 높이는 방법

![키보드속도](./images/키보드속도.png)

**Ing....**
