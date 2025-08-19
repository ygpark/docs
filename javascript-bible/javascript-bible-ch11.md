---
title: '11장: 모듈 시스템'
slug: javascript-bible-module-system
description: 'ES6 모듈, 동적 import, 모듈 번들러, npm 패키지 관리를 통해 모던 자바스크립트 프로젝트 구조를 배워보세요'
keywords:
  [
    'JavaScript 모듈',
    'ES6 모듈',
    'import export',
    '동적 import',
    'npm',
    '패키지 관리',
    '모듈 번들러',
    '코드 스플리팅',
    '모던 프로젝트 구조',
  ]
sidebar_position: 11
---

# 11장: 모듈 시스템

모던 자바스크립트 개발에서 모듈 시스템은 필수적인 요소입니다. 코드를 재사용 가능한 작은 단위로 분리하고, 각 모듈의 역할을 명확히 하여 유지보수성을 높이는 것이 모듈 시스템의 핵심입니다. 이번 장에서는 ES6에서 도입된 표준 모듈 문법부터 최신 동적 import, 그리고 실제 프로젝트에서 활용되는 패키지 관리까지 모두 다뤄보겠습니다.

**학습 목표:**

- ES6 모듈의 import/export 문법을 완전히 익히기
- 동적 import를 활용한 코드 스플리팅 이해하기
- npm을 통한 효율적인 패키지 관리 방법 습득하기
- 모던 프로젝트 구조 설계 원칙 파악하기

---

## ES6 모듈 기초

ES6에서 도입된 모듈 시스템은 자바스크립트에 공식적인 모듈 기능을 제공합니다. 이전에는 AMD, CommonJS 등 다양한 모듈 시스템이 혼재했지만, 이제는 표준화된 문법을 사용할 수 있습니다.

### 기본 Export와 Import

모듈에서 다른 파일로 함수나 변수를 내보내는 방법을 알아보겠습니다.

```javascript title="math.js"
// Named Export
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export const PI = 3.14159;

// 여러 개를 한번에 export
function subtract(a, b) {
  return a - b;
}

function divide(a, b) {
  return a / b;
}

export { subtract, divide };
```

```javascript title="main.js"
// Named Import
import { add, multiply, PI } from './math.js';

console.log(add(5, 3)); // 8
console.log(multiply(4, 2)); // 8
console.log(PI); // 3.14159

// 별명 사용
import { subtract as minus, divide } from './math.js';
console.log(minus(10, 3)); // 7

// 모든 export를 한번에 가져오기
import * as math from './math.js';
console.log(math.add(2, 3)); // 5
```

### Default Export

하나의 주요 기능을 내보낼 때는 default export를 사용합니다.

```javascript title="calculator.js"
// Default Export
export default class Calculator {
  constructor() {
    this.result = 0;
  }

  add(num) {
    this.result += num;
    return this;
  }

  multiply(num) {
    this.result *= num;
    return this;
  }

  getResult() {
    return this.result;
  }

  reset() {
    this.result = 0;
    return this;
  }
}

// Named export와 함께 사용 가능
export const VERSION = '1.0.0';
```

```javascript title="app.js"
// Default Import
import Calculator from './calculator.js';
import { VERSION } from './calculator.js';

const calc = new Calculator();
const result = calc.add(10).multiply(2).getResult();
console.log(result); // 20
console.log(`Calculator v${VERSION}`); // Calculator v1.0.0
```

### 모듈 재사용 예제

온라인 쇼핑몰의 상품 관리 시스템을 모듈로 구현해보겠습니다. 이 예제는 실제 프로젝트에서 모듈을 어떻게 활용하는지 보여줍니다.

```javascript title="product.js"
// 상품 클래스
export default class Product {
  constructor(id, name, price, category) {
    this.id = id;
    this.name = name;
    this.price = price;
    this.category = category;
  }

  getDiscountedPrice(discountRate) {
    return this.price * (1 - discountRate);
  }

  toString() {
    return `${this.name} - ₩${this.price.toLocaleString()}`;
  }
}

// 상품 카테고리 상수
export const CATEGORIES = {
  ELECTRONICS: 'electronics',
  CLOTHING: 'clothing',
  BOOKS: 'books',
  HOME: 'home',
};
```

