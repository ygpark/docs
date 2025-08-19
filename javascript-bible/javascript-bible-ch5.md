---
title: '5장: 모던 함수'
slug: javascript-bible-modern-functions
description: '모던 자바스크립트의 함수 문법을 마스터하세요. 화살표 함수, 매개변수 기본값, 나머지 매개변수와 전개 구문까지 실전 예제와 함께 배워보겠습니다.'
keywords:
  [
    '자바스크립트 함수',
    '화살표 함수',
    'Arrow Functions',
    '매개변수 기본값',
    '나머지 매개변수',
    'Rest Parameters',
    '전개 구문',
    'Spread Syntax',
    '함수 선언',
    '함수 표현식',
    'ES6 함수',
    '모던 자바스크립트',
  ]
sidebar_position: 5
---

# 5장: 모던 함수

## 학습 목표

이 장에서는 자바스크립트 함수의 다양한 형태와 모던한 문법을 익혀보겠습니다. 전통적인 함수 선언부터 ES6+에서 도입된 화살표 함수, 매개변수 기본값, 나머지 매개변수와 전개 구문까지 실무에서 자주 사용되는 패턴들을 실습해보겠습니다.

---

## 5.1 함수 선언과 함수 표현식

함수는 자바스크립트에서 가장 중요한 구성 요소 중 하나입니다. 코드의 재사용성을 높이고 프로그램을 논리적으로 구조화하는 데 핵심적인 역할을 합니다. 모던 자바스크립트에서는 함수를 정의하는 여러 가지 방법이 있으며, 각각의 특징을 이해하는 것이 중요합니다.

### 함수 선언문 (Function Declaration)

함수 선언문은 가장 전통적인 함수 정의 방법입니다. 호이스팅이 발생하여 선언 전에도 호출할 수 있습니다.

```javascript title="function-declaration.js"
// 함수 선언문 - 호이스팅 됨
console.log(add(2, 3)); // 5 (선언 전에 호출 가능)

function add(a, b) {
  return a + b;
}

// 더 복잡한 예제
function greetUser(name, timeOfDay) {
  const greeting = timeOfDay === 'morning' ? '좋은 아침이에요' : '안녕하세요';

  return `${greeting}, ${name}님!`;
}

console.log(greetUser('김철수', 'morning')); // 좋은 아침이에요, 김철수님!
```

### 함수 표현식 (Function Expression)

함수 표현식은 변수에 함수를 할당하는 방식입니다. 호이스팅되지 않으므로 선언 후에만 사용할 수 있습니다.

```javascript title="function-expression.js"
// 함수 표현식 - 호이스팅 안됨
const multiply = function (a, b) {
  return a * b;
};

console.log(multiply(4, 5)); // 20

// 즉시 실행 함수 표현식 (IIFE)
const result = (function (x, y) {
  return x + y;
})(10, 20);

console.log(result); // 30
```

**배경 설명**: 함수 선언문과 표현식의 차이점을 이해하는 것은 호이스팅과 스코프를 다룰 때 매우 중요합니다. 특히 모듈 시스템에서는 함수 표현식이 더 안전한 선택이 될 수 있습니다.

---

## 5.2 화살표 함수 (Arrow Functions)

화살표 함수는 ES6에서 도입된 간결한 함수 문법입니다. 전통적인 함수와는 this 바인딩이 다르며, 더 짧고 읽기 쉬운 코드를 작성할 수 있게 해줍니다. 특히 콜백 함수나 간단한 연산에서 매우 유용합니다.

### 기본 화살표 함수 문법

```javascript title="arrow-functions.js"
// 기본 문법
const square = x => x * x;
console.log(square(5)); // 25

// 매개변수가 하나일 때 괄호 생략 가능
const double = x => x * 2;
console.log(double(3)); // 6

// 매개변수가 없을 때
const sayHello = () => console.log('Hello!');
sayHello(); // Hello!

// 여러 줄의 함수 본문
const calculateArea = (width, height) => {
  const area = width * height;
  return `면적은 ${area}입니다.`;
};

console.log(calculateArea(4, 5)); // 면적은 20입니다.
```

### 화살표 함수와 배열 메서드

