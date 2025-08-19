---
title: '2장: 값, 타입, 연산자'
slug: javascript-bible-values-types-operators
description: '자바스크립트의 기본 데이터 타입과 연산자를 배우고, ES2020+의 새로운 기능들을 실습을 통해 익혀보세요.'
keywords:
  [
    '자바스크립트',
    '데이터 타입',
    '원시 타입',
    'BigInt',
    'Symbol',
    '연산자',
    'Nullish coalescing',
    '논리 할당 연산자',
    '타입 변환',
    'JavaScript Bible',
  ]
sidebar_position: 2
---

# 2장: 값, 타입, 연산자

프로그래밍에서 데이터는 모든 것의 기반이 됩니다. 자바스크립트는 다양한 종류의 데이터를 다룰 수 있으며, 각각의 데이터는 특정한 타입을 가지고 있어요. 이 장에서는 자바스크립트의 기본 데이터 타입들과 이들을 조작하는 연산자들을 배워보겠습니다. 또한 ES2020+에서 새롭게 추가된 현대적인 기능들도 함께 살펴볼 예정입니다.

## 학습 목표

이 장을 마치면 다음과 같은 내용을 할 수 있게 됩니다:

- 자바스크립트의 7가지 원시 타입을 이해하고 활용할 수 있습니다
- 다양한 연산자를 사용하여 데이터를 처리할 수 있습니다
- 타입 변환의 원리를 이해하고 안전한 코드를 작성할 수 있습니다
- ES2020+의 새로운 연산자들을 실무에 적용할 수 있습니다

---

## 원시 타입 (Primitive Types)

자바스크립트에는 7가지 원시 타입이 있습니다. 원시 타입은 가장 기본적인 데이터 형태로, 변경할 수 없는(immutable) 특성을 가지고 있어요. 각각의 특징과 사용법을 알아보겠습니다.

### Number 타입

Number 타입은 정수와 실수를 모두 표현할 수 있는 타입입니다. 자바스크립트는 내부적으로 모든 숫자를 64비트 부동소수점으로 저장합니다.

```javascript title="number-examples.js"
// 정수
let age = 25;
let temperature = -10;

// 실수
let price = 299.99;
let ratio = 0.75;

// 특별한 값들
let infinity = Infinity;
let negativeInfinity = -Infinity;
let notANumber = NaN;

// 숫자 연산
console.log(10 / 3); // 3.3333333333333335
console.log(0.1 + 0.2); // 0.30000000000000004 (부동소수점 오차)
console.log(Number.isNaN(NaN)); // true
console.log(Number.isFinite(100)); // true
```

### String 타입

String 타입은 텍스트 데이터를 표현합니다. 자바스크립트에서 문자열은 변경할 수 없으며, 문자열 메서드들은 새로운 문자열을 반환합니다.

```javascript title="string-examples.js"
// 다양한 문자열 생성 방법
let singleQuote = '안녕하세요';
let doubleQuote = 'Hello World';
let templateLiteral = `현재 시간: ${new Date()}`;

// 문자열 조작
let message = 'JavaScript';
console.log(message.length); // 10
console.log(message.toUpperCase()); // "JAVASCRIPT"
console.log(message.slice(0, 4)); // "Java"

// 이스케이프 문자
let multiLine = '첫 번째 줄\n두 번째 줄';
let withQuotes = '그는 "안녕"이라고 말했다';
```

### Boolean 타입

Boolean 타입은 참(true) 또는 거짓(false) 값만을 가질 수 있습니다. 조건문과 논리 연산에서 핵심적인 역할을 합니다.

```javascript title="boolean-examples.js"
let isLoggedIn = true;
let isComplete = false;

// Boolean 변환
console.log(Boolean(1)); // true
console.log(Boolean(0)); // false
console.log(Boolean('')); // false
console.log(Boolean('hello')); // true
console.log(Boolean(null)); // false
console.log(Boolean(undefined)); // false

// Truthy와 Falsy 값들
// Falsy: false, 0, "", null, undefined, NaN
// 나머지는 모두 Truthy
```

