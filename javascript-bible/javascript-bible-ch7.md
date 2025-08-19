---
title: '7장: 기초 다지기'
slug: javascript-bible-foundation-building
description: '지금까지 배운 자바스크립트 기초 지식을 실전 프로젝트와 코드 리뷰를 통해 튼튼히 다지고, 일반적인 실수들을 해결하는 방법을 알아봅니다.'
keywords:
  [
    '자바스크립트 기초',
    '코드 리뷰',
    '리팩토링',
    '도서 관리 앱',
    'DOM 조작',
    '자바스크립트 실습',
    '초보자 프로젝트',
    '코딩 테스트',
    '자바스크립트 실수',
  ]
sidebar_position: 7
---

# 7장: 기초 다지기

## 학습 목표

이번 장에서는 지금까지 배운 자바스크립트 기초 지식들을 실제 프로젝트를 통해 종합적으로 활용해보고, 코드의 품질을 높이는 방법을 배웁니다. 간단한 도서 관리 앱을 만들어보면서 객체, 배열, 함수, DOM 조작을 실전에서 어떻게 사용하는지 경험하고, 자주 발생하는 실수들을 미리 파악하여 더 나은 개발자로 성장하는 밑거름을 마련합니다.

---

## 종합 실습: 간단한 도서 관리 앱

지금까지 배운 모든 개념들을 활용하여 실제 동작하는 웹 애플리케이션을 만들어보겠습니다. 이 프로젝트를 통해 변수, 함수, 객체, 배열, DOM 조작이 어떻게 유기적으로 연결되는지 직접 체험할 수 있습니다.

우리가 만들 도서 관리 앱은 책을 추가하고, 목록을 보여주고, 검색하는 기능을 제공합니다. 사용자가 책 제목과 저자를 입력하면 화면에 추가되고, 읽음 상태를 토글할 수 있으며, 제목으로 검색도 가능합니다.

```html title=index.html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>도서 관리 앱</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-gray-100 min-h-screen py-8">
    <div class="container mx-auto max-w-4xl px-4">
      <h1 class="text-3xl font-bold text-center text-gray-800 mb-8">나만의 도서관</h1>

      <!-- 도서 추가 폼 -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-6">
        <h2 class="text-xl font-semibold mb-4">새 도서 추가</h2>
        <form id="bookForm" class="space-y-4">
          <div>
            <input
              type="text"
              id="titleInput"
              placeholder="책 제목을 입력하세요"
              class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
          <div>
            <input
              type="text"
              id="authorInput"
              placeholder="저자를 입력하세요"
              class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
            />
          </div>
          <button
            type="submit"
            class="w-full bg-blue-500 text-white p-3 rounded-lg hover:bg-blue-600 transition duration-200"
          >
            도서 추가
          </button>
        </form>
      </div>

      <!-- 검색 -->
      <div class="bg-white rounded-lg shadow-md p-6 mb-6">
        <input
          type="text"
          id="searchInput"
          placeholder="책 제목으로 검색..."
          class="w-full p-3 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-green-500"
        />
      </div>

      <!-- 도서 목록 -->
      <div id="bookList" class="space-y-4">
        <!-- 도서들이 여기에 동적으로 추가됩니다 -->
      </div>
    </div>

    <script src="app.js"></script>
  </body>
</html>
```

