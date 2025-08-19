---
title: '16장: 프로젝트 1 - 할 일 관리 앱'
slug: javascript-bible-todo-app-project
description: '실전 DOM 조작, 로컬 스토리지, 이벤트 처리를 활용한 할 일 관리 앱 구현하기'
keywords:
  [
    'JavaScript',
    '할 일 관리 앱',
    'TODO 앱',
    'DOM 조작',
    '로컬 스토리지',
    '이벤트 처리',
    'TailwindCSS',
    '웹 개발',
    '실전 프로젝트',
  ]
sidebar_position: 16
---

# 16장: 프로젝트 1 - 할 일 관리 앱

이 장에서는 지금까지 배운 자바스크립트 기초 지식을 종합하여 실용적인 할 일 관리 앱을 만들어보겠습니다. DOM 조작, 이벤트 처리, 로컬 스토리지를 활용한 데이터 저장, 그리고 TailwindCSS를 사용한 반응형 디자인까지 모던 웹 개발의 핵심 요소들을 실제 프로젝트에 적용해볼 수 있습니다. 단순한 예제가 아닌 실제로 사용할 수 있는 수준의 앱을 만들면서 개발자로서의 실력을 한 단계 올려보세요.

---

## 학습 목표

이 장을 통해 다음과 같은 내용을 학습할 수 있습니다:

- 실제 웹 애플리케이션의 설계와 구현 과정 이해
- DOM 조작과 이벤트 처리를 활용한 동적 UI 구현
- 로컬 스토리지를 통한 클라이언트 사이드 데이터 저장
- 모듈화와 코드 구조화 방법
- 사용자 경험을 고려한 인터페이스 설계
- 성능 최적화와 접근성 개선 방법

---

## 프로젝트 개요와 요구사항 분석

할 일 관리 앱은 사용자가 일상의 작업들을 효율적으로 관리할 수 있도록 도와주는 도구입니다. 기본적인 CRUD(Create, Read, Update, Delete) 기능을 포함하여 필터링, 검색, 로컬 저장 등의 기능을 구현해보겠습니다.

### 핵심 기능 요구사항

**기본 기능:**

- 새로운 할 일 추가
- 할 일 목록 조회 및 표시
- 할 일 완료 상태 토글
- 할 일 삭제
- 브라우저를 닫아도 데이터 유지 (로컬 스토리지)

**추가 기능:**

- 할 일 필터링 (전체, 완료, 미완료)
- 남은 할 일 개수 표시
- 완료된 할 일 일괄 삭제
- 반응형 디자인

```html title="index.html"
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>할 일 관리 앱</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-gray-100 min-h-screen py-8">
    <div class="max-w-md mx-auto bg-white rounded-lg shadow-lg">
      <div class="p-6">
        <h1 class="text-2xl font-bold text-gray-800 mb-6 text-center">할 일 관리</h1>

        <!-- 입력 폼 -->
        <form id="todo-form" class="mb-6">
          <div class="flex gap-2">
            <input
              type="text"
              id="todo-input"
              placeholder="새로운 할 일을 입력하세요"
              class="flex-1 px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              required
            />
            <button
              type="submit"
              class="px-6 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 focus:outline-none focus:ring-2 focus:ring-blue-500"
            >
              추가
            </button>
          </div>
        </form>

        <!-- 필터 버튼들 -->
        <div class="flex gap-2 mb-4">
          <button id="filter-all" class="filter-btn active px-3 py-1 rounded-lg text-sm">
            전체
          </button>
          <button id="filter-active" class="filter-btn px-3 py-1 rounded-lg text-sm">미완료</button>
          <button id="filter-completed" class="filter-btn px-3 py-1 rounded-lg text-sm">
            완료
          </button>
        </div>

        <!-- 할 일 목록 -->
        <ul id="todo-list" class="space-y-2 mb-4"></ul>

        <!-- 상태 표시 -->
        <div class="flex justify-between items-center text-sm text-gray-600">
          <span id="todo-count">0개의 할 일</span>
          <button id="clear-completed" class="text-red-500 hover:text-red-700">
            완료된 항목 삭제
          </button>
        </div>
      </div>
    </div>

    <script src="todo-app.js"></script>
  </body>
</html>
```

---

## 기본 데이터 구조와 모듈 설계