### Undefined와 Null

undefined는 값이 할당되지 않은 상태를, null은 의도적으로 빈 값을 나타냅니다.

```javascript title="undefined-null-examples.js"
let notAssigned;
console.log(notAssigned); // undefined

let emptyValue = null;
console.log(emptyValue); // null

// 타입 확인
console.log(typeof undefined); // "undefined"
console.log(typeof null); // "object" (JavaScript의 역사적 버그)

// 비교
console.log(null == undefined); // true (타입 변환 후 비교)
console.log(null === undefined); // false (엄격한 비교)
```

---

## BigInt와 Symbol (ES2020+)

ES2020에서 추가된 BigInt와 ES2015에서 추가된 Symbol은 특별한 용도를 가진 원시 타입입니다.

### BigInt 타입

BigInt는 Number 타입으로 표현할 수 없는 큰 정수를 다룰 때 사용합니다. Number 타입은 안전하게 표현할 수 있는 정수의 범위가 제한되어 있어요.

```javascript title="bigint-examples.js"
// BigInt 생성
let bigNumber1 = 123456789012345678901234567890n;
let bigNumber2 = BigInt('123456789012345678901234567890');

// Number의 한계
console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
console.log(9007199254740991 + 1); // 9007199254740992
console.log(9007199254740991 + 2); // 9007199254740992 (부정확!)

// BigInt 연산
let result = 123n + 456n;
console.log(result); // 579n

// 주의: BigInt와 Number는 직접 연산 불가
// console.log(123n + 456); // TypeError!
console.log(123n + BigInt(456)); // 579n
```

### Symbol 타입

Symbol은 고유한 식별자를 만들 때 사용되는 타입입니다. 주로 객체의 프로퍼티 키로 사용되어 이름 충돌을 방지합니다.

```javascript title="symbol-examples.js"
// Symbol 생성
let symbol1 = Symbol();
let symbol2 = Symbol('description');
let symbol3 = Symbol('description');

console.log(symbol2 === symbol3); // false (항상 고유함)

// 객체에서 Symbol 사용
let obj = {};
let uniqueKey = Symbol('uniqueKey');
obj[uniqueKey] = '비밀 값';

console.log(obj[uniqueKey]); // "비밀 값"
console.log(Object.keys(obj)); // [] (Symbol 키는 나타나지 않음)

// 전역 Symbol
let globalSymbol1 = Symbol.for('shared');
let globalSymbol2 = Symbol.for('shared');
console.log(globalSymbol1 === globalSymbol2); // true
```

---

## 연산자와 표현식

연산자는 값들을 조작하고 새로운 값을 만들어내는 도구입니다. 자바스크립트는 다양한 종류의 연산자를 제공하며, 각각 고유한 역할을 가지고 있어요.

### 산술 연산자

기본적인 수학 연산을 수행하는 연산자들입니다.

```javascript title="arithmetic-operators.js"
let a = 10;
let b = 3;

console.log(a + b); // 13 (덧셈)
console.log(a - b); // 7 (뺄셈)
console.log(a * b); // 30 (곱셈)
console.log(a / b); // 3.3333333333333335 (나눗셈)
console.log(a % b); // 1 (나머지)
console.log(a ** b); // 1000 (거듭제곱, ES2016+)

// 증감 연산자
let count = 5;
console.log(++count); // 6 (전위 증가)
console.log(count++); // 6 (후위 증가, 출력 후 증가)
console.log(count); // 7
```

### 비교 연산자

두 값을 비교하여 boolean 결과를 반환하는 연산자들입니다.

```javascript title="comparison-operators.js"
// 동등성 비교
console.log(5 == '5'); // true (타입 변환 후 비교)
console.log(5 === '5'); // false (엄격한 비교, 타입도 확인)
console.log(5 != '6'); // true
console.log(5 !== '5'); // true

// 크기 비교
console.log(10 > 5); // true
console.log(10 >= 10); // true
console.log(5 < 3); // false
console.log(3 <= 3); // true

// 문자열 비교 (사전식 순서)
console.log('apple' < 'banana'); // true
console.log('Apple' < 'apple'); // true (대문자가 소문자보다 작음)
```

