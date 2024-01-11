# 브라우저의 랜더링 순서

웹 브라우저는 HTML 문서를 해석하고, 화면을 통해 해석된 결과를 보여준다.
이걸 가능하게 하는 것이 바로 `렌더링 Rendering`.

# 렌더링이란?

: HTML, CSS, 자바스크립트 등의 리소스가 브라우저에서 출력되는 과정

## 렌더링 엔진

- 크롬 : 블링크(Blink)
- 사파리 : 웹킷(Webkit)
- 파이어폭스 : 게코(Gecko)

# 렌더링 과정

![개발자도구의 네트워크 탭 이미지](/assets/browser-html-css/network-tab.png)

유저가 브라우저를 통해 웹사이트에 접속하면 서버로부터 HTML, CSS 등 웹사이트에 필요한 리소스를 다운로드 받는다.

### HTML, CSS 파일은 단순한 텍스트

스크립트 언어와 프로그래밍 언어로 웹 문서를 제어하기 위해서는 이 텍스트 덩어리들을 디코딩해서 문서를 객체화 해야한다.
연산과 관리가 유리하도록!

따라서 브라우저가 페이지를 렌더링하기 위해서는 HTML코드는 **DOM 트리**, CSS는 **CSSOM 트리**를 생성한다.

- `DOM(Document Object Model)`
  html 문서에 대한 인터페이스로 스크립트 언어와 프로그래밍 언어를 통해 웹 문서를 쉽게 제어하기 위해 문서를 객체화한 것을 말한다.
- `CSSOM(CSS Object Modal)`
  위와 동일하게 사용자가 CSS 스타일을 동적으로 읽고 수정할 수 있는 방법.

## 1. HTML 파싱 -> DOM 트리 생성

HTML 코드를 Document Object Model 트리로 변환하는 과정에는 총 4가지 단계가 있다.

`[바이트 코드를 문자로 변환]` -> `[토큰화]` -> `[노드화]` -> `[트리구조로 연결]`

![dom tree image](/assets/browser-html-css/dom-tree.png)
**이미지 출처** https://bit.ly/2WochoN

### 1-1. 바이트코드 -> 문자로 변환 (Bytes -> Characters)

**`인코딩`**
"문자를 바이트로"
문자를 컴퓨터가 이해할 수 있는 바이너리 데이터로 변환하는 과정.

![html encoding](/assets/browser-html-css/html-encoding.png)

**`디코딩`**
"바이트를 문자로"
변형된 형태로 저장된 파일을 원래 상태로 되돌리는 것, 복호화.

![html decoding](/assets/browser-html-css/html-decoding.png)

#### 이때 변환이 필요한 이유?

HTML 문서 안에는 스크립트와 같이 특수한 기능을 하는 문자들이 포함되는데, 게시판이나 웹 메일 등에 아래와 같은 코드를 input에 넣었을 때 작동하는 경우가 있다.(특수문자들이 그대로 전송이 되기 때문에) <br/>

```
js <script>alert("스크립트를 직접 넣어서 XSS 테스트합니다");</script>
```

이는 매우 위험하다.
스크립트가 포함된 게시글을 타 유저가 클릭하는 것만으로도 공격을 가할 수 있고,
또는 공격자가 스크립트를 포함한 URL을 유저에게 노출시켜서 서버에 스크립트가 포함된 URL로 데이터를 요청하고, response 또한 해당 스크립트를 포함하게 될 수 있다.

또는 게시글을 클릭하는 것 자체만으로도 공격이 전송되게

유저는 특정 게시글만 클릭해도
스크립트를 넣어 기능을 동작시키는 위와 같은 **_`XSS(Cross Site Scripting)` 이슈의 방어 대책_**으로 HTML 인코딩을 사용한다.

### 1-2. 토큰화

브라우저가 문자열을 W3C 표준에 지정된 고유 토큰으로 변환한다.

### 1-3. 노드화

토큰을 해당 속성 및 규칙을 정의하는 노드로 변환한다

### 1-4. DOM 생성

