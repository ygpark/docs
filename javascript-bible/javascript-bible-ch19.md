---
title: '부록 A: 개발 환경 설정 가이드'
slug: javascript-bible-dev-environment-setup-guide
description: '모던 자바스크립트 개발을 위한 완전한 환경 설정 가이드 - Windows, macOS, Linux 환경별 설정부터 VS Code, Node.js, 브라우저 개발자 도구까지'
keywords:
  [
    '자바스크립트 개발환경',
    'Node.js 설치',
    'VS Code 설정',
    '브라우저 개발자도구',
    'JavaScript 개발',
    '온라인 에디터',
    'CodeSandbox',
    'StackBlitz',
    '개발 도구 설정',
  ]
sidebar_position: 1
---

# 부록 A: 개발 환경 설정 가이드

자바스크립트 개발을 시작하기 전에 올바른 개발 환경을 구축하는 것은 매우 중요합니다. 이 가이드에서는 운영체제별 설정 방법부터 필수 도구들의 설치와 설정, 그리고 온라인 개발 환경까지 단계별로 안내합니다. 초보자도 쉽게 따라할 수 있도록 각 과정을 상세히 설명하고, 실제 개발에서 자주 사용하는 설정들을 포함했습니다.

---

## 1. 운영체제별 기본 설정

### Windows 환경 설정

Windows에서 자바스크립트 개발을 시작하려면 먼저 기본적인 개발 도구들을 설치해야 합니다. PowerShell을 관리자 권한으로 실행하여 패키지 매니저를 설치하는 것부터 시작하겠습니다.

```powershell title="PowerShell (관리자 권한)"
# Chocolatey 패키지 매니저 설치
Set-ExecutionPolicy Bypass -Scope Process -Force
iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Git 설치
choco install git -y

# Node.js 설치
choco install nodejs -y

# VS Code 설치
choco install vscode -y

# 시스템 재시작 후 설치 확인
node --version
npm --version
git --version
```

```bash title="Windows Terminal 설정"
# Windows Terminal 설치 (Microsoft Store에서)
# 프로파일 설정 파일 위치: %LOCALAPPDATA%\Packages\Microsoft.WindowsTerminal_8wekyb3d8bbwe\LocalState\settings.json

# PowerShell 프로파일 생성
New-Item -Type File -Path $PROFILE -Force

# 유용한 별칭 추가
echo 'Set-Alias ll ls' >> $PROFILE
echo 'Set-Alias grep findstr' >> $PROFILE

# Git 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
```

### macOS 환경 설정

macOS에서는 Homebrew 패키지 매니저를 통해 개발 도구들을 쉽게 설치할 수 있습니다. Terminal 앱을 열고 다음 명령어들을 순서대로 실행하세요.

```bash title="Terminal"
# Homebrew 설치
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Homebrew 경로 설정
echo 'export PATH="/opt/homebrew/bin:$PATH"' >> ~/.zshrc
source ~/.zshrc

# 개발 도구 설치
brew install node
brew install git
brew install --cask visual-studio-code

# 설치 확인
node --version
npm --version
git --version
```

```bash title="macOS 개발 환경 최적화"
# Xcode Command Line Tools 설치
xcode-select --install

# Oh My Zsh 설치 (선택사항)
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# 유용한 별칭 설정
echo 'alias ll="ls -la"' >> ~/.zshrc
echo 'alias grep="grep --color=auto"' >> ~/.zshrc

# Git 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main
```

### Linux (Ubuntu/Debian) 환경 설정

Linux 환경에서는 apt 패키지 매니저와 NodeSource를 통해 최신 Node.js를 설치합니다. 터미널을 열고 다음 과정을 따라하세요.

```bash title="Terminal"
# 시스템 업데이트
sudo apt update && sudo apt upgrade -y

# 필수 패키지 설치
sudo apt install -y curl wget software-properties-common apt-transport-https

# NodeSource 저장소 추가 (최신 Node.js)
curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

# Node.js와 개발 도구 설치
sudo apt install -y nodejs git build-essential

# VS Code 설치
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list

sudo apt update
sudo apt install code
```