### 논리 연산자

boolean 값들을 조합하는 연산자들입니다.

```javascript title="logical-operators.js"
let isTrue = true;
let isFalse = false;

// AND 연산자 (&&)
console.log(isTrue && isFalse); // false
console.log(isTrue && true); // true

// OR 연산자 (||)
console.log(isTrue || isFalse); // true
console.log(isFalse || false); // false

// NOT 연산자 (!)
console.log(!isTrue); // false
console.log(!isFalse); // true
console.log(!!isTrue); // true (이중 부정으로 boolean 변환)

// 단축 평가 (Short-circuit evaluation)
let user = null;
let defaultName = user && user.name; // null (user가 falsy이므로 뒤쪽 평가 안함)
let name = user || '익명'; // "익명" (user가 falsy이므로 뒤쪽 값 반환)
```

---

## 새로운 연산자: Nullish coalescing (??) 와 논리 할당 연산자

ES2020과 ES2021에서 추가된 현대적인 연산자들은 더 안전하고 간결한 코드 작성을 도와줍니다.

### Nullish Coalescing 연산자 (??)

OR 연산자(||)와 비슷하지만, null과 undefined에 대해서만 동작하는 더 안전한 연산자입니다.

```javascript title="nullish-coalescing.js"
// 기존 OR 연산자의 문제점
let count = 0;
let defaultCount = count || 10; // 10 (0이 falsy이므로)
console.log(defaultCount); // 원하지 않는 결과!

// Nullish coalescing 연산자 사용
let safeDefaultCount = count ?? 10; // 0 (count가 null/undefined가 아니므로)
console.log(safeDefaultCount); // 0 (올바른 결과!)

// 실용적인 예제
function createUser(options = {}) {
  return {
    name: options.name ?? '익명',
    age: options.age ?? 0,
    isActive: options.isActive ?? true,
  };
}

let user1 = createUser({ name: '김철수', age: 0 });
console.log(user1); // { name: "김철수", age: 0, isActive: true }

let user2 = createUser({ name: '이영희', isActive: false });
console.log(user2); // { name: "이영희", age: 0, isActive: false }
```

### 논리 할당 연산자 (ES2021)

조건부 할당을 더 간결하게 표현할 수 있는 연산자들입니다.

```javascript title="logical-assignment.js"
// AND 할당 (&&=)
let user = { name: '김철수' };
user.name &&= user.name.toUpperCase(); // name이 truthy일 때만 할당
console.log(user.name); // "김철수"

// OR 할당 (||=)
let config = {};
config.theme ||= 'light'; // theme이 falsy일 때만 할당
config.timeout ||= 5000;
console.log(config); // { theme: "light", timeout: 5000 }

// Nullish 할당 (??=)
let settings = { timeout: 0 };
settings.timeout ??= 3000; // null/undefined일 때만 할당
settings.retries ??= 3;
console.log(settings); // { timeout: 0, retries: 3 }

// 실용적인 예제: 캐시 구현
let cache = {};
function expensiveOperation(key) {
  cache[key] ??= performExpensiveCalculation(key);
  return cache[key];
}

function performExpensiveCalculation(key) {
  console.log(`계산 중: ${key}`);
  return key * 2;
}

console.log(expensiveOperation('test')); // "계산 중: test", "testtest"
console.log(expensiveOperation('test')); // "testtest" (캐시에서 가져옴)
```

---

## 타입 변환과 엄격한 비교

자바스크립트는 필요에 따라 자동으로 타입을 변환하는 특성이 있습니다. 이를 이해하고 제어하는 것이 안전한 코드 작성의 핵심입니다.

### 암시적 타입 변환

자바스크립트 엔진이 자동으로 수행하는 타입 변환을 살펴보겠습니다.