HTML 마크업에 정의된 태그 간의 관계를 해석해서 트리 구조로 연결

> 브라우저는 HTML 마크업을 처리할 때 마다 위의 단계를 수행한다.

## 2. CSSOM 트리 생성

스타일 작성에는 세가지 방법이 있다.

- HTML 마크업 내에 inline으로 스타일 선언하는 방법
- (external) head 태그 내부에 css 파일을 참조하는 방법
- (internal) style 태그를 정의하는 방법

HTML에 직접 마크업한 첫번째와 마찬가지로 두번째, 세번째 방법으로 작성된 스타일 또한
브라우저가 이해하고 처리할 수 있도록 변환해야한다.

![cssom](/assets/browser-html-css/cssom.png)
**이미지 출처** https://bit.ly/2Vg5Nb0

따라서 DOM 트리와 동일한 과정으로 CSSOM 트리를 생성한다.
전달받은 css 파일을 문자로 해석하고, 가지고 있는 토큰과 비교해서 노드를 발행한다.
이 노드들이 모여서 하나의 Css Object Model 트리가 된다.

![cssom tree](/assets/browser-html-css/cssom-tree.png)
**이미지 출처** https://bit.ly/2WxQefH

## 3. 렌더 트리 생성 단계

### **`DOM tree`** + **`CSSOM tree`** = **`Render tree`**

DOM 트리와 CSSOM 트리가 만들어지면 이 둘을 결합해서 렌더링 트리를 생성한다.

모든 요소들의 구조와 텍스트가 존재하는 `DOM tree`와는 달리 `Render tree`에는 스타일 정보가 설정되어 있다.

그리고 이 `Render tree`에는 렌더링에 필요한, <u>**실제 화면에 표현되는 노드만**</u> 포함된다.

![rendering tree image](/assets/browser-html-css/rendering-tree.png)
**이미지 출처** https://bit.ly/3iQ3ovQ

### 그럼 실제 화면에 표현이 안되는 노드도 있어?

`display: none;`과 같은 속성이 설정된 노드는 화면에 공간을 차지하지 않기 때문에,
렌더트리를 만드는 과정에서 제외된다.

`visibility: invisible;`은 비슷하게 동작하지만, 공간은 차지하고 요소만 보이지않기 때문에 렌더트리에 포함된다!
(CSS 파트에서 자세히 설명)

## 4. 레이아웃(Layout) 단계 - `리플로우` : 위치, 크기 계산 📐

위의 렌더 트리를 배치하는 단계.
앞서 만들어진 돔과 계산된 스타일을 따라가면서
요소의 크기, 좌표 등의 정보를 담은 **레이아웃 트리를 생성**한다.