```bash title="Linux 개발 환경 설정"
# Zsh 설치 및 설정 (선택사항)
sudo apt install zsh
chsh -s $(which zsh)

# 개발용 별칭 설정
echo 'alias ll="ls -la"' >> ~/.bashrc
echo 'alias la="ls -A"' >> ~/.bashrc
echo 'alias l="ls -CF"' >> ~/.bashrc

# Git 전역 설정
git config --global user.name "Your Name"
git config --global user.email "your.email@example.com"
git config --global init.defaultBranch main

# 설치 확인
echo "Node.js: $(node --version)"
echo "npm: $(npm --version)"
echo "Git: $(git --version)"
```

---

## 2. Node.js 상세 설정

### Node.js 버전 관리

개발 프로젝트마다 다른 Node.js 버전이 필요한 경우가 있습니다. 버전 관리자를 사용하면 여러 버전을 쉽게 전환할 수 있습니다.

```bash title="NVM (Node Version Manager) 설치"
# Windows (nvm-windows)
# GitHub에서 nvm-setup.zip 다운로드 후 설치
# https://github.com/coreybutler/nvm-windows

# macOS/Linux (nvm)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 터미널 재시작 후 사용법
nvm install node        # 최신 버전 설치
nvm install 18.17.0     # 특정 버전 설치
nvm use 18.17.0         # 버전 전환
nvm alias default 18.17.0  # 기본 버전 설정
nvm list               # 설치된 버전 확인
```

```bash title="npm 글로벌 패키지 설정"
# npm 캐시 확인 및 정리
npm cache verify
npm cache clean --force

# 유용한 글로벌 패키지 설치
npm install -g nodemon          # 개발 서버 자동 재시작
npm install -g http-server      # 간단한 HTTP 서버
npm install -g live-server      # 라이브 리로드 서버
npm install -g json-server      # JSON API 모킹 서버

# 설치된 글로벌 패키지 확인
npm list -g --depth=0

# npm 설정 확인
npm config list
```

### Package.json 프로젝트 초기 설정

새로운 자바스크립트 프로젝트를 시작할 때 사용하는 기본 설정입니다. 프로젝트 폴더를 만들고 다음 과정을 따라하세요.

```bash title="프로젝트 초기화"
# 새 프로젝트 폴더 생성
mkdir my-js-project
cd my-js-project

# package.json 생성 (대화형)
npm init

# 또는 기본값으로 빠른 생성
npm init -y

# 개발용 의존성 설치
npm install --save-dev eslint prettier nodemon

# 프로덕션 의존성 예시
npm install lodash axios
```

```json title="package.json 예시"
{
  "name": "my-js-project",
  "version": "1.0.0",
  "description": "모던 자바스크립트 실습 프로젝트",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "dev": "nodemon index.js",
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint": "eslint .",
    "format": "prettier --write ."
  },
  "keywords": ["javascript", "es6", "modern"],
  "author": "Your Name",
  "license": "MIT",
  "devDependencies": {
    "eslint": "^8.45.0",
    "nodemon": "^3.0.1",
    "prettier": "^3.0.0"
  },
  "dependencies": {
    "axios": "^1.4.0",
    "lodash": "^4.17.21"
  }
}
```

---

## 3. VS Code 완벽 설정

### 필수 확장 프로그램

VS Code의 강력함은 확장 프로그램에서 나옵니다. 자바스크립트 개발에 필수적인 확장 프로그램들을 설치하고 설정해보겠습니다.

```bash title="VS Code 확장 프로그램 설치 (명령어)"
# 터미널에서 확장 프로그램 설치
code --install-extension ms-vscode.vscode-typescript-next
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-eslint
code --install-extension ms-vscode.live-server
code --install-extension formulahendry.auto-rename-tag
code --install-extension ms-vscode.vscode-json
code --install-extension oderwat.indent-rainbow
code --install-extension pkief.material-icon-theme

# 설치된 확장 프로그램 확인
code --list-extensions
```

