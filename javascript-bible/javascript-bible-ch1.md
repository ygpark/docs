---
title: '1. 프로그래밍과 자바스크립트 시작하기'
slug: javascript-bible-chapter1-programming-javascript-start
description: '자바스크립트의 기본 개념부터 개발 환경 설정, 첫 번째 프로그램 작성까지 초급자를 위한 완벽한 가이드'
keywords:
  [
    '자바스크립트',
    '프로그래밍 시작',
    'JavaScript',
    '개발환경 설정',
    'VS Code',
    'Node.js',
    '브라우저 개발자 도구',
    'Hello World',
    'CodePen',
    '온라인 에디터',
    '웹 개발',
    '프론트엔드',
  ]
sidebar_position: 1
---

# 1장: 프로그래밍과 자바스크립트 시작하기

프로그래밍의 세계에 첫 발을 내딛는 여러분을 환영합니다! 이 장에서는 자바스크립트가 무엇인지, 왜 배워야 하는지, 그리고 어떻게 시작해야 하는지에 대해 알아보겠습니다. 자바스크립트는 웹 브라우저에서 시작되었지만, 이제는 서버, 모바일 앱, 데스크톱 애플리케이션까지 다양한 영역에서 사용되는 강력한 언어입니다. 여러분이 꿈꾸는 디지털 세상을 만들어가는 첫 걸음을 함께 시작해봅시다.

## 학습 목표

이 장을 마치면 다음과 같은 것들을 할 수 있게 됩니다:

- 자바스크립트가 무엇인지 이해하고 설명할 수 있습니다
- 개발에 필요한 환경을 설정하고 활용할 수 있습니다
- 브라우저 개발자 도구를 사용하여 간단한 프로그램을 실행할 수 있습니다
- 첫 번째 자바스크립트 프로그램을 작성하고 실행할 수 있습니다
- 온라인 에디터를 활용하여 실습할 수 있습니다

---

## 자바스크립트란 무엇인가?

자바스크립트는 1995년 넷스케이프의 브렌든 아이크(Brendan Eich)가 단 10일 만에 만든 프로그래밍 언어입니다. 처음에는 웹 페이지에 간단한 동적 기능을 추가하기 위해 만들어졌지만, 지금은 전 세계에서 가장 인기 있는 프로그래밍 언어 중 하나가 되었습니다.

### 자바스크립트의 특징

자바스크립트는 다음과 같은 독특한 특징들을 가지고 있습니다:

**인터프리터 언어**: 컴파일 과정 없이 바로 실행됩니다. 이는 빠른 개발과 테스트를 가능하게 합니다.

**동적 타입 언어**: 변수의 타입을 미리 정하지 않아도 됩니다. 실행 시점에 자동으로 타입이 결정됩니다.

**멀티 패러다임**: 절차적, 객체지향적, 함수형 프로그래밍 스타일을 모두 지원합니다.

**크로스 플랫폼**: 웹 브라우저, 서버(Node.js), 모바일 앱, 데스크톱 앱 등 다양한 환경에서 실행됩니다.

### 자바스크립트로 할 수 있는 일들

현대의 자바스크립트는 정말 다양한 일들을 할 수 있습니다:

- **웹사이트 제작**: 인터랙티브한 웹 페이지와 웹 애플리케이션
- **서버 개발**: Node.js를 이용한 백엔드 서비스
- **모바일 앱**: React Native, Ionic 등을 이용한 모바일 애플리케이션
- **데스크톱 앱**: Electron을 이용한 크로스 플랫폼 데스크톱 애플리케이션
- **게임 개발**: 브라우저 게임부터 3D 게임까지
- **인공지능**: TensorFlow.js를 이용한 머신러닝

---

## 개발 환경 설정 (VS Code, Node.js)

좋은 도구는 효율적인 개발의 시작입니다. 자바스크립트 개발을 위해 필요한 핵심 도구들을 설정해보겠습니다.

### Visual Studio Code 설치

Visual Studio Code(VS Code)는 마이크로소프트에서 개발한 무료 코드 에디터입니다. 가볍지만 강력한 기능들을 제공하여 전 세계 개발자들이 가장 많이 사용하는 에디터입니다.

**설치 방법:**

