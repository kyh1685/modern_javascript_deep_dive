## 31장 RegExp

🔖 오늘 읽은 범위 : 

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

## 31.1 정규 표현식이란?

- 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
- 정규 표현식은 자바스크립트의 고유 문법이 아니며, 대부분의 프로그래밍 언어와 코드 에디터에 내장되어 있다.
- 자바스크립트는 펄의 정규 표현식 문법을 ES3부터 도입했다.
- 정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다. 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.
- 정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다. 다만, 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

## 31.2 정규 표현식의 생성

- 정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.
- 정규 표현식 리터럴은 패턴과 플래그로 구성된다.
    
    ```jsx
    const target = 'Is this all there is?';
    
    // 패턴: is
    // 플래그: i => 대소문자를 구별하지 않고 검색한다.
    const regexp = /is/i;
    
    // test 메서드는 target 문자열에 대해 정규표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
    regexp.test(target); // -> true
    ```
    
- RegExp 생성자 함수를 사용하여 RegExp 객체를 생성할 수도 있다.
    
    ```jsx
    const target = 'Is this all there is?';
    
    const regexp = new RegExp(/is/i); // ES6
    // const regexp = new RegExp(/is/, 'i');
    // const regexp = new RegExp('is', 'i');
    
    regexp.test(target); // -> true
    ```
    
- RegExp 생성자 함수를 사용하면 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.
    
    ```jsx
    const count = (str, char) => (str.match(new RegExp(char, 'gi')) ?? []).length;
    
    count('Is this all there is?', 'is'); // -> 3
    count('Is this all there is?', 'xx'); // -> 0
    ```
    

## 31.3 RegExp 메서드