```json title="VS Code settings.json 설정"
{
  "editor.fontSize": 14,
  "editor.fontFamily": "'Fira Code', 'Cascadia Code', Consolas, monospace",
  "editor.fontLigatures": true,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": true,
  "editor.bracketPairColorization.enabled": true,
  "editor.guides.bracketPairs": true,
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  },
  "files.autoSave": "onFocusChange",
  "emmet.includeLanguages": {
    "javascript": "javascriptreact"
  },
  "javascript.suggest.autoImports": true,
  "javascript.updateImportsOnFileMove.enabled": "always"
}
```

### 사용자 정의 스니펫

반복적으로 사용하는 코드 패턴을 스니펫으로 만들어 생산성을 높일 수 있습니다. VS Code에서 사용자 정의 스니펫을 만들어보겠습니다.

```json title="javascript.json (사용자 스니펫)"
{
  "Console log": {
    "prefix": "clg",
    "body": ["console.log('$1:', $1);"],
    "description": "콘솔 로그 출력"
  },
  "Arrow function": {
    "prefix": "arrf",
    "body": ["const $1 = ($2) => {", "  $3", "};"],
    "description": "화살표 함수 생성"
  },
  "Async function": {
    "prefix": "asyncf",
    "body": [
      "const $1 = async ($2) => {",
      "  try {",
      "    $3",
      "  } catch (error) {",
      "    console.error('Error:', error);",
      "  }",
      "};"
    ],
    "description": "비동기 함수 with try-catch"
  },
  "Import statement": {
    "prefix": "imp",
    "body": ["import { $2 } from '$1';"],
    "description": "ES6 import 문"
  },
  "Export default": {
    "prefix": "exp",
    "body": ["export default $1;"],
    "description": "기본 내보내기"
  }
}
```

```json title="html.json (HTML 스니펫)"
{
  "HTML5 Boilerplate": {
    "prefix": "html5",
    "body": [
      "<!DOCTYPE html>",
      "<html lang=\"ko\">",
      "<head>",
      "  <meta charset=\"UTF-8\">",
      "  <meta name=\"viewport\" content=\"width=device-width, initial-scale=1.0\">",
      "  <title>$1</title>",
      "  <script src=\"https://cdn.tailwindcss.com\"></script>",
      "</head>",
      "<body class=\"bg-gray-100 min-h-screen\">",
      "  $2",
      "  <script src=\"script.js\"></script>",
      "</body>",
      "</html>"
    ],
    "description": "Tailwind CSS가 포함된 HTML5 기본 템플릿"
  }
}
```

---

## 4. 브라우저 개발자 도구

### Chrome DevTools 마스터하기

Chrome 개발자 도구는 자바스크립트 개발에서 가장 중요한 디버깅 도구입니다. 각 패널의 기능과 활용법을 알아보겠습니다.

```html title="디버깅 실습용 HTML"
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>DevTools 실습</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-gray-100 p-8">
    <div class="max-w-md mx-auto bg-white rounded-lg shadow-md p-6">
      <h1 class="text-2xl font-bold text-gray-800 mb-4">계산기</h1>
      <input
        type="number"
        id="num1"
        class="w-full p-2 border rounded mb-2"
        placeholder="첫 번째 숫자"
      />
      <input
        type="number"
        id="num2"
        class="w-full p-2 border rounded mb-2"
        placeholder="두 번째 숫자"
      />
      <button
        onclick="calculate()"
        class="w-full bg-blue-500 text-white p-2 rounded hover:bg-blue-600"
      >
        계산하기
      </button>
      <div id="result" class="mt-4 p-2 bg-gray-50 rounded"></div>
    </div>
  </body>
</html>
```

