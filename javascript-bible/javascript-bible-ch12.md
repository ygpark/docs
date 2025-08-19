---
title: '12장: DOM 조작과 이벤트'
slug: javascript-bible-dom-manipulation-events
description: 'DOM 트리 구조부터 이벤트 처리까지, 웹 페이지를 동적으로 만드는 핵심 기술을 친근하게 배워보세요. 실용적인 예제로 DOM 조작과 이벤트 리스너 활용법을 마스터해보겠습니다.'
keywords:
  [
    'DOM 조작',
    'JavaScript 이벤트',
    'DOM 트리',
    'querySelector',
    'addEventListener',
    '이벤트 위임',
    'Web Components',
    '동적 테이블',
    '드래그 앤 드롭',
  ]
sidebar_position: 12
---

# 12장: DOM 조작과 이벤트

이제 자바스크립트를 이용해 웹 페이지를 실제로 움직이게 만들어봅시다! DOM(Document Object Model)은 웹 페이지의 구조를 자바스크립트가 이해할 수 있는 트리 형태로 표현한 것입니다. 이 장에서는 HTML 요소를 찾고, 수정하고, 사용자의 클릭이나 입력에 반응하는 방법을 배워보겠습니다. 정적인 웹 페이지가 사용자와 상호작용하는 살아있는 애플리케이션으로 변화하는 마법을 경험해보세요.

**학습 목표:**

- DOM 트리 구조를 이해하고 요소를 선택하는 다양한 방법 익히기
- 요소의 내용, 스타일, 속성을 동적으로 변경하는 방법 마스터하기
- 이벤트 리스너를 통한 사용자 상호작용 처리 및 이벤트 위임 활용하기
- 현대적인 DOM 조작 패턴과 Web Components 기초 개념 이해하기

---

## DOM 트리 이해하기

DOM은 HTML 문서를 트리 구조로 나타낸 것입니다. 각 HTML 태그는 노드(Node)가 되고, 이들 간의 부모-자식 관계가 트리를 형성합니다. 자바스크립트는 이 트리를 탐색하고 조작할 수 있습니다.

```html title="index.html"
<!DOCTYPE html>
<html>
  <head>
    <title>DOM 예제</title>
  </head>
  <body>
    <div id="container" class="main-content">
      <h1>안녕하세요!</h1>
      <p class="description">DOM 조작을 배워봅시다.</p>
      <button id="change-btn">텍스트 변경</button>
    </div>
  </body>
</html>
```

위 HTML에서 `html`이 루트 노드이고, `head`와 `body`가 그 자식 노드가 됩니다. `body` 안의 `div`는 `body`의 자식이면서 동시에 `h1`, `p`, `button`의 부모 노드입니다.

```javascript title="dom-tree.js"
// DOM이 완전히 로드된 후 실행
document.addEventListener('DOMContentLoaded', function () {
  // 루트 요소 확인
  console.log(document.documentElement); // <html> 태그

  // body 요소의 자식 노드들 확인
  const container = document.getElementById('container');
  console.log(container.children); // HTMLCollection
  console.log(container.childNodes); // NodeList (텍스트 노드 포함)

  // 부모-자식 관계 탐색
  const heading = document.querySelector('h1');
  console.log(heading.parentElement); // div#container
  console.log(heading.nextElementSibling); // p.description
});
```

---

## 요소 선택과 조작

DOM 요소를 선택하는 방법은 여러 가지가 있습니다. 현대적인 방법들을 중심으로 살펴보겠습니다.

### 요소 선택 방법들

```javascript title="element-selection.js"
// ID로 선택 (단일 요소)
const container = document.getElementById('container');

// CSS 선택자로 선택 (첫 번째 요소)
const firstParagraph = document.querySelector('p');
const descriptionEl = document.querySelector('.description');
const button = document.querySelector('#change-btn');

// CSS 선택자로 모든 요소 선택
const allParagraphs = document.querySelectorAll('p');
const allButtons = document.querySelectorAll('button');

// 클래스명으로 선택
const mainContentEls = document.getElementsByClassName('main-content');

// 태그명으로 선택
const allDivs = document.getElementsByTagName('div');

// 현대적인 방법: querySelector 활용
const specificElement = document.querySelector('div.main-content > p:first-child');
```

