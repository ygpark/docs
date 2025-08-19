---
title: '9. 에러 처리와 디버깅'
slug: javascript-bible-error-handling-debugging
description: '자바스크립트에서 try-catch문을 활용한 에러 처리 방법과 효과적인 디버깅 기법을 학습합니다. 커스텀 에러 생성부터 개발자 도구 활용까지 실전 디버깅 스킬을 익혀보세요.'
keywords:
  [
    '자바스크립트 에러 처리',
    'try catch',
    'finally',
    '커스텀 에러',
    '디버깅',
    '개발자 도구',
    'console.log',
    'breakpoint',
    'ESLint',
    'JSON 파싱',
  ]
sidebar_position: 9
---

# 9장: 에러 처리와 디버깅

개발 과정에서 예상치 못한 에러는 피할 수 없는 부분입니다. 중요한 것은 이러한 에러를 어떻게 우아하게 처리하고, 문제가 발생했을 때 빠르게 원인을 찾아 해결하는지입니다. 이 장에서는 자바스크립트의 에러 처리 메커니즘과 효과적인 디버깅 기법을 배워서 더 안정적이고 유지보수가 쉬운 코드를 작성하는 방법을 익혀보겠습니다.

**학습 목표:**

- try...catch...finally 문을 활용한 에러 처리 방법 이해
- 커스텀 에러 클래스 생성과 활용
- 브라우저 개발자 도구를 이용한 디버깅 기법
- 코드 품질 향상을 위한 도구 활용법

---

## 기본 에러 처리: try...catch...finally

에러 처리는 프로그램이 예상치 못한 상황에서도 안정적으로 동작할 수 있도록 하는 중요한 기법입니다. 자바스크립트에서는 try...catch...finally 구문을 사용하여 에러를 처리할 수 있습니다.

### try...catch 기본 구문

try 블록에서 발생한 에러를 catch 블록에서 처리하는 기본적인 구조입니다.

```javascript title=basic-error-handling.js
// 기본적인 try-catch 사용법
function safeDivision(a, b) {
  try {
    if (b === 0) {
      throw new Error('0으로 나눌 수 없습니다');
    }
    return a / b;
  } catch (error) {
    console.error('에러 발생:', error.message);
    return null;
  }
}

console.log(safeDivision(10, 2)); // 5
console.log(safeDivision(10, 0)); // 에러 발생: 0으로 나눌 수 없습니다, null
```

### finally 블록 활용

finally 블록은 에러 발생 여부와 관계없이 항상 실행되는 코드 블록입니다.

```javascript title=finally-example.js
function processData(data) {
  let resource = null;

  try {
    resource = '데이터 처리 중...';
    console.log('리소스 할당:', resource);

    if (!data) {
      throw new Error('데이터가 없습니다');
    }

    return `처리 완료: ${data}`;
  } catch (error) {
    console.error('처리 실패:', error.message);
    return '처리 실패';
  } finally {
    // 에러 발생 여부와 관계없이 항상 실행
    console.log('리소스 정리 완료');
  }
}

console.log(processData('중요한 데이터'));
console.log(processData(null));
```

---

## 커스텀 에러 만들기

내장된 Error 클래스를 확장하여 더 구체적이고 의미있는 에러를 만들 수 있습니다. 이를 통해 에러의 종류를 명확히 구분하고 적절한 처리를 할 수 있습니다.

### 커스텀 에러 클래스 정의

```javascript title=custom-error.js
// 커스텀 에러 클래스들
class ValidationError extends Error {
  constructor(message) {
    super(message);
    this.name = 'ValidationError';
  }
}

class NetworkError extends Error {
  constructor(message, statusCode) {
    super(message);
    this.name = 'NetworkError';
    this.statusCode = statusCode;
  }
}

// 사용자 검증 함수
function validateUser(user) {
  if (!user.name) {
    throw new ValidationError('이름은 필수 항목입니다');
  }

  if (!user.email || !user.email.includes('@')) {
    throw new ValidationError('올바른 이메일 형식이 아닙니다');
  }

  if (user.age < 0 || user.age > 150) {
    throw new ValidationError('나이는 0-150 사이여야 합니다');
  }

  return true;
}

// 에러 타입별 처리
function processUser(userData) {
  try {
    validateUser(userData);
    console.log('사용자 검증 성공:', userData.name);
  } catch (error) {
    if (error instanceof ValidationError) {
      console.error('검증 오류:', error.message);
    } else if (error instanceof NetworkError) {
      console.error(`네트워크 오류 [${error.statusCode}]:`, error.message);
    } else {
      console.error('알 수 없는 오류:', error.message);
    }
  }
}

// 테스트
processUser({ name: '김철수', email: 'kim@example.com', age: 25 });
processUser({ name: '', email: 'invalid-email', age: -5 });
```

---

## 디버깅 기법과 도구

