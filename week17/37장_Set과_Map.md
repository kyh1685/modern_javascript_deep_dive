# 37장 Set과 Map

## 37.1 Set
- **Set 객체는 중복되지 않는 유일한 값들의 집합이다.** 
- Set 객체는 배열과 유사하지만 아래와 같은 차이가 있다.

| 구분 | 배열 | Set 객체 |
| --- | --- | --- |
| 동일한 값을 중복하여 포함할 수 있다. | O | X |
| 요소 순서에 의미가 있다. | O | X |
| 인덱스로 요소에 접근할 수 있다. | O | X |

- Set은 수학적 집합을 구현하기 위한 자료구조다. Set을 통해 교집합, 합집합, 여집합 등을 구현할 수 있다.

### 37.1.1 Set 객체의 생성
- Set 객체는 Set 생성자 함수로 생성한다. 인수를 전달하지 않으면 빈 Set 객체가 생성된다. `const set = new Set();`
- **Set 생성자 함수는 이터러블을 인수로 받아 Set 객체를 생성하며, 이터러블의 중복된 값은 저장되지 않는다.**
```js
const set1 = new Set([1, 2, 3, 3]);
console.log(set1); // Set(3) {1, 2, 3}

const set2 = new Set('hello');
console.log(set2); // Set(4) {"h", "e", "l", "o"}
```

### 37.1.2 요소 개수 확인
- Set 객체의 요소 개수를 확인할 때는 `Set.prototype.size` 프로퍼티를 사용한다.
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티다. 따라서 숫자를 할당하여 요소 개수를 변경할 수 없다.

### 37.1.3 요소 추가
- Set 객체에 요소를 추가할 때는 `Set.prototype.add` 메서드를 사용한다.
- add 메서드는 새로운 요소가 추가된 Set 객체를 반환한다. 따라서 add 메서드를 연속으로 호출할 수 있다. `set.add(1).add(2);`
- 중복된 요소의 추가는 허용되지 않으므로 에러가 발생하지 않고 무시된다. `set.add(1).add(2).add(2);`
- 일치 비교 연산자(===)를 사용하면 NaN과 NaN을 다르다고 평가하지만, Set 객체는 같다고 평가하여 중복 추가를 허용하지 않는다.
- Set 객체는 객체나 배열과 같이 자바스크립트의 모든 값을 요소로 저장할 수 있다.

### 37.1.4 요소 존재 여부 확인
- Set 객체에 특정 요소가 존재하는지 확인하려면 `Set.prototype.has` 메서드를 사용한다. has 메서드는 존재 여부를 불리언 값으로 반환한다.

### 37.1.5 요소 삭제
- Set 객체의 특정 요소를 삭제하려면 `Set.prototype.delete` 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 불리언 값으로 반환한다.
- delete 메서드는 인덱스가 아니라 삭제하려는 요소값을 인수로 전달해야 한다.
- 만약 존재하지 않는 요소를 삭제하려 하면 에러없이 무시된다.

### 37.1.6 요소 일괄 삭제
- Set 객체의 모든 요소를 일괄 삭제하려면 `Set.prototype.clear` 메서드를 사용한다. clear 메서드는 undefined를 반환한다.

### 37.1.7 요소 순회
- Set 객체의 요소를 순회하려면 `Set.prototype.forEach` 메서드를 사용한다. 
- `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용된 객체를 인수로 전달한다.
- 콜백 함수는 `(현재 순회 중인 요소값, 현재 순회 중인 요소값, 현재 순회 중인 Set 객체 자체)`를 인수로 받는다.
- 첫 번째 인수와 두 번째 인수는 같은 값이지만 이렇게 동작하는 이유는 `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.
- `Array.prototype.forEach`는 두번째 인수로 현재 순회 중인 요소의 인덱스를 전달받지만 Set 객체는 순서에 의미가 없어 배열과 같이 인덱스를 갖지 않는다. 
- **Set 객체는 이터러블이다.** 따라서 for of문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다. 
- Set 객체는 요소의 순서에 의미를 갖지 않지만, Set 객체를 순회하는 순서는 요소가 추가된 순서를 따른다. 이는 다른 이터러블의 순회화 호환성을 유지하기 위함이다.

### 37.1.8 집합 연산