```javascript title=app.js
// 도서 데이터를 저장할 배열
let books = [];
let nextId = 1;

// DOM 요소들 가져오기
const bookForm = document.getElementById('bookForm');
const titleInput = document.getElementById('titleInput');
const authorInput = document.getElementById('authorInput');
const searchInput = document.getElementById('searchInput');
const bookList = document.getElementById('bookList');

// 도서 객체 생성 함수
function createBook(title, author) {
  return {
    id: nextId++,
    title: title.trim(),
    author: author.trim(),
    isRead: false,
    addedDate: new Date().toLocaleDateString(),
  };
}

// 도서 추가 함수
function addBook(title, author) {
  if (!title || !author) {
    alert('제목과 저자를 모두 입력해주세요!');
    return;
  }

  const newBook = createBook(title, author);
  books.push(newBook);
  renderBooks();

  // 입력 필드 초기화
  titleInput.value = '';
  authorInput.value = '';
}

// 도서 삭제 함수
function deleteBook(bookId) {
  books = books.filter(book => book.id !== bookId);
  renderBooks();
}

// 읽음 상태 토글 함수
function toggleReadStatus(bookId) {
  const book = books.find(book => book.id === bookId);
  if (book) {
    book.isRead = !book.isRead;
    renderBooks();
  }
}

// 도서 목록 렌더링 함수
function renderBooks(filteredBooks = books) {
  bookList.innerHTML = '';

  if (filteredBooks.length === 0) {
    bookList.innerHTML = '<p class="text-center text-gray-500 py-8">등록된 도서가 없습니다.</p>';
    return;
  }

  filteredBooks.forEach(book => {
    const bookElement = document.createElement('div');
    bookElement.className = `bg-white rounded-lg shadow-md p-6 ${book.isRead ? 'bg-green-50 border-l-4 border-green-500' : ''}`;

    bookElement.innerHTML = `
            <div class="flex justify-between items-start">
                <div class="flex-1">
                    <h3 class="text-lg font-semibold ${book.isRead ? 'line-through text-gray-500' : 'text-gray-800'}">${book.title}</h3>
                    <p class="text-gray-600 mt-1">저자: ${book.author}</p>
                    <p class="text-sm text-gray-400 mt-2">등록일: ${book.addedDate}</p>
                </div>
                <div class="flex space-x-2 ml-4">
                    <button onclick="toggleReadStatus(${book.id})" 
                            class="px-4 py-2 rounded ${book.isRead ? 'bg-gray-500 hover:bg-gray-600' : 'bg-green-500 hover:bg-green-600'} text-white transition duration-200">
                        ${book.isRead ? '읽음 취소' : '읽음 완료'}
                    </button>
                    <button onclick="deleteBook(${book.id})" 
                            class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600 transition duration-200">
                        삭제
                    </button>
                </div>
            </div>
        `;

    bookList.appendChild(bookElement);
  });
}

// 도서 검색 함수
function searchBooks(query) {
  const filtered = books.filter(book => book.title.toLowerCase().includes(query.toLowerCase()));
  renderBooks(filtered);
}

// 이벤트 리스너 등록
bookForm.addEventListener('submit', e => {
  e.preventDefault();
  addBook(titleInput.value, authorInput.value);
});

searchInput.addEventListener('input', e => {
  searchBooks(e.target.value);
});

// 초기 렌더링
renderBooks();
```

---

## 코드 리뷰와 리팩토링 연습

좋은 코드는 처음부터 완벽하게 작성되는 것이 아닙니다. 지속적인 개선을 통해 더 읽기 쉽고 유지보수하기 좋은 코드로 발전시킬 수 있습니다. 위에서 작성한 도서 관리 앱을 더욱 개선해보겠습니다.

코드 리뷰는 버그를 찾는 것뿐만 아니라 코드의 가독성, 성능, 확장성을 검토하는 과정입니다. 우리의 도서 관리 앱에서 개선할 수 있는 부분들을 찾아보고, 더 나은 구조로 리팩토링해보겠습니다.