효과적인 디버깅은 개발 생산성을 크게 향상시킵니다. 다양한 디버깅 도구와 기법을 활용하여 문제를 빠르게 찾고 해결해보겠습니다.

### console 객체 활용

console 객체는 다양한 메서드를 제공하여 디버깅을 도와줍니다.

```javascript title=console-debugging.js
// 다양한 console 메서드 활용
function debugExample() {
  const users = [
    { id: 1, name: '김철수', role: 'admin' },
    { id: 2, name: '이영희', role: 'user' },
    { id: 3, name: '박민수', role: 'user' },
  ];

  // 기본 로그
  console.log('사용자 목록:', users);

  // 경고 메시지
  console.warn('관리자 권한이 필요합니다');

  // 에러 메시지
  console.error('인증 실패');

  // 테이블 형태로 출력
  console.table(users);

  // 그룹화
  console.group('사용자 처리');
  users.forEach(user => {
    console.log(`처리 중: ${user.name}`);
  });
  console.groupEnd();

  // 시간 측정
  console.time('데이터 처리');
  // 무거운 작업 시뮬레이션
  for (let i = 0; i < 1000000; i++) {
    // 작업 수행
  }
  console.timeEnd('데이터 처리');

  // 조건부 로그
  const isDebugMode = true;
  console.assert(isDebugMode, '디버그 모드가 아닙니다');
}

debugExample();
```

### 브라우저 개발자 도구 활용

브라우저의 개발자 도구는 강력한 디버깅 환경을 제공합니다.

```javascript title=debugging-tips.js
// 디버깅을 위한 유틸리티 함수들
class DebugHelper {
  static log(message, data = null) {
    if (process.env.NODE_ENV === 'development') {
      console.log(`[DEBUG] ${message}`, data || '');
    }
  }

  static trace(message) {
    console.trace(message);
  }

  static performance(label, fn) {
    console.time(label);
    const result = fn();
    console.timeEnd(label);
    return result;
  }

  static memoryUsage() {
    if (performance.memory) {
      const memory = performance.memory;
      console.log('메모리 사용량:', {
        used: Math.round(memory.usedJSHeapSize / 1048576) + ' MB',
        total: Math.round(memory.totalJSHeapSize / 1048576) + ' MB',
        limit: Math.round(memory.jsHeapSizeLimit / 1048576) + ' MB',
      });
    }
  }
}

// 사용 예시
function complexCalculation(data) {
  DebugHelper.log('계산 시작', { dataLength: data.length });

  return DebugHelper.performance('복잡한 계산', () => {
    // debugger; // 브라우저에서 실행 중단점 설정

    const result = data.reduce((sum, item) => sum + item.value, 0);
    DebugHelper.log('계산 완료', { result });
    return result;
  });
}
```

---

## 코드 품질과 린터 활용

코드 품질 도구를 사용하면 런타임 에러를 미리 방지하고 일관된 코딩 스타일을 유지할 수 있습니다.

### ESLint 설정 예시

```javascript title=eslint-example.js
// ESLint 규칙을 따르는 좋은 예시
function calculateTotal(items, taxRate = 0.1) {
  // 입력 검증
  if (!Array.isArray(items)) {
    throw new Error('items는 배열이어야 합니다');
  }

  if (typeof taxRate !== 'number' || taxRate < 0) {
    throw new Error('taxRate는 0 이상의 숫자여야 합니다');
  }

  // 계산 로직
  const subtotal = items.reduce((sum, item) => {
    if (typeof item.price !== 'number' || item.price < 0) {
      throw new ValidationError(`잘못된 가격: ${item.price}`);
    }
    return sum + item.price;
  }, 0);

  const tax = subtotal * taxRate;
  const total = subtotal + tax;

  return {
    subtotal: Math.round(subtotal * 100) / 100,
    tax: Math.round(tax * 100) / 100,
    total: Math.round(total * 100) / 100,
  };
}

// 타입 체크 유틸리티
function isValidItem(item) {
  return item && typeof item === 'object' && typeof item.price === 'number' && item.price >= 0;
}
```

---

## 실습: 안전한 JSON 파서 만들기

JSON 데이터를 안전하게 파싱하고 검증하는 유틸리티를 만들어보겠습니다. 이 실습을 통해 에러 처리와 데이터 검증 기법을 종합적으로 활용해볼 수 있습니다.

