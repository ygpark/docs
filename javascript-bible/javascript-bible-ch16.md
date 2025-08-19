---
title: '16장: 현대 자바스크립트 생태계'
slug: javascript-bible-modern-ecosystem
description: '현대 자바스크립트 생태계를 탐험해보세요. TypeScript, 프레임워크, 테스팅, 성능 최적화, 그리고 최신 런타임까지 다루는 포괄적인 가이드입니다.'
keywords:
  [
    'TypeScript',
    '자바스크립트 프레임워크',
    'React',
    'Vue',
    'Svelte',
    '테스팅',
    'Jest',
    'Vitest',
    '성능 최적화',
    'Deno',
    'Bun',
    '현대 웹 개발',
    '자바스크립트 생태계',
  ]
sidebar_position: 16
---

# 16장: 현대 자바스크립트 생태계

자바스크립트는 단순한 웹 페이지 스크립팅 언어에서 시작해 현재는 프론트엔드, 백엔드, 모바일, 데스크톱 앱 개발까지 가능한 범용 언어로 발전했습니다. 이 장에서는 현대 자바스크립트 개발에 필수적인 도구들과 기술들을 살펴보고, 여러분이 앞으로 나아갈 방향을 제시해드리겠습니다. 복잡해 보일 수 있지만, 차근차근 따라가다 보면 현대 웹 개발의 전체적인 그림을 그릴 수 있을 거예요.

---

## TypeScript 소개

TypeScript는 자바스크립트에 타입 시스템을 추가한 언어입니다. 코드를 작성할 때 변수나 함수의 타입을 명시함으로써 개발 과정에서 오류를 미리 찾아낼 수 있어요. 마치 자바스크립트에 안전장치를 더한 것과 같습니다.

### TypeScript의 기본 문법

간단한 TypeScript 코드를 통해 자바스크립트와의 차이점을 알아봅시다.

```typescript title="basic-typescript.ts"
// 기본 타입 선언
let name: string = '김개발';
let age: number = 25;
let isStudent: boolean = true;

// 함수 타입 정의
function greet(name: string): string {
  return `안녕하세요, ${name}님!`;
}

// 인터페이스 정의
interface User {
  id: number;
  name: string;
  email?: string; // 선택적 속성
}

// 객체 타입 활용
const user: User = {
  id: 1,
  name: '이개발자',
};

console.log(greet(user.name));
```

### 실용적인 TypeScript 예제

실제 프로젝트에서 자주 사용되는 TypeScript 패턴들을 살펴보겠습니다.

```typescript title="practical-typescript.ts"
// 유니온 타입과 타입 가드
type Status = 'loading' | 'success' | 'error';

interface ApiResponse<T> {
  status: Status;
  data?: T;
  message?: string;
}

// 제네릭 함수
function fetchData<T>(url: string): Promise<ApiResponse<T>> {
  return fetch(url)
    .then(response => response.json())
    .then(data => ({
      status: 'success' as Status,
      data,
    }))
    .catch(error => ({
      status: 'error' as Status,
      message: error.message,
    }));
}

// 사용 예시
interface UserData {
  id: number;
  username: string;
}

fetchData<UserData>('/api/user/1').then(response => {
  if (response.status === 'success' && response.data) {
    console.log(`사용자: ${response.data.username}`);
  }
});
```

---

## 프레임워크 개요

현대 웹 개발에서는 다양한 프레임워크와 라이브러리가 사용됩니다. 각각의 특징과 장단점을 이해하고 프로젝트에 맞는 선택을 하는 것이 중요해요.

### React 기본 개념

React는 컴포넌트 기반의 사용자 인터페이스 라이브러리입니다. 재사용 가능한 컴포넌트를 만들어 복잡한 UI를 구성할 수 있어요.