애플리케이션의 핵심이 되는 데이터 구조와 기본적인 모듈을 설계해보겠습니다. 할 일 항목은 고유 ID, 내용, 완료 상태, 생성 시간 등의 정보를 포함해야 합니다.

```javascript title="todo-app.js"
// 할 일 관리 앱 - 기본 구조
class TodoApp {
  constructor() {
    this.todos = this.loadTodos();
    this.currentFilter = 'all';
    this.init();
  }

  // 초기화
  init() {
    this.bindEvents();
    this.render();
  }

  // 새로운 할 일 생성
  createTodo(text) {
    return {
      id: Date.now().toString(),
      text: text.trim(),
      completed: false,
      createdAt: new Date().toISOString(),
    };
  }

  // 로컬 스토리지에서 데이터 로드
  loadTodos() {
    try {
      const stored = localStorage.getItem('todos');
      return stored ? JSON.parse(stored) : [];
    } catch (error) {
      console.error('할 일 데이터 로드 실패:', error);
      return [];
    }
  }

  // 로컬 스토리지에 데이터 저장
  saveTodos() {
    try {
      localStorage.setItem('todos', JSON.stringify(this.todos));
    } catch (error) {
      console.error('할 일 데이터 저장 실패:', error);
    }
  }
}

// 앱 인스턴스 생성
const app = new TodoApp();
```

---

## 할 일 추가 기능 구현

사용자가 새로운 할 일을 입력하고 추가할 수 있는 기능을 구현해보겠습니다. 폼 제출 이벤트를 처리하고, 입력값 검증, 데이터 추가 및 UI 업데이트까지의 전체 과정을 다룹니다.

```javascript title="todo-app.js"
// TodoApp 클래스에 추가할 메서드들

// 이벤트 바인딩
bindEvents() {
    const form = document.getElementById('todo-form');
    const input = document.getElementById('todo-input');

    // 폼 제출 이벤트
    form.addEventListener('submit', (e) => {
        e.preventDefault();
        this.addTodo(input.value);
        input.value = '';
    });

    // 필터 버튼 이벤트
    document.getElementById('filter-all').addEventListener('click', () => {
        this.setFilter('all');
    });

    document.getElementById('filter-active').addEventListener('click', () => {
        this.setFilter('active');
    });

    document.getElementById('filter-completed').addEventListener('click', () => {
        this.setFilter('completed');
    });

    // 완료된 항목 삭제 버튼
    document.getElementById('clear-completed').addEventListener('click', () => {
        this.clearCompleted();
    });
}

// 할 일 추가
addTodo(text) {
    if (!text.trim()) return;

    const todo = this.createTodo(text);
    this.todos.unshift(todo); // 최신 항목을 위에 표시
    this.saveTodos();
    this.render();
}
```

---

## 할 일 목록 렌더링

저장된 할 일 데이터를 화면에 표시하는 렌더링 기능을 구현합니다. 각 할 일 항목은 체크박스, 텍스트, 삭제 버튼을 포함하며, 완료 상태에 따라 다른 스타일을 적용합니다.

```javascript title="todo-app.js"
// 할 일 목록 렌더링
render() {
    const todoList = document.getElementById('todo-list');
    const filteredTodos = this.getFilteredTodos();

    // 목록 초기화
    todoList.innerHTML = '';

    // 할 일이 없는 경우
    if (filteredTodos.length === 0) {
        this.renderEmptyState(todoList);
        this.updateCountDisplay();
        return;
    }

    // 각 할 일 항목 렌더링
    filteredTodos.forEach(todo => {
        const todoItem = this.createTodoElement(todo);
        todoList.appendChild(todoItem);
    });

    this.updateCountDisplay();
    this.updateFilterButtons();
}

// 빈 상태 렌더링
renderEmptyState(container) {
    const emptyDiv = document.createElement('div');
    emptyDiv.className = 'text-center py-8 text-gray-500';
    emptyDiv.innerHTML = `
        <p class="text-lg mb-2">📝</p>
        <p>할 일이 없습니다</p>
        <p class="text-sm">새로운 할 일을 추가해보세요!</p>
    `;
    container.appendChild(emptyDiv);
}
```

---

## 할 일 항목 UI 생성

개별 할 일 항목의 HTML 요소를 동적으로 생성하는 기능을 구현합니다. 체크박스로 완료 상태를 토글하고, 삭제 버튼으로 항목을 제거할 수 있도록 각 요소에 이벤트 리스너를 추가합니다.