```javascript title=safe-json-parser.js
class SafeJSONParser {
  static parse(jsonString, options = {}) {
    const { defaultValue = null, required = [], schema = null } = options;

    try {
      // JSON 파싱
      if (typeof jsonString !== 'string') {
        throw new ValidationError('입력값은 문자열이어야 합니다');
      }

      const data = JSON.parse(jsonString);

      // 필수 필드 검증
      this.validateRequired(data, required);

      // 스키마 검증
      if (schema) {
        this.validateSchema(data, schema);
      }

      return { success: true, data, error: null };
    } catch (error) {
      console.error('JSON 파싱 실패:', error.message);
      return { success: false, data: defaultValue, error: error.message };
    }
  }

  static validateRequired(data, required) {
    for (const field of required) {
      if (!(field in data)) {
        throw new ValidationError(`필수 필드 누락: ${field}`);
      }
    }
  }

  static validateSchema(data, schema) {
    for (const [key, validator] of Object.entries(schema)) {
      if (key in data) {
        if (!validator(data[key])) {
          throw new ValidationError(`스키마 검증 실패: ${key}`);
        }
      }
    }
  }

  static stringify(data, space = 2) {
    try {
      return {
        success: true,
        result: JSON.stringify(data, null, space),
        error: null,
      };
    } catch (error) {
      return {
        success: false,
        result: null,
        error: error.message,
      };
    }
  }
}

// 사용 예시
const jsonData = '{"name": "김철수", "age": 25, "email": "kim@example.com"}';
const invalidJson = '{"name": "김철수", "age": }';

// 스키마 정의
const userSchema = {
  name: value => typeof value === 'string' && value.length > 0,
  age: value => typeof value === 'number' && value > 0,
  email: value => typeof value === 'string' && value.includes('@'),
};

// 정상 데이터 파싱
const result1 = SafeJSONParser.parse(jsonData, {
  required: ['name', 'email'],
  schema: userSchema,
});

console.log('파싱 결과 1:', result1);

// 잘못된 JSON 파싱
const result2 = SafeJSONParser.parse(invalidJson, {
  defaultValue: { name: '알 수 없음', age: 0 },
});

console.log('파싱 결과 2:', result2);
```

---

## 실습: 디버깅 시나리오 해결하기

실제 개발에서 자주 발생하는 디버깅 시나리오를 통해 문제 해결 능력을 기르어보겠습니다.

```javascript title=debugging-scenarios.js
// 시나리오 1: 비동기 처리에서의 에러
class DataProcessor {
  static async processUserData(userId) {
    try {
      console.log(`사용자 ${userId} 데이터 처리 시작`);

      // 사용자 데이터 가져오기 (시뮬레이션)
      const userData = await this.fetchUserData(userId);

      // 데이터 검증
      this.validateUserData(userData);

      // 데이터 변환
      const processedData = this.transformData(userData);

      console.log('데이터 처리 완료:', processedData);
      return processedData;
    } catch (error) {
      // 에러 타입별 처리
      if (error instanceof ValidationError) {
        console.error('검증 오류:', error.message);
      } else if (error.name === 'NetworkError') {
        console.error('네트워크 오류:', error.message);
      } else {
        console.error('알 수 없는 오류:', error);
      }

      // 에러 리포팅 (실제 환경에서는 모니터링 서비스로 전송)
      this.reportError(error, { userId });

      throw error; // 상위로 에러 전파
    }
  }

  static async fetchUserData(userId) {
    // API 호출 시뮬레이션
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        if (userId === 'invalid') {
          reject(new Error('사용자를 찾을 수 없습니다'));
        } else {
          resolve({
            id: userId,
            name: '김철수',
            email: 'kim@example.com',
            age: 25,
          });
        }
      }, 100);
    });
  }

  static validateUserData(userData) {
    if (!userData.name) {
      throw new ValidationError('사용자 이름이 없습니다');
    }

    if (!userData.email) {
      throw new ValidationError('이메일이 없습니다');
    }
  }

  static transformData(userData) {
    return {
      ...userData,
      displayName: userData.name.toUpperCase(),
      emailDomain: userData.email.split('@')[1],
      isAdult: userData.age >= 18,
    };
  }

  static reportError(error, context) {
    // 에러 로깅 및 모니터링
    console.log('에러 리포트:', {
      message: error.message,
      stack: error.stack,
      context,
      timestamp: new Date().toISOString(),
    });
  }
}

// 테스트 실행
async function runTests() {
  console.log('=== 디버깅 시나리오 테스트 ===');

  // 정상 케이스
  try {
    await DataProcessor.processUserData('user123');
  } catch (error) {
    console.log('예상된 에러 처리 완료');
  }

  // 에러 케이스
  try {
    await DataProcessor.processUserData('invalid');
  } catch (error) {
    console.log('에러 케이스 처리 완료');
  }
}

runTests();
```

이 장에서는 자바스크립트의 에러 처리 메커니즘과 디버깅 기법을 배웠습니다. try...catch...finally를 활용한 에러 처리, 커스텀 에러 클래스 생성, 다양한 디버깅 도구 활용법까지 실전에서 바로 활용할 수 있는 기술들을 익혔습니다. 이러한 기법들을 통해 더 안정적이고 유지보수가 쉬운 코드를 작성할 수 있게 될 것입니다.
