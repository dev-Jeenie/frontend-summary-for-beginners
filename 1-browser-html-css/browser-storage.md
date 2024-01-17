# 브라우저 저장소

|               | cookie           | local storage         | session storage         |
| ------------- | ---------------- | --------------------- | ----------------------- |
| 생성자        | 클라이언트/서버  | 클라이언트            | 클라이언트              |
| 지속시간      | 설정 여부에 따름 | 명시적으로 지울때까지 | 탭 / 윈도우 닫을 때까지 |
| 용량          | 5KB              | 5MB / 10MB            | 5MB                     |
| 서버와의 통신 | O                | X                     | X                       |
| 취약점        | XSS / CSRF 공격  | XSS 공격              | XSS 공격                |

## Local Storage 로컬 스토리지

## Session Storage 세션 스토리지

## Cookie 쿠키

https://dev-jeenie.github.io/cs/browserStorage

https://dev-jeenie.github.io/life/sameSiteCookie

https://dev-jeenie.github.io/cs/CookieSession