```javascript title="todo-app.js"
// 할 일 항목 요소 생성
createTodoElement(todo) {
    const li = document.createElement('li');
    li.className = `flex items-center gap-3 p-3 border border-gray-200 rounded-lg ${
        todo.completed ? 'bg-gray-50' : 'bg-white'
    }`;
    li.dataset.id = todo.id;

    // 체크박스
    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.checked = todo.completed;
    checkbox.className = 'w-4 h-4 text-blue-600 rounded focus:ring-blue-500';
    checkbox.addEventListener('change', () => {
        this.toggleTodo(todo.id);
    });

    // 텍스트
    const textSpan = document.createElement('span');
    textSpan.textContent = todo.text;
    textSpan.className = `flex-1 ${
        todo.completed ? 'line-through text-gray-500' : 'text-gray-800'
    }`;

    // 삭제 버튼
    const deleteBtn = document.createElement('button');
    deleteBtn.innerHTML = '🗑️';
    deleteBtn.className = 'text-red-500 hover:text-red-700 p-1 rounded';
    deleteBtn.addEventListener('click', () => {
        this.deleteTodo(todo.id);
    });

    li.appendChild(checkbox);
    li.appendChild(textSpan);
    li.appendChild(deleteBtn);

    return li;
}
```

---

## 할 일 상태 관리 기능

할 일의 완료 상태 토글, 삭제, 필터링 등 상태를 변경하는 핵심 기능들을 구현합니다. 각 작업 후에는 데이터를 저장하고 화면을 다시 렌더링하여 일관성을 유지합니다.

```javascript title="todo-app.js"
// 할 일 완료 상태 토글
toggleTodo(id) {
    const todo = this.todos.find(t => t.id === id);
    if (todo) {
        todo.completed = !todo.completed;
        this.saveTodos();
        this.render();
    }
}

// 할 일 삭제
deleteTodo(id) {
    this.todos = this.todos.filter(t => t.id !== id);
    this.saveTodos();
    this.render();
}

// 완료된 할 일 모두 삭제
clearCompleted() {
    const completedCount = this.todos.filter(t => t.completed).length;

    if (completedCount === 0) {
        alert('완료된 할 일이 없습니다.');
        return;
    }

    if (confirm(`완료된 ${completedCount}개의 할 일을 삭제하시겠습니까?`)) {
        this.todos = this.todos.filter(t => !t.completed);
        this.saveTodos();
        this.render();
    }
}

// 필터 설정
setFilter(filter) {
    this.currentFilter = filter;
    this.render();
}

// 필터링된 할 일 목록 반환
getFilteredTodos() {
    switch (this.currentFilter) {
        case 'active':
            return this.todos.filter(t => !t.completed);
        case 'completed':
            return this.todos.filter(t => t.completed);
        default:
            return this.todos;
    }
}
```

---

## UI 상태 업데이트 기능

사용자에게 현재 상태를 명확히 보여주는 UI 업데이트 기능을 구현합니다. 남은 할 일 개수 표시, 활성 필터 버튼 하이라이트 등 사용자 경험을 개선하는 세부 기능들을 다룹니다.

```javascript title="todo-app.js"
// 할 일 개수 표시 업데이트
updateCountDisplay() {
    const countElement = document.getElementById('todo-count');
    const activeCount = this.todos.filter(t => !t.completed).length;
    const totalCount = this.todos.length;

    let countText;
    if (totalCount === 0) {
        countText = '할 일이 없습니다';
    } else if (activeCount === 0) {
        countText = '모든 할 일을 완료했습니다!';
    } else {
        countText = `${activeCount}개의 할 일이 남았습니다`;
    }

    countElement.textContent = countText;
}

// 필터 버튼 상태 업데이트
updateFilterButtons() {
    const buttons = document.querySelectorAll('.filter-btn');

    buttons.forEach(btn => {
        btn.classList.remove('active', 'bg-blue-500', 'text-white');
        btn.classList.add('bg-gray-200', 'text-gray-700', 'hover:bg-gray-300');
    });

    const activeButton = document.getElementById(`filter-${this.currentFilter}`);
    if (activeButton) {
        activeButton.classList.remove('bg-gray-200', 'text-gray-700', 'hover:bg-gray-300');
        activeButton.classList.add('active', 'bg-blue-500', 'text-white');
    }
}
```

