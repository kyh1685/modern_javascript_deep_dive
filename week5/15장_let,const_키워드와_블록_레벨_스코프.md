## 15장 let, const 키워드와 블록 레벨 스코프

🔖 오늘 읽은 범위 : 242 ~ 252p

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

### 15.1 var 키워드로 선언한 변수의 문제점

- `var` 키워드로 선언한 변수는 중복 선언이 가능하다.
    
    ```jsx
    var x = 1;
    var y = 1;
    
    // 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
    var x = 100;
    // 초기화문이 없는 변수 선언문은 무시된다.
    var y;
    
    console.log(x); // 100
    console.log(y); // 1
    ```
    
- `var` 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다. 함수 외부에서 `var` 키워드로 선언한 변수는 모두 전역 변수가 된다.
- `var` 키워드로 변수를 선언하면 변수 호이스팅에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다. 즉, 변수 선언문 이전에 참조할 수 있다.

### 15.2 let 키워드(var 키워드와 차이점)

- `var` 키워드의 단점을 보완하기 위해 ES6에서는 `let`과 `const`를 도입했다.
- `let` 키워드로 선언한 변수는 이름이 같은 변수를 중복 선언하면 문법 에러가 발생한다.
- `let` 키워드로 선언한 변수는 모든 코드 블록을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
- `let` 키워드로 선언한 변수는 호이스팅이 발생하지 않는 것처럼 동작한다. 변수 선언문 이전에 참조하면 참조 에러가 발생한다.

<aside>
🔎 `var` 키워드로 선언한 변수는 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 **선언 단계와 초기화 단계가 한번에 진행**된다. 즉, 선언 단계에서 스코프(실행 컨텍스트의 렉시컬 환경)에 변수 식별자를 등록해 자바스크립트 엔진에 변수의 존재를 알린다. 그리고 즉시 초기화 단계에서 undefined로 변수를 초기화한다. 따라서 변수 선언문 이전에 변수에 접근해도 스코프에 변수가 존재하기 때문에 에러가 발생하지 않는다.

`let` 키워드로 선언한 변수는 **선언 단계와 초기화 단계가 분리되어 진행**된다. 즉, 런타임 이전에 자바스크립트 엔진에 의해 암묵적으로 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다. 만약 초기화 단계가 실행되기 이전에 변수에 접근하려고 하면 참조 에러가 발생한다. `let` 키워드로 선언한 변수는 스코프 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다. 이렇게 스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 일시적 사각지대(Temporal Dead Zone, TDZ)라고 부른다.

</aside>

- `let` 키워드로 선언한 변수는 `var` 키워드와 달리 전역 객체의 프로퍼티가 아니므로 window.foo와 같이 접근할 수 없다. `let` 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.

### 15.3 const 키워드(let 키워드와 차이점)

- `const` 키워드는 상수를 선언하기 위해 사용하지만 반드시 상수만을 위해 사용하지는 않는다.
- `const` 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다. 그렇지 않으면 문법 에러가 발생한다.
- `const` 키워드로 선언한 변수는 재할당이 금지된다.
- `const` 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다. 원시 값은  변경 불가능한 값이므로 재할당없이 값을 변경할 수 있는 방법이 없기 때문이다. 이러한 특징을 이용해 상수를 표현하는데 사용한다. 상수는 상태 유지와 가독성, 유지보수의 편의를 위해 적극 사용해야 한다.
- `const` 키워드로 선언한 변수에 객체를 할당할 경우 값을 변경할 수 있다. 변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문이다. 객체가 변경되더라도 변수에 할당된 참조 값은 변경되지 않는다.

### 15.4 var VS let VS const

- 변수 선언에는 기본적으로 `const`를 사용하고 `let`은 재할당이 필요한 경우에 한정해 사용하는 것이 좋다. `const` 키워드를 사용하면 의도치않은 재할당을 방지하기 때문에 좀 더 안전하다.

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

평소에 `let`을 많이 써서 어떨 때 `const`를 쓰고 `let`을 써야하는지 헷갈렸는데 앞으로는 `const` 키워드를 기본으로 써야겠다. 

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

x

### 📝 3줄 요약

- `var` 키워드로 선언한 변수는 `중복 선언 허용`, `함수 레벨 스코프`, `변수 호이스팅`의 문제점이 있다.
- `let`은 `var` 키워드로 선언한 변수와 달리 `중복 선언 불가능`, `블록 레벨 스코프`, `호이스팅이 발생하지 않는 것처럼 동작`한다는 차이점이 있다.
- `const`는 `let` 키워드로 선언한 변수와 달리 `선언과 동시에 초기화`, `재할당 금지`, `원시 값 할당 시 값 변경 불가능`하다는 차이점이 있다.
