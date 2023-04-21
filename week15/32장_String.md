## 32장 String

🔖 오늘 읽은 범위 : 

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

## 32.1 String 생성자 함수

- 표준 빌트인 객체인 String 객체는 생성자 함수 객체다. 따라서 new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.
- 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 `[[StringData]]` 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.
- String 래퍼 객체는 배열과 마찬가지로 **length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블**이다.
- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 문자열로 강제 변환한 후, `[[StringData]]` 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.
- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다. 이를 이용해 명시적으로 타입을 변환하기도 한다.

## 32.3 String 메서드

- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드가 존재하지 않는다. 즉, String 객체의 메서드는 **언제나 새로운 문자열을 반환**한다.
- 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.
    
    ```jsx
    const strObj = new String('Lee');
    
    console.log(Object.getOwnPropertyDescriptors(strObj));
    /* String 래퍼 객체는 읽기 전용 객체다. 즉, writable 프로퍼티 어트리뷰트 값이 false다.
    {
      '0': { value: 'L', writable: false, enumerable: true, configurable: false },
      '1': { value: 'e', writable: false, enumerable: true, configurable: false },
      '2': { value: 'e', writable: false, enumerable: true, configurable: false },
      length: { value: 3, writable: false, enumerable: false, configurable: false }
    }
    */
    ```
    
- 만약 String 래퍼 객체가 읽기 전용 객체가 아니라면, 변경된 String 래퍼 객체를 문자열로 되돌릴 때 문자열이 변경된다. 따라서 String 객체의 모든 메서드는 String 래퍼 객체를 직접 변경할 수 없고 새로운 문자열을 생성하여 반환한다.
- String.prototype.indexOf
    - indexOf는 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다. 검색에 실패하면 -1을 반환한다.
    - 두번째 인수로 검색을 시작할 인덱스를 전달할 수도 있다.
- String.prototype.search
    - 대상 문자열에서 인수로 전달받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다.
    - 검색에 실패하면 -1을 반환한다.
- String.prototype.includes
    - ES6에서 도입됐으며 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 결과를 불리언 값으로 반환한다.
    - 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- String.prototype.startWith
    
    ```jsx
    const str = 'Hello world';
    
    // 문자열 str이 'He'로 시작하는지 확인
    str.startsWith('He'); // -> true
    // 문자열 str이 'x'로 시작하는지 확인
    str.startsWith('x'); // -> false
    ```
    
    - ES6에서 도입됐으며 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 그 결과를 불리언 값으로 반환한다.
    - 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- String.prototype.endsWith
    - ES6에서 도입됐으며 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 결과를 불리언 값으로 반환한다.
    - 두번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
- String.prototype.charAt
    - 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.
    - 인덱스가 문자열의 범위를 벗어난 경우 빈 문자열을 반환한다.
- String.prototype.substring
    - 대상 문자열에서 첫번째 인수로 전달받은 인덱스에 위치하는 문자부터 두번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 문자열을 반환한다.
    - 두번째 인수를 생략하면 첫번째 인덱스부터 마지막 문자까지의 부분 문자열을 반환한다.
    - 첫번째 인수가 두번째 인수보다 큰 경우 두 인수는 교환된다.
    - 인수가 0보다 작거나 NaN인 경우 0으로 취급된다.
    - 인수가 문자열의 길이보다 클 경우 문자열의 길이로 취급된다.
- String.prototype.slice
    - substring 메서드와 동일하게 동작한다. 단, slice는 음수인 인수를 전달할 수 있다.
    - 음수인 인수를 전달하면 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.
- String.prototype.toUpperCase
    
    대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.
    
- String.prototype.toLowerCase
    
    대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.
    
- String.prototype.trim
    
    대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.
    
- String.prototype.repeat
    - ES6에서 도입됐으며 대상 문자열을 인수로 전달받은 정수만큼 반복해 연결한 새로운 문자열을 반환한다.
    - 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수면 RangError를 발생시킨다.
    - 인수를 생략하면 기본값 0이 설정된다.
- String.prototype.replace
    - 대상 문자열에서 첫번째 인수로 전달받은 문자열 또는 정규 표현식을 검색하여 두번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.
    - 검색된 문자열이 여러개일 경우 첫 번째로 검색된 문자열만 치환한다.
    - 특수한 교체 패턴을 사용할 수 있다. 예를 들어 $&는 검색된 문자열을 의미한다.
        
        ```jsx
        const str = 'Hello world';
        
        // 특수한 교체 패턴을 사용할 수 있다. ($& => 검색된 문자열)
        str.replace('world', '<strong>$&</strong>');
        ```
        
    - 두번째 인수로 치환 함수를 전달할 수 있다. 첫번째 인수로 전달한 문자열 또는 정규 표현식에 매치한 결과를 두번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 치환한다.
    - 예를 들어, 카멜 케이스를 스네이크 케이스로, 스네이크 케이스를 카멜 케이스로 변경하는 함수는 아래와 같다.
        
        ```jsx
        // 카멜 케이스를 스네이크 케이스로 변환하는 함수
        function camelToSnake(camelCase) {
          // /.[A-Z]/g는 임의의 한 문자와 대문자로 이루어진 문자열에 매치한다.
          // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
          return camelCase.replace(/.[A-Z]/g, match => {
            console.log(match); // 'oW'
            return match[0] + '_' + match[1].toLowerCase();
          });
        }
        
        const camelCase = 'helloWorld';
        camelToSnake(camelCase); // -> 'hello_world'
        
        // 스네이크 케이스를 카멜 케이스로 변환하는 함수
        function snakeToCamel(snakeCase) {
          // /_[a-z]/g는 _와 소문자로 이루어진 문자열에 매치한다.
          // 치환 함수의 인수로 매치 결과가 전달되고, 치환 함수가 반환한 결과와 매치 결과를 치환한다.
          return snakeCase.replace(/_[a-z]]/g, match => {
            console.log(match); // '_w'
            return match[1].toUpperCase();
          });
        }
        
        const snakeCase = 'hello_world';
        snakeToCamel(snakeCase); // -> 'helloWorld'
        ```
        
- String.prototype.split
    - 대상 문자열에서 첫번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여 문자열을 구분한 후 분리된 문자열로 이루어진 배열을 반환한다.
    - 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
    - 두번째 인수로 배열의 길이를 지정할 수 있다.

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

### 📝 3줄 요약