![layout tree](/assets/browser-html-css/layout-tree.png)
**이미지 출처** 제주코딩베이스캠프 [브라우저는 어떻게 화면을 렌더링할까?](https://www.youtube.com/watch?v=z1Jj7Xg-TkU)

위의 그림처럼, DOM 정보와 함께 CSSOM 트리에 들어있던 css 관련 정보를 합친 하나의 트리가 바로 렌더 트리.

#### display: none은 렌더트리에 포함되지 않는다

렌더트리 안에 요소가 없어서 화면을 읽는 스크린 리더가 읽지 못하는 문제가 있다.

![layout](/assets/browser-html-css/layout.png)
**이미지 출처** https://bit.ly/3xdGFyN

뷰포트 내에서 각 요소의 정확한 위치와 크기를 정확하게 캡처하는 Box 모델이 출력된다.
모든 상대적인 측정값은 화면에서 절대적인 픽셀로 변환된다

## 5. 페인트 단계 - `리페인트` : 꾸미기 🎨

앞서 만들어진 렌더 트리를 따라서 페인트 기록이 생성된다.
요소 렌더링 순서와 지금까지의 정보를 바탕으로 해서 페이지를 여러개의 레이어로 나누고,
텍스트, 색, 이미지, 그림자, 보더 등 **모든 시각적인 부분을 그리는 작업**.

## 6. Composite

앞서 페인트에 단계에서 만들어둔 여러가지 레이어를 스크린에 픽셀로 표현.
나눠져있던 레이어들을 하나로 합성해서 페이지로 완성.

# CSS

## 초기렌더링 완료 후

Reflow(크기나 위치부터 변경), Repaint()

요소의 스타일을 변경하기 위해서는 JS가 실행되고 Style 변경, 계산이 다시 일어난다.

변경사항이 생길 때마다 JavaScript > Style > Layout > Paint > Composite 의 이 모든 과정을 거칠 필요는 없다.
레이아웃 크기나 위치에 영향이 가지 않는 Paint Only 속성을 사용하면 렌더트

### 대표적인 Reflow 속성 (레이아웃 단계, 렌더트리부터 재생성)

|              |             |             |            |             |
| ------------ | ----------- | ----------- | ---------- | ----------- |
| position     | width       | height      | left       | top         |
| right        | bottom      | margin      | padding    | border      |
| border-width | clear       | display     | float      | font-family |
| font-size    | font-weight | line-height | min-height | overflow    |

### 대표적인 Repaint 속성 (페인트 단계, 리페인트만 실행)

|               |                  |                     |                   |                 |
| ------------- | ---------------- | ------------------- | ----------------- | --------------- |
| background    | background-image | background-position | background-repeat | background-size |
| border-radius | border-style     | box-shadow          | color             | line-style      |
| outline       | outline-color    | outline-style       | outline-width     | text-decoration |
| visibility    | ...              |

### **Reflow, Repaint 모두 제외하는 속성**
`transform`, `opacity`

이 두가지 속성은 돔 트리를 변경하지 않도록 설계가 되어있기 때문에 Reflow, Repaint를 모두 일으키기 않는다.
(GPU가 관여할 수 있는 속성이기 때문)

브라우저 매우 쾌적 🌎👍


---


# 위의 내용을 한 흐름으로 이해하려면

제주코딩베이스캠프의 아래 영상으로 정리하기 👍
[브라우저는 어떻게 화면을 렌더링할까?](https://www.youtube.com/watch?v=z1Jj7Xg-TkU)

---


# 브라우저 랜더링 기술적으로 뜯어보기
- [면접에서 브라우저 렌더링을 묻는 이유](https://www.reason-to-code.com/blog/why-do-they-ask-about-browser-rendering/)


# 정리 : 웹 브라우저에 URL을 입력하면 무슨 일이 일어나는가?
- [웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://www.youtube.com/watch?v=GAyZ_QgYYYo)


# 이에 따른 렌더링 최적화
- [브라우저 렌더링 과정 - Reflow Repaint, 그리고 성능 최적화](https://boxfoxs.tistory.com/408)



## 참고

- [면접에서 브라우저 렌더링을 묻는 이유](https://www.reason-to-code.com/blog/why-do-they-ask-about-browser-rendering/)
- [DOM, CSSOM 누구보다 쉽게 설명해보기](https://taeyoungcoding.tistory.com/3)
- [브라우저의 렌더링 과정](https://medium.com/%EA%B0%9C%EB%B0%9C%EC%9E%90%EC%9D%98%ED%92%88%EA%B2%A9/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-5c01c4158ce)
- [인코딩(Encoding)이란 ? : ASCII, URL, HTML, Base64, MS Script 인코딩](https://ghdwn0217.tistory.com/76)

- [웹 브라우저에 URL 입력하면 일어나는 일 - 인프라 위주](https://www.youtube.com/watch?v=GAyZ_QgYYYo)
- [브라우저는 어떻게 화면을 렌더링할까](https://www.youtube.com/watch?v=z1Jj7Xg-TkU)
- [문자열을 변환해볼 수 있는 사이트](https://www.convertstring.com/ko/EncodeDecode/HtmlDecode)
- [브라우저 렌더링 과정 - Reflow Repaint, 그리고 성능 최적화](https://boxfoxs.tistory.com/408)
