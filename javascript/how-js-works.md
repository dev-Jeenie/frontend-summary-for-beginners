# Execution Context 실행 컨텍스트

## 정의

실행할 코드에 제공할 환경 정보들을 모아둔 객체.
자바스크립트 엔진에 의해 만들어지고 사용되는 **코드 정보들을 담은 객체의 집합**이라고 할 수 있다.

## 종류

자바스크립트의 코드는 자신만의 실행 컨텍스트를 생성한다

**Global 실행 컨텍스트** : 자바스크립트 엔진에 의해 기본적으로 생성

**함수 실행 컨텍스트** : 함수가 실행될 때마다 생성

**Eval 실행 컨텍스트** : eval 함수 내에서 생성

## 실행컨텍스트의 작동방식

1. `글로벌 실행 컨텍스트(Global Exection Context)` 생성
2. 엔진이 스크립트 파일 실행
3. 함수 호출할 때마다 `함수 실행 컨텍스트(Function Execution Context)` 생성

![excution-context-visualized](/assets/javascript/callstack-excution-context.gif)

주의 : 스크립트 파일을 실행하기 전에 글로벌 실행 컨텍스트를 생성하지만 함수는 호출할 때 생성된다는 점

### 예시

ui.dev가 제작한 [JavaScript Visualizer 도구](https://ui.dev/javascript-visualizer/)에서 실행 컨텍스트의 작동방식을 시각화해서 볼 수 있다.

![excution-context-visualized](/assets/javascript/excution-context-visualized.gif)

```js
console.log("1 : name is undefined", name);

var name = "my name";

console.log("2 : now name has a value", name);

console.log("3 : next message");

function sum(a, b) {
  console.log("5 : sum execution");
  return a + b;
}

function invokeSum() {
  console.log("4 : invokeSum execution");
  return sum(10, 20);
}

var result = invokeSum();

console.log("6 : final result ", result);
```

1. **`글로벌 실행 컨텍스트`**는 엔진이 스크립트를 실행하기 전에 먼저 **생성된다**

- 변수와 함수가 쭉 hoisting되어 메모리만 할당된다. 아직 값은 없음
- 지금은 name, sum, invokeSum, result의 위치만 있고 값은 없는 단계
- (자바스크립트 엔진은 코드가 없는 상황에서도 전역 실행 컨텍스트를 생성하기 때문에)

2. 엔진이 스크립트 파일을 순서대로 한줄씩 **실행한다**

- 변수에 값을 할당하고 코드를 수행한다 (이제서야 console가 출력한다)
- 이제 한줄씩 실행하면서 name, sum, invokeSum, result의 값을 할당하는 단계

3. 함수를 호출하면 **`함수 실행 컨텍스트`**가 생성된다

4. result에 값을 할당하고 난 뒤 맨마지막 코드 (console log)가 실행된다

## 실행 컨텍스트 스택 (Execution Context Stack)

실행 컨텍스트가 생성되면 실행컨텍스트 스택(=콜 스택)에 쌓이게 된다.

`글로벌 실행 컨텍스트(Global Exection Context)`는 코드를 실행하기 전에 쌓이고, 모든 코드를 실행하면 제거된다.
`함수 실행 컨텍스트(Function Execution Context)`는 호출할 때 쌓이고 호출이 끝나면 제거된다.

![callstack-excution-context](https://miro.medium.com/v2/resize%253Afit%253A1100/1%252AdUl6qPEaDJJTXWythQsEtQ.gif)


## Execution Context와 Scope

execution context와 scope는 같은 개념 아니다.
### Scope
: Scope is function-based

함수 내에서 변수에 대한 접근을 관리한다.
JavaScript에서는 전역 스코프와 함수 스코프(또는 지역 스코프) 두 가지 스코프만 존재

### Execution Context
: Execution context is object-based

현재 코드가 실행되는 환경에 대한 정보를 가지는 추상적 개념.
함수의 context는 해당 함수의 **`this 포인터`**가 가리키는 값

# closer

# 변수 범위 (Variable Scope)

변수 범위 : 변수가 존재하는 컨텍스트.
어디에서 변수에 접근할 수 있는지, 그 컨텍스트에서 변수에 접근할 수 있는지를 명시적으로 나타낸다.
https://www.frontendinterviewhandbook.com/kr/javascript-questions#%ED%98%B8%EC%9D%B4%EC%8A%A4%ED%8C%85%EC%97%90-%EB%8C%80%ED%95%B4-%EC%84%A4%EB%AA%85%ED%95%98%EC%84%B8%EC%9A%94

# hoisting

https://github.com/JaeYeopHan/Interview_Question_for_Beginner/blob/main/JavaScript/README.md#hoisting
함수 안에 있는 선언들을 모두 위로 끌어올려서, 해당 함수의 유효범위(스코프) 최상단에 선언하는 것.

자바 스크립트 Parser가 함수 내에서 아래쪽에 존재하는 내용 중 필요한 값들을 끌어올리는 것이다.

# use strict

# 함수 선언과 함수 표현식

# what is difference between `forEach` and `map`

https://www.frontendinterviewhandbook.com/kr/javascript-questions/#foreach-%EB%A3%A8%ED%94%84%EC%99%80-map-%EB%A3%A8%ED%94%84-%EC%82%AC%EC%9D%B4%EC%9D%98-%EC%A3%BC%EC%9A%94-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%84-%EC%84%A4%EB%AA%85%ED%95%A0-%EC%88%98-%EC%9E%88%EB%82%98%EC%9A%94-%EC%99%9C-%EB%91%98-%EC%A4%91-%ED%95%98%EB%82%98%EB%A5%BC-%EC%84%A0%ED%83%9D%ED%95%A0-%EA%B2%83%EC%9D%B8%EA%B0%80%EC%9A%94

# 참고

- [호이스팅](https://developer.mozilla.org/ko/docs/Glossary/Hoisting)
- [JavaScript Hoisting](https://www.w3schools.com/js/js_hoisting.asp)
- [자바스크립트의 스코프와 클로저](https://meetup.nhncloud.com/posts/86)
- [[JS] 자바스크립트 실행 컨텍스트(Execution Context)란?](https://heycoding.tistory.com/86)
- [실행 컨텍스트(Execution Context)](https://blog.burt.pe.kr/posts/skyfe79-blog.contents-1729427336-post-57/)
