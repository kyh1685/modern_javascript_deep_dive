## 30장 Date

🔖 오늘 읽은 범위 : 

---

### 😃 책에서 정리하고 싶은 내용을 써보세요

- 표준 빌트인 객체인 Date는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.
- UTC(or GMT)는 국제 표준시를 말한다. UTC와 GMT는 초의 소수점 단위에서만 차이가 나기 때문에 혼용되어 사용된다. 기술적인 표기에는 UTC가 사용된다.
- KST는 한국 표준시로 UTC에 9시간을 더한 시간이다.
- 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

## 30.1 Date 생성자 함수

- Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
- 이 값은 1970년 1월1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- Date 생성자 함수로 생성한 Date 객체는 기본적으로 현재 날짜와 시간을 나타내는 정수값을 가진다. 다른 날짜와 시간을 다루고 싶은 경우 생성자 함수에 해당 날짜와 시간 정보를 인수로 전달한다.

### Date 생성자 함수로 객체를 생성하는 방법

- new Date()
    - Date 생성자 함수를 인수없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
    - Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖지만 Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
    - Date 생성자 함수를 new 연산자 없이 호출하면 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.
- new Date(milliseconds)
    
    Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월1일 00:00:00(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다. 
    
- new Date(dateString)
    
    Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다. 이때 인수는 Date.parse에 의해 해석 가능한 형식이어야 한다.
    
- new Date(year, month[,day, hour, minute, second, millisecond])
    - Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
    - 이때 연, 월은 반드시 지정해야한다. 연, 월을 지정하지 않은 경우  1970년 1월1일 00:00:00(UTC)을 나타내는 Date 객체를 반환한다.
    - 지정하지 않은 옵션은 0 또는 1로 초기화된다.

## 30.2 Date 메서드

### 정적 메서드

- [Date.now](http://Date.now) :  1970년 1월1일 00:00:00(UTC)을 기점으로 현재까지 경과한 밀리초를 숫자로 반환한다.
- Date.parse :  1970년 1월1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- Date.UTC :  1970년 1월1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
    - Date.UTC는 new Date(year, month[,day, hour, minute, second, millisecond])와 같은 형식의 인수를 사용해야 한다.
    - Date.UTC의 인수는 로컬 타임이 아닌 UTC로 인식된다.
    - month는 월을 의미하는 0 ~ 11까지의 정수다. 0부터 시작하므로 주의해야 한다.

### 메서드

- Date.prototype.getFullYear : Date 객체의 연도를 나타내는 정수를 반환한다.
- Date.prototype.setFullYear : Date 객체에 연도를 나타내는 정수를 설정한다. 월, 일도 설정할 수 있다.
- Date.prototype.getMonth : Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다.
- Date.prototype.setMonth : Date 객체의 월을 나타내는 0 ~ 11의 정수를 설정한다. 일도 설정할 수 있다.
- Date.prototype.getDate : Date 객체의 날짜를 나타내는 정수를 반환한다.
- Date.prototype.setDate : Date 객체의 날짜를 나타내는 정수를 설정한다.
- Date.prototype.getDay : Date 객체의 요일을 나타내는 정수를 반환한다.
- Date.prototype.getHours : Date 객체의 시간을 나타내는 정수를 반환한다.
- Date.prototype.setHours : Date 객체의 시간을 나타내는 정수를 설정한다. 분, 초, 밀리초도 설정할 수 있다.
- Date.prototype.getMinutes : Date 객체의 분을 나타내는 정수를 반환한다.
- Date.prototype.setMinutes : Date 객체의 분을 나타내는 정수를 설정한다. 초, 밀리초도 설정할 수 있다.
- Date.prototype.getSecondes: Date 객체의 초를 나타내는 정수를 반환한다.
- Date.prototype.setSecondes: Date 객체의 초를 나타내는 정수를 설정한다. 밀리초도 설정할 수 있다.
- Date.prototype.getMillisecondes: Date 객체의 밀리초초를 나타내는 정수를 반환한다.
- Date.prototype.setMillisecondes: Date 객체의 밀리초를 나타내는 정수를 설정한다.
- Date.prototype.getTime : 1970년 1월1일 00:00:00(UTC)을 기점으로 Date 객체의 시간까지 경과된 밀리초를 반환한다.
- Date.prototype.setTime : 1970년 1월1일 00:00:00(UTC)을 기점으로 경과된 밀리초를 설정한다.
- Date.prototype.getTimezoneOffset : UTC와 Date 객체에 지정된 로캘 시간과의 차이를 분단위로 반환한다.
- Date.prototype.toDateString : 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.
- Date.prototype.toTimeString : 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 시간을 표현한 문자열을 반환한다.
- Date.prototype.toISOString : ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.
- Date.prototype.toLocaleString: 인수로 전달한 로캘을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.
- Date.prototype.toLocaleTimeString: 인수로 전달한 로캘을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

### 🤔 오늘 읽은 소감은? 떠오르는 생각을 가볍게 적어보세요

### 🔎 궁금한 내용이 있거나, 잘 이해되지 않는 내용이 있다면 적어보세요

### 📝 3줄 요약