```jsx title="react-component.jsx"
import React, { useState, useEffect } from 'react';

// 함수형 컴포넌트와 훅 사용
function TodoApp() {
  const [todos, setTodos] = useState([]);
  const [inputValue, setInputValue] = useState('');

  // 할 일 추가
  const addTodo = () => {
    if (inputValue.trim()) {
      setTodos([
        ...todos,
        {
          id: Date.now(),
          text: inputValue,
          completed: false,
        },
      ]);
      setInputValue('');
    }
  };

  // 완료 상태 토글
  const toggleTodo = id => {
    setTodos(todos.map(todo => (todo.id === id ? { ...todo, completed: !todo.completed } : todo)));
  };

  return (
    <div className='max-w-md mx-auto p-6 bg-white rounded-lg shadow-lg'>
      <h1 className='text-2xl font-bold mb-4'>할 일 목록</h1>

      <div className='flex mb-4'>
        <input
          type='text'
          value={inputValue}
          onChange={e => setInputValue(e.target.value)}
          className='flex-1 px-3 py-2 border rounded-l-md'
          placeholder='새 할 일 입력'
        />
        <button
          onClick={addTodo}
          className='px-4 py-2 bg-blue-500 text-white rounded-r-md hover:bg-blue-600'
        >
          추가
        </button>
      </div>

      <ul className='space-y-2'>
        {todos.map(todo => (
          <li
            key={todo.id}
            onClick={() => toggleTodo(todo.id)}
            className={`p-2 rounded cursor-pointer ${
              todo.completed
                ? 'bg-green-100 line-through text-gray-500'
                : 'bg-gray-100 hover:bg-gray-200'
            }`}
          >
            {todo.text}
          </li>
        ))}
      </ul>
    </div>
  );
}

export default TodoApp;
```

### Vue.js 기본 구조

Vue.js는 점진적으로 도입할 수 있는 프레임워크로, 템플릿 기반의 직관적인 문법을 제공합니다.

```vue title="vue-component.vue"
<template>
  <div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-lg">
    <h1 class="text-2xl font-bold mb-4">{{ title }}</h1>

    <div class="flex mb-4">
      <input
        v-model="newItem"
        @keyup.enter="addItem"
        class="flex-1 px-3 py-2 border rounded-l-md"
        placeholder="새 항목 입력"
      />
      <button
        @click="addItem"
        class="px-4 py-2 bg-green-500 text-white rounded-r-md hover:bg-green-600"
      >
        추가
      </button>
    </div>

    <ul class="space-y-2">
      <li
        v-for="item in items"
        :key="item.id"
        @click="toggleItem(item.id)"
        :class="[
          'p-2 rounded cursor-pointer transition-colors',
          item.completed
            ? 'bg-green-100 line-through text-gray-500'
            : 'bg-gray-100 hover:bg-gray-200',
        ]"
      >
        {{ item.text }}
      </li>
    </ul>
  </div>
</template>

<script>
export default {
  name: 'ItemList',
  data() {
    return {
      title: '목록 관리',
      newItem: '',
      items: [],
    };
  },
  methods: {
    addItem() {
      if (this.newItem.trim()) {
        this.items.push({
          id: Date.now(),
          text: this.newItem,
          completed: false,
        });
        this.newItem = '';
      }
    },
    toggleItem(id) {
      const item = this.items.find(item => item.id === id);
      if (item) {
        item.completed = !item.completed;
      }
    },
  },
};
</script>
```

### Svelte 맛보기

Svelte는 컴파일 타임에 최적화되는 혁신적인 프레임워크입니다. 런타임 오버헤드가 거의 없어 빠른 성능을 제공해요.

```svelte title="svelte-component.svelte"
<script>
  let title = "간단한 카운터";
  let count = 0;
  let history = [];

  function increment() {
    count += 1;
    history = [...history, { action: '증가', value: count, time: new Date() }];
  }

  function decrement() {
    count -= 1;
    history = [...history, { action: '감소', value: count, time: new Date() }];
  }

  function reset() {
    count = 0;
    history = [...history, { action: '리셋', value: count, time: new Date() }];
  }

  // 반응형 선언
  $: isEven = count % 2 === 0;
  $: color = count > 10 ? 'text-red-500' : count < 0 ? 'text-blue-500' : 'text-gray-800';
</script>

<div class="max-w-md mx-auto p-6 bg-white rounded-lg shadow-lg">
  <h1 class="text-2xl font-bold mb-4">{title}</h1>

  <div class="text-center mb-6">
    <p class="text-4xl font-bold {color} mb-2">{count}</p>
    <p class="text-sm text-gray-600">
      현재 값은 {isEven ? '짝수' : '홀수'}입니다
    </p>
  </div>

  <div class="flex justify-center space-x-2 mb-4">
    <button
      on:click={decrement}
      class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600"
    >
      -1
    </button>
    <button
      on:click={reset}
      class="px-4 py-2 bg-gray-500 text-white rounded hover:bg-gray-600"
    >
      리셋
    </button>
    <button
      on:click={increment}
      class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
    >
      +1
    </button>
  </div>

  {#if history.length > 0}
    <div class="mt-4">
      <h3 class="font-semibold mb-2">최근 활동</h3>
      <div class="max-h-32 overflow-y-auto">
        {#each history.slice(-5) as entry}
          <p class="text-sm text-gray-600">
            {entry.action}: {entry.value} ({entry.time.toLocaleTimeString()})
          </p>
        {/each}
      </div>
    </div>
  {/if}
</div>
```

