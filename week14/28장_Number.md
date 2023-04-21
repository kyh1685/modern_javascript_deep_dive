## 28장 Number

🔖 오늘 읽은 범위 : 

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

## 28.1 Number 생성자 함수

- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체이다.
- Number 생성자 함수에 인수를 전달하지 않고 `new` 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 0을 할당한 Number 래퍼 객체를 생성한다
- 아래 예제를 크롬 브라우저의 개발자 도구에서 실행해보면 `[[PrimitiveVaule]]`라는 접근할 수 없는 프로퍼티가 보인다. 이는 `[[NumberData]]` 내부 슬롯을 가리킨다(ES5에서 `[[NumberData]]` 를 `[[PrimitiveVaule]]`라 부름 )
    
    ```jsx
    const numObj = new Number();
    console.log(numObj); // Number {[[PrimitiveVaule]]: 0}
    ```
    
- Number 생성자 함수의 인수로 숫자를 전달하면서 `new` 연산자와 함께 호출하면 `[[NumberData]]` 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.
- Number 생성자 함수의 인수로 숫자가 아닌 값을 전달하면 인수를 숫자로 강제 변환한 후, `[[NumberData]]` 내부 슬롯에 변환된 숫자를 할당한 Number 래퍼 객체를 생성한다. 인수를 숫자로 변환할 수 없다면 NaN을 `[[NumberData]]` 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.
- `new` 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다. 이를 이용해 명시적으로 타입 변환을 하기도 한다.

## 28.2 Number 프로퍼티

- Number.EPSILON
    - ES6에서 도입됐으며 Number.EPSILON은 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다. Number.EPSILON은 약 2.22044604925031308084 * 10(-16승)과 같다.
    - 부동소수점 산술 연산은 정확한 결과를 기대하기 어렵다. 정수는 2진법으로 오차없이 저장 가능하지만 부동소수점을 표현하기 위해 널리 쓰이는 표준인 IEEE754는 2진법으로 변환했을 때 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계가 있다.
        
        ```jsx
        0.1 + 0.2; // 0.3000000
        0.1 + 0.2 === 0.3; // false
        ```
        
    - Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.
        
        ```jsx
        function isEqual(a, b){
        	// a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
        	return Math.abs(a - b) < Number.EPSILON;
        }
        
        isEqual(0.1 + 0.2, 0.3); // true
        ```
        
- Number.MAX_VALUE
    - Number.MAX_VALUE는 자바스크립트에서 표현할 수 있는 가장 큰 양수 값이다. Number.MAX_VALUE보다 큰 숫자는 Infinity다.
- Number.MIN_VALUE
    - Number.MIN_VALUE는 자바스크립트에서 표현할 수 있는 가장 작은 양수 값이다. Number.MIN_VALUE보다 작은 숫자는 0이다.
- Number.MAX_SAFE_INTEGER
    
    Number.MAX_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값이다.
    
- Number.MIN_SAFE_INTEGER
    
    Number.MIN_SAFE_INTEGER는 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값이다.
    
- Number.POSITIVE_INFINITY
    
    Number.POSITIVE_INFINITY는 양의 무한대를 나타내는 숫자값 Infinity와 같다.
    
- Number.NEGATIVE_INFINITY
    
    Number.NEGATIVE_INFINITY는 음의 무한대를 나타내는 -Infinity와 같다.
    
- Number.NaN
    
    Number.NaN은 숫자가 아님을 나타내는 숫자값이다. Number.NaN은 window.NaN과 같다.
    

## 28.3 Number 메서드

### 정적 메서드

- Number.isFinite
    - ES6에서 도입된 정적 메서드이며 인수로 전달된 숫자값이 정상적인 유한수, 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.
    - 빌트인 전역 함수 isFinite는 전달받은 인수를 숫자로 암묵적으로 타입 변환하여 검사를 수행하지만, Number.isFinite는 하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.
- Number.isInteger
    
    ES6에서 도입된 정적 메서드이며 인수로 전달된 숫자값이 정수인지 검사하여 결과를 불리언 값으로 반환한다. 검사하기 전 인수를 숫자로 암묵적 타입 변환하지 않는다.
    
- Number.isNaN
    - ES6에서 도입된 정적 메서드이며 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.
    - 빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하지만 Number.isNaN은 하지 않는다. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false다.
- Number.isSafeInteger
    - ES6에서 도입된 정적 메서드이며 인수로 전달된 숫자값이 안전한 정수인지 검사하여 결과를 불리언 값으로 반환한다.
    - 검사 전에 인수를 숫자로 암묵적 타입 변환하지 않는다.

### 메서드

- Number.prototype.toExponential
    - toExponential는 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.
    - 지수 표기법이란 매우 크거나 작은 숫자를 표기할 때 주로 사용하여 e 앞에 있는 숫자에 10의 n승을 곱하는 형식으로 수를 나타내는 방식이다. 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다. `(77.1234).toExponential(); // 7.71234e+1`
    - 숫자 리터럴과 함께 사용하면 에러가 발생한다. 자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석한다. 그러나 `77.toExponential()`에서 77은 Number 래퍼 객체다. 따라서 77 뒤의 .을 소수 구분 기호로 해석하면 뒤에 이어지는 .toExponential()를 프로퍼티로 해석할 수 없으므로 에러가 발생한다.
- Number.prototype.toFixed
    - toFixed는 숫자를 반올림하여 문자열로 반환한다.
    - 반올림하는 소수점 이하 자릿수를 나타내는 0 ~ 20사이의 정수값을 인수로 전달할 수 있다.
    - 인수를 생략하면 기본값 0이 지정된다.
- Number.prototype.toPrecision
    - toPrecision는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
    - 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.
    - 전체 자릿수를 나타내는 0 ~ 21 사이의 정수값을 인수로 전달할 수 있다.
    - 인수를 생략하면 기본값 0이 지정된다.
- Number.prototype.toString
    - toString은 숫자를 문자열로 변환하여 반환한다.
    - 진법을 나타내는 2 ~ 36 사이의 정수값을 인수로 전달할 수 있다.
    - 인수를 생략하면 기본값 10진법이 지정된다.

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

### 📝 3줄 요약