```javascript title="script.js (디버깅 실습용)"
// 전역 변수 (Sources 패널에서 확인)
let calculationHistory = [];

// 계산 함수 (브레이크포인트 설정 연습)
function calculate() {
  debugger; // 자동 브레이크포인트

  const num1 = parseFloat(document.getElementById('num1').value);
  const num2 = parseFloat(document.getElementById('num2').value);

  // Console에서 변수 확인: num1, num2
  console.log('입력값:', { num1, num2 });

  if (isNaN(num1) || isNaN(num2)) {
    showResult('올바른 숫자를 입력하세요!', 'error');
    return;
  }

  const result = num1 + num2;

  // Application 패널에서 확인할 로컬 스토리지
  const calculation = {
    num1,
    num2,
    result,
    timestamp: new Date().toLocaleString(),
  };

  calculationHistory.push(calculation);
  localStorage.setItem('calculations', JSON.stringify(calculationHistory));

  showResult(`결과: ${result}`, 'success');

  // Performance 패널에서 확인할 수 있는 성능 마크
  performance.mark('calculation-end');
}
```

### 개발자 도구 활용 팁

각 브라우저별로 개발자 도구의 특징과 유용한 기능들을 알아보겠습니다. 실제 개발에서 자주 사용하는 팁들을 정리했습니다.

```javascript title="디버깅 유틸리티 함수들"
// 콘솔에서 사용할 수 있는 유틸리티 함수들
const debug = {
  // 객체를 테이블 형태로 출력
  table: data => console.table(data),

  // 성능 측정
  time: label => console.time(label),
  timeEnd: label => console.timeEnd(label),

  // 스택 트레이스 출력
  trace: message => console.trace(message),

  // 조건부 로깅
  assert: (condition, message) => console.assert(condition, message),

  // 그룹으로 로그 정리
  group: label => {
    console.group(label);
    return {
      log: console.log,
      end: console.groupEnd,
    };
  },
};

// 사용 예시
debug.time('함수 실행 시간');
// 함수 실행...
debug.timeEnd('함수 실행 시간');

debug.table([
  { name: 'Alice', age: 25 },
  { name: 'Bob', age: 30 },
]);
```

```javascript title="네트워크 및 성능 모니터링"
// Fetch 요청 모니터링
const monitoredFetch = async (url, options = {}) => {
  const startTime = performance.now();

  try {
    console.group(`🌐 API 요청: ${url}`);
    console.log('요청 옵션:', options);

    const response = await fetch(url, options);
    const endTime = performance.now();

    console.log(`✅ 응답 시간: ${(endTime - startTime).toFixed(2)}ms`);
    console.log('응답 상태:', response.status);
    console.log('응답 헤더:', [...response.headers.entries()]);

    return response;
  } catch (error) {
    console.error('❌ 요청 실패:', error);
    throw error;
  } finally {
    console.groupEnd();
  }
};

// 메모리 사용량 모니터링 (Chrome)
const checkMemory = () => {
  if (performance.memory) {
    console.log('메모리 사용량:', {
      used: `${(performance.memory.usedJSHeapSize / 1048576).toFixed(2)} MB`,
      total: `${(performance.memory.totalJSHeapSize / 1048576).toFixed(2)} MB`,
      limit: `${(performance.memory.jsHeapSizeLimit / 1048576).toFixed(2)} MB`,
    });
  }
};
```

---

## 5. 온라인 개발 환경

### CodeSandbox 활용법

CodeSandbox는 브라우저에서 바로 사용할 수 있는 강력한 온라인 개발 환경입니다. 빠른 프로토타이핑과 학습에 매우 유용합니다.

