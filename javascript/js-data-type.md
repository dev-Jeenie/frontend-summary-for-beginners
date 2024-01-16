# primitve values 원시타입

Object를 제외한 모든 타입은 언어의 최하위 수준에서 직접 표현되는 Immutable(불변)의 값을 뜻한다.
원시값이 생성되면 변경할 수 없다.
`null`은 원시타입 데이터가 맞지만, `typeof`의 결과값으로는 object라고 나온다.
null을 제외한 모든 값은 typeof 연산자로 타입을 알아낼 수 있다.

| Type           | typeof return value | Object wrapper |
| -------------- | ------------------- | -------------- |
| Null 타입      | `object`            | N/A            |
| Undefined 타입 | `undefined`         | N/A            |
| Boolean 타입   | `boolean`           | Boolean        |
| Number 타입    | `number`            | Number         |
| BigInt 타입    | `bigint`            | BigInt         |
| String 타입    | `string`            | String         |
| Symbol 타입    | `symbol`            | Symbol         |

# object type 객체타입

객체타입 데이터가 생성되면 새 값을 다시 할당하지 않고도 속성과 요소를 변경할 수 있다.

# `null` vs `undefined` vs `undeclared`