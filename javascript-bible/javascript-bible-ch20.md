---
title: '부록 B: 자바스크립트 레퍼런스'
slug: javascript-bible-reference-guide
description: '모던 자바스크립트의 핵심 문법, 내장 객체, 메서드를 간결하게 정리한 빠른 참조 가이드입니다.'
keywords:
  [
    '자바스크립트',
    'JavaScript',
    '레퍼런스',
    '내장객체',
    '메서드',
    '문법',
    'ES6',
    '모던자바스크립트',
    '치트시트',
  ]
sidebar_position: 20
---

# 부록 B: 자바스크립트 레퍼런스

---

## 1. 기본 데이터 타입

```javascript title="types.js"
// 원시 타입
const num = 42;
const str = '문자열';
const bool = true;
const undef = undefined;
const nullVal = null;
const bigint = 123n;
const sym = Symbol('id');

// 타입 확인
typeof value;
Array.isArray(value);
value instanceof Constructor;
```

---

## 2. 변수 선언

```javascript title="variables.js"
// 권장 방식
const 상수 = '변경불가';
let 변수 = '변경가능';

// 템플릿 리터럴
const message = `안녕 ${name}님`;

// 구조 분해
const { a, b } = obj;
const [x, y] = arr;
```

---

## 3. 연산자

```javascript title="operators.js"
// 비교
=== // 엄격한 비교
!== // 엄격한 불일치

// 최신 연산자
?? // Nullish coalescing
?.  // Optional chaining
||= // 논리 OR 할당
&&= // 논리 AND 할당
??= // Nullish 할당

// 전개 구문
[...arr1, ...arr2]
{...obj1, ...obj2}
```

---

## 4. 함수

```javascript title="functions.js"
// 함수 정의
function func() {}
const func = function() {};
const func = () => {};

// 매개변수
function(a, b = 기본값, ...rest) {}

// 고차 함수
const 곱하기 = x => y => x * y;
const 두배 = 곱하기(2);
```

---

## 5. 배열 메서드

```javascript title="array-methods.js"
// 변환
arr.map(item => 변환된값);
arr.filter(item => 조건);
arr.reduce((acc, item) => acc + item, 0);

// 검색
arr.find(item => 조건);
arr.findIndex(item => 조건);
arr.includes(값);
arr.indexOf(값);

// 최신 메서드
arr.at(-1); // 마지막 요소
arr.findLast(조건);
arr.toSorted();
arr.toReversed();
arr.with(index, 새값);

// 기타
arr.forEach(item => {});
arr.some(조건);
arr.every(조건);
```

---

## 6. 객체 메서드

```javascript title="object-methods.js"
// 속성 조작
Object.keys(obj)
Object.values(obj)
Object.entries(obj)
Object.assign(target, source)

// 생성/복사
Object.create(prototype)
{...obj} // 얕은 복사
structuredClone(obj) // 깊은 복사

// 검사
Object.hasOwnProperty(key)
Object.getOwnPropertyNames(obj)
Object.getPrototypeOf(obj)
```

---

## 7. 문자열 메서드

```javascript title="string-methods.js"
// 검색/확인
str.includes(substring);
str.startsWith(prefix);
str.endsWith(suffix);
str.indexOf(substring);

// 변환
str.toLowerCase();
str.toUpperCase();
str.trim();
str.replace(찾을값, 바꿀값);
str.replaceAll(찾을값, 바꿀값);

// 분할/결합
str.split(구분자);
arr.join(구분자);
str.slice(start, end);
str.substring(start, end);

// 패딩
str.padStart(길이, 문자);
str.padEnd(길이, 문자);
```

---

## 8. 숫자 메서드

```javascript title="number-methods.js"
// 파싱
Number(값);
parseInt(값, 진수);
parseFloat(값);

// 검사
Number.isNaN(값);
Number.isInteger(값);
Number.isFinite(값);

// 반올림
Math.round(값);
Math.floor(값);
Math.ceil(값);
값.toFixed(소수점자리);

// 기타
Math.max(...arr);
Math.min(...arr);
Math.random();
```

---

## 9. 날짜 (Date)