```javascript title="CodeSandbox JavaScript 프로젝트 예시"
// index.js
import './styles.css';

// DOM 요소 생성 함수
const createElement = (tag, className, textContent) => {
  const element = document.createElement(tag);
  if (className) element.className = className;
  if (textContent) element.textContent = textContent;
  return element;
};

// 앱 초기화
const initApp = () => {
  const app = document.getElementById('app');

  // 헤더 생성
  const header = createElement('h1', 'header', '온라인 개발 환경 실습');

  // 인터랙티브 버튼
  const button = createElement('button', 'btn btn-primary', '클릭하세요!');
  let clickCount = 0;

  button.addEventListener('click', () => {
    clickCount++;
    updateCounter(clickCount);
  });

  // 카운터 표시
  const counter = createElement('p', 'counter', `클릭 횟수: ${clickCount}`);

  // DOM에 추가
  app.appendChild(header);
  app.appendChild(button);
  app.appendChild(counter);
};

const updateCounter = count => {
  const counter = document.querySelector('.counter');
  counter.textContent = `클릭 횟수: ${count}`;
};

// 앱 시작
document.addEventListener('DOMContentLoaded', initApp);
```

```css title="styles.css (Tailwind 대신 순수 CSS)"
/* CodeSandbox용 스타일 */
body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Roboto', sans-serif;
  margin: 0;
  padding: 20px;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
}

#app {
  background: white;
  padding: 2rem;
  border-radius: 10px;
  box-shadow: 0 10px 25px rgba(0, 0, 0, 0.2);
  text-align: center;
  max-width: 400px;
}

.header {
  color: #333;
  margin-bottom: 1.5rem;
}

.btn {
  padding: 12px 24px;
  border: none;
  border-radius: 6px;
  cursor: pointer;
  font-size: 16px;
  transition: all 0.3s ease;
}

.btn-primary {
  background: #4285f4;
  color: white;
}

.btn-primary:hover {
  background: #3367d6;
  transform: translateY(-2px);
}

.counter {
  margin-top: 1rem;
  font-size: 18px;
  color: #666;
}
```

### StackBlitz와 Replit 활용

다양한 온라인 개발 환경의 특징과 장단점을 비교하고, 상황에 맞는 선택 방법을 알아보겠습니다.

```javascript title="StackBlitz Vite 프로젝트 예시"
// main.js
import { createApp } from './mini-framework.js';

const todos = [
  { id: 1, text: '자바스크립트 공부하기', completed: false },
  { id: 2, text: '프로젝트 만들기', completed: true },
];

// 간단한 상태 관리
const state = {
  todos,
  filter: 'all', // all, active, completed
};

// 액션 정의
const actions = {
  addTodo: text => {
    state.todos.push({
      id: Date.now(),
      text,
      completed: false,
    });
    render();
  },

  toggleTodo: id => {
    const todo = state.todos.find(t => t.id === id);
    if (todo) {
      todo.completed = !todo.completed;
      render();
    }
  },

  setFilter: filter => {
    state.filter = filter;
    render();
  },
};

// 렌더링 함수
const render = () => {
  const app = document.getElementById('app');
  const filteredTodos = getFilteredTodos();

  app.innerHTML = `
    <div class="todo-app">
      <h1>할 일 관리</h1>
      <div class="input-section">
        <input type="text" id="todoInput" placeholder="새 할 일 입력">
        <button onclick="addTodoFromInput()">추가</button>
      </div>
      <div class="filter-section">
        <button onclick="actions.setFilter('all')" ${state.filter === 'all' ? 'class="active"' : ''}>전체</button>
        <button onclick="actions.setFilter('active')" ${state.filter === 'active' ? 'class="active"' : ''}>진행중</button>
        <button onclick="actions.setFilter('completed')" ${state.filter === 'completed' ? 'class="active"' : ''}>완료</button>
      </div>
      <ul class="todo-list">
        ${filteredTodos
          .map(
            todo => `
          <li class="todo-item ${todo.completed ? 'completed' : ''}">
            <input type="checkbox" ${todo.completed ? 'checked' : ''} 
                   onchange="actions.toggleTodo(${todo.id})">
            <span>${todo.text}</span>
          </li>
        `
          )
          .join('')}
      </ul>
    </div>
  `;
};

// 전역 함수 (인라인 이벤트용)
window.actions = actions;
window.addTodoFromInput = () => {
  const input = document.getElementById('todoInput');
  if (input.value.trim()) {
    actions.addTodo(input.value.trim());
    input.value = '';
  }
};

const getFilteredTodos = () => {
  switch (state.filter) {
    case 'active':
      return state.todos.filter(t => !t.completed);
    case 'completed':
      return state.todos.filter(t => t.completed);
    default:
      return state.todos;
  }
};

// 초기 렌더링
render();
```