---

## 테스팅 기초

코드의 품질을 보장하고 버그를 줄이기 위해서는 테스트가 필수입니다. 자동화된 테스트를 통해 코드 변경 시에도 안정성을 유지할 수 있어요.

### Jest를 활용한 단위 테스트

Jest는 가장 인기 있는 자바스크립트 테스팅 프레임워크 중 하나입니다.

```javascript title="math-utils.js"
// 테스트할 함수들
export function add(a, b) {
  return a + b;
}

export function multiply(a, b) {
  return a * b;
}

export function divide(a, b) {
  if (b === 0) {
    throw new Error('0으로 나눌 수 없습니다');
  }
  return a / b;
}

export function factorial(n) {
  if (n < 0) {
    throw new Error('음수는 팩토리얼을 계산할 수 없습니다');
  }
  if (n === 0 || n === 1) {
    return 1;
  }
  return n * factorial(n - 1);
}

export function isPrime(num) {
  if (num < 2) return false;
  for (let i = 2; i <= Math.sqrt(num); i++) {
    if (num % i === 0) return false;
  }
  return true;
}
```

```javascript title="math-utils.test.js"
import { add, multiply, divide, factorial, isPrime } from './math-utils.js';

describe('수학 유틸리티 함수 테스트', () => {
  describe('add 함수', () => {
    test('두 양수를 더한다', () => {
      expect(add(2, 3)).toBe(5);
    });

    test('음수와 양수를 더한다', () => {
      expect(add(-1, 1)).toBe(0);
    });

    test('소수점 계산', () => {
      expect(add(0.1, 0.2)).toBeCloseTo(0.3);
    });
  });

  describe('divide 함수', () => {
    test('정상적인 나눗셈', () => {
      expect(divide(10, 2)).toBe(5);
    });

    test('0으로 나누면 에러 발생', () => {
      expect(() => divide(10, 0)).toThrow('0으로 나눌 수 없습니다');
    });
  });

  describe('factorial 함수', () => {
    test('5! = 120', () => {
      expect(factorial(5)).toBe(120);
    });

    test('0! = 1', () => {
      expect(factorial(0)).toBe(1);
    });

    test('음수는 에러 발생', () => {
      expect(() => factorial(-1)).toThrow();
    });
  });

  describe('isPrime 함수', () => {
    test('소수 판별', () => {
      expect(isPrime(2)).toBe(true);
      expect(isPrime(17)).toBe(true);
      expect(isPrime(97)).toBe(true);
    });

    test('합성수 판별', () => {
      expect(isPrime(4)).toBe(false);
      expect(isPrime(15)).toBe(false);
      expect(isPrime(100)).toBe(false);
    });

    test('경계값 테스트', () => {
      expect(isPrime(1)).toBe(false);
      expect(isPrime(0)).toBe(false);
      expect(isPrime(-5)).toBe(false);
    });
  });
});
```

### Vitest로 모던 테스팅

Vitest는 Vite 기반의 빠른 테스트 러너로, Jest와 호환되는 API를 제공하면서도 더 빠른 성능을 자랑합니다.

