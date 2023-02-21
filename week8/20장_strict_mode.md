## 20장 strict mode

🔖 오늘 읽은 범위 : 347 ~ 353p

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

## 20.1 strict mode란?

- 아래 코드에서 foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당했다. 이때 x 변수를 찾아야 값을 할당할 수 있기 때문에 자바스크립트 엔진은 x 변수가 어디서 선언되었는지 스코프 체인을 통해 검색한다. foo 함수의 스코프를 먼저 검색하고 그 다음 상위 스코프(아래 예제는 전역 스코프)에서 검색한다. 전역 스코프에도 x 변수의 선언이 존재하지 않기 때문에 오류를 발생시킬 것 같지만 **자바스크립트 엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다. 이때 전역 객체의 x 프로퍼티는 마치 전역 변수처럼 사용할 수 있다. 이러한 현상을 암묵적 전역(implicit global)이라 한다.**
    
    ```jsx
    function foo() {
      x = 10;
    }
    foo();
    
    console.log(x); // ?
    ```
    
- 이러한 오타나 문법 지식의 미비로 인한 실수를 방지하기 위해 `ES5`부터 strict mode(엄격 모드)각 추가되었다. **strict mode는 자바스크립트 언어의 문법을 좀 더 엄격하게 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.**
- ESLint 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있다. **린트 도구는 정적 분석 기능을 통해 소스코드를 실행하기 전에 소스코드를 스캔하여 문법적 오류뿐만 아니라 잠재적 오류까지 찾아내고 오류의 원인을 리포팅해주는 유용한 도구다.** 린트 도구는 strict mode가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있다.
- ES6에서 도입된 클래스와 모듈은 기본적으로 strict mode가 적용된다.

## 20.2 strict mode의 적용

- strict mode를 적용하려면 전역이나 함수 몸체의 선두에 `“use strict”;`를 추가한다. 전역의 선두에 추가하면 스크립트 전체에 적용되고 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 적용된다.

## 20.3 전역에 strict mode를 적용하는 것은 피하자

- 전역에 적용한 strict mode는 스크립트 단위로 적용된다. 즉, 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다. 하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하는 것은 바람직하지 않다. 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 사용한다.

```jsx
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

- 어떤 함수는 strict mode를 적용하고 어떤 함수는 적용하지 않는 것은 바람직하지 않으며 모든 함수에 일일이 적용하는 것은 번거로운 일이다. 그리고 strict mode가 적용된 함수가 참조할 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있다.
- 따라서 strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## 20.5 strict mode가 발생시키는 에러

다음은 strict mode를 적용했을 때 에러가 발생하는 대표적인 사례다.

- **암묵적 전역**
    
    선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.
    
    ```jsx
    (function () {
      'use strict';
    
      x = 1;
      console.log(x); // ReferenceError: x is not defined
    }());
    ```
    
- **변수, 함수, 매개변수의 삭제**
    
    delete 연산자로 변수, 함수, 매개변수를 삭제하려면 SyntaxError가 발생한다.
    
    ```jsx
    (function () {
      'use strict';
    
      var x = 1;
      delete x;
      // SyntaxError: Delete of an unqualified identifier in strict mode.
    
      function foo(a) {
        delete a;
        // SyntaxError: Delete of an unqualified identifier in strict mode.
      }
      delete foo;
      // SyntaxError: Delete of an unqualified identifier in strict mode.
    }());
    ```
    
- **매개변수 이름의 중복**
    
    중복된 매개변수 이름을 사용하면 SystaxError가 발생한다.
    
    ```jsx
    (function () {
      'use strict';
    
      //SyntaxError: Duplicate parameter name not allowed in this context
      function foo(x, x) {
        return x + x;
      }
      console.log(foo(1, 2));
    }());
    ```
    
- **with 문의 사용**
    
    with문을 사용하면 SyntaxError가 발생한다. **with문은 전달된 객체를 스코프 체인에 추가한다.** with문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체의 이름을 생략할 수 있어서 코드가 간단해지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다. 따라서 **with문은 사용하지 않는 것이 좋다.**
    
    ```jsx
    (function () {
      'use strict';
    
      // SyntaxError: Strict mode code may not include a with statement
      with({ x: 1 }) {
        console.log(x);
      }
    }());
    ```
    

## 20.6 strict mode 적용에 의한 변화

### 20.6.1 일반 함수의 this

- strict mode에서 함수를 일반 함수로서 호출하면 **this에 undefined가 바인딩**된다. 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.
- strict mode 모드를 적용하지 않은 경우에는 전역 객체가 바인딩된다.

### 20.6.2 arguments 객체

- strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해 arguments 객체에 반영되지 않는다.

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

strict mode가 발생시키는 에러 중에서 매개변수 이름의 중복이 strict mode가 적용되지 않으면 허용된다는 게 놀라웠다😧 또 with문이라는 걸 처음 알아서 신기했다. 권장하지 않는다니 *with문이라는게 있다~* 정도만 알아둬야겠다!

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

```jsx
function x() {
  y = 1;   // strict 모드에서는 ReferenceError를 출력합니다.
  var z = 2;
}

x();

console.log(y); // 로그에 "1" 출력합니다.
console.log(z); // ReferenceError: z is not defined outside x를 출력합니다.
```

🤔 키워드 없는 변수는 암묵적으로 `var 키워드`로 선언된 걸로 간주되지 않나? 그럼 함수 레벨 스코프를 가져야 하는데 **예제 20-01**에서 왜 함수 밖에서도 출력이 되는 걸까? 

👉 [**MDN**](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var)을 보면 *“선언된 변수들은 변수가 선언된 실행 콘텍스트 안에서 만들어집니다. 선언되지 않은 변수들은 항상 전역변수 입니다.”* 라고 나와있다. 즉, `var` 키워드로 전역에 선언하거나 선언되지 않은 변수들은 항상 전역 변수이다. 그리고 전역 객체의 프로퍼티로 할당된다. 추가로 선언되지 않은 변수들은 변수들을 할당하는 코드가 실행되기 전까지는 존재하지 않는다!

### 📝 3줄 요약

- strict mode는 자바스크립트 언어의 문법을 좀 더 엄격하게 적용하여 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.
- 전역이나 함수 몸체 선두에 strict mode를 적용시키는 것을 피하고  즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.