---

## 고급 기능 추가

사용자 경험을 향상시키는 고급 기능들을 추가해보겠습니다. 할 일 편집, 드래그 앤 드롭 정렬, 키보드 단축키 등 실제 사용할 때 편리한 기능들을 구현합니다.

```javascript title="todo-app.js"
// 할 일 편집 기능
editTodo(id, newText) {
    const todo = this.todos.find(t => t.id === id);
    if (todo && newText.trim()) {
        todo.text = newText.trim();
        this.saveTodos();
        this.render();
    }
}

// 더블클릭으로 편집 모드 전환
enableEditMode(todoElement, todo) {
    const textSpan = todoElement.querySelector('span');
    const currentText = textSpan.textContent;

    const input = document.createElement('input');
    input.type = 'text';
    input.value = currentText;
    input.className = 'flex-1 px-2 py-1 border border-blue-500 rounded focus:outline-none';

    const saveEdit = () => {
        if (input.value.trim() && input.value !== currentText) {
            this.editTodo(todo.id, input.value);
        } else {
            this.render(); // 변경사항 없으면 원래 상태로
        }
    };

    input.addEventListener('blur', saveEdit);
    input.addEventListener('keydown', (e) => {
        if (e.key === 'Enter') {
            saveEdit();
        } else if (e.key === 'Escape') {
            this.render(); // ESC로 취소
        }
    });

    todoElement.replaceChild(input, textSpan);
    input.focus();
    input.select();
}
```

---

## 데이터 백업과 복원

사용자가 데이터를 안전하게 백업하고 복원할 수 있는 기능을 구현합니다. JSON 파일로 내보내기와 파일에서 가져오기 기능을 통해 데이터 이동성을 제공합니다.

```javascript title="todo-app.js"
// 데이터 내보내기 (백업)
exportData() {
    const data = {
        todos: this.todos,
        exportDate: new Date().toISOString(),
        version: '1.0'
    };

    const dataStr = JSON.stringify(data, null, 2);
    const dataBlob = new Blob([dataStr], { type: 'application/json' });

    const link = document.createElement('a');
    link.href = URL.createObjectURL(dataBlob);
    link.download = `todos_backup_${new Date().toISOString().split('T')[0]}.json`;
    link.click();

    URL.revokeObjectURL(link.href);
}

// 데이터 가져오기 (복원)
importData(file) {
    const reader = new FileReader();

    reader.onload = (e) => {
        try {
            const data = JSON.parse(e.target.result);

            if (data.todos && Array.isArray(data.todos)) {
                if (confirm('기존 데이터를 덮어쓰시겠습니까?')) {
                    this.todos = data.todos;
                    this.saveTodos();
                    this.render();
                    alert('데이터를 성공적으로 가져왔습니다.');
                }
            } else {
                alert('올바르지 않은 파일 형식입니다.');
            }
        } catch (error) {
            alert('파일을 읽는 중 오류가 발생했습니다.');
        }
    };

    reader.readAsText(file);
}
```

---

## 성능 최적화와 메모리 관리

애플리케이션의 성능을 개선하고 메모리 누수를 방지하는 최적화 기법들을 적용해보겠습니다. 이벤트 위임, 효율적인 렌더링, 메모리 관리 등의 기술을 사용합니다.

```javascript title="todo-app.js"
// 성능 최적화된 렌더링
renderOptimized() {
    const todoList = document.getElementById('todo-list');
    const filteredTodos = this.getFilteredTodos();

    // 기존 요소들의 ID 수집
    const existingIds = new Set(
        Array.from(todoList.children).map(el => el.dataset.id)
    );

    // 현재 필요한 ID 수집
    const currentIds = new Set(filteredTodos.map(todo => todo.id));

    // 제거할 요소들 삭제
    existingIds.forEach(id => {
        if (!currentIds.has(id)) {
            const element = todoList.querySelector(`[data-id="${id}"]`);
            if (element) element.remove();
        }
    });

    // 새로운 요소들 추가
    filteredTodos.forEach((todo, index) => {
        let element = todoList.querySelector(`[data-id="${todo.id}"]`);

        if (!element) {
            element = this.createTodoElement(todo);
            todoList.appendChild(element);
        }

        // 정확한 위치로 이동
        if (todoList.children[index] !== element) {
            todoList.insertBefore(element, todoList.children[index] || null);
        }
    });

    this.updateCountDisplay();
    this.updateFilterButtons();
}
```