```javascript title="date-methods.js"
// 생성
new Date();
new Date('2024-01-01');
Date.now();

// 조회
date.getFullYear();
date.getMonth(); // 0부터 시작
date.getDate();
date.getDay(); // 요일 (0=일요일)

// 설정
date.setFullYear(년도);
date.setMonth(월);
date.setDate(일);

// 문자열 변환
date.toISOString();
date.toLocaleDateString('ko-KR');
date.toLocaleTimeString('ko-KR');
```

---

## 10. 정규표현식

```javascript title="regex.js"
// 생성
/패턴/플래그
new RegExp('패턴', '플래그')

// 플래그
/패턴/g // 전역 검색
/패턴/i // 대소문자 무시
/패턴/m // 멀티라인

// 메서드
regex.test(문자열) // 불린 반환
str.match(regex) // 매치 배열
str.replace(regex, 대체값)
str.split(regex)

// 자주 쓰는 패턴
/\d+/ // 숫자
/\w+/ // 단어 문자
/\s+/ // 공백
/^시작/
/끝$/
```

---

## 11. Promise와 비동기

```javascript title="async.js"
// Promise 생성
new Promise((resolve, reject) => {});

// 사용
promise.then(값 => {});
promise.catch(에러 => {});
promise.finally(() => {});

// async/await
async function 함수명() {
  try {
    const 결과 = await promise;
  } catch (에러) {}
}

// 조합
Promise.all([p1, p2, p3]);
Promise.allSettled([p1, p2, p3]);
Promise.race([p1, p2, p3]);
Promise.any([p1, p2, p3]);
```

---

## 12. 모듈

```javascript title="modules.js"
// 내보내기
export const 변수 = 값;
export function 함수() {}
export default 기본값;

// 가져오기
import { 변수, 함수 } from './파일.js';
import 기본값 from './파일.js';
import * as 별칭 from './파일.js';

// 동적 import
const 모듈 = await import('./파일.js');
```

---

## 13. DOM 조작

```javascript title="dom.js"
// 선택
document.querySelector('선택자');
document.querySelectorAll('선택자');
document.getElementById('id');

// 생성/조작
document.createElement('태그');
element.innerHTML = 'HTML';
element.textContent = '텍스트';
element.setAttribute('속성', '값');

// 클래스 조작
element.classList.add('클래스');
element.classList.remove('클래스');
element.classList.toggle('클래스');
element.classList.contains('클래스');

// 이벤트
element.addEventListener('이벤트', 핸들러);
element.removeEventListener('이벤트', 핸들러);
```

---

## 14. 이벤트

```javascript title="events.js"
// 마우스 이벤트
('click', 'dblclick', 'mousedown', 'mouseup');
('mouseover', 'mouseout', 'mousemove');

// 키보드 이벤트
('keydown', 'keyup', 'keypress');

// 폼 이벤트
('submit', 'change', 'input', 'focus', 'blur');

// 윈도우 이벤트
('load', 'resize', 'scroll', 'beforeunload');

// 이벤트 객체
event.preventDefault();
event.stopPropagation();
event.target;
event.currentTarget;
```

---

## 15. 에러 처리

```javascript title="errors.js"
// 기본 에러 처리
try {
  // 코드
} catch (error) {
  console.error(error.message);
} finally {
  // 정리 코드
}

// 커스텀 에러
class CustomError extends Error {
  constructor(message) {
    super(message);
    this.name = 'CustomError';
  }
}

throw new CustomError('에러 메시지');
```

---

## 16. JSON

```javascript title="json.js"
// 변환
JSON.stringify(객체); // 객체 → JSON 문자열
JSON.parse(JSON문자열); // JSON 문자열 → 객체

// 옵션
JSON.stringify(객체, null, 2); // 들여쓰기
JSON.stringify(객체, ['키1', '키2']); // 특정 속성만

// 안전한 파싱
function safeJsonParse(str, defaultValue = null) {
  try {
    return JSON.parse(str);
  } catch {
    return defaultValue;
  }
}
```

---

## 17. 스토리지

```javascript title="storage.js"
// 로컬 스토리지 (영구 저장)
localStorage.setItem('키', '값');
localStorage.getItem('키');
localStorage.removeItem('키');
localStorage.clear();

// 세션 스토리지 (탭 닫으면 삭제)
sessionStorage.setItem('키', '값');
sessionStorage.getItem('키');

// JSON 객체 저장
localStorage.setItem('키', JSON.stringify(객체));
JSON.parse(localStorage.getItem('키'));
```