```javascript title=app-refactored.js
// 도서 관리 앱 - 리팩토링 버전
class BookManager {
  constructor() {
    this.books = [];
    this.nextId = 1;
    this.initializeElements();
    this.attachEventListeners();
    this.render();
  }

  // DOM 요소 초기화
  initializeElements() {
    this.elements = {
      form: document.getElementById('bookForm'),
      titleInput: document.getElementById('titleInput'),
      authorInput: document.getElementById('authorInput'),
      searchInput: document.getElementById('searchInput'),
      bookList: document.getElementById('bookList'),
    };
  }

  // 이벤트 리스너 등록
  attachEventListeners() {
    this.elements.form.addEventListener('submit', e => {
      e.preventDefault();
      this.handleAddBook();
    });

    this.elements.searchInput.addEventListener('input', e => {
      this.handleSearch(e.target.value);
    });
  }

  // 도서 추가 처리
  handleAddBook() {
    const title = this.elements.titleInput.value.trim();
    const author = this.elements.authorInput.value.trim();

    if (!this.validateInputs(title, author)) {
      return;
    }

    this.addBook(title, author);
    this.clearInputs();
  }

  // 입력값 검증
  validateInputs(title, author) {
    if (!title || !author) {
      this.showMessage('제목과 저자를 모두 입력해주세요!', 'error');
      return false;
    }

    if (title.length < 2) {
      this.showMessage('제목은 2글자 이상 입력해주세요!', 'error');
      return false;
    }

    return true;
  }

  // 메시지 표시
  showMessage(text, type = 'info') {
    // 간단한 알림 대신 더 나은 UI 피드백
    const messageDiv = document.createElement('div');
    const bgColor = type === 'error' ? 'bg-red-500' : 'bg-blue-500';

    messageDiv.className = `fixed top-4 right-4 ${bgColor} text-white px-6 py-3 rounded-lg shadow-lg z-50`;
    messageDiv.textContent = text;

    document.body.appendChild(messageDiv);

    setTimeout(() => {
      messageDiv.remove();
    }, 3000);
  }

  // 도서 추가
  addBook(title, author) {
    const book = {
      id: this.nextId++,
      title,
      author,
      isRead: false,
      addedDate: new Date().toLocaleDateString('ko-KR'),
    };

    this.books.push(book);
    this.render();
    this.showMessage('도서가 성공적으로 추가되었습니다!');
  }

  // 입력 필드 초기화
  clearInputs() {
    this.elements.titleInput.value = '';
    this.elements.authorInput.value = '';
    this.elements.titleInput.focus();
  }

  // 도서 삭제
  deleteBook(bookId) {
    if (!confirm('정말로 이 도서를 삭제하시겠습니까?')) {
      return;
    }

    this.books = this.books.filter(book => book.id !== bookId);
    this.render();
    this.showMessage('도서가 삭제되었습니다.');
  }

  // 읽음 상태 토글
  toggleReadStatus(bookId) {
    const book = this.books.find(book => book.id === bookId);
    if (book) {
      book.isRead = !book.isRead;
      this.render();
    }
  }

  // 검색 처리
  handleSearch(query) {
    const filteredBooks = query
      ? this.books.filter(
          book =>
            book.title.toLowerCase().includes(query.toLowerCase()) ||
            book.author.toLowerCase().includes(query.toLowerCase())
        )
      : this.books;

    this.render(filteredBooks);
  }

  // 화면 렌더링
  render(booksToRender = this.books) {
    if (booksToRender.length === 0) {
      this.renderEmptyState();
      return;
    }

    this.elements.bookList.innerHTML = booksToRender
      .map(book => this.createBookHTML(book))
      .join('');
  }

  // 빈 상태 렌더링
  renderEmptyState() {
    this.elements.bookList.innerHTML = `
            <div class="text-center py-12">
                <p class="text-gray-500 text-lg mb-4">등록된 도서가 없습니다</p>
                <p class="text-gray-400">첫 번째 도서를 추가해보세요!</p>
            </div>
        `;
  }

  // 도서 HTML 생성
  createBookHTML(book) {
    const readStatusClass = book.isRead ? 'bg-green-50 border-l-4 border-green-500' : '';
    const titleClass = book.isRead ? 'line-through text-gray-500' : 'text-gray-800';
    const buttonClass = book.isRead
      ? 'bg-gray-500 hover:bg-gray-600'
      : 'bg-green-500 hover:bg-green-600';
    const buttonText = book.isRead ? '읽음 취소' : '읽음 완료';

    return `
            <div class="bg-white rounded-lg shadow-md p-6 ${readStatusClass}">
                <div class="flex justify-between items-start">
                    <div class="flex-1">
                        <h3 class="text-lg font-semibold ${titleClass}">${book.title}</h3>
                        <p class="text-gray-600 mt-1">저자: ${book.author}</p>
                        <p class="text-sm text-gray-400 mt-2">등록일: ${book.addedDate}</p>
                    </div>
                    <div class="flex space-x-2 ml-4">
                        <button onclick="bookManager.toggleReadStatus(${book.id})" 
                                class="px-4 py-2 rounded ${buttonClass} text-white transition duration-200">
                            ${buttonText}
                        </button>
                        <button onclick="bookManager.deleteBook(${book.id})" 
                                class="px-4 py-2 bg-red-500 text-white rounded hover:bg-red-600 transition duration-200">
                            삭제
                        </button>
                    </div>
                </div>
            </div>
        `;
  }
}

// 앱 초기화
const bookManager = new BookManager();
```

---

## 일반적인 실수와 해결법

자바스크립트를 배우면서 자주 마주치는 실수들과 그 해결 방법을 알아보겠습니다. 이런 실수들을 미리 알고 있으면 개발 시간을 크게 단축할 수 있고, 더 안정적인 코드를 작성할 수 있습니다.

실제 개발에서 가장 자주 발생하는 실수들은 대부분 변수 스코프, 타입 변환, 비동기 처리와 관련이 있습니다. 각각의 문제 상황과 올바른 해결 방법을 비교해보면서 좋은 코딩 습관을 만들어가겠습니다.

```javascript title=common-mistakes.js
// 실수 1: var vs let/const 혼동
// 잘못된 예시
console.log('=== Scope 실수 예시 ===');
for (var i = 0; i < 3; i++) {
  setTimeout(() => {
    console.log('잘못된 결과:', i); // 모두 3이 출력됨
  }, 100);
}

// 올바른 해결법
for (let j = 0; j < 3; j++) {
  setTimeout(() => {
    console.log('올바른 결과:', j); // 0, 1, 2가 출력됨
  }, 200);
}

// 실수 2: == vs === 혼동
console.log('\n=== 비교 연산자 실수 ===');
console.log('0 == false:', 0 == false); // true (타입 변환)
console.log('0 === false:', 0 === false); // false (엄격한 비교)
console.log('"123" == 123:', '123' == 123); // true (타입 변환)
console.log('"123" === 123:', '123' === 123); // false (엄격한 비교)

// 실수 3: 배열/객체 복사 문제
console.log('\n=== 얕은 복사 vs 깊은 복사 ===');
const originalArray = [1, 2, { name: 'John' }];
const shallowCopy = [...originalArray];
const wrongCopy = originalArray; // 이는 복사가 아님!

// 원본 수정
originalArray[2].name = 'Jane';
console.log('원본:', originalArray[2].name); // Jane
console.log('얕은 복사:', shallowCopy[2].name); // Jane (객체는 참조가 복사됨)
console.log('잘못된 복사:', wrongCopy[2].name); // Jane (같은 참조)

// 올바른 깊은 복사 방법
const deepCopy = JSON.parse(JSON.stringify(originalArray));
originalArray[2].name = 'Bob';
console.log('깊은 복사:', deepCopy[2].name); // Jane (독립적)

// 실수 4: 함수 호이스팅 혼동
console.log('\n=== 호이스팅 실수 ===');

// 함수 선언문은 호이스팅됨
sayHello(); // 정상 작동

function sayHello() {
  console.log('Hello from function declaration!');
}

// 함수 표현식은 호이스팅되지 않음
try {
  sayGoodbye(); // 에러 발생
} catch (e) {
  console.log('에러:', e.message);
}

const sayGoodbye = function () {
  console.log('Goodbye from function expression!');
};

// 실수 5: 이벤트 리스너에서 this 혼동
console.log('\n=== this 바인딩 실수 ===');

class ButtonHandler {
  constructor(name) {
    this.name = name;
  }

  // 잘못된 방법 (this가 버튼 요소를 가리킴)
  handleClickWrong() {
    console.log('잘못된 this:', this.name); // undefined
  }

  // 올바른 방법 1: 화살표 함수 사용
  handleClickCorrect = () => {
    console.log('올바른 this (화살표):', this.name);
  };

  // 올바른 방법 2: bind 사용
  handleClickBind() {
    console.log('올바른 this (bind):', this.name);
  }
}

// 실수 6: 비동기 처리에서 순서 보장 안 함
async function demonstrateAsyncMistakes() {
  console.log('\n=== 비동기 처리 실수 ===');

  // 잘못된 예시: 순서가 보장되지 않음
  console.log('잘못된 비동기 처리 시작');
  Promise.resolve().then(() => console.log('Promise 1'));
  Promise.resolve().then(() => console.log('Promise 2'));
  console.log('동기 코드');

  // 올바른 예시: 순서 보장
  console.log('\n올바른 비동기 처리 시작');
  await new Promise(resolve => {
    console.log('첫 번째 작업');
    resolve();
  });
  await new Promise(resolve => {
    console.log('두 번째 작업');
    resolve();
  });
  console.log('모든 작업 완료');
}

demonstrateAsyncMistakes();

// 실수 7: 메모리 누수
console.log('\n=== 메모리 누수 방지 ===');

// 잘못된 예시: 이벤트 리스너 제거 안 함
function createLeakyElement() {
  const element = document.createElement('div');
  const data = new Array(1000000).fill('memory leak');

  element.addEventListener('click', function () {
    console.log(data.length); // data가 메모리에 계속 남아있음
  });

  return element;
}

// 올바른 예시: 정리 함수 제공
function createCleanElement() {
  const element = document.createElement('div');
  const data = new Array(1000000).fill('clean memory');

  const clickHandler = function () {
    console.log(data.length);
  };

  element.addEventListener('click', clickHandler);

  // 정리 함수 반환
  return {
    element,
    cleanup: () => {
      element.removeEventListener('click', clickHandler);
      // data는 가비지 컬렉션됨
    },
  };
}
```

