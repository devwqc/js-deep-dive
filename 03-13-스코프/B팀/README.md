# 13장 스코프

## 1. 스코프란?

스코프는 식별자(변수, 함수, 클래스 등)의 유효 범위를 의미한다. 자바스크립트 엔진은 스코프를 통해 어떤 변수를 참조해야 할지 결정하며, 스코프 내에서는 같은 이름의 변수를 사용할 수 없다. 그러나 다른 스코프에서는 같은 이름의 식별자를 사용할 수 있다.

<br />

## 2. 스코프의 종류

| 구분 | 설명                  | 스코프      | 변수      |
| ---- | --------------------- | ----------- | --------- |
| 전역 | 코드의 가장 바깥 영역 | 전역 스코프 | 전역 변수 |
| 지역 | 함수 몸체 내부        | 지역 스코프 | 지역 변수 |

- **전역 스코프**: 코드의 가장 바깥 영역. 여기서 선언된 변수는 어디서든 참조할 수 있다.
- **지역 스코프**: 함수 몸체 내부. 여기서 선언된 변수는 해당 함수 내부와 하위 함수에서만 참조할 수 있다.

<br />

## 3. 스코프 체인

스코프가 계층적으로 연결된 것

### 3-1. 스코프 체인에 의한 변수 검색

자바스크립트 엔진은 스코프 체인을 따라 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다. 이는 상위 스코프에서 유효한 변수를 하위 스코프에서 자유롭게 참조할 수 있지만, 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수 없음을 의미한다.

### 3-2. 스코프 체인에 의한 함수 검색

함수도 식별자에 해당되기 때문에 스코프를 갖는다. 함수는 식별자에 함수 객체가 할당된 것 외에는 일반 변수와 다를 바 없다.

<br />

## 4. 함수 레벨 스코프

- **블록 레벨 스코프**: 함수 몸체만이 아니라 모든 코드 블록이 지역 스코프를 만드는 특성.
- **함수 레벨 스코프**: `var` 키워드로 선언된 변수는 오로지 함수의 코드 블록(함수 몸체)만을 지역 스코프로 인정하는 특성.

<br />

## 5. 렉시컬 스코프

- **동적 스코프**: 함수가 호출되는 시점에 동적으로 상위 스코프를 결정하는 것.
- **정적 스코프(렉시컬 스코프)**: 함수 정의가 평가되는 시점에 상위 스코프가 정적으로 결정되는 것. 대부분의 프로그래밍 언어는 렉시컬 스코프를 따른다.

자바스크립트는 렉시컬 스코프를 따르므로 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다. 함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향을 주지 않는다. 즉, 함수의 상위 스코프는 언제나 자신이 정의된 스코프다.

함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의(함수 선언문 또는 함수 표현식)가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필요가 있기 때문이다.
