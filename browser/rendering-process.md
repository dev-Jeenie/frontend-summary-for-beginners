# 브라우저의 랜더링 순서

웹 브라우저는 HTML 문서를 해석하고, 화면을 통해 해석된 결과를 보여준다.
이걸 가능하게 하는 것이 바로 `렌더링 Rendering`.

## 렌더링이란?
: HTML, CSS, 자바스크립트 등의 리소스가 브라우저에서 출력되는 과정

### 렌더링 엔진
- 크롬 : 블링크(Blink)
ㅋㅋㅋㅋㅋ
- 사파리 : 웹킷(Webkit)
- 파이어폭스 : 게코(Gecko)

## 렌더링 과정

![개발자도구의 네트워크 탭 이미지](/assets/browser/network-tab.png)

유저가 브라우저를 통해 웹사이트에 접속하면 서버로부터 HTML, CSS 등 웹사이트에 필요한 리소스를 다운로드 받는다.

브라우저가 페이지를 렌더링하기 위해서는 HTML코드는 **DOM 트리**, CSS는 **CSSOM 트리**를 생성해야한다.

> DOM과 CSSOM
`DOM(Document Object Model)`, html 문서에 대한 인터페이스로 스크립트 언어와 프로그래밍 언어를 통해 웹 문서를 쉽게 제어하기 위해 문서를 객체화한 것을 말한다.
`CSSOM(CSS Object Modal)`, 위와 동일하게 사용자가 CSS 스타일을 동적으로 읽고 수정할 수 있는 방법.

### 1-1. DOM 트리 생성


### 1-2. CSSOM 트리 생성






# 브라우저 랜더링 기술적으로 뜯어보기



## 참고

* [면접에서 브라우저 렌더링을 묻는 이유] (https://www.reason-to-code.com/blog/why-do-they-ask-about-browser-rendering/)
* [DOM, CSSOM 누구보다 쉽게 설명해보기] (https://taeyoungcoding.tistory.com/3)