```javascript title="implicit-conversion.js"
// 문자열 변환
console.log('5' + 3); // "53" (숫자가 문자열로 변환)
console.log('5' + true); // "5true"
console.log('5' + null); // "5null"

// 숫자 변환
console.log('5' - 3); // 2 (문자열이 숫자로 변환)
console.log('5' * '2'); // 10
console.log(true + 1); // 2 (true는 1로 변환)
console.log(false + 1); // 1 (false는 0으로 변환)

// Boolean 변환 (조건문에서)
if ('') console.log('빈 문자열'); // 실행되지 않음
if ('hello') console.log('문자열 있음'); // 실행됨
if (0) console.log('숫자 0'); // 실행되지 않음
if (42) console.log('숫자 42'); // 실행됨

// 예상하기 어려운 변환들
console.log([] + []); // "" (빈 문자열)
console.log([] + {}); // "[object Object]"
console.log({} + []); // "[object Object]" 또는 0 (환경에 따라)
```

### 명시적 타입 변환

개발자가 의도적으로 수행하는 타입 변환 방법들입니다.

```javascript title="explicit-conversion.js"
// 문자열로 변환
let num = 123;
console.log(String(num)); // "123"
console.log(num.toString()); // "123"
console.log(`${num}`); // "123" (템플릿 리터럴 활용)

// 숫자로 변환
let str = '456';
console.log(Number(str)); // 456
console.log(parseInt(str)); // 456
console.log(parseFloat('123.45')); // 123.45
console.log(+str); // 456 (단항 플러스 연산자)

// Boolean으로 변환
console.log(Boolean('hello')); // true
console.log(Boolean('')); // false
console.log(!!str); // true (이중 부정으로 변환)

// 안전한 변환 함수들
function safeParseInt(value, defaultValue = 0) {
  const result = parseInt(value);
  return Number.isNaN(result) ? defaultValue : result;
}

console.log(safeParseInt('123')); // 123
console.log(safeParseInt('abc')); // 0
console.log(safeParseInt('abc', -1)); // -1
```

### 엄격한 비교 (===)

타입 변환 없이 값과 타입을 모두 비교하는 안전한 비교 방법입니다.

```javascript title="strict-comparison.js"
// 느슨한 비교 (==)의 문제점
console.log(0 == false); // true
console.log('' == false); // true
console.log(null == undefined); // true
console.log('0' == 0); // true

// 엄격한 비교 (===) 사용
console.log(0 === false); // false
console.log('' === false); // false
console.log(null === undefined); // false
console.log('0' === 0); // false

// 실용적인 예제
function findUserById(users, id) {
  // 엄격한 비교로 정확한 매칭
  return users.find(user => user.id === id);
}

let users = [
  { id: 1, name: '김철수' },
  { id: '2', name: '이영희' },
  { id: 3, name: '박민수' },
];

console.log(findUserById(users, 1)); // { id: 1, name: "김철수" }
console.log(findUserById(users, '1')); // undefined (타입이 다름)

// 특별한 값들의 비교
console.log(NaN === NaN); // false! (NaN은 자기 자신과도 다름)
console.log(Number.isNaN(NaN)); // true (올바른 NaN 확인 방법)
console.log(Object.is(NaN, NaN)); // true (ES2015+)
```

---

## 실습: 타입 체커 함수 만들기

학습한 내용을 바탕으로 다양한 타입을 정확하게 판별할 수 있는 유틸리티 함수를 만들어보겠습니다. 이 실습을 통해 자바스크립트의 타입 시스템을 더 깊이 이해할 수 있습니다.