```javascript title="async-utils.test.js"
import { describe, it, expect, vi, beforeEach, afterEach } from 'vitest';

// 테스트할 비동기 함수들
async function fetchUserData(userId) {
  const response = await fetch(`/api/users/${userId}`);
  if (!response.ok) {
    throw new Error('사용자를 찾을 수 없습니다');
  }
  return response.json();
}

async function delay(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

describe('비동기 함수 테스트', () => {
  beforeEach(() => {
    // fetch를 모킹
    global.fetch = vi.fn();
  });

  afterEach(() => {
    vi.restoreAllMocks();
  });

  it('사용자 데이터를 성공적으로 가져온다', async () => {
    const mockUser = { id: 1, name: '김개발', email: 'dev@example.com' };

    fetch.mockResolvedValueOnce({
      ok: true,
      json: async () => mockUser,
    });

    const result = await fetchUserData(1);

    expect(fetch).toHaveBeenCalledWith('/api/users/1');
    expect(result).toEqual(mockUser);
  });

  it('사용자를 찾을 수 없으면 에러가 발생한다', async () => {
    fetch.mockResolvedValueOnce({
      ok: false,
      status: 404,
    });

    await expect(fetchUserData(999)).rejects.toThrow('사용자를 찾을 수 없습니다');
  });

  it('딜레이 함수 테스트', async () => {
    const start = Date.now();
    await delay(100);
    const end = Date.now();

    expect(end - start).toBeGreaterThanOrEqual(90); // 약간의 여유 허용
  });
});
```

---

## 성능 최적화 기법

웹 애플리케이션의 성능은 사용자 경험에 직접적인 영향을 미칩니다. 다양한 최적화 기법들을 알아보고 실제로 적용해봅시다.

### 코드 스플리팅과 지연 로딩

큰 애플리케이션을 작은 청크로 나누어 필요할 때만 로드하는 기법입니다.

```javascript title="lazy-loading.js"
// 동적 import를 활용한 코드 스플리팅
class ComponentLoader {
  constructor() {
    this.loadedComponents = new Map();
    this.loadingPromises = new Map();
  }

  async loadComponent(componentName) {
    // 이미 로드된 컴포넌트는 캐시에서 반환
    if (this.loadedComponents.has(componentName)) {
      return this.loadedComponents.get(componentName);
    }

    // 로딩 중인 컴포넌트는 기존 Promise 반환
    if (this.loadingPromises.has(componentName)) {
      return this.loadingPromises.get(componentName);
    }

    // 새로 로드하는 컴포넌트
    const loadingPromise = this.importComponent(componentName);
    this.loadingPromises.set(componentName, loadingPromise);

    try {
      const component = await loadingPromise;
      this.loadedComponents.set(componentName, component);
      this.loadingPromises.delete(componentName);
      return component;
    } catch (error) {
      this.loadingPromises.delete(componentName);
      throw error;
    }
  }

  async importComponent(componentName) {
    const componentMap = {
      UserProfile: () => import('./components/UserProfile.js'),
      Dashboard: () => import('./components/Dashboard.js'),
      Settings: () => import('./components/Settings.js'),
    };

    if (!componentMap[componentName]) {
      throw new Error(`컴포넌트 ${componentName}을 찾을 수 없습니다`);
    }

    const module = await componentMap[componentName]();
    return module.default;
  }

  // 미리 로드하기 (프리로딩)
  preloadComponent(componentName) {
    if (!this.loadedComponents.has(componentName) && !this.loadingPromises.has(componentName)) {
      this.loadComponent(componentName).catch(console.error);
    }
  }
}

// 사용 예시
const loader = new ComponentLoader();

// 라우터에서 사용
async function navigateToPage(pageName) {
  const loadingElement = document.getElementById('loading');
  const contentElement = document.getElementById('content');

  try {
    loadingElement.style.display = 'block';

    const Component = await loader.loadComponent(pageName);
    const instance = new Component();

    contentElement.innerHTML = '';
    contentElement.appendChild(instance.render());
  } catch (error) {
    console.error('페이지 로딩 실패:', error);
    contentElement.innerHTML = '<p>페이지를 불러올 수 없습니다.</p>';
  } finally {
    loadingElement.style.display = 'none';
  }
}
```

### 메모이제이션과 캐싱

계산 결과를 캐싱하여 중복 연산을 방지하는 최적화 기법입니다.