---

## 6. 실전 환경 구축

### 프로젝트 템플릿 생성

실제 개발에서 사용하는 프로젝트 템플릿을 만들어 보겠습니다. 이 템플릿은 ESLint, Prettier, 그리고 기본적인 빌드 설정을 포함합니다.

```bash title="프로젝트 템플릿 스크립트"
#!/bin/bash
# create-js-project.sh

PROJECT_NAME=$1

if [ -z "$PROJECT_NAME" ]; then
  echo "사용법: ./create-js-project.sh [프로젝트명]"
  exit 1
fi

# 프로젝트 폴더 생성
mkdir $PROJECT_NAME
cd $PROJECT_NAME

# package.json 생성
npm init -y

# 의존성 설치
npm install --save-dev eslint prettier eslint-config-prettier eslint-plugin-prettier nodemon
npm install --save-dev @babel/core @babel/preset-env babel-loader webpack webpack-cli webpack-dev-server

# 기본 폴더 구조 생성
mkdir src public
touch src/index.js src/styles.css public/index.html

echo "✅ 프로젝트 '$PROJECT_NAME' 생성 완료!"
echo "📁 cd $PROJECT_NAME && npm run dev"
```

```json title=".eslintrc.json"
{
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": ["eslint:recommended", "prettier"],
  "plugins": ["prettier"],
  "parserOptions": {
    "ecmaVersion": 2021,
    "sourceType": "module"
  },
  "rules": {
    "prettier/prettier": "error",
    "no-unused-vars": "warn",
    "no-console": "warn",
    "prefer-const": "error",
    "no-var": "error"
  },
  "ignorePatterns": ["dist/", "node_modules/"]
}
```

### 환경별 설정 파일

개발, 스테이징, 프로덕션 환경별로 다른 설정을 관리하는 방법을 알아보겠습니다.

```javascript title="config/environment.js"
// 환경별 설정 관리
const environments = {
  development: {
    API_URL: 'http://localhost:3000/api',
    DEBUG: true,
    LOG_LEVEL: 'debug',
  },

  staging: {
    API_URL: 'https://staging-api.example.com',
    DEBUG: true,
    LOG_LEVEL: 'info',
  },

  production: {
    API_URL: 'https://api.example.com',
    DEBUG: false,
    LOG_LEVEL: 'error',
  },
};

// 현재 환경 감지
const getCurrentEnvironment = () => {
  if (typeof window !== 'undefined') {
    // 브라우저 환경
    if (window.location.hostname === 'localhost') return 'development';
    if (window.location.hostname.includes('staging')) return 'staging';
    return 'production';
  } else {
    // Node.js 환경
    return process.env.NODE_ENV || 'development';
  }
};

const currentEnv = getCurrentEnvironment();
export const config = environments[currentEnv];

// 로깅 유틸리티
export const logger = {
  debug: (...args) => config.DEBUG && console.log('[DEBUG]', ...args),
  info: (...args) => console.info('[INFO]', ...args),
  warn: (...args) => console.warn('[WARN]', ...args),
  error: (...args) => console.error('[ERROR]', ...args),
};
```

이 가이드를 통해 자바스크립트 개발을 위한 완전한 환경을 구축할 수 있습니다. 각 운영체제별 특성을 고려한 설정 방법부터 실전에서 사용하는 도구들의 활용법까지 체계적으로 다루었습니다. 이제 여러분만의 개발 환경을 구축하고 모던 자바스크립트 개발의 여정을 시작해보세요!