```javascript title="arrow-with-arrays.js"
const numbers = [1, 2, 3, 4, 5];

// map과 화살표 함수
const doubled = numbers.map(num => num * 2);
console.log(doubled); // [2, 4, 6, 8, 10]

// filter와 화살표 함수
const evenNumbers = numbers.filter(num => num % 2 === 0);
console.log(evenNumbers); // [2, 4]

// reduce와 화살표 함수
const sum = numbers.reduce((acc, curr) => acc + curr, 0);
console.log(sum); // 15
```

**배경 설명**: 화살표 함수는 특히 함수형 프로그래밍 패턴에서 빛을 발합니다. 배열의 map, filter, reduce 등과 함께 사용하면 매우 간결하고 표현력 있는 코드를 작성할 수 있습니다.

---

## 5.3 매개변수 기본값

함수의 매개변수에 기본값을 설정할 수 있는 기능은 ES6에서 도입되었습니다. 이를 통해 함수 호출 시 인수를 전달하지 않아도 기본값이 사용되어 더 안전하고 유연한 함수를 만들 수 있습니다.

### 기본값 설정하기

```javascript title="default-parameters.js"
// 기본 매개변수 설정
function createUser(name = '익명', age = 0, role = 'user') {
  return {
    name,
    age,
    role,
    createdAt: new Date(),
  };
}

console.log(createUser());
// { name: '익명', age: 0, role: 'user', createdAt: ... }

console.log(createUser('김영희', 25));
// { name: '김영희', age: 25, role: 'user', createdAt: ... }

console.log(createUser('박관리', 30, 'admin'));
// { name: '박관리', age: 30, role: 'admin', createdAt: ... }
```

### 동적 기본값과 함수 호출

```javascript title="dynamic-defaults.js"
// 함수 호출로 기본값 설정
function getCurrentTime() {
  return new Date().toLocaleTimeString();
}

function log(message, timestamp = getCurrentTime()) {
  console.log(`[${timestamp}] ${message}`);
}

log('첫 번째 메시지');
// [오후 2:30:45] 첫 번째 메시지

// 잠시 후...
setTimeout(() => {
  log('두 번째 메시지');
  // [오후 2:30:47] 두 번째 메시지
}, 2000);
```

**배경 설명**: 매개변수 기본값은 API 설계에서 매우 유용합니다. 선택적 옵션을 가진 함수를 만들 때 사용자가 모든 인수를 제공하지 않아도 되므로 사용성이 크게 향상됩니다.

---

## 5.4 나머지 매개변수와 전개 구문 (Rest/Spread)

나머지 매개변수와 전개 구문은 ES6의 강력한 기능으로, 함수에서 가변 인수를 다루거나 배열과 객체를 효율적으로 조작할 수 있게 해줍니다. 이 두 기능은 비슷해 보이지만 사용 맥락에 따라 다른 역할을 합니다.

### 나머지 매개변수 (Rest Parameters)

```javascript title="rest-parameters.js"
// 나머지 매개변수로 가변 인수 받기
function sum(...numbers) {
  return numbers.reduce((total, num) => total + num, 0);
}

console.log(sum(1, 2, 3)); // 6
console.log(sum(1, 2, 3, 4, 5)); // 15
console.log(sum()); // 0

// 첫 번째 인수와 나머지 분리
function introduce(name, ...hobbies) {
  const hobbyList = hobbies.length > 0 ? hobbies.join(', ') : '특별한 취미가 없어요';

  return `안녕하세요! 저는 ${name}이고, 취미는 ${hobbyList}.`;
}

console.log(introduce('김철수'));
// 안녕하세요! 저는 김철수이고, 취미는 특별한 취미가 없어요.

console.log(introduce('이영희', '독서', '영화감상', '등산'));
// 안녕하세요! 저는 이영희이고, 취미는 독서, 영화감상, 등산.
```

### 전개 구문 (Spread Syntax)

```javascript title="spread-syntax.js"
// 배열 전개
const arr1 = [1, 2, 3];
const arr2 = [4, 5, 6];
const combined = [...arr1, ...arr2];
console.log(combined); // [1, 2, 3, 4, 5, 6]

// 함수 호출 시 전개
function multiply(a, b, c) {
  return a * b * c;
}

const numbers = [2, 3, 4];
console.log(multiply(...numbers)); // 24

// 객체 전개 (ES2018+)
const baseConfig = {
  theme: 'dark',
  language: 'ko',
};

const userConfig = {
  ...baseConfig,
  fontSize: 16,
  theme: 'light', // 덮어쓰기
};

console.log(userConfig);
// { theme: 'light', language: 'ko', fontSize: 16 }
```