```javascript title="productManager.js"
import Product, { CATEGORIES } from './product.js';

export class ProductManager {
  constructor() {
    this.products = [];
  }

  addProduct(name, price, category) {
    const id = Date.now().toString();
    const product = new Product(id, name, price, category);
    this.products.push(product);
    return product;
  }

  getProductsByCategory(category) {
    return this.products.filter(p => p.category === category);
  }

  getTotalValue() {
    return this.products.reduce((sum, p) => sum + p.price, 0);
  }
}

// 유틸리티 함수들
export function formatPrice(price) {
  return `₩${price.toLocaleString()}`;
}

export function calculateTax(price, taxRate = 0.1) {
  return price * taxRate;
}
```

```javascript title="main.js"
import Product, { CATEGORIES } from './product.js';
import { ProductManager, formatPrice, calculateTax } from './productManager.js';

// 상품 관리자 생성
const manager = new ProductManager();

// 상품 추가
manager.addProduct('노트북', 1500000, CATEGORIES.ELECTRONICS);
manager.addProduct('셔츠', 50000, CATEGORIES.CLOTHING);
manager.addProduct('자바스크립트 책', 35000, CATEGORIES.BOOKS);

// 전자제품만 조회
const electronics = manager.getProductsByCategory(CATEGORIES.ELECTRONICS);
console.log('전자제품:', electronics);

// 총 가격과 세금 계산
const totalValue = manager.getTotalValue();
const tax = calculateTax(totalValue);

console.log(`총 상품 가치: ${formatPrice(totalValue)}`);
console.log(`세금: ${formatPrice(tax)}`);
```

---

## 동적 Import와 코드 스플리팅

동적 import는 런타임에 모듈을 비동기적으로 로드할 수 있게 해주는 기능입니다. 이를 통해 초기 번들 크기를 줄이고 필요할 때만 코드를 로드하는 코드 스플리팅을 구현할 수 있습니다.

### 동적 Import 기본 사용법

```javascript title="heavyModule.js"
// 무거운 라이브러리나 기능을 시뮬레이션
export function processLargeData(data) {
  console.log('대용량 데이터 처리 중...');
  // 복잡한 처리 로직
  return data.map(item => item * 2).filter(item => item > 100);
}

export function generateReport(data) {
  console.log('리포트 생성 중...');
  return {
    total: data.length,
    average: data.reduce((a, b) => a + b, 0) / data.length,
    max: Math.max(...data),
    min: Math.min(...data),
  };
}

export default {
  processLargeData,
  generateReport,
};
```

```javascript title="app.js"
// 사용자가 특정 기능을 요청할 때만 모듈을 로드
class DataProcessor {
  constructor() {
    this.heavyModule = null;
  }

  async loadHeavyModule() {
    if (!this.heavyModule) {
      console.log('무거운 모듈 로딩 중...');
      // 동적으로 모듈 import
      this.heavyModule = await import('./heavyModule.js');
      console.log('모듈 로딩 완료!');
    }
    return this.heavyModule;
  }

  async processData(data) {
    const module = await this.loadHeavyModule();
    return module.processLargeData(data);
  }

  async createReport(data) {
    const module = await this.loadHeavyModule();
    return module.generateReport(data);
  }
}

// 사용 예제
const processor = new DataProcessor();
const sampleData = Array.from({ length: 100 }, (_, i) => i + 1);

// 버튼 클릭 등 사용자 액션에 의해 실행
document.getElementById('processBtn')?.addEventListener('click', async () => {
  try {
    const result = await processor.processData(sampleData);
    console.log('처리 결과:', result);

    const report = await processor.createReport(result);
    console.log('리포트:', report);
  } catch (error) {
    console.error('처리 중 오류:', error);
  }
});
```

### 조건부 모듈 로딩

환경이나 조건에 따라 다른 모듈을 로드하는 예제입니다.

```javascript title="theme.js"
// 테마 관련 유틸리티
export async function loadTheme(themeName) {
  try {
    let themeModule;

    switch (themeName) {
      case 'dark':
        themeModule = await import('./themes/darkTheme.js');
        break;
      case 'light':
        themeModule = await import('./themes/lightTheme.js');
        break;
      case 'blue':
        themeModule = await import('./themes/blueTheme.js');
        break;
      default:
        themeModule = await import('./themes/defaultTheme.js');
    }

    return themeModule.default;
  } catch (error) {
    console.error(`테마 로딩 실패: ${themeName}`, error);
    // 기본 테마로 fallback
    const defaultTheme = await import('./themes/defaultTheme.js');
    return defaultTheme.default;
  }
}

export function applyTheme(theme) {
  document.documentElement.style.setProperty('--primary-color', theme.primaryColor);
  document.documentElement.style.setProperty('--background-color', theme.backgroundColor);
  document.documentElement.style.setProperty('--text-color', theme.textColor);
}
```

---

## npm과 패키지 관리