---

## 온라인 코딩 테스트 플랫폼 활용법

실력 향상을 위해서는 지속적인 연습이 필요합니다. 온라인 코딩 플랫폼을 활용하면 다양한 문제를 해결하면서 자바스크립트 실력을 체계적으로 키울 수 있습니다.

각 플랫폼마다 특색이 다르므로 자신의 학습 목표에 맞는 플랫폼을 선택하는 것이 중요합니다. 알고리즘 문제 해결 능력을 기르고 싶다면 LeetCode나 Programmers를, 실무 중심의 코딩 스킬을 연마하고 싶다면 Codewars나 HackerRank를 추천합니다.

```javascript title=coding-practice-tips.js
// 코딩 테스트에서 자주 사용되는 자바스크립트 패턴들

// 1. 배열 다루기
function arrayPractice() {
  const numbers = [1, 2, 3, 4, 5];

  // 합계 구하기
  const sum = numbers.reduce((acc, num) => acc + num, 0);
  console.log('합계:', sum);

  // 최대값/최소값
  const max = Math.max(...numbers);
  const min = Math.min(...numbers);
  console.log('최대값:', max, '최소값:', min);

  // 중복 제거
  const duplicates = [1, 2, 2, 3, 3, 4];
  const unique = [...new Set(duplicates)];
  console.log('중복 제거:', unique);

  // 배열 뒤집기
  const reversed = numbers.slice().reverse();
  console.log('뒤집기:', reversed);
}

// 2. 문자열 다루기
function stringPractice() {
  const text = 'Hello World';

  // 문자 빈도수 계산
  const charCount = {};
  for (const char of text.toLowerCase()) {
    if (char !== ' ') {
      charCount[char] = (charCount[char] || 0) + 1;
    }
  }
  console.log('문자 빈도:', charCount);

  // 팰린드롬 검사
  function isPalindrome(str) {
    const cleaned = str.toLowerCase().replace(/[^a-z0-9]/g, '');
    return cleaned === cleaned.split('').reverse().join('');
  }

  console.log('팰린드롬 검사:', isPalindrome('A man a plan a canal Panama'));
}

// 3. 객체 다루기
function objectPractice() {
  const users = [
    { name: 'Alice', age: 25, city: 'Seoul' },
    { name: 'Bob', age: 30, city: 'Busan' },
    { name: 'Charlie', age: 25, city: 'Seoul' },
  ];

  // 나이별 그룹핑
  const groupedByAge = users.reduce((acc, user) => {
    const age = user.age;
    if (!acc[age]) acc[age] = [];
    acc[age].push(user);
    return acc;
  }, {});

  console.log('나이별 그룹:', groupedByAge);

  // 조건에 맞는 사용자 찾기
  const seoulUsers = users.filter(user => user.city === 'Seoul');
  console.log('서울 거주자:', seoulUsers);
}

// 4. 실용적인 유틸리티 함수들
const utils = {
  // 디바운스 함수
  debounce(func, wait) {
    let timeout;
    return function executedFunction(...args) {
      const later = () => {
        clearTimeout(timeout);
        func(...args);
      };
      clearTimeout(timeout);
      timeout = setTimeout(later, wait);
    };
  },

  // 깊은 복사
  deepClone(obj) {
    if (obj === null || typeof obj !== 'object') return obj;
    if (obj instanceof Date) return new Date(obj.getTime());
    if (obj instanceof Array) return obj.map(item => this.deepClone(item));
    if (typeof obj === 'object') {
      const clonedObj = {};
      for (const key in obj) {
        if (obj.hasOwnProperty(key)) {
          clonedObj[key] = this.deepClone(obj[key]);
        }
      }
      return clonedObj;
    }
  },

  // 배열 청크 분할
  chunk(array, size) {
    const chunks = [];
    for (let i = 0; i < array.length; i += size) {
      chunks.push(array.slice(i, i + size));
    }
    return chunks;
  },
};

// 함수들 실행
arrayPractice();
stringPractice();
objectPractice();

// 유틸리티 함수 테스트
console.log('\n=== 유틸리티 함수 테스트 ===');
const testArray = [1, 2, 3, 4, 5, 6, 7, 8, 9];
console.log('청크 분할:', utils.chunk(testArray, 3));

const testObj = { a: 1, b: { c: 2, d: [3, 4] } };
const cloned = utils.deepClone(testObj);
console.log('깊은 복사 성공:', cloned.b !== testObj.b);
```