---

## 접근성 개선

모든 사용자가 편리하게 사용할 수 있도록 웹 접근성을 개선하는 방법을 살펴봅니다. 키보드 탐색, 스크린 리더 지원, ARIA 속성 등을 적용하여 포용적인 웹 애플리케이션을 만듭니다.

```javascript title="todo-app.js"
// 접근성 개선된 할 일 항목 생성
createAccessibleTodoElement(todo) {
    const li = document.createElement('li');
    li.className = `flex items-center gap-3 p-3 border border-gray-200 rounded-lg ${
        todo.completed ? 'bg-gray-50' : 'bg-white'
    }`;
    li.dataset.id = todo.id;
    li.setAttribute('role', 'listitem');

    // 접근성 개선된 체크박스
    const checkbox = document.createElement('input');
    checkbox.type = 'checkbox';
    checkbox.id = `todo-${todo.id}`;
    checkbox.checked = todo.completed;
    checkbox.className = 'w-4 h-4 text-blue-600 rounded focus:ring-blue-500';
    checkbox.setAttribute('aria-describedby', `todo-text-${todo.id}`);

    // 라벨과 텍스트
    const label = document.createElement('label');
    label.htmlFor = `todo-${todo.id}`;
    label.className = 'flex-1 cursor-pointer';

    const textSpan = document.createElement('span');
    textSpan.id = `todo-text-${todo.id}`;
    textSpan.textContent = todo.text;
    textSpan.className = `${
        todo.completed ? 'line-through text-gray-500' : 'text-gray-800'
    }`;

    label.appendChild(textSpan);

    // 접근성 개선된 삭제 버튼
    const deleteBtn = document.createElement('button');
    deleteBtn.innerHTML = '🗑️';
    deleteBtn.className = 'text-red-500 hover:text-red-700 p-1 rounded focus:outline-none focus:ring-2 focus:ring-red-500';
    deleteBtn.setAttribute('aria-label', `"${todo.text}" 할 일 삭제`);
    deleteBtn.setAttribute('title', '할 일 삭제');

    // 이벤트 리스너 추가
    checkbox.addEventListener('change', () => this.toggleTodo(todo.id));
    deleteBtn.addEventListener('click', () => this.deleteTodo(todo.id));

    li.appendChild(checkbox);
    li.appendChild(label);
    li.appendChild(deleteBtn);

    return li;
}
```

---

## 마무리와 다음 단계

축하합니다! 완전히 작동하는 할 일 관리 앱을 성공적으로 구현했습니다. 이 프로젝트를 통해 DOM 조작, 이벤트 처리, 로컬 스토리지, 모던 JavaScript 문법 등 웹 개발의 핵심 기술들을 실전에서 활용해보았습니다.

### 학습한 핵심 개념들

이 프로젝트에서 다룬 주요 개념들을 정리해보면:

- **클래스 기반 애플리케이션 구조**: 객체 지향적 접근으로 코드를 체계적으로 구성
- **DOM 조작과 이벤트 처리**: 동적인 사용자 인터페이스 구현
- **로컬 스토리지**: 브라우저에서 데이터 영속성 관리
- **상태 관리**: 애플리케이션 상태의 일관성 유지
- **성능 최적화**: 효율적인 렌더링과 메모리 관리
- **접근성**: 모든 사용자를 고려한 포용적 디자인

### 추가 개선 아이디어

현재 앱을 더욱 발전시킬 수 있는 다양한 방향들:

- **우선순위 시스템**: 할 일에 중요도를 설정하고 정렬
- **카테고리/태그**: 할 일을 분류하고 태그로 관리
- **마감일 설정**: 날짜와 시간 기반 알림 시스템
- **드래그 앤 드롭**: 직관적인 순서 변경 기능
- **PWA 전환**: 모바일 앱처럼 설치 가능한 웹 앱
- **동기화**: 클라우드 저장소와 연동하여 기기간 동기화

다음 장에서는 API를 활용한 날씨 앱을 만들어보면서 비동기 처리와 외부 데이터 연동에 대해 더 깊이 학습해보겠습니다!