```javascript title="memoization.js"
// 메모이제이션 유틸리티
function memoize(fn, keyGenerator = (...args) => JSON.stringify(args)) {
  const cache = new Map();

  function memoized(...args) {
    const key = keyGenerator(...args);

    if (cache.has(key)) {
      console.log(`캐시 히트: ${key}`);
      return cache.get(key);
    }

    console.log(`계산 수행: ${key}`);
    const result = fn.apply(this, args);
    cache.set(key, result);

    return result;
  }

  // 캐시 관리 메서드들
  memoized.cache = cache;
  memoized.clear = () => cache.clear();
  memoized.delete = key => cache.delete(key);
  memoized.has = key => cache.has(key);

  return memoized;
}

// 피보나치 수열 - 메모이제이션 적용
const fibonacci = memoize(function (n) {
  if (n <= 1) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
});

// API 호출 캐싱
const fetchUserData = memoize(
  async function (userId) {
    console.log(`API 호출: 사용자 ${userId}`);
    const response = await fetch(`/api/users/${userId}`);
    return response.json();
  },
  userId => `user-${userId}` // 간단한 키 생성
);

// 사용 예시
console.log(fibonacci(40)); // 계산 수행
console.log(fibonacci(40)); // 캐시에서 반환

// TTL(Time To Live) 기능이 있는 캐시
class TTLCache {
  constructor(defaultTTL = 5000) {
    this.cache = new Map();
    this.timers = new Map();
    this.defaultTTL = defaultTTL;
  }

  set(key, value, ttl = this.defaultTTL) {
    // 기존 타이머 제거
    if (this.timers.has(key)) {
      clearTimeout(this.timers.get(key));
    }

    // 값 저장
    this.cache.set(key, value);

    // TTL 타이머 설정
    const timer = setTimeout(() => {
      this.cache.delete(key);
      this.timers.delete(key);
      console.log(`캐시 만료: ${key}`);
    }, ttl);

    this.timers.set(key, timer);
  }

  get(key) {
    return this.cache.get(key);
  }

  has(key) {
    return this.cache.has(key);
  }

  clear() {
    this.timers.forEach(timer => clearTimeout(timer));
    this.cache.clear();
    this.timers.clear();
  }
}
```

---

## 최신 런타임: Deno, Bun 소개

Node.js를 넘어서는 새로운 자바스크립트 런타임들이 등장하고 있습니다. 각각의 특징과 장점을 살펴보겠습니다.

### Deno 기본 사용법

Deno는 TypeScript를 기본 지원하고 보안을 중시하는 모던 런타임입니다.

```typescript title="deno-example.ts"
// Deno에서는 URL을 통해 모듈을 직접 import 가능
import { serve } from 'https://deno.land/std@0.200.0/http/server.ts';
import { parse } from 'https://deno.land/std@0.200.0/flags/mod.ts';

// 명령행 인수 파싱
const args = parse(Deno.args, {
  default: { port: 8000 },
  alias: { p: 'port' },
});

// 간단한 웹 서버
async function handler(req: Request): Promise<Response> {
  const url = new URL(req.url);

  // API 라우팅
  if (url.pathname === '/api/time') {
    return new Response(
      JSON.stringify({
        timestamp: Date.now(),
        date: new Date().toISOString(),
      }),
      {
        headers: { 'content-type': 'application/json' },
      }
    );
  }

  if (url.pathname === '/api/health') {
    return new Response(
      JSON.stringify({
        status: 'ok',
        uptime: performance.now(),
      }),
      {
        headers: { 'content-type': 'application/json' },
      }
    );
  }

  // 정적 파일 서빙 (보안 권한 필요)
  if (url.pathname === '/' || url.pathname === '/index.html') {
    try {
      const html = await Deno.readTextFile('./public/index.html');
      return new Response(html, {
        headers: { 'content-type': 'text/html' },
      });
    } catch {
      return new Response('파일을 찾을 수 없습니다', { status: 404 });
    }
  }

  return new Response('Not Found', { status: 404 });
}

console.log(`서버가 포트 ${args.port}에서 실행 중입니다...`);
await serve(handler, { port: args.port });

// 실행 명령: deno run --allow-net --allow-read deno-example.ts
```

### Bun의 고성능 특징

Bun은 극도로 빠른 성능을 자랑하는 올인원 자바스크립트 런타임입니다.