### 요소 내용 조작

HTML 요소의 내용을 변경하는 가장 일반적인 방법들입니다.

```javascript title="content-manipulation.js"
const heading = document.querySelector('h1');
const paragraph = document.querySelector('.description');

// 텍스트 내용 변경 (HTML 태그는 문자열로 처리)
heading.textContent = '새로운 제목입니다!';

// HTML 내용 변경 (HTML 태그 해석됨)
paragraph.innerHTML = '<strong>강조된</strong> 내용입니다.';

// 안전한 HTML 삽입 (최신 브라우저)
if (paragraph.setHTML) {
  paragraph.setHTML('<em>안전하게 삽입된</em> 내용');
}

// 속성 조작
const button = document.querySelector('#change-btn');
button.setAttribute('data-clicked', 'false');
button.classList.add('btn', 'btn-primary');
button.style.backgroundColor = '#007bff';
button.style.color = 'white';

// 새로운 요소 생성과 추가
const newParagraph = document.createElement('p');
newParagraph.textContent = '동적으로 생성된 문단입니다.';
newParagraph.className = 'dynamic-content';
container.appendChild(newParagraph);
```

---

## 이벤트 리스너와 이벤트 위임

사용자의 행동(클릭, 입력, 스크롤 등)에 반응하는 것이 바로 이벤트 처리입니다. 현대적인 이벤트 처리 방법을 배워보겠습니다.

### 기본 이벤트 리스너

```javascript title="event-listeners.js"
const button = document.querySelector('#change-btn');
const paragraph = document.querySelector('.description');

// 클릭 이벤트 리스너 추가
button.addEventListener('click', function (event) {
  console.log('버튼이 클릭되었습니다!');
  paragraph.textContent = '버튼을 클릭하셨네요!';

  // 이벤트 객체 활용
  console.log('이벤트 타입:', event.type);
  console.log('클릭된 요소:', event.target);
  console.log('이벤트가 등록된 요소:', event.currentTarget);
});

// 화살표 함수 사용
button.addEventListener('mouseover', event => {
  event.target.style.backgroundColor = '#0056b3';
});

button.addEventListener('mouseout', event => {
  event.target.style.backgroundColor = '#007bff';
});

// 키보드 이벤트
document.addEventListener('keydown', event => {
  if (event.key === 'Enter' && event.ctrlKey) {
    console.log('Ctrl + Enter가 눌렸습니다!');
  }
});
```

### 이벤트 위임 패턴

많은 요소에 개별적으로 이벤트를 등록하는 대신, 부모 요소에서 이벤트를 처리하는 효율적인 방법입니다.

```javascript title="event-delegation.js"
// 동적으로 생성되는 버튼들을 처리하는 예제
const container = document.getElementById('container');

// 이벤트 위임: 부모 요소에서 모든 클릭 처리
container.addEventListener('click', function (event) {
  // 클릭된 요소가 버튼인지 확인
  if (event.target.tagName === 'BUTTON') {
    const buttonText = event.target.textContent;
    console.log(`${buttonText} 버튼이 클릭되었습니다!`);

    // 버튼별 동작 구분
    if (event.target.classList.contains('delete-btn')) {
      event.target.remove();
    } else if (event.target.classList.contains('edit-btn')) {
      const newText = prompt('새로운 텍스트를 입력하세요:');
      if (newText) {
        event.target.textContent = newText;
      }
    }
  }
});

// 동적으로 버튼 추가하는 함수
function addButton(text, className) {
  const button = document.createElement('button');
  button.textContent = text;
  button.className = className + ' px-4 py-2 m-2 rounded bg-blue-500 text-white hover:bg-blue-600';
  container.appendChild(button);
}

// 여러 버튼 동적 생성
addButton('편집', 'edit-btn');
addButton('삭제', 'delete-btn');
addButton('복사', 'copy-btn');
```

---

## 모던 이벤트 처리 패턴

최신 자바스크립트에서 사용되는 효율적인 이벤트 처리 패턴들을 살펴보겠습니다.