**배경 설명**: 전개 구문은 불변성을 유지하면서 데이터를 조작할 때 매우 유용합니다. React와 같은 라이브러리에서 상태를 업데이트할 때 자주 사용되는 패턴입니다.

---

## 5.5 실습: 함수 팩토리 만들기

다양한 타입의 함수를 생성하는 팩토리 함수를 만들어보겠습니다. 이 실습을 통해 클로저와 함수형 프로그래밍의 기초를 이해할 수 있습니다.

```javascript title="function-factory.js"
// 함수 팩토리: 특정 배수를 확인하는 함수들을 생성
function createMultipleChecker(divisor) {
  return function (number) {
    return number % divisor === 0;
  };
}

// 또는 화살표 함수로
const createMultipleChecker2 = divisor => number => number % divisor === 0;

// 사용 예제
const isEven = createMultipleChecker(2);
const isMultipleOf3 = createMultipleChecker(3);
const isMultipleOf5 = createMultipleChecker(5);

console.log(isEven(4)); // true
console.log(isEven(5)); // false
console.log(isMultipleOf3(9)); // true
console.log(isMultipleOf5(25)); // true

// 배열과 함께 사용
const numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10];
const evenNumbers = numbers.filter(isEven);
const multiplesOf3 = numbers.filter(isMultipleOf3);

console.log('짝수:', evenNumbers); // [2, 4, 6, 8, 10]
console.log('3의 배수:', multiplesOf3); // [3, 6, 9]
```

### 고급 팩토리 함수 예제

```javascript title="advanced-factory.js"
// 검증 함수 팩토리
function createValidator(rules) {
  return function (data) {
    const errors = [];

    for (const [field, rule] of Object.entries(rules)) {
      const value = data[field];

      if (rule.required && (!value || value.trim() === '')) {
        errors.push(`${field}는 필수 항목입니다.`);
        continue;
      }

      if (value && rule.minLength && value.length < rule.minLength) {
        errors.push(`${field}는 최소 ${rule.minLength}자 이상이어야 합니다.`);
      }

      if (value && rule.pattern && !rule.pattern.test(value)) {
        errors.push(`${field}의 형식이 올바르지 않습니다.`);
      }
    }

    return {
      isValid: errors.length === 0,
      errors,
    };
  };
}

// 사용자 등록 폼 검증기 생성
const validateUser = createValidator({
  name: { required: true, minLength: 2 },
  email: {
    required: true,
    pattern: /^[^\s@]+@[^\s@]+\.[^\s@]+$/,
  },
  password: { required: true, minLength: 8 },
});

// 검증 테스트
const userData1 = {
  name: '김',
  email: 'invalid-email',
  password: '123',
};

const result1 = validateUser(userData1);
console.log(result1);
// {
//   isValid: false,
//   errors: [
//     'name는 최소 2자 이상이어야 합니다.',
//     'email의 형식이 올바르지 않습니다.',
//     'password는 최소 8자 이상이어야 합니다.'
//   ]
// }

const userData2 = {
  name: '김철수',
  email: 'kim@example.com',
  password: 'password123',
};

const result2 = validateUser(userData2);
console.log(result2); // { isValid: true, errors: [] }
```

**배경 설명**: 함수 팩토리 패턴은 재사용 가능한 함수를 동적으로 생성할 때 매우 유용합니다. 특히 설정이나 규칙이 다른 여러 함수가 필요할 때 코드 중복을 줄일 수 있습니다.

---

## 5.6 실습: 가변 인수를 받는 유틸리티 함수들

나머지 매개변수와 전개 구문을 활용한 실용적인 유틸리티 함수들을 만들어보겠습니다.