#### 교집합
교집합은 집합 A와 집합 B의 공통 요소로 구성된다.
```js
Set.prototype.intersection = function (set) {
  const result = new Set();

  for (const value of set) {
    // 2개의 set의 요소가 공통되는 요소이면 교집합의 대상이다.
    if (this.has(value)) result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter(v => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

#### 합집합
합집합은 집합 A와 집합B의 중복 없는 모든 요소로 구성된다.
```js
Set.prototype.union = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 합집합은 2개의 Set 객체의 모든 요소로 구성된 집합이다. 중복된 요소는 포함되지 않는다.
    result.add(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

#### 차집합
차집합은 집합 A에는 존재하지만 집합 B에는 존재하지 않는 요소로 구성된다.
```js
Set.prototype.difference = function (set) {
  // this(Set 객체)를 복사
  const result = new Set(this);

  for (const value of set) {
    // 차집합은 어느 한쪽 집합에는 존재하지만 다른 한쪽 집합에는 존재하지 않는 요소로 구성된 집합이다.
    result.delete(value);
  }

  return result;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter(v => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

#### 부분 집합과 상위 집합
집합 A가 집합 B에 포함되는 경우 집합 A는 집합 B의 부분 집합이며, 집합B는 집합 A의 상위 집합이다.
```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  for (const value of subset) {
    // superset의 모든 요소가 subset의 모든 요소를 포함하는지 확인
    if (!this.has(value)) return false;
  }

  return true;
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every(v => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## 37.2 Map
- **Map 객체는 키와 값의 쌍으로 이루어진 컬렉션이다.** Map 객체는 객체와 유사하지만 아래와 같은 차이가 있다.
  
| 구분 | 객체 | Map 객체 |
| --- | --- | --- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값 | 객체를 포함한 모든 값 |
| 이터러블 | X | O |
| 요소 개수 확인 | Object.keys(obj).length | map.size |

### 37.2.1 Map 객체의 생성
- Map 객체는 Map 생성자 함수로 생성한다. 인수를 전달하지 않으면 빈 Map 객체가 생성된다. `const map = new Map();`
- **Map 생성자 함수는 이터러블을 인수로 전달받아 Map 객체를 생성한다. 인수로 전달되는 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.**
```jsx
const map1 = new Map([['key1', 'value1'], ['key2', 'value2']]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

- Map 객체에는 중복된 키를 갖는 요소가 존재할 수 없다. Map 생성자 함수의 인수로 전달한 이터러블에 중복된 키를 갖는 요소가 존재하면 값이 덮어써진다.

### 37.2.2 요소 개수 확인

- Map 객체의 요소 개수를 확인할 때는 `Map.prototype.size` 프로퍼티를 사용한다.
- size 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티다. 따라서 숫자를 할당하여 요소 개수를 변경할 수 없다.

### 37.2.3 요소 추가

- Map객체에 요소를 추가할 때는 `Map.prototype.set` 메서드를 사용한다.
- set 메서드는 새로운 요소가 추가된 Map 객체를 반환한다. 따라서 set 메서드를 연속으로 호출할 수 있다. `map.set("key1", "value1").set("key2", "value2");`
- 중복된 키를 갖는 요소는 존재할 수 없기 때문에 중복된 키를 갖는 요소를 추가하면 값이 덮어써진다. 이때 에러가 발생하지 않는다. `map.set("key1", "value1").set("key1", "value2");`
- 일치 비교 연산자(===)를 사용하면 NaN과 NaN을 다르다고 평가하지만, Map 객체는 같다고 평가하여 중복 추가를 허용하지 않는다.
- 객체는 문자열 또는 심벌 값만 키로 사용할 수 있지만, Map 객체는 키 타입에 제한이 없다. 따라서 객체를 포함한 모든 값을 키로 사용할 수 있다.

### 37.2.4 요소 취득

- Map 객체에서 특정 요소를 취득하려면 `Map.prototype.get` 메서드를 사용한다.
- get 메서드의 인수로 키를 전달하면 키를 갖는 값을 반환한다. 만약 존재하지 않으면 undefined를 반환한다.

### 37.2.5 요소 존재 여부 확인

- Map객체에 특정 요소가 존재하는지 확인하려면 `Map.prototype.has` 메서드를 사용한다. has 메서드는 존재 여부를 불리언 값으로 반환한다.

### 37.2.6 요소 삭제

- Map객체의 특정 요소를 삭제하려면 `Map.prototype.delete` 메서드를 사용한다. delete 메서드는 삭제 성공 여부를 불리언 값으로 반환한다.
- delete 메서드는 삭제하려는 요소의 키를 인수로 전달해야 한다.
- 만약 존재하지 않는 키로 Map 객체의 요소를 삭제하려 하면 에러없이 무시된다.

### 37.2.7 요소 일괄 삭제

- Map객체의 모든 요소를 일괄 삭제하려면 `Map.prototype.clear` 메서드를 사용한다. clear 메서드는 undefined를 반환한다.

### 37.2.8 요소 순회

- Map객체의 요소를 순회하려면 `Map.prototype.forEach` 메서드를 사용한다.
- `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 forEach 메서드의 콜백 함수 내부에서 this로 사용될 객체를 인수로 전달한다.
- 콜백 함수는 `(현재 순회 중인 요소값, 현재 순회 중인 요소키, 현재 순회 중인 Map 객체 자체)`를 인수로 받는다.
- **Map 객체는 이터러블이다.** 따라서 for of문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링의 대상이 될 수도 있다.
- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| Map 메서드 | 설명 |
| --- | --- |
| Map.prototype.keys | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |
| Map.prototype.values | Map 객체에서 요소값를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |
| Map.prototype.entries | Map 객체에서 요소키와 요소값를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |
- Map 객체는 요소의 순서에 의미를 갖지 않지만, Map  객체를 순회하는 순서는 요소가 추가된 순서를 따른다. 이는 다른 이터러블의 순회화 호환성을 유지하기 위함이다.

## 느낀점
- 중복 값 제거할 때만 써서 잘 몰랐는데 Set 객체는 인덱스로 접근할 수 없다는 사실을 새삼 깨닫게 됐다.
- 배열과 달리 문자열을 생성자 함수에 인수로 넘겨주면 각 문자를 쪼개서 Set 객체를 만들어준다는 점이 신기했다.
- 일치 연산자는 NaN 끼리 비교하면 다르다고 판단하는데 Set은 같다고 판단하는 걸 보니 내부적으로 얕은 비교나 깊은 비교를 하는 건지 궁금해졌다.
- every라는 함수를 처음 봐서 신기했다. 주어진 함수를 통과하는지 테스트하는 함수라니 유용해보이니 기억해둬야겠다.
- 객체만 써왔는데 객체를 map과 비교해보니 map을 사용하는게 더 나을 것 같다. 특히 요소 개수를 확인할 때 keys, values를 써서 배열로 바꿔서 얻어야 하는 점이 번거로웠는데 좋은 것 같다.