```javascript title="modern-event-patterns.js"
// AbortController를 사용한 이벤트 리스너 제거
const controller = new AbortController();

button.addEventListener('click', handleClick, {
  signal: controller.signal,
  once: true, // 한 번만 실행
  passive: true, // 성능 최적화
});

function handleClick(event) {
  console.log('한 번만 실행되는 이벤트');
}

// 필요시 모든 이벤트 리스너 제거
// controller.abort();

// 커스텀 이벤트 생성과 발생
const customEvent = new CustomEvent('userAction', {
  detail: {
    action: 'button-click',
    timestamp: Date.now(),
  },
  bubbles: true,
});

// 커스텀 이벤트 리스너 등록
document.addEventListener('userAction', event => {
  console.log('사용자 액션:', event.detail);
});

// 이벤트 발생
button.addEventListener('click', () => {
  button.dispatchEvent(customEvent);
});

// 디바운싱 패턴 (검색 입력 최적화)
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

const searchInput = document.createElement('input');
searchInput.placeholder = '검색어를 입력하세요...';
searchInput.className = 'w-full p-2 border rounded';

const debouncedSearch = debounce(value => {
  console.log('검색 실행:', value);
}, 300);

searchInput.addEventListener('input', event => {
  debouncedSearch(event.target.value);
});

container.appendChild(searchInput);
```

---

## Web Components 기초 개념

Web Components는 재사용 가능한 커스텀 HTML 요소를 만드는 웹 표준 기술입니다. 현대적인 웹 개발에서 점점 중요해지고 있는 개념입니다.

```javascript title="web-components-intro.js"
// 간단한 커스텀 요소 정의
class GreetingCard extends HTMLElement {
  constructor() {
    super();

    // Shadow DOM 생성 (캡슐화)
    const shadow = this.attachShadow({ mode: 'open' });

    // 템플릿 생성
    const template = document.createElement('template');
    template.innerHTML = `
            <style>
                .card {
                    border: 2px solid #e1e5e9;
                    border-radius: 8px;
                    padding: 16px;
                    margin: 8px;
                    background: white;
                    box-shadow: 0 2px 4px rgba(0,0,0,0.1);
                }
                .name {
                    font-size: 1.2em;
                    font-weight: bold;
                    color: #2c3e50;
                }
                .message {
                    margin-top: 8px;
                    color: #7f8c8d;
                }
            </style>
            <div class="card">
                <div class="name"></div>
                <div class="message"></div>
            </div>
        `;

    // Shadow DOM에 템플릿 추가
    shadow.appendChild(template.content.cloneNode(true));
  }

  // 컴포넌트가 DOM에 추가될 때 호출
  connectedCallback() {
    this.render();
  }

  // 속성이 변경될 때 감시할 속성들
  static get observedAttributes() {
    return ['name', 'message'];
  }

  // 속성 변경 시 호출
  attributeChangedCallback(name, oldValue, newValue) {
    this.render();
  }

  render() {
    const shadow = this.shadowRoot;
    const nameEl = shadow.querySelector('.name');
    const messageEl = shadow.querySelector('.message');

    nameEl.textContent = this.getAttribute('name') || '이름 없음';
    messageEl.textContent = this.getAttribute('message') || '메시지 없음';
  }
}

// 커스텀 요소 등록
customElements.define('greeting-card', GreetingCard);

// 사용 예제
const cardContainer = document.createElement('div');
cardContainer.innerHTML = `
    <greeting-card name="김철수" message="안녕하세요! 잘 부탁드립니다."></greeting-card>
    <greeting-card name="이영희" message="오늘 날씨가 정말 좋네요."></greeting-card>
`;
container.appendChild(cardContainer);
```

---

## 실습 1: 동적 테이블 생성기

사용자가 입력한 데이터를 바탕으로 동적으로 테이블을 생성하고 편집할 수 있는 시스템을 만들어봅시다. 이 예제는 DOM 조작의 핵심 기능들을 종합적으로 활용합니다.