npm(Node Package Manager)은 자바스크립트 패키지를 관리하는 도구입니다. 외부 라이브러리를 쉽게 설치하고 관리할 수 있으며, 프로젝트의 의존성을 체계적으로 관리할 수 있습니다.

### package.json 설정

```json title="package.json"
{
  "name": "my-awesome-project",
  "version": "1.0.0",
  "description": "모던 자바스크립트 프로젝트",
  "main": "index.js",
  "type": "module",
  "scripts": {
    "start": "node index.js",
    "dev": "node --watch index.js",
    "build": "vite build",
    "test": "jest"
  },
  "dependencies": {
    "lodash": "^4.17.21",
    "axios": "^1.4.0"
  },
  "devDependencies": {
    "vite": "^4.4.0",
    "jest": "^29.6.0",
    "@types/lodash": "^4.14.195"
  },
  "keywords": ["javascript", "modern", "es6"],
  "author": "Your Name",
  "license": "MIT"
}
```

### 외부 라이브러리 활용 예제

날짜 처리와 HTTP 요청을 위한 외부 라이브러리를 활용한 예제입니다.

```javascript title="userService.js"
// 외부 라이브러리 import
import axios from 'axios';
import _ from 'lodash';

export class UserService {
  constructor(baseURL = 'https://jsonplaceholder.typicode.com') {
    this.api = axios.create({
      baseURL,
      timeout: 5000,
      headers: {
        'Content-Type': 'application/json',
      },
    });
  }

  async getUsers() {
    try {
      const response = await this.api.get('/users');
      return response.data;
    } catch (error) {
      console.error('사용자 목록 조회 실패:', error);
      throw new Error('사용자 데이터를 가져올 수 없습니다.');
    }
  }

  async getUsersByCity(city) {
    const users = await this.getUsers();
    return users.filter(user => user.address.city.toLowerCase() === city.toLowerCase());
  }

  async searchUsers(searchTerm) {
    const users = await this.getUsers();

    // lodash의 debounce 함수 활용
    const debouncedSearch = _.debounce(term => {
      return users.filter(
        user =>
          user.name.toLowerCase().includes(term.toLowerCase()) ||
          user.email.toLowerCase().includes(term.toLowerCase())
      );
    }, 300);

    return debouncedSearch(searchTerm);
  }

  groupUsersByCompany(users) {
    // lodash의 groupBy 함수 활용
    return _.groupBy(users, 'company.name');
  }
}

// 캐시 기능이 있는 사용자 서비스
export class CachedUserService extends UserService {
  constructor(baseURL) {
    super(baseURL);
    this.cache = new Map();
    this.cacheExpiry = 5 * 60 * 1000; // 5분
  }

  async getUsers() {
    const cacheKey = 'all_users';
    const cached = this.cache.get(cacheKey);

    if (cached && Date.now() - cached.timestamp < this.cacheExpiry) {
      console.log('캐시에서 사용자 데이터 반환');
      return cached.data;
    }

    const users = await super.getUsers();
    this.cache.set(cacheKey, {
      data: users,
      timestamp: Date.now(),
    });

    return users;
  }
}
```

```javascript title="main.js"
import { UserService, CachedUserService } from './userService.js';

async function demonstrateUserService() {
  const userService = new CachedUserService();

  try {
    // 사용자 목록 조회
    console.log('=== 전체 사용자 조회 ===');
    const users = await userService.getUsers();
    console.log(`총 ${users.length}명의 사용자`);

    // 도시별 사용자 조회
    console.log('\n=== 도시별 사용자 ===');
    const gwangjuUsers = await userService.getUsersByCity('Gwangju');
    console.log('광주 거주 사용자:', gwangjuUsers.length);

    // 사용자 검색
    console.log('\n=== 사용자 검색 ===');
    const searchResults = await userService.searchUsers('Bret');
    console.log('검색 결과:', searchResults);

    // 회사별 그룹화
    console.log('\n=== 회사별 그룹화 ===');
    const groupedUsers = userService.groupUsersByCompany(users);
    console.log('회사별 사용자 수:');
    Object.entries(groupedUsers).forEach(([company, users]) => {
      console.log(`  ${company}: ${users.length}명`);
    });
  } catch (error) {
    console.error('서비스 실행 중 오류:', error.message);
  }
}

// 실행
demonstrateUserService();
```

---

## 모던 프로젝트 구조

대규모 프로젝트에서는 체계적인 폴더 구조와 모듈 구성이 중요합니다. 유지보수성과 확장성을 고려한 프로젝트 구조를 살펴보겠습니다.

### 추천 프로젝트 구조