```javascript title="type-checker.js"
// 정확한 타입 검사 함수
function getType(value) {
  // null은 typeof로 "object"가 나오므로 따로 처리
  if (value === null) return 'null';

  // 기본 typeof 결과
  const type = typeof value;

  // object 타입을 더 세분화
  if (type === 'object') {
    if (Array.isArray(value)) return 'array';
    if (value instanceof Date) return 'date';
    if (value instanceof RegExp) return 'regexp';
    return 'object';
  }

  return type;
}

// 타입별 검사 함수들
const typeCheckers = {
  isString: value => typeof value === 'string',
  isNumber: value => typeof value === 'number' && !Number.isNaN(value),
  isBoolean: value => typeof value === 'boolean',
  isUndefined: value => typeof value === 'undefined',
  isNull: value => value === null,
  isBigInt: value => typeof value === 'bigint',
  isSymbol: value => typeof value === 'symbol',
  isArray: value => Array.isArray(value),
  isObject: value => value !== null && typeof value === 'object' && !Array.isArray(value),
  isFunction: value => typeof value === 'function',
  isPrimitive: value => {
    const type = typeof value;
    return type !== 'object' || value === null;
  },
  isNullish: value => value === null || value === undefined,
  isFalsy: value => !value,
  isTruthy: value => !!value,
};

// 테스트 코드
const testValues = [
  42,
  'hello',
  true,
  null,
  undefined,
  123n,
  Symbol('test'),
  [],
  {},
  function () {},
  new Date(),
  /pattern/,
  NaN,
  Infinity,
];

console.log('=== 타입 검사 결과 ===');
testValues.forEach(value => {
  console.log(`값: ${String(value).padEnd(15)} | 타입: ${getType(value)}`);
});

console.log('\n=== 특별한 검사들 ===');
console.log(`isNullish(null): ${typeCheckers.isNullish(null)}`);
console.log(`isNullish(undefined): ${typeCheckers.isNullish(undefined)}`);
console.log(`isNullish(0): ${typeCheckers.isNullish(0)}`);
console.log(`isPrimitive("hello"): ${typeCheckers.isPrimitive('hello')}`);
console.log(`isPrimitive({}): ${typeCheckers.isPrimitive({})}`);
console.log(`isFalsy(0): ${typeCheckers.isFalsy(0)}`);
console.log(`isFalsy(""): ${typeCheckers.isFalsy('')}`);
console.log(`isTruthy("0"): ${typeCheckers.isTruthy('0')}`);
```

---

## 실습: 다양한 연산자 활용 예제

실제 개발에서 자주 사용되는 연산자 패턴들을 실습해보겠습니다. 이 예제들은 실무에서 바로 활용할 수 있는 유용한 패턴들입니다.