- RegExp.prototype.exec
    - exc 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
    - 매칭 결과가 없는 경우 null을 반환한다.
    - 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하므로 주의해야 한다.
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/;
    
    regExp.exec(target); // -> ["is", index: 5, input: "Is this all there is?", groups: undefined]
    ```
    
- RegExp.prototype.test
    
    test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
    
- String.prototype.match
    - String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.
    - exec 메서드는 g플래그를 지정해도 첫 번째 매칭 결과만 반환하지만 match는 g플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.

## 31.4 플래그

| 플래그 | 의미 | 설명 |
| --- | --- | --- |
| i | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다. |
| g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다. |
- 패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
- 플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.
- 어떠한 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색한다. 그리고 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.

## 31.5 패턴

- 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.
- 패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지 패턴에 포함되어 검색된다.
- 패턴은 특별한 의미를 가지는 메타 문자 또는 기호로 표현할 수도 있다.

- 임의의 문자열 검색
    - .은 임의의 문자 한 개를 의미한다.
    - 아래 코드는 .을 3개 연속해서 패턴을 생성했으므로 문자와 상관없이 3자리 문자열과 매치한다.
        
        ```jsx
        const target = 'Is this all there is?';
        
        // 임의의 3자리 문자열을 대소문자를 구별하여 전역 검색한다.
        const regExp = /.../g;
        
        target.match(regExp); // -> ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
        ```
        
- 반복 검색
    - {m, n}은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
    - 콤마 뒤에 공백이 있으면 정삭 동작하지 않으므로 주의해야한다.
        
        ```jsx
        const target = 'A AA B BB Aa Bb AAA';
        
        // 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색한다.
        const regExp = /A{1,2}/g;
        
        target.match(regExp); // -> ["A", "AA", "A", "AA", "A"]
        ```
        
    - {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다.
    - {n, }은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
    - +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다. 즉, {1,}과 같다.
        
        ```jsx
        const target = 'A AA B BB Aa Bb AAA';
        
        // 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색한다.
        const regExp = /A{2,}/g;
        
        target.match(regExp); // -> ["AA", "AAA"]
        ```
        
    - ?는 앞선 패턴이 최대 한번(0번 포함)이상 반복되는 문자열을 의미한다. 즉, {0, 1}과 같다.
        
        ```jsx
        const target = 'color colour';
        
        // 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는 문자열 'color', 'colour'를 전역 검색한다.
        const regExp = /colou?r/g;
        
        target.match(regExp); // -> ["color", "colour"]
        ```
        
    
- OR 검색
    - |은 or의 의미를 가진다.
        
        ```jsx
        const target = 'A AA B BB Aa Bb';
        
        // 'A' 또는 'B'를 전역 검색한다.
        const regExp = /A|B/g; 
        
        target.match(regExp); // -> ["A", "A", "A", "B", "B", "B", "A", "B"]
        ```
        
    - 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용한다. `/A+|B+/g`
    - `/A+|B+/g`를 더 간단히 표현하면 `/[AB]+/g`와 같다. []내의 문자는 or로 동작한다. 그 뒤에 +를 사용하면 앞선 패턴을 한번 이상 반복한다.
    - 범위를 지정하려면 [] 내에 -를 사용한다. `/[A-Z]+/g` 이 경우 대문자 알파벳을 검색한다. 대소문자를 구별하지 않고 검색하려면 `/[A-Za-z]+/g`를 사용한다.
    - 숫자를 검색하는 방법은 `/[0-9]+/g`이다. 더 간단히 표현하면 `/[\d]+/g`와 같다. \d는 [0-9]와 같다. \D는 \d와 반대로 동작한다. 즉, 문자를 의미한다.
    - \w는 알파벳, 숫자, 언더스코어를 의미한다. 즉 [A-Za-z0-9_]와 같다. \W는 반대로 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.
- NOT 검색
    
    []내의 ^은 not의 의미를 갖는다.
    
- 위치로 검색
    - 시작 위치로 검색 : []밖의 ^은 문자열의 시작을 의미한다.
        
        ```jsx
        const target = 'https://poiemaweb.com';
        
        // 'https'로 시작하는지 검사한다.
        const regExp = /^https/;
        
        regExp.test(target); // -> true
        ```
        
    - 마지막 위치로 검색 : $는 문자열의 마지막을 의미한다.
        
        ```jsx
        const target = 'https://poiemaweb.com';
        
        // 'com'으로 끝나는지 검사한다.
        const regExp = /com$/;
        
        regExp.test(target); // -> true
        ```
        

## 31.6 자주 사용하는 정규 표현식

- 특정 단어로 시작하는지 검사
    
    [] 바깥의 ^은 문자열의 시작을 의미하고, ?은 앞선 패턴이 최대 한 번이상 반복되는지 의미한다.
    
    ```jsx
    const url = 'https://example.com';
    
    // 'http://' 또는 'https://'로 시작하는지 검사한다.
    /^https?:\/\//.test(url); // -> true
    
    // 위 예제와 동일하게 동작
    /^(http|https):\/\//.test(url); // -> true
    ```
    
- 특정 단어로 끝나는지 검사
    
    ```jsx
    const fileName = 'index.html';
    
    // 'html'로 끝나는지 검사한다.
    /html$/.test(fileName); // -> true
    ```
    
- 숫자로만 이루어진 문자열인지 검사
    
    [] 바깥의 ^은 문자열의 시작을, $는 문자열의 마지막을 의미한다. \d는 숫자를 의미하고 +는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
    
    ```jsx
    const target = '12345';
    
    // 숫자로만 이루어진 문자열인지 검사한다.
    /^\d+$/.test(target); // -> true
    ```
    
- 하나 이상의 고백으로 시작하는지 검사
    
    \s는 여러 가지 공백 문자(스페이스, 탭 등)을 의미한다. 즉, [\t\r\n\v\f]와 같다.
    
    ```jsx
    const target = ' Hi!';
    
    // 하나 이상의 공백으로 시작하는지 검사한다.
    /^[\s]+/.test(target); // -> true
    ```
    
- 아이디로 사용 가능한지 검사
    
    ```jsx
    const id = 'abc123';
    
    // 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
    /^[A-Za-z0-9]{4,10}$/.test(id); // -> true
    ```
    
- 메일 주소 형식에 맞는지 검사
    
    ```jsx
    const email = 'ungmo2@gmail.com';
    
    /^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(email); // -> true
    ```
    
- 핸드폰 번호 형식에 맞는지 검사
    
    ```jsx
    const cellphone = '010-1234-5678';
    
    /^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // -> true
    ```
    
- 특수 문자 표함 여부 검사
    
    ```jsx
    const target = 'abc#123';
    
    // A-Za-z0-9 이외의 문자가 있는지 검사한다.
    (/[^A-Za-z0-9]/gi).test(target); // -> true
    
    // 위 방식과 동일하게 동작한다. 특수 문자를 선택적으로 검사할 수 있다는 장점이 있다.
    // (/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi).test(target); // -> true
    ```
    

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

### 📝 3줄 요약