```javascript title="dynamic-table.js"
class DynamicTable {
  constructor(containerId) {
    this.container = document.getElementById(containerId);
    this.data = [
      { name: '김철수', age: 25, job: '개발자' },
      { name: '이영희', age: 30, job: '디자이너' },
      { name: '박민수', age: 28, job: '기획자' },
    ];
    this.init();
  }

  init() {
    this.createControls();
    this.createTable();
    this.bindEvents();
  }

  createControls() {
    const controls = document.createElement('div');
    controls.className = 'mb-4 p-4 bg-gray-100 rounded';
    controls.innerHTML = `
            <h3 class="text-lg font-bold mb-2">새 데이터 추가</h3>
            <div class="flex gap-2 flex-wrap">
                <input type="text" id="nameInput" placeholder="이름" 
                       class="px-3 py-2 border rounded">
                <input type="number" id="ageInput" placeholder="나이" 
                       class="px-3 py-2 border rounded">
                <input type="text" id="jobInput" placeholder="직업" 
                       class="px-3 py-2 border rounded">
                <button id="addBtn" 
                        class="px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
                    추가
                </button>
            </div>
        `;
    this.container.appendChild(controls);
  }

  createTable() {
    const tableContainer = document.createElement('div');
    tableContainer.className = 'overflow-x-auto';

    this.table = document.createElement('table');
    this.table.className = 'w-full border-collapse border border-gray-300';
    this.table.innerHTML = `
            <thead>
                <tr class="bg-gray-200">
                    <th class="border border-gray-300 px-4 py-2">이름</th>
                    <th class="border border-gray-300 px-4 py-2">나이</th>
                    <th class="border border-gray-300 px-4 py-2">직업</th>
                    <th class="border border-gray-300 px-4 py-2">작업</th>
                </tr>
            </thead>
            <tbody></tbody>
        `;

    tableContainer.appendChild(this.table);
    this.container.appendChild(tableContainer);
    this.updateTable();
  }

  updateTable() {
    const tbody = this.table.querySelector('tbody');
    tbody.innerHTML = '';

    this.data.forEach((item, index) => {
      const row = document.createElement('tr');
      row.innerHTML = `
                <td class="border border-gray-300 px-4 py-2">${item.name}</td>
                <td class="border border-gray-300 px-4 py-2">${item.age}</td>
                <td class="border border-gray-300 px-4 py-2">${item.job}</td>
                <td class="border border-gray-300 px-4 py-2">
                    <button class="edit-btn px-2 py-1 bg-yellow-500 text-white rounded mr-1 hover:bg-yellow-600" 
                            data-index="${index}">편집</button>
                    <button class="delete-btn px-2 py-1 bg-red-500 text-white rounded hover:bg-red-600" 
                            data-index="${index}">삭제</button>
                </td>
            `;
      tbody.appendChild(row);
    });
  }

  bindEvents() {
    // 추가 버튼 이벤트
    document.getElementById('addBtn').addEventListener('click', () => {
      this.addData();
    });

    // Enter 키로 추가
    ['nameInput', 'ageInput', 'jobInput'].forEach(id => {
      document.getElementById(id).addEventListener('keypress', e => {
        if (e.key === 'Enter') this.addData();
      });
    });

    // 테이블 이벤트 위임
    this.table.addEventListener('click', e => {
      const index = parseInt(e.target.dataset.index);

      if (e.target.classList.contains('edit-btn')) {
        this.editData(index);
      } else if (e.target.classList.contains('delete-btn')) {
        this.deleteData(index);
      }
    });
  }

  addData() {
    const name = document.getElementById('nameInput').value.trim();
    const age = parseInt(document.getElementById('ageInput').value);
    const job = document.getElementById('jobInput').value.trim();

    if (name && age && job) {
      this.data.push({ name, age, job });
      this.updateTable();
      this.clearInputs();
    } else {
      alert('모든 필드를 입력해주세요!');
    }
  }

  editData(index) {
    const item = this.data[index];
    const newName = prompt('이름:', item.name);
    const newAge = prompt('나이:', item.age);
    const newJob = prompt('직업:', item.job);

    if (newName && newAge && newJob) {
      this.data[index] = {
        name: newName.trim(),
        age: parseInt(newAge),
        job: newJob.trim(),
      };
      this.updateTable();
    }
  }

  deleteData(index) {
    if (confirm('정말 삭제하시겠습니까?')) {
      this.data.splice(index, 1);
      this.updateTable();
    }
  }

  clearInputs() {
    document.getElementById('nameInput').value = '';
    document.getElementById('ageInput').value = '';
    document.getElementById('jobInput').value = '';
  }
}

// 테이블 생성
document.addEventListener('DOMContentLoaded', () => {
  new DynamicTable('container');
});
```