---

## 18. Fetch API

```javascript title="fetch.js"
// 기본 GET
fetch('url').then(response => response.json());

// POST 요청
fetch('url', {
  method: 'POST',
  headers: {
    'Content-Type': 'application/json',
  },
  body: JSON.stringify(데이터),
});

// async/await 사용
async function fetchData() {
  const response = await fetch('url');
  const data = await response.json();
  return data;
}

// 에러 처리
if (!response.ok) {
  throw new Error(`HTTP ${response.status}`);
}
```

---

## 19. 클래스

```javascript title="classes.js"
// 기본 클래스
class MyClass {
  constructor(값) {
    this.속성 = 값;
  }

  메서드() {
    return this.속성;
  }

  static 정적메서드() {
    return '정적';
  }
}

// 상속
class 자식클래스 extends 부모클래스 {
  constructor(값) {
    super(값);
  }

  메서드() {
    return super.메서드() + ' 확장';
  }
}

// 프라이빗 필드
class PrivateClass {
  #private = '비공개';

  getPrivate() {
    return this.#private;
  }
}
```

---

## 20. Map과 Set

```javascript title="map-set.js"
// Map (키-값 쌍)
const map = new Map();
map.set('키', '값');
map.get('키');
map.has('키');
map.delete('키');
map.clear();
map.size;

// Map 순회
for (const [키, 값] of map) {
}

// Set (고유값 집합)
const set = new Set();
set.add('값');
set.has('값');
set.delete('값');
set.clear();
set.size;

// Set 순회
for (const 값 of set) {
}
```

---

## 21. 유용한 유틸리티

```javascript title="utils.js"
// 배열 중복 제거
[...new Set(배열)];

// 객체 깊은 복사
structuredClone(객체);

// 랜덤 정수
Math.floor(Math.random() * (최대 - 최소 + 1)) + 최소;

// 지연 함수
const delay = ms => new Promise(resolve => setTimeout(resolve, ms));

// 디바운스
function debounce(func, wait) {
  let timeout;
  return function (...args) {
    clearTimeout(timeout);
    timeout = setTimeout(() => func.apply(this, args), wait);
  };
}

// 쓰로틀
function throttle(func, limit) {
  let inThrottle;
  return function (...args) {
    if (!inThrottle) {
      func.apply(this, args);
      inThrottle = true;
      setTimeout(() => (inThrottle = false), limit);
    }
  };
}
```

---

## 22. 최신 문법 치트시트

```javascript title="modern-js.js"
// ES2020+
obj?.prop?.method?.() // Optional chaining
arr?.[0] // 배열 optional chaining
value ?? '기본값' // Nullish coalescing

// ES2021
||= // 논리 OR 할당
&&= // 논리 AND 할당
??= // Nullish 할당

// ES2022
arr.at(-1) // 음수 인덱스
/regex/d // 정규식 인덱스
class { #private } // 프라이빗 필드
await import('module') // 탑레벨 await

// ES2023
arr.findLast(조건)
arr.findLastIndex(조건)
arr.toSorted()
arr.toReversed()
arr.with(index, value)
```

---

## 23. 디버깅과 성능

```javascript title="debug-performance.js"
// 콘솔 메서드
console.log(값);
console.error(에러);
console.warn(경고);
console.table(배열 / 객체);
console.group('그룹명');
console.time('타이머명');
console.timeEnd('타이머명');

// 성능 측정
performance.now();
performance.mark('시작');
performance.measure('측정', '시작');

// 메모리 사용량
console.log(performance.memory);

// 에러 추적
console.trace();
```

---

## 24. 브라우저 호환성 체크리스트

```javascript title="compatibility.js"
// 기능 감지
if ('serviceWorker' in navigator) {
}
if (typeof fetch === 'function') {
}
if (window.IntersectionObserver) {
}

// 폴리필이 필요한 기능들
// - Promise (IE)
// - fetch (IE)
// - Array.from (IE)
// - Object.assign (IE)
// - Array.includes (IE)

// 최신 문법 지원 여부
// - Optional chaining (2020+)
// - Nullish coalescing (2020+)
// - Private fields (2022+)
// - Top-level await (2022+)
```