```javascript title="utility-functions.js"
// 1. 수학 유틸리티 함수들
const mathUtils = {
  // 최댓값 찾기
  max: (...numbers) => Math.max(...numbers),

  // 최솟값 찾기
  min: (...numbers) => Math.min(...numbers),

  // 평균 계산
  average: (...numbers) => {
    if (numbers.length === 0) return 0;
    return numbers.reduce((sum, num) => sum + num, 0) / numbers.length;
  },

  // 합계 계산
  sum: (...numbers) => numbers.reduce((sum, num) => sum + num, 0),

  // 범위 내 숫자들 생성
  range: (start, end, step = 1) => {
    const result = [];
    for (let i = start; i <= end; i += step) {
      result.push(i);
    }
    return result;
  },
};

// 사용 예제
console.log(mathUtils.max(1, 5, 3, 9, 2)); // 9
console.log(mathUtils.average(10, 20, 30)); // 20
console.log(mathUtils.range(1, 10, 2)); // [1, 3, 5, 7, 9]

// 2. 배열 유틸리티 함수들
const arrayUtils = {
  // 배열들을 하나로 합치기
  concat: (...arrays) => [].concat(...arrays),

  // 중복 제거
  unique: (...arrays) => [...new Set([].concat(...arrays))],

  // 배열의 교집합
  intersection: (arr1, ...arrays) => {
    return arr1.filter(item => arrays.every(arr => arr.includes(item)));
  },

  // 배열을 청크로 나누기
  chunk: (array, size) => {
    const chunks = [];
    for (let i = 0; i < array.length; i += size) {
      chunks.push(array.slice(i, i + size));
    }
    return chunks;
  },
};

// 사용 예제
const arr1 = [1, 2, 3];
const arr2 = [3, 4, 5];
const arr3 = [5, 6, 7];

console.log(arrayUtils.concat(arr1, arr2, arr3));
// [1, 2, 3, 3, 4, 5, 5, 6, 7]

console.log(arrayUtils.unique(arr1, arr2, arr3));
// [1, 2, 3, 4, 5, 6, 7]

console.log(arrayUtils.intersection([1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]));
// [3, 4]

console.log(arrayUtils.chunk([1, 2, 3, 4, 5, 6, 7, 8], 3));
// [[1, 2, 3], [4, 5, 6], [7, 8]]

// 3. 객체 유틸리티 함수들
const objectUtils = {
  // 여러 객체를 깊게 병합
  deepMerge: (...objects) => {
    const result = {};

    objects.forEach(obj => {
      Object.keys(obj).forEach(key => {
        if (typeof obj[key] === 'object' && obj[key] !== null && !Array.isArray(obj[key])) {
          result[key] = objectUtils.deepMerge(result[key] || {}, obj[key]);
        } else {
          result[key] = obj[key];
        }
      });
    });

    return result;
  },

  // 특정 키들만 선택
  pick: (obj, ...keys) => {
    return keys.reduce((result, key) => {
      if (key in obj) {
        result[key] = obj[key];
      }
      return result;
    }, {});
  },

  // 특정 키들 제외
  omit: (obj, ...keys) => {
    const result = { ...obj };
    keys.forEach(key => delete result[key]);
    return result;
  },
};

// 사용 예제
const config1 = { theme: 'dark', size: 'large' };
const config2 = { theme: 'light', animation: true };
const config3 = { size: 'medium', debug: false };

console.log(objectUtils.deepMerge(config1, config2, config3));
// { theme: 'light', size: 'medium', animation: true, debug: false }

const user = { name: '김철수', age: 30, email: 'kim@example.com', password: '123456' };
console.log(objectUtils.pick(user, 'name', 'email'));
// { name: '김철수', email: 'kim@example.com' }

console.log(objectUtils.omit(user, 'password'));
// { name: '김철수', age: 30, email: 'kim@example.com' }
```

**배경 설명**: 이러한 유틸리티 함수들은 실제 프로젝트에서 매우 자주 사용되는 패턴들입니다. Lodash 같은 라이브러리의 기본 기능들을 직접 구현해봄으로써 자바스크립트의 함수형 프로그래밍 개념을 더 깊이 이해할 수 있습니다.

---

## 5.7 마무리

이 장에서는 모던 자바스크립트의 함수 기능들을 살펴보았습니다. 함수 선언과 표현식의 차이점부터 화살표 함수의 간결함, 매개변수 기본값의 편리함, 그리고 나머지 매개변수와 전개 구문의 강력함까지 배웠습니다.

특히 함수 팩토리 패턴과 가변 인수를 다루는 유틸리티 함수들을 통해 실무에서 어떻게 이러한 기능들을 활용할 수 있는지도 경험해보았습니다. 이러한 패턴들은 코드의 재사용성을 높이고 더 표현력 있는 프로그래밍을 가능하게 합니다.

다음 장에서는 객체와 배열을 다루면서 구조 분해 할당과 최신 배열 메서드들을 살펴보겠습니다. 함수와 함께 자바스크립트의 핵심을 이루는 이 개념들을 마스터하면 한층 더 정교한 프로그램을 작성할 수 있을 것입니다.
