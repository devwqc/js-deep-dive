## 22.1 this 키워드

- 생성자 함수를 정의하는 시점은 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
- 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자 this가 필요하다.
- this: 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수
  - this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
  - this는 암묵적으로 생성되고, 코드 어디서든 참조할 수 있다.
  - this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.
- this는 상황에 따라 가리키는 대상이 다르다.
  - 전역, 일반 함수 내부에선 전역 객체 window를 가리킨다.
  - strict mode가 적용된 일반 함수 내부의 this엔 undefined가 바인딩된다. ⇒ 일반 함수 내부에서 this를 사용할 필요가 없기 때문이다.

## 22.2 함수 호출 방식과 this 바인딩

- 렉시컬 스코프는 함수 정의가 평가돼 함수 객체가 생성되는 시점에 상위 스코프를 결정한다. 하지만 this 바인딩은 함수 호출 시점에 결정된다.
- 일반 함수(중첩 함수, 콜백 함수 포함) 호출
  - this에 전역 객체가 바인딩된다.
  - 객체를 생성하지 않는 일반 함수에서 this는 의미가 없다. strict mode에서 undefined가 바인딩된다.
  - 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 this 바인딩을 변수에 할당해 콜백 함수 내부에서 해당 변수를 참조하는 것이다.
- 메서드 호출
  - 메서드 내부의 this는 메서드를 호출한 객체가 바인딩 된다.
  - 메서드를 소유한 객체가 아니라 메서드를 **호출**한 객체에 바인딩 된다.
- 생성자 함수 호출
  - 생성자 함수 내부의 this엔 미래에 생성할 인스턴스가 바인딩된다.
  - 만약 new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다. ⇒ 내부 this는 전역 객체를 가리킨다.
- Function.prototype.apply/call/bind 메서드에 의한 간접 호출
  - 이 메서드들은 함수를 호출할 때 사용하며, 모든 함수가 상속받아 사용할 수 있다.
  - this는 메서드에 첫 번째 인수로 전달한 객체에 바인딩 된다.
  - apply와 call 메서드는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용할 때 사용한다.
    - 예시: Array.prototype.slice.call(유사 배열 객체)
  - apply 메서드는 호출할 함수 인수를 배열로 묶고, call 메서드는 인수를 리스트 형식으로 전달한다.
    - getThisBinding.apply(thisArg, [1, 2, 3])
    - getThisBinding.call(thisArg, 1, 2, 3)
  - bind 메서드는 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 this 바인딩이 교체된 함수를 새롭게 생성해 반환한다.❓
  - bind 메서드로 callback 함수 내부의 this 바인딩을 전달해 콜백 함수 내부와 외부 함수 내부의 this를 일치시킬 수 있다.