---

## 기초 DOM 조작 맛보기

웹 페이지를 동적으로 만들기 위해서는 DOM(Document Object Model) 조작이 필수입니다. 앞서 도서 관리 앱에서 간단히 사용해봤지만, 이번에는 더 다양한 DOM 조작 기법들을 알아보겠습니다.

DOM 조작은 사용자와의 상호작용을 만들어내는 핵심 기술입니다. 버튼 클릭, 폼 입력, 애니메이션 등 모든 동적인 웹 경험은 DOM 조작을 통해 구현됩니다. 기본적인 조작법을 익혀두면 더 복잡한 웹 애플리케이션을 만들 때도 자신감을 가질 수 있습니다.

```html title=dom-practice.html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>DOM 조작 연습</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-gray-100 p-8">
    <div class="max-w-4xl mx-auto space-y-6">
      <h1 class="text-3xl font-bold text-center">DOM 조작 연습장</h1>

      <!-- 요소 생성/삭제 연습 -->
      <div class="bg-white p-6 rounded-lg shadow">
        <h2 class="text-xl font-semibold mb-4">동적 요소 생성</h2>
        <button id="addBox" class="bg-blue-500 text-white px-4 py-2 rounded mr-2">박스 추가</button>
        <button id="clearBoxes" class="bg-red-500 text-white px-4 py-2 rounded">모두 삭제</button>
        <div id="boxContainer" class="mt-4 flex flex-wrap gap-2"></div>
      </div>

      <!-- 스타일 조작 연습 -->
      <div class="bg-white p-6 rounded-lg shadow">
        <h2 class="text-xl font-semibold mb-4">스타일 변경</h2>
        <div
          id="colorBox"
          class="w-32 h-32 bg-blue-500 rounded-lg mb-4 transition-all duration-300"
        ></div>
        <button id="changeColor" class="bg-green-500 text-white px-4 py-2 rounded mr-2">
          색상 변경
        </button>
        <button id="toggleSize" class="bg-purple-500 text-white px-4 py-2 rounded">
          크기 토글
        </button>
      </div>

      <!-- 이벤트 연습 -->
      <div class="bg-white p-6 rounded-lg shadow">
        <h2 class="text-xl font-semibold mb-4">이벤트 처리</h2>
        <input
          type="text"
          id="textInput"
          placeholder="여기에 입력하세요"
          class="w-full p-2 border rounded mb-4"
        />
        <div id="output" class="p-4 bg-gray-100 rounded min-h-[100px]">
          여기에 결과가 표시됩니다
        </div>
      </div>
    </div>

    <script src="dom-practice.js"></script>
  </body>
</html>
```