```
my-project/
├── src/
│   ├── components/        # 재사용 가능한 컴포넌트
│   │   ├── Button/
│   │   │   ├── Button.js
│   │   │   ├── Button.css
│   │   │   └── index.js   # re-export
│   │   └── Modal/
│   ├── services/          # API 통신, 비즈니스 로직
│   │   ├── userService.js
│   │   ├── productService.js
│   │   └── index.js
│   ├── utils/            # 유틸리티 함수들
│   │   ├── formatters.js
│   │   ├── validators.js
│   │   └── index.js
│   ├── config/           # 설정 파일들
│   │   ├── api.js
│   │   └── constants.js
│   └── main.js           # 진입점
├── public/               # 정적 파일들
├── tests/               # 테스트 파일들
├── package.json
└── README.md
```

### 배럴 Export 패턴

모듈을 그룹화하여 export하는 배럴(barrel) 패턴을 활용해보겠습니다.

```javascript title="src/utils/formatters.js"
export function formatCurrency(amount, currency = 'KRW') {
  return new Intl.NumberFormat('ko-KR', {
    style: 'currency',
    currency: currency,
  }).format(amount);
}

export function formatDate(date, format = 'short') {
  return new Intl.DateTimeFormat('ko-KR', {
    dateStyle: format,
  }).format(new Date(date));
}

export function formatNumber(number) {
  return new Intl.NumberFormat('ko-KR').format(number);
}
```

```javascript title="src/utils/validators.js"
export function isEmail(email) {
  const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
  return emailRegex.test(email);
}

export function isPhoneNumber(phone) {
  const phoneRegex = /^01[016789]-?\d{3,4}-?\d{4}$/;
  return phoneRegex.test(phone);
}

export function isRequired(value) {
  return value !== null && value !== undefined && value.toString().trim() !== '';
}

export function minLength(value, min) {
  return value && value.length >= min;
}
```

```javascript title="src/utils/index.js"
// 배럴 export - 모든 유틸리티를 한 곳에서 export
export * from './formatters.js';
export * from './validators.js';

// 기본 export도 함께 제공
export { formatCurrency as currency } from './formatters.js';
export { isEmail as validateEmail } from './validators.js';
```

```javascript title="src/main.js"
// 모든 유틸리티를 한번에 import
import {
  formatCurrency,
  formatDate,
  isEmail,
  isRequired,
  currency,
  validateEmail,
} from './utils/index.js';

// 사용 예제
const price = 29900;
const date = new Date();
const email = 'user@example.com';

console.log(formatCurrency(price)); // ₩29,900
console.log(currency(price)); // 별명으로 사용
console.log(formatDate(date)); // 2024. 8. 20.
console.log(isEmail(email)); // true
console.log(validateEmail(email)); // 별명으로 사용
```

### 환경별 설정 관리

개발, 테스트, 프로덕션 환경별로 다른 설정을 관리하는 방법입니다.

```javascript title="src/config/environments.js"
const environments = {
  development: {
    API_BASE_URL: 'http://localhost:3000/api',
    DEBUG: true,
    CACHE_ENABLED: false,
  },

  production: {
    API_BASE_URL: 'https://api.myapp.com',
    DEBUG: false,
    CACHE_ENABLED: true,
  },

  test: {
    API_BASE_URL: 'http://localhost:3001/api',
    DEBUG: true,
    CACHE_ENABLED: false,
  },
};

const currentEnv = process.env.NODE_ENV || 'development';

export const config = environments[currentEnv];
export const isDevelopment = currentEnv === 'development';
export const isProduction = currentEnv === 'production';
export const isTest = currentEnv === 'test';
```

## 마무리

이번 장에서는 모던 자바스크립트의 모듈 시스템을 종합적으로 살펴보았습니다. ES6 모듈의 기본 문법부터 동적 import를 활용한 코드 스플리팅, npm을 통한 패키지 관리, 그리고 실제 프로젝트에서 적용할 수 있는 구조화 방법까지 다뤘습니다.

모듈 시스템을 제대로 활용하면 코드의 재사용성이 높아지고, 팀 개발에서의 협업이 원활해지며, 프로젝트의 유지보수성이 크게 향상됩니다. 특히 동적 import를 통한 성능 최적화와 적절한 프로젝트 구조는 대규모 애플리케이션 개발에서 필수적인 기술입니다.

다음 장에서는 브라우저 환경에서의 DOM 조작과 이벤트 처리에 대해 알아보겠습니다. 지금까지 배운 모듈 시스템을 활용하여 더욱 체계적인 웹 애플리케이션을 만들어보세요!