```javascript title="bun-example.js"
// Bun 내장 서버 (매우 빠름)
const server = Bun.serve({
  port: 3000,

  async fetch(req) {
    const url = new URL(req.url);

    // 빠른 파일 서빙
    if (url.pathname.startsWith('/static/')) {
      const filePath = `.${url.pathname}`;
      const file = Bun.file(filePath);

      if (await file.exists()) {
        return new Response(file);
      }
    }

    // API 엔드포인트
    if (url.pathname === '/api/benchmark') {
      const start = performance.now();

      // 무거운 계산 작업 시뮬레이션
      let result = 0;
      for (let i = 0; i < 1000000; i++) {
        result += Math.sqrt(i);
      }

      const end = performance.now();

      return Response.json({
        result: result,
        executionTime: `${(end - start).toFixed(2)}ms`,
        runtime: 'Bun',
      });
    }

    // WebSocket 업그레이드
    if (url.pathname === '/ws') {
      const success = server.upgrade(req);
      if (success) {
        return undefined;
      }
    }

    return new Response('Hello from Bun!', {
      headers: { 'content-type': 'text/plain' },
    });
  },

  // WebSocket 처리
  websocket: {
    message(ws, message) {
      console.log(`메시지 수신: ${message}`);
      ws.send(`에코: ${message}`);
    },

    open(ws) {
      console.log('WebSocket 연결됨');
      ws.send('연결이 성공했습니다!');
    },

    close(ws) {
      console.log('WebSocket 연결 종료');
    },
  },
});

console.log(`Bun 서버가 http://localhost:${server.port}에서 실행 중...`);

// 빠른 파일 I/O 예제
async function fileOperations() {
  const data = { users: [], posts: [], comments: [] };

  // 매우 빠른 JSON 파싱
  const configFile = Bun.file('./config.json');
  if (await configFile.exists()) {
    const config = await configFile.json();
    console.log('설정 로드됨:', config);
  }

  // 빠른 파일 쓰기
  await Bun.write('./output.json', JSON.stringify(data, null, 2));
  console.log('파일 저장 완료');
}

fileOperations().catch(console.error);
```

---

## 실습: 간단한 TypeScript 프로젝트

실제로 TypeScript를 사용하여 작은 프로젝트를 만들어봅시다. 간단한 작업 관리 시스템을 구현해보겠습니다.

```typescript title="task-manager.ts"
// 타입 정의
interface Task {
  id: string;
  title: string;
  description?: string;
  completed: boolean;
  priority: 'low' | 'medium' | 'high';
  createdAt: Date;
  dueDate?: Date;
}

interface TaskFilter {
  completed?: boolean;
  priority?: Task['priority'];
  search?: string;
}

// 작업 관리자 클래스
class TaskManager {
  private tasks: Task[] = [];
  private listeners: ((tasks: Task[]) => void)[] = [];

  // 작업 추가
  addTask(taskData: Omit<Task, 'id' | 'createdAt'>): Task {
    const task: Task = {
      id: crypto.randomUUID(),
      createdAt: new Date(),
      ...taskData,
    };

    this.tasks.push(task);
    this.notifyListeners();
    return task;
  }

  // 작업 완료 토글
  toggleTask(id: string): boolean {
    const task = this.tasks.find(t => t.id === id);
    if (task) {
      task.completed = !task.completed;
      this.notifyListeners();
      return true;
    }
    return false;
  }

  // 작업 삭제
  deleteTask(id: string): boolean {
    const index = this.tasks.findIndex(t => t.id === id);
    if (index !== -1) {
      this.tasks.splice(index, 1);
      this.notifyListeners();
      return true;
    }
    return false;
  }

  // 필터링된 작업 목록 반환
  getTasks(filter: TaskFilter = {}): Task[] {
    return this.tasks.filter(task => {
      if (filter.completed !== undefined && task.completed !== filter.completed) {
        return false;
      }

      if (filter.priority && task.priority !== filter.priority) {
        return false;
      }

      if (filter.search) {
        const searchLower = filter.search.toLowerCase();
        return (
          task.title.toLowerCase().includes(searchLower) ||
          task.description?.toLowerCase().includes(searchLower)
        );
      }

      return true;
    });
  }

  // 이벤트 리스너 등록
  onChange(listener: (tasks: Task[]) => void): () => void {
    this.listeners.push(listener);

    // 언등록 함수 반환
    return () => {
      const index = this.listeners.indexOf(listener);
      if (index !== -1) {
        this.listeners.splice(index, 1);
      }
    };
  }

  private notifyListeners(): void {
    this.listeners.forEach(listener => listener([...this.tasks]));
  }

