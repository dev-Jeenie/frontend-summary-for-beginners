# call stack, event loop, and task queue

https://medium.com/jsninja/call-stack-event-loop-and-task-queue-d49eff2ec7bb

# event delegation(이벤트 위임)

https://ko.javascript.info/event-delegation

## event bubbling

## envent capturing

---

# 동기/비동기 함수 sync, async

블로킹 유무의 차이.

#### `블로킹`이란?

- 네트워크 요청, 이미지 프로세싱은 느리다.
  이러한 느린 동작이 스택에 남아있는 것을 보통 블로킹이라고 말한다.

동기 함수는 앞 명령문이 완료된 뒤에야 다음 명령문이 실행된다.
따라서 동기함수는 싱글 스레드인 JS의 콜스택을 `블로킹`하고, 이 함수 중 하나가 매우 오랜 시간이 걸리면 프로그램 실행이 일시중지된다. => 브라우저 UI 중지(이것저것 눌러도 변화를 보여줄 수 없음. 멈춰있어)

비동기 함수는 일반적으로 파라미터를 통해 콜백을 받고,
비동기 함수가 호출된 직후 바로 다음 명령문이 실행된다.
전달받은 콜백은 비동기 작업이 완료된 뒤 콜스택이 비어있을 때에만 호출된다.

![sync, asylc](/assets/javascript/sync-async-console.png)

#### 예제

동기적으로 AJAX 요청을 보내는 jQuery 함수 getSync가 있다고 가정하자.

![getsync](/assets/javascript/getsync.png)

동기적으로 동작하고 있기 때문에 네트워크 요청을 하고 마냥 끝날 때까지 기다린다.
그럼 위 코드를 하나 실행시키고 다 멈추고 기다리고, 완료되면 그제서야 Stack 에서 제거한 뒤 다음 코드를 실행한다.

### 이게 문제인 이유

이 코드를 브라우저가 동작시키기 때문에!

![getsync-in-browser](/assets/javascript/getsync-in-browser.png)

**`foo` 실행 -> 완료되면 `bar` 실행 -> 완료되면 `qux` 실행**

브라우저는 모든 리퀘스트를 완료할 때까지 멈춰있다.
멈춰있는 동안의 행동을 기억하고 있지만 렌더링할 수는 없다.

콜스택에 어떤 것들이 남아있으면,
이 동기적으로 실행되는 네트워크 요청들이 콜 스택을 블로킹해서 브라우저는 다른 일을 할 수 없다.
렌더링이나 다른 코드를 실행하지 못함!

**_=> 유려한 UI를 만들기 위해서는 콜스택이 멈추면 안된다._**

비동기 콜백으로 이 문제를 해결한다.

동기적으로는 네트워크

- [어쨌든 이벤트 루프는 무엇입니까? | Philip Roberts | JSConf EU](https://www.youtube.com/watch?v=8aGhZQkoFbQ)
- [이벤트 위임](https://ko.javascript.info/event-delegation)
