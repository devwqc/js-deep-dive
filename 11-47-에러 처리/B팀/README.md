# 47장 에러 처리

## 1. 에러 처리의 필요성

에러나 예외적인 상황은 언제나 발생할 수 있다.

발생한 에러에 대해 대처하지 않고 방치하면 프로그램은 강제 종료된다.

`try … catch` 문을 사용해 발생한 에러에 적절하게 대응하면 프로그램이 강제 종료되지 않고 계속해서 코드를 실행시킬 수 있다.

<br />

## 2. `try … catch … finally` 문

### 기본적으로 에러 처리를 구현하는 방법

1️⃣ 예외적인 상황이 발생하면 반환하는 값을 `if` 문이나 단축 평가 또는 옵셔널 체이닝 연산자를 통해 확인해서 처리하는 방법 (`querySelector`, `Array#find` 메서드 등)

2️⃣ 에러 처리 코드를 미리 등록해 두고 에러가 발생하면 에러 처리 코드로 점프하도록 하는 방법 (`try … catch … finally` 문 등)

---

```jsx
try {
  // 실행할 코드(에러가 발생할 가능성이 있는 코드)
} catch (err) {
  // try 코드 블록에서 에러가 발생하면 이 코드 블록의 코드가 실행
  // err에는 try 코드 블록에서 발생한 Error 객체가 전달
} finally {
  // 에러 발생과 상관없이 반드시 한 번 실행
}
```

`try … catch … finally` 문은 3개의 코드 블록으로 구성된다.

`finally` 문은 불필요하다면 생략 가능하다. `catch` 문도 생략 가능하지만 `catch` 문이 없는 `try` 문은 의미가 없으므로 생략하지 않는다.

### 동작 원리

`try … catch … finally` 문을 실행하면 먼저 `try` 코드 블록이 실행된다. 이때 `try` 코드 블록에 포함된 문 중에서 에러가 발생하면 발생한 에러는 `catch` 문의 `err` 변수에 전달되고 `catch` 코드 블록이 실행된다. `catch` 문의 `err` 변수는 `try` 코드 블록에 포함된 문 중에서 에러가 발생하면 생성되고 `catch` 코드 블록에서만 유효하다. `finally` 코드 블록은 에러 발생과 상관없이 반드시 한 번 실행된다.

<br />

## 3. `Error`객체

`Error` 생성자 함수는 에러 객체를 생성한다. `Error` 생성자 함수에는 에러를 상세히 설명하는 에러 메세지를 인수로 전달할 수 있다.

```jsx
const error = new Error("invalid");
```

`Error` 생성자 함수가 생성한 에러 객체는 `message` 프로퍼티와 `stack` 프로퍼티를 갖는다.

- `message` 프로퍼티의 값: `Error` 생성자 함수에 인수로 전달한 에러 메세지
- `stack` 프로퍼티의 값: 에러를 발생시킨 콜 스택의 호출 정보를 나타내는 문자열이며 디버깅 목적으로 사용한다.

---

자바스크립트는 `Error` 생성자 함수를 포함해 7가지의 에러 객체를 생성할 수 있는 `Error` 생성자 함수를 제공한다. `SyntaxError`, `ReferenceError`, `TypeError`, `RangeError`, `URIError`, `EvalError` 생성자 함수가 생성한 에러 객체의 프로토타입은 모두 `Error.prototype`을 상속받는다.

| 생성자 함수      | 인스턴스                                                                     |
| ---------------- | ---------------------------------------------------------------------------- |
| `Error`          | 일반적 에러 객체                                                             |
| `SyntaxError`    | 자바스크립트 문법에 맞지 않는 문을 해석할 때 발생하는 에러 객체              |
| `ReferenceError` | 참조할 수 없는 식별자를 참조했을 때 발생하는 에러 객체                       |
| `TypeError`      | 피연산자 또는 인수의 데이터 타입이 유효하지 않을 때 발생하는 에러 객체       |
| `RangeError`     | 숫자값의 허용 범위를 벗어났을 때 발생하는 에러 객체                          |
| `URIError`       | encodeURI또는 decodeURI함수에 부적절한 인수를 전달했을 때 발생하는 에러 객체 |
| `EvalError`      | eval함수에서 발생하는 에러 객체                                              |

<br />

## 4. `throw` 문

에러를 발생시키려면 `try` 코드 블록에서 `throw` 문으로 에러 객체를 던져야 한다.

```jsx
throw 표현식;
```

`throw` 문의 표현식은 어떤 값이라도 상관없지만 일반적으로 에러 객체를 지정한다. 에러를 던지면 `catch` 문의 에러 변수가 생성되고 던져진 에러 객체가 할당된다. 그리고 `catch` 코드 블록이 실행되기 시작한다.

<br />

## 5. 에러의 전파

`throw`된 에러를 캐치하지 않으면 호출자 방향으로 전파된다. 이때 `throw`된 에러를 캐치하여 적절히 대응하면 프로그램을 강제 종료시키지 않고 코드의 실행 흐름을 복구할 수 있지만 어디에서도 캐치하지 않으면 프로그램은 강제 종료된다.

### 주의

> 비동기 함수인 `setTimeout`이나 프로미스 후속 처리 메서드의 콜백 함수는 호출자가 없다는 것

`setTimeout`이나 프로미스 후속 처리 메서드의 콜백 함수는 태스크 큐나 마이크로태스크 큐에 일시 저장되었다가 콜 스택이 비면 이벤트 루프에 의해 콜 스택으로 푸시되어 실행된다. 이때 콜 스택에 푸시된 콜백 함수의 실행 컨텍스트는 콜 스택의 가장 하부에 존재하기 때문에 에러를 전파할 호출자가 존재하지 않는다.