```javascript title="operator-patterns.js"
// 1. 안전한 객체 속성 접근 (Optional Chaining과 Nullish Coalescing 활용)
function getUserDisplayName(user) {
  // 기존 방식 (복잡하고 길음)
  // return user && user.profile && user.profile.name ? user.profile.name : "익명 사용자";

  // 현대적 방식 (간결하고 안전함)
  return user?.profile?.name ?? '익명 사용자';
}

const users = [{ profile: { name: '김철수' } }, { profile: {} }, {}, null];

users.forEach((user, index) => {
  console.log(`사용자 ${index + 1}: ${getUserDisplayName(user)}`);
});

// 2. 설정 객체 병합하기
function createConfig(userConfig = {}) {
  const defaultConfig = {
    theme: 'light',
    timeout: 5000,
    retries: 3,
    debug: false,
  };

  // 각 속성별로 안전하게 병합
  return {
    theme: userConfig.theme ?? defaultConfig.theme,
    timeout: userConfig.timeout ?? defaultConfig.timeout,
    retries: userConfig.retries ?? defaultConfig.retries,
    debug: userConfig.debug ?? defaultConfig.debug,
  };
}

console.log('\n=== 설정 병합 예제 ===');
console.log(createConfig()); // 기본값 사용
console.log(createConfig({ theme: 'dark', timeout: 0 })); // 0도 유지됨

// 3. 캐시 시스템 구현
class SimpleCache {
  constructor() {
    this.cache = {};
    this.hitCount = 0;
    this.missCount = 0;
  }

  get(key, factory) {
    // Nullish 할당으로 캐시 구현
    this.cache[key] ??= factory(key);

    // 기존에 값이 있었는지 확인
    if (this.cache[key] === factory(key)) {
      this.missCount++;
    } else {
      this.hitCount++;
    }

    return this.cache[key];
  }

  getStats() {
    const total = this.hitCount + this.missCount;
    return {
      hits: this.hitCount,
      misses: this.missCount,
      hitRate: total > 0 ? ((this.hitCount / total) * 100).toFixed(2) + '%' : '0%',
    };
  }
}

const cache = new SimpleCache();

// 비싼 연산을 시뮬레이션
function expensiveCalculation(n) {
  console.log(`계산 실행: fibonacci(${n})`);
  if (n <= 1) return n;
  return expensiveCalculation(n - 1) + expensiveCalculation(n - 2);
}

console.log('\n=== 캐시 시스템 테스트 ===');
console.log(cache.get('fib10', () => expensiveCalculation(10)));
console.log(cache.get('fib10', () => expensiveCalculation(10))); // 캐시에서 가져옴
console.log(cache.getStats());

// 4. 안전한 수학 연산
function safeMath(operation, a, b) {
  // 입력값 검증
  if (typeof a !== 'number' || typeof b !== 'number') {
    throw new TypeError('숫자만 입력 가능합니다');
  }

  if (!Number.isFinite(a) || !Number.isFinite(b)) {
    throw new RangeError('유한한 숫자만 입력 가능합니다');
  }

  let result;

  switch (operation) {
    case '+':
      result = a + b;
      break;
    case '-':
      result = a - b;
      break;
    case '*':
      result = a * b;
      break;
    case '/':
      if (b === 0) throw new Error('0으로 나눌 수 없습니다');
      result = a / b;
      break;
    case '**':
      result = a ** b;
      break;
    case '%':
      if (b === 0) throw new Error('0으로 나눌 수 없습니다');
      result = a % b;
      break;
    default:
      throw new Error(`지원하지 않는 연산: ${operation}`);
  }

  // 결과 검증
  if (!Number.isFinite(result)) {
    throw new RangeError('연산 결과가 유한하지 않습니다');
  }

  return result;
}

console.log('\n=== 안전한 수학 연산 테스트 ===');
try {
  console.log(safeMath('+', 10, 5)); // 15
  console.log(safeMath('/', 10, 2)); // 5
  console.log(safeMath('**', 2, 3)); // 8
  console.log(safeMath('/', 10, 0)); // Error!
} catch (error) {
  console.log(`오류: ${error.message}`);
}

// 5. 스마트한 비교 함수
function smartEqual(a, b) {
  // 타입이 같으면 엄격한 비교
  if (typeof a === typeof b) {
    // NaN 특별 처리
    if (Number.isNaN(a) && Number.isNaN(b)) return true;
    return a === b;
  }

  // 숫자와 문자열 비교
  if (
    (typeof a === 'number' && typeof b === 'string') ||
    (typeof a === 'string' && typeof b === 'number')
  ) {
    return Number(a) === Number(b) && !Number.isNaN(Number(a));
  }

  // null과 undefined는 같은 것으로 취급
  if ((a === null && b === undefined) || (a === undefined && b === null)) {
    return true;
  }

  return false;
}

console.log('\n=== 스마트한 비교 함수 테스트 ===');
console.log(smartEqual(5, '5')); // true
console.log(smartEqual(0, false)); // false
console.log(smartEqual(null, undefined)); // true
console.log(smartEqual(NaN, NaN)); // true
console.log(smartEqual('hello', 'hello')); // true
```

이 장에서 배운 내용들은 자바스크립트 프로그래밍의 기초가 되는 매우 중요한 개념들입니다. 특히 타입 시스템과 연산자들을 정확히 이해하면 더 안전하고 예측 가능한 코드를 작성할 수 있어요. 다음 장에서는 이러한 기초 위에 변수 선언과 스코프에 대해 자세히 알아보겠습니다.