---

## 실습 2: 드래그 앤 드롭 파일 업로더

현대적인 웹 애플리케이션에서 자주 사용되는 드래그 앤 드롭 파일 업로드 기능을 구현해봅시다. 이 예제는 고급 이벤트 처리와 현대적인 웹 API를 활용합니다.

```javascript title="drag-drop-uploader.js"
class DragDropUploader {
  constructor(containerId) {
    this.container = document.getElementById(containerId);
    this.files = [];
    this.init();
  }

  init() {
    this.createUploadArea();
    this.createFileList();
    this.bindEvents();
  }

  createUploadArea() {
    this.uploadArea = document.createElement('div');
    this.uploadArea.className = `
            border-2 border-dashed border-gray-300 rounded-lg p-8 text-center
            transition-colors duration-200 hover:border-blue-400 hover:bg-blue-50
        `;
    this.uploadArea.innerHTML = `
            <div class="upload-content">
                <svg class="mx-auto h-12 w-12 text-gray-400 mb-4" stroke="currentColor" fill="none" viewBox="0 0 48 48">
                    <path d="M28 8H12a4 4 0 00-4 4v20m32-12v8m0 0v8a4 4 0 01-4 4H12a4 4 0 01-4-4v-4m32-4l-3.172-3.172a4 4 0 00-5.656 0L28 28M8 32l9.172-9.172a4 4 0 015.656 0L28 28m0 0l4 4m4-24h8m-4-4v8m-12 4h.02" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
                </svg>
                <p class="text-lg text-gray-600 mb-2">파일을 드래그하여 놓거나</p>
                <button class="browse-btn px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600">
                    파일 선택
                </button>
                <input type="file" class="file-input hidden" multiple accept="image/*,.pdf,.txt">
                <p class="text-sm text-gray-500 mt-2">이미지, PDF, 텍스트 파일을 지원합니다</p>
            </div>
        `;
    this.container.appendChild(this.uploadArea);
  }

  createFileList() {
    this.fileListContainer = document.createElement('div');
    this.fileListContainer.className = 'mt-6';
    this.fileListContainer.innerHTML = `
            <h3 class="text-lg font-semibold mb-3">업로드된 파일</h3>
            <div class="file-list space-y-2"></div>
        `;
    this.container.appendChild(this.fileListContainer);
  }

  bindEvents() {
    const fileInput = this.uploadArea.querySelector('.file-input');
    const browseBtn = this.uploadArea.querySelector('.browse-btn');

    // 파일 선택 버튼
    browseBtn.addEventListener('click', () => fileInput.click());
    fileInput.addEventListener('change', e => this.handleFiles(e.target.files));

    // 드래그 앤 드롭 이벤트
    ['dragenter', 'dragover', 'dragleave', 'drop'].forEach(eventName => {
      this.uploadArea.addEventListener(eventName, this.preventDefaults, false);
    });

    ['dragenter', 'dragover'].forEach(eventName => {
      this.uploadArea.addEventListener(eventName, () => this.highlight(), false);
    });

    ['dragleave', 'drop'].forEach(eventName => {
      this.uploadArea.addEventListener(eventName, () => this.unhighlight(), false);
    });

    this.uploadArea.addEventListener('drop', e => {
      const files = e.dataTransfer.files;
      this.handleFiles(files);
    });
  }

  preventDefaults(e) {
    e.preventDefault();
    e.stopPropagation();
  }

  highlight() {
    this.uploadArea.classList.add('border-blue-500', 'bg-blue-100');
    this.uploadArea.classList.remove('border-gray-300');
  }

  unhighlight() {
    this.uploadArea.classList.remove('border-blue-500', 'bg-blue-100');
    this.uploadArea.classList.add('border-gray-300');
  }

  handleFiles(fileList) {
    const validTypes = ['image/', 'application/pdf', 'text/'];

    Array.from(fileList).forEach(file => {
      const isValidType = validTypes.some(type => file.type.startsWith(type));

      if (isValidType && file.size < 10 * 1024 * 1024) {
        // 10MB 제한
        this.addFile(file);
      } else {
        alert(`${file.name}은 지원하지 않는 파일이거나 크기가 너무 큽니다.`);
      }
    });

    this.updateFileList();
  }

  addFile(file) {
    const fileObj = {
      file: file,
      id: Date.now() + Math.random(),
      name: file.name,
      size: this.formatFileSize(file.size),
      type: file.type,
      progress: 0,
    };

    this.files.push(fileObj);
    this.simulateUpload(fileObj);
  }

  simulateUpload(fileObj) {
    const interval = setInterval(() => {
      fileObj.progress += Math.random() * 20;
      if (fileObj.progress >= 100) {
        fileObj.progress = 100;
        clearInterval(interval);
      }
      this.updateFileItem(fileObj);
    }, 200);
  }

  updateFileList() {
    const fileList = this.fileListContainer.querySelector('.file-list');
    fileList.innerHTML = '';

    this.files.forEach(fileObj => {
      const fileItem = this.createFileItem(fileObj);
      fileList.appendChild(fileItem);
    });
  }

  createFileItem(fileObj) {
    const item = document.createElement('div');
    item.className = 'flex items-center justify-between p-3 bg-gray-50 rounded border';
    item.dataset.fileId = fileObj.id;

    item.innerHTML = `
            <div class="flex items-center space-x-3">
                <div class="file-icon">
                    ${this.getFileIcon(fileObj.type)}
                </div>
                <div>
                    <p class="font-medium text-gray-900">${fileObj.name}</p>
                    <p class="text-sm text-gray-500">${fileObj.size}</p>
                </div>
            </div>
            <div class="flex items-center space-x-3">
                <div class="w-32 bg-gray-200 rounded-full h-2">
                    <div class="bg-blue-600 h-2 rounded-full transition-all duration-300" 
                         style="width: ${fileObj.progress}%"></div>
                </div>
                <span class="text-sm text-gray-600">${Math.round(fileObj.progress)}%</span>
                <button class="remove-btn text-red-500 hover:text-red-700" data-file-id="${fileObj.id}">
                    ✕
                </button>
            </div>
        `;

    // 삭제 버튼 이벤트
    item.querySelector('.remove-btn').addEventListener('click', () => {
      this.removeFile(fileObj.id);
    });

    return item;
  }

  updateFileItem(fileObj) {
    const item = this.fileListContainer.querySelector(`[data-file-id="${fileObj.id}"]`);
    if (item) {
      const progressBar = item.querySelector('.bg-blue-600');
      const progressText = item.querySelector('.text-sm.text-gray-600');

      progressBar.style.width = `${fileObj.progress}%`;
      progressText.textContent = `${Math.round(fileObj.progress)}%`;

      if (fileObj.progress === 100) {
        progressText.textContent = '완료';
        progressText.className = 'text-sm text-green-600';
      }
    }
  }

  removeFile(fileId) {
    this.files = this.files.filter(f => f.id !== fileId);
    this.updateFileList();
  }

  getFileIcon(type) {
    if (type.startsWith('image/')) {
      return '🖼️';
    } else if (type === 'application/pdf') {
      return '📄';
    } else if (type.startsWith('text/')) {
      return '📝';
    }
    return '📎';
  }

  formatFileSize(bytes) {
    if (bytes === 0) return '0 Bytes';
    const k = 1024;
    const sizes = ['Bytes', 'KB', 'MB', 'GB'];
    const i = Math.floor(Math.log(bytes) / Math.log(k));
    return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
  }
}

// 업로더 초기화
document.addEventListener('DOMContentLoaded', () => {
  new DragDropUploader('container');
});
```

이제 DOM 조작과 이벤트 처리의 핵심을 모두 배웠습니다! 이 장에서 학습한 내용들은 모든 웹 애플리케이션 개발의 기초가 됩니다. 다음 장에서는 웹 API와 서버 통신을 통해 더욱 동적인 애플리케이션을 만들어보겠습니다.