  // 통계 정보
  getStats(): { total: number; completed: number; pending: number } {
    const completed = this.tasks.filter(t => t.completed).length;
    return {
      total: this.tasks.length,
      completed,
      pending: this.tasks.length - completed,
    };
  }
}
```

---

## 실습: 단위 테스트 작성해보기

위에서 만든 TaskManager를 위한 테스트를 작성해봅시다.

```typescript title="task-manager.test.ts"
import { describe, it, expect, beforeEach, vi } from 'vitest';
import { TaskManager } from './task-manager';

describe('TaskManager', () => {
  let taskManager: TaskManager;

  beforeEach(() => {
    taskManager = new TaskManager();
    // crypto.randomUUID 모킹
    vi.stubGlobal('crypto', {
      randomUUID: vi.fn(() => 'test-id-123'),
    });
  });

  describe('작업 추가', () => {
    it('새 작업을 추가할 수 있다', () => {
      const taskData = {
        title: '테스트 작업',
        description: '테스트 설명',
        completed: false,
        priority: 'medium' as const,
      };

      const task = taskManager.addTask(taskData);

      expect(task.id).toBe('test-id-123');
      expect(task.title).toBe('테스트 작업');
      expect(task.completed).toBe(false);
      expect(task.createdAt).toBeInstanceOf(Date);
    });

    it('작업 추가 시 리스너가 호출된다', () => {
      const listener = vi.fn();
      taskManager.onChange(listener);

      taskManager.addTask({
        title: '테스트',
        completed: false,
        priority: 'low',
      });

      expect(listener).toHaveBeenCalledTimes(1);
    });
  });

  describe('작업 토글', () => {
    it('작업 완료 상태를 토글할 수 있다', () => {
      const task = taskManager.addTask({
        title: '테스트 작업',
        completed: false,
        priority: 'high',
      });

      const result = taskManager.toggleTask(task.id);
      const tasks = taskManager.getTasks();

      expect(result).toBe(true);
      expect(tasks[0].completed).toBe(true);
    });

    it('존재하지 않는 작업은 토글할 수 없다', () => {
      const result = taskManager.toggleTask('nonexistent-id');
      expect(result).toBe(false);
    });
  });

  describe('작업 필터링', () => {
    beforeEach(() => {
      taskManager.addTask({
        title: '완료된 작업',
        completed: true,
        priority: 'high',
      });

      taskManager.addTask({
        title: '미완료 작업',
        description: '중요한 작업입니다',
        completed: false,
        priority: 'medium',
      });
    });

    it('완료된 작업만 필터링할 수 있다', () => {
      const completedTasks = taskManager.getTasks({ completed: true });

      expect(completedTasks).toHaveLength(1);
      expect(completedTasks[0].title).toBe('완료된 작업');
    });

    it('우선순위로 필터링할 수 있다', () => {
      const highPriorityTasks = taskManager.getTasks({ priority: 'high' });

      expect(highPriorityTasks).toHaveLength(1);
      expect(highPriorityTasks[0].priority).toBe('high');
    });

    it('검색어로 필터링할 수 있다', () => {
      const searchResults = taskManager.getTasks({ search: '중요' });

      expect(searchResults).toHaveLength(1);
      expect(searchResults[0].description).toContain('중요한');
    });
  });

  describe('통계', () => {
    it('작업 통계를 정확히 계산한다', () => {
      taskManager.addTask({ title: '작업1', completed: true, priority: 'low' });
      taskManager.addTask({ title: '작업2', completed: false, priority: 'medium' });
      taskManager.addTask({ title: '작업3', completed: false, priority: 'high' });

      const stats = taskManager.getStats();

      expect(stats).toEqual({
        total: 3,
        completed: 1,
        pending: 2,
      });
    });
  });
});
```

이번 장에서는 현대 자바스크립트 생태계의 핵심 요소들을 살펴봤습니다. TypeScript로 타입 안전성을 확보하고, 다양한 프레임워크의 특징을 이해하며, 테스팅을 통해 코드 품질을 보장하는 방법을 배웠어요. 또한 성능 최적화와 새로운 런타임들까지 경험해봤습니다.

이제 여러분은 현대 웹 개발의 전체적인 그림을 그릴 수 있고, 프로젝트에 맞는 도구와 기술을 선택할 수 있는 기초를 다졌습니다. 다음 장에서는 이 모든 지식을 바탕으로 미래를 위한 준비를 해보겠습니다.