1. [https://code.visualstudio.com](https://code.visualstudio.com)에 접속
2. 운영체제에 맞는 버전 다운로드
3. 다운받은 파일을 실행하여 설치

**유용한 확장 프로그램:**

- **Live Server**: 실시간으로 HTML 파일을 브라우저에서 미리보기
- **JavaScript (ES6) code snippets**: 자바스크립트 코드 자동완성
- **Prettier**: 코드 자동 포맷팅
- **Bracket Pair Colorizer**: 괄호 쌍을 색상으로 구분

### Node.js 설치

Node.js는 브라우저 밖에서도 자바스크립트를 실행할 수 있게 해주는 런타임 환경입니다. 이를 통해 터미널에서 자바스크립트 파일을 실행하거나 npm(Node Package Manager)을 사용할 수 있습니다.

**설치 방법:**

1. [https://nodejs.org](https://nodejs.org)에 접속
2. LTS(Long Term Support) 버전 다운로드
3. 설치 후 터미널에서 확인

```bash title="terminal"
node --version
npm --version
```

설치가 정상적으로 완료되었다면 버전 번호가 출력됩니다.

---

## 브라우저 개발자 도구 활용

브라우저의 개발자 도구는 자바스크립트 학습과 디버깅에 매우 유용한 도구입니다. 모든 모던 브라우저에 내장되어 있어 별도 설치 없이 바로 사용할 수 있습니다.

### 개발자 도구 열기

**Windows/Linux**: `F12` 또는 `Ctrl + Shift + I`
**macOS**: `Cmd + Option + I`
**우클릭**: 페이지에서 우클릭 → "검사" 또는 "개발자 도구"

### Console 탭 활용하기

Console 탭은 자바스크립트 코드를 직접 입력하고 실행할 수 있는 REPL(Read-Eval-Print Loop) 환경을 제공합니다. 간단한 코드 테스트나 계산에 매우 유용합니다.

**기본 사용법 예제:**

```javascript title="browser-console"
// 간단한 계산
2 + 3;

// 변수 선언과 사용
let message = '안녕하세요!';
console.log(message);

// 함수 정의와 호출
function greet(name) {
  return `안녕하세요, ${name}님!`;
}
greet('자바스크립트');
```

Console은 즉석에서 자바스크립트의 동작을 확인하고 싶을 때 가장 빠르고 편리한 도구입니다. 복잡한 설정 없이 바로 코드를 실행해볼 수 있어 학습 초기에 특히 유용합니다.

---

## Hello, World! 첫 번째 프로그램

프로그래밍 언어를 배울 때의 전통적인 첫 번째 프로그램인 "Hello, World!"를 자바스크립트로 작성해보겠습니다. 이 간단한 프로그램을 통해 자바스크립트의 기본 문법과 실행 방법을 익힐 수 있습니다.

### 방법 1: HTML 파일에 포함하기

가장 기본적인 방법으로, HTML 파일 안에 자바스크립트 코드를 포함시키는 방법입니다.

```html title="hello-world.html"
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>나의 첫 번째 자바스크립트</title>
  </head>
  <body>
    <h1>자바스크립트 시작하기</h1>

    <script>
      // 브라우저 화면에 팝업으로 메시지 표시
      alert('Hello, World!');

      // 브라우저 콘솔에 메시지 출력
      console.log('안녕하세요, 자바스크립트!');

      // 웹 페이지에 직접 텍스트 추가
      document.write('<p>자바스크립트로 생성된 텍스트입니다!</p>');
    </script>
  </body>
</html>
```

### 방법 2: 별도 JavaScript 파일 사용하기

실제 개발에서는 HTML과 JavaScript를 분리하는 것이 좋은 관례입니다.

```javascript title="script.js"
// 콘솔에 메시지 출력
console.log('Hello, World!');

// 사용자 이름 입력받기
let userName = prompt('이름을 입력해주세요:');

// 개인화된 인사말
if (userName) {
  console.log(`안녕하세요, ${userName}님! 자바스크립트 세계에 오신 것을 환영합니다.`);
} else {
  console.log('이름을 입력해주시면 더 친근하게 인사드릴 수 있어요!');
}

// 현재 시간 표시
let now = new Date();
console.log(`현재 시간: ${now.toLocaleString()}`);
```

```html title="index.html"
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>외부 스크립트 사용하기</title>
  </head>
  <body>
    <h1>자바스크립트 외부 파일 연결</h1>
    <p>개발자 도구의 Console 탭을 확인해보세요!</p>

    <!-- 외부 JavaScript 파일 연결 -->
    <script src="script.js"></script>
  </body>
</html>
```

### 방법 3: Node.js로 실행하기

터미널에서 직접 자바스크립트 파일을 실행하는 방법입니다.

```javascript title="hello-node.js"
// Node.js 환경에서의 Hello World
console.log('Hello, World from Node.js!');

// 명령행 인수 처리
const args = process.argv.slice(2);
if (args.length > 0) {
  console.log(`안녕하세요, ${args[0]}님!`);
} else {
  console.log('사용법: node hello-node.js [이름]');
}

// 간단한 계산기 기능
function calculate(a, b, operation) {
  switch (operation) {
    case '+':
      return a + b;
    case '-':
      return a - b;
    case '*':
      return a * b;
    case '/':
      return b !== 0 ? a / b : '0으로 나눌 수 없습니다';
    default:
      return '지원하지 않는 연산입니다';
  }
}

console.log(`5 + 3 = ${calculate(5, 3, '+')}`);
console.log(`10 - 4 = ${calculate(10, 4, '-')}`);
```

터미널에서 실행:

```bash title="terminal"
node hello-node.js
node hello-node.js "개발자"
```

---

## 실습: CodePen과 온라인 에디터 활용법

온라인 에디터는 설치 과정 없이 바로 코딩을 시작할 수 있는 편리한 도구입니다. 특히 학습 초기나 간단한 실험을 할 때 매우 유용합니다. 여러 온라인 에디터의 특징과 활용법을 알아보고, 실제로 간단한 프로젝트를 만들어보겠습니다.

### 주요 온라인 에디터 소개

**CodePen (https://codepen.io)**

- HTML, CSS, JavaScript를 실시간으로 테스트
- 커뮤니티에서 다른 개발자들의 작품 구경 가능
- 무료 계정으로도 충분한 기능 제공

**JSFiddle (https://jsfiddle.net)**

- 간단한 인터페이스로 빠른 테스트에 적합
- 다양한 JavaScript 라이브러리 지원

**CodeSandbox (https://codesandbox.io)**

- 실제 프로젝트와 유사한 환경 제공
- React, Vue 등 프레임워크 템플릿 지원

### CodePen 실습: 인터랙티브 인사말 카드

CodePen을 사용하여 사용자와 상호작용하는 간단한 인사말 카드를 만들어보겠습니다. 이 예제를 통해 HTML, CSS, JavaScript가 어떻게 함께 작동하는지 경험할 수 있습니다.

**HTML 부분:**

```html title="codepen-html"
<div class="container">
  <div class="card">
    <h1 class="title">안녕하세요!</h1>
    <p class="message" id="greeting">자바스크립트 세계에 오신 것을 환영합니다!</p>

    <div class="input-group">
      <input type="text" id="nameInput" placeholder="이름을 입력해주세요" class="name-input" />
      <button onclick="updateGreeting()" class="btn">인사하기</button>
    </div>

    <div class="features">
      <button onclick="changeColor()" class="btn secondary">색상 변경</button>
      <button onclick="addEmoji()" class="btn secondary">이모지 추가</button>
    </div>

    <p class="info">현재 시간: <span id="currentTime"></span></p>
  </div>
</div>
```

**CSS 부분 (Tailwind CSS 스타일로 작성):**

```css title="codepen-css"
@import url('https://cdn.jsdelivr.net/npm/tailwindcss@2.2.19/dist/tailwind.min.css');

body {
  margin: 0;
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  display: flex;
  align-items: center;
  justify-content: center;
  font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
}

.container {
  @apply max-w-md mx-auto p-4;
}

.card {
  @apply bg-white rounded-lg shadow-xl p-8 text-center;
  animation: fadeIn 0.5s ease-in;
}

.title {
  @apply text-3xl font-bold text-gray-800 mb-4;
  color: #4f46e5;
}

.message {
  @apply text-lg text-gray-600 mb-6 leading-relaxed;
  min-height: 60px;
  transition: all 0.3s ease;
}

.input-group {
  @apply flex flex-col gap-3 mb-4;
}

.name-input {
  @apply px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500;
  transition: all 0.2s ease;
}

.name-input:focus {
  @apply border-blue-500;
}

.btn {
  @apply px-6 py-2 bg-blue-500 text-white rounded-lg hover:bg-blue-600 transition-colors;
  cursor: pointer;
  border: none;
  font-weight: 500;
}

.btn.secondary {
  @apply bg-gray-500 hover:bg-gray-600 text-sm px-4 py-1;
}

.features {
  @apply flex gap-2 justify-center mb-4;
}

.info {
  @apply text-sm text-gray-500 mt-4;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(20px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.message.highlight {
  @apply text-xl font-semibold;
  color: #059669;
  transform: scale(1.05);
}
```

**JavaScript 부분:**

```javascript title="codepen-js"
// 색상 배열
const colors = ['#4f46e5', '#059669', '#dc2626', '#7c3aed', '#ea580c'];
let colorIndex = 0;

// 이모지 배열
const emojis = ['👋', '🎉', '✨', '🌟', '💫', '🎈', '🎊'];

// 페이지 로드 시 현재 시간 표시
function updateTime() {
  const now = new Date();
  const timeString = now.toLocaleString('ko-KR', {
    year: 'numeric',
    month: 'long',
    day: 'numeric',
    hour: '2-digit',
    minute: '2-digit',
    second: '2-digit',
  });
  document.getElementById('currentTime').textContent = timeString;
}

// 인사말 업데이트 함수
function updateGreeting() {
  const nameInput = document.getElementById('nameInput');
  const greetingElement = document.getElementById('greeting');
  const name = nameInput.value.trim();

  if (name) {
    // 이름이 입력된 경우
    const messages = [
      `안녕하세요, ${name}님! 멋진 하루 보내세요!`,
      `${name}님, 자바스크립트 학습을 응원합니다!`,
      `환영합니다, ${name}님! 함께 코딩해봐요!`,
      `반갑습니다, ${name}님! 오늘도 화이팅!`,
    ];

    const randomMessage = messages[Math.floor(Math.random() * messages.length)];
    greetingElement.textContent = randomMessage;

    // 강조 효과 추가
    greetingElement.classList.add('highlight');

    // 입력 필드 초기화
    nameInput.value = '';

    // 2초 후 강조 효과 제거
    setTimeout(() => {
      greetingElement.classList.remove('highlight');
    }, 2000);
  } else {
    // 이름이 입력되지 않은 경우
    greetingElement.textContent = '이름을 입력해주시면 더 친근하게 인사드릴게요!';
    greetingElement.style.color = '#ef4444';

    // 2초 후 원래 색상으로 복원
    setTimeout(() => {
      greetingElement.style.color = '';
    }, 2000);
  }
}

// 색상 변경 함수
function changeColor() {
  const title = document.querySelector('.title');
  colorIndex = (colorIndex + 1) % colors.length;
  title.style.color = colors[colorIndex];

  // 카드에 살짝 애니메이션 효과
  const card = document.querySelector('.card');
  card.style.transform = 'scale(1.02)';
  setTimeout(() => {
    card.style.transform = 'scale(1)';
  }, 200);
}

// 이모지 추가 함수
function addEmoji() {
  const greetingElement = document.getElementById('greeting');
  const randomEmoji = emojis[Math.floor(Math.random() * emojis.length)];

  if (
    !greetingElement.textContent.includes('👋') &&
    !greetingElement.textContent.includes('🎉') &&
    !greetingElement.textContent.includes('✨')
  ) {
    greetingElement.textContent += ` ${randomEmoji}`;
  }
}

// Enter 키로 인사하기 기능
document.getElementById('nameInput').addEventListener('keypress', function (event) {
  if (event.key === 'Enter') {
    updateGreeting();
  }
});

// 1초마다 시간 업데이트
setInterval(updateTime, 1000);

// 페이지 로드 시 초기화
updateTime();

// 환영 메시지
console.log('🎉 CodePen 실습에 오신 것을 환영합니다!');
console.log('💡 개발자 도구를 열어서 콘솔 메시지도 확인해보세요!');
```

### 실습 안내

1. **CodePen 접속**: [https://codepen.io](https://codepen.io)에 접속하여 "Create" → "Pen" 클릭
2. **코드 입력**: 위의 HTML, CSS, JavaScript 코드를 각각 해당 패널에 복사
3. **실시간 미리보기**: 코드를 입력하면 오른쪽에서 실시간으로 결과 확인
4. **기능 테스트**:
   - 이름 입력 후 "인사하기" 버튼 클릭
   - "색상 변경" 버튼으로 제목 색상 변경
   - "이모지 추가" 버튼으로 메시지에 이모지 추가
   - Enter 키로도 인사하기 기능 작동 확인

이 실습을 통해 자바스크립트가 어떻게 웹 페이지를 동적으로 만드는지, 사용자와 어떻게 상호작용하는지 직접 경험할 수 있습니다.

---

## 마무리

첫 번째 장을 마무리하며, 여러분은 이제 자바스크립트 개발의 기본기를 갖추었습니다. 자바스크립트가 무엇인지 이해했고, 개발 환경을 설정했으며, 첫 번째 프로그램을 작성하고 실행해보았습니다.

**이 장에서 배운 주요 내용들:**

- 자바스크립트의 정의와 특징, 활용 분야
- VS Code와 Node.js 개발 환경 설정
- 브라우저 개발자 도구 활용법
- 다양한 방법으로 "Hello, World!" 프로그램 작성
- CodePen을 활용한 실습과 온라인 에디터 활용법

프로그래밍은 하루아침에 완성되는 것이 아닙니다. 꾸준한 연습과 호기심이 가장 중요합니다. 다음 장에서는 자바스크립트의 값, 타입, 연산자에 대해 자세히 알아보겠습니다. 계속해서 즐거운 코딩 여정을 함께해요!