```javascript title=dom-practice.js
// DOM 조작 연습 스크립트

// 1. 동적 요소 생성과 삭제
class BoxManager {
  constructor() {
    this.boxContainer = document.getElementById('boxContainer');
    this.addBoxBtn = document.getElementById('addBox');
    this.clearBoxesBtn = document.getElementById('clearBoxes');
    this.boxCount = 0;

    this.init();
  }

  init() {
    this.addBoxBtn.addEventListener('click', () => this.addBox());
    this.clearBoxesBtn.addEventListener('click', () => this.clearBoxes());
  }

  addBox() {
    const box = document.createElement('div');
    const colors = ['bg-red-400', 'bg-blue-400', 'bg-green-400', 'bg-yellow-400', 'bg-purple-400'];
    const randomColor = colors[Math.floor(Math.random() * colors.length)];

    box.className = `w-16 h-16 ${randomColor} rounded cursor-pointer flex items-center justify-center text-white font-bold`;
    box.textContent = ++this.boxCount;

    // 클릭 시 삭제 이벤트 추가
    box.addEventListener('click', () => {
      box.remove();
    });

    this.boxContainer.appendChild(box);
  }

  clearBoxes() {
    this.boxContainer.innerHTML = '';
    this.boxCount = 0;
  }
}

// 2. 스타일 동적 변경
class StyleChanger {
  constructor() {
    this.colorBox = document.getElementById('colorBox');
    this.changeColorBtn = document.getElementById('changeColor');
    this.toggleSizeBtn = document.getElementById('toggleSize');
    this.isLarge = false;

    this.init();
  }

  init() {
    this.changeColorBtn.addEventListener('click', () => this.changeColor());
    this.toggleSizeBtn.addEventListener('click', () => this.toggleSize());
  }

  changeColor() {
    const colors = [
      'bg-red-500',
      'bg-blue-500',
      'bg-green-500',
      'bg-yellow-500',
      'bg-purple-500',
      'bg-pink-500',
    ];
    const randomColor = colors[Math.floor(Math.random() * colors.length)];

    // 기존 색상 클래스 제거
    this.colorBox.className = this.colorBox.className.replace(/bg-\w+-\d+/g, '');
    this.colorBox.classList.add(randomColor);
  }

  toggleSize() {
    if (this.isLarge) {
      this.colorBox.classList.remove('w-64', 'h-64');
      this.colorBox.classList.add('w-32', 'h-32');
    } else {
      this.colorBox.classList.remove('w-32', 'h-32');
      this.colorBox.classList.add('w-64', 'h-64');
    }
    this.isLarge = !this.isLarge;
  }
}

// 3. 고급 이벤트 처리
class EventHandler {
  constructor() {
    this.textInput = document.getElementById('textInput');
    this.output = document.getElementById('output');

    this.init();
  }

  init() {
    // 다양한 이벤트 리스너 등록
    this.textInput.addEventListener('input', e => this.handleInput(e));
    this.textInput.addEventListener('focus', () => this.handleFocus());
    this.textInput.addEventListener('blur', () => this.handleBlur());
    this.textInput.addEventListener('keydown', e => this.handleKeyDown(e));
  }

  handleInput(e) {
    const value = e.target.value;
    this.updateOutput(`
            <div class="mb-2">
                <strong>입력 중:</strong> "${value}"<br>
                <strong>글자 수:</strong> ${value.length}<br>
                <strong>단어 수:</strong> ${value.trim() ? value.trim().split(/\s+/).length : 0}
            </div>
        `);
  }

  handleFocus() {
    this.textInput.classList.add('ring-2', 'ring-blue-500');
    this.appendOutput('<div class="text-blue-600 text-sm">📝 입력 필드에 포커스됨</div>');
  }

  handleBlur() {
    this.textInput.classList.remove('ring-2', 'ring-blue-500');
    this.appendOutput('<div class="text-gray-600 text-sm">⬜ 입력 필드에서 포커스 해제됨</div>');
  }

  handleKeyDown(e) {
    if (e.key === 'Enter') {
      this.appendOutput(
        `<div class="text-green-600 text-sm">✅ Enter 키 눌림: "${e.target.value}"</div>`
      );
    } else if (e.key === 'Escape') {
      e.target.value = '';
      this.appendOutput('<div class="text-red-600 text-sm">🗑️ Escape 키로 내용 삭제됨</div>');
    }
  }

  updateOutput(content) {
    this.output.innerHTML = content;
  }

  appendOutput(content) {
    this.output.innerHTML += content;
    // 스크롤을 맨 아래로
    this.output.scrollTop = this.output.scrollHeight;
  }
}

// 앱 초기화
document.addEventListener('DOMContentLoaded', () => {
  new BoxManager();
  new StyleChanger();
  new EventHandler();

  console.log('DOM 조작 연습 앱이 시작되었습니다!');
});
```

이번 장에서는 지금까지 배운 모든 자바스크립트 기초 지식을 실전 프로젝트를 통해 종합적으로 활용해보았습니다. 도서 관리 앱을 만들면서 변수, 함수, 객체, 배열, DOM 조작이 어떻게 연결되는지 경험했고, 코드 리뷰와 리팩토링을 통해 더 나은 코드를 작성하는 방법을 배웠습니다.

또한 자주 발생하는 실수들을 미리 파악하고 해결 방법을 익혔으며, 온라인 코딩 플랫폼을 활용한 지속적인 학습 방법과 기초적인 DOM 조작 기법들도 알아보았습니다. 이제 여러분은 자바스크립트의 기초를 탄탄히 다졌으므로, 더 복잡한 주제들을 학습할 준비가 되었습니다!
