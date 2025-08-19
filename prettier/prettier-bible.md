# Prettier 독학

코드 스타일의 일관성은 개발자에게 필수적인 요소입니다. 매번 들여쓰기를 고민하고, 따옴표 스타일을 맞추고, 세미콜론을 붙였다 뺐다 하는 시간이 얼마나 비생산적인지 아시나요? Prettier는 이런 모든 고민을 덜어주는 자동 코드 포맷터입니다. 이 가이드를 통해 Prettier의 모든 기능을 체계적으로 배우고, 개발 워크플로우를 한층 더 효율적으로 만들어보세요.

**학습 목표:**

- Prettier의 기본 개념과 설치 방법 이해
- 다양한 설정 옵션을 활용한 맞춤형 포맷팅 구성
- 팀 협업을 위한 Prettier 환경 구축
- ESLint와의 통합 및 고급 활용법 습득

## 1. Prettier 기초 개념

코드 포맷팅은 개발에서 중요하지만 시간 소모적인 작업입니다. Prettier는 이런 반복적인 작업을 자동화하여 개발자가 로직에 집중할 수 있게 도와주는 도구입니다. 단순히 코드를 예쁘게 만드는 것을 넘어서, 팀 전체의 코드 스타일을 통일하고 코드 리뷰에서 스타일 관련 논의를 줄여주는 역할을 합니다.

### 1.1 Prettier란?

**코드 포맷터(Code Formatter)의 정의**

- 소스 코드의 형식을 자동으로 정리하는 도구
- 들여쓰기, 줄바꿈, 공백 등을 일관된 규칙으로 정리
- 코드의 의미는 변경하지 않고 형식만 개선

**Prettier의 특징과 장점**

- Opinionated: 설정 옵션이 제한적이어서 결정 피로 감소
- 다양한 언어 지원: JavaScript부터 HTML, CSS까지
- 에디터 통합: 대부분의 현대적 에디터에서 플러그인 지원
- Zero-config: 기본 설정만으로도 바로 사용 가능

**ESLint와의 차이점**

- ESLint: 코드 품질과 버그 방지에 중점 (논리적 오류 검출)
- Prettier: 코드 형식과 스타일에 중점 (시각적 일관성)
- 상호 보완적 관계: 함께 사용하여 시너지 효과 창출

**협업에서의 중요성**

- 코드 리뷰 시 스타일 논의 최소화
- 새로운 팀원의 빠른 적응 지원
- 일관된 코드베이스 유지로 가독성 향상

### 1.2 지원 언어 및 환경

**주요 지원 언어**

- JavaScript, TypeScript: 완벽한 지원
- HTML, CSS, SCSS, Less: 웹 기술 전반
- Vue.js, React JSX: 프레임워크별 특화 지원
- JSON, Markdown: 설정 파일 및 문서
- GraphQL, YAML: 추가 데이터 형식

## 2. 설치 및 환경설정

개발 환경에 Prettier를 설정하는 것은 생산성 향상의 첫걸음입니다. VS Code 확장 프로그램으로 시작하여 npm 패키지 설치까지, 단계별로 환경을 구성해보겠습니다. 올바른 설정은 이후 모든 작업의 기초가 되므로 신중하게 진행해야 합니다.

### 2.1 VS Code 확장 프로그램 설치

가장 간단한 시작 방법은 VS Code 확장 프로그램을 설치하는 것입니다.

```text title="설치 과정"
1. VS Code 열기
2. 확장(Extension) 탭 클릭 (Ctrl+Shift+X)
3. "Prettier - Code formatter" 검색
4. Install 클릭
5. VS Code 재시작 (권장)
```

**기본 포매터 설정**
VS Code에서 Prettier를 기본 포매터로 설정해야 자동 포맷팅이 작동합니다.

```json title="settings.json"
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true
}
```

### 2.2 npm 패키지 설치

프로젝트 차원에서 Prettier를 관리하려면 npm 패키지로 설치해야 합니다.

**프로젝트별 설치 (권장)**

```bash title="terminal"
npm install --save-dev --save-exact prettier
```

**전역 설치**

```bash title="terminal"
npm install -g prettier
```

**devDependencies로 설치하는 이유**

- 운영 환경에서는 불필요한 도구
- 빌드 크기 최적화
- 개발 도구와 운영 코드 분리

**버전 고정 (--save-exact) 권장사항**
Prettier는 마이너 버전 업데이트에서도 포맷팅 결과가 바뀔 수 있어 정확한 버전 고정이 중요합니다.

### 2.3 VS Code 설정

```json title="settings.json"
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

## 3. 설정 파일 및 옵션

Prettier의 진정한 힘은 세밀한 설정을 통해 프로젝트에 맞는 코딩 스타일을 정의할 수 있다는 점입니다. 설정 방법부터 주요 옵션들의 의미와 활용법을 알아보겠습니다. 올바른 설정은 팀의 코딩 컨벤션을 코드에 자동으로 반영하는 강력한 도구가 됩니다.

### 3.1 설정 방법 3가지

**1. CLI 옵션으로 설정**

```bash title="terminal"
prettier --single-quote --trailing-comma es5 --write src/**/*.js
```

**2. package.json의 prettier 필드**

```json title="package.json"
{
  "name": "my-project",
  "prettier": {
    "singleQuote": true,
    "trailingComma": "es5"
  }
}
```

**3. 별도 설정 파일 (.prettierrc)**

```json title=".prettierrc"
{
  "singleQuote": true,
  "trailingComma": "es5",
  "tabWidth": 2,
  "semi": true
}
```

### 3.2 주요 설정 옵션

#### 3.2.1 기본 포맷팅 옵션

**printWidth (기본값: 80)**
한 줄의 최대 글자 수를 설정합니다.

```json title=".prettierrc"
{
  "printWidth": 100
}
```

**tabWidth (기본값: 2)**
들여쓰기 크기를 설정합니다.

```json title=".prettierrc"
{
  "tabWidth": 4
}
```

**useTabs (기본값: false)**
탭 문자 사용 여부를 결정합니다.

```json title=".prettierrc"
{
  "useTabs": false
}
```

**semi (기본값: true)**
세미콜론 사용 여부를 설정합니다.

```javascript title="example.js (semi: true)"
const message = "Hello World";
console.log(message);
```

```javascript title="example.js (semi: false)"
const message = "Hello World";
console.log(message);
```

**singleQuote (기본값: false)**
홑따옴표와 쌍따옴표 선택을 설정합니다.

```javascript title="example.js (singleQuote: true)"
const greeting = "Hello World";
const template = `Welcome ${name}!`;
```

**trailingComma (기본값: "es5")**
후행 쉼표 정책을 설정합니다.

- "none": 후행 쉼표 없음
- "es5": ES5에서 유효한 곳에만
- "all": 가능한 모든 곳에

```javascript title="example.js (trailingComma: 'all')"
const config = {
  name: "myApp",
  version: "1.0.0",
  dependencies: ["react", "vue"],
};
```

#### 3.2.2 고급 포맷팅 옵션

**arrowParens (기본값: "always")**
화살표 함수의 매개변수 괄호 처리를 설정합니다.

```javascript title="example.js (arrowParens: 'always')"
const square = (x) => x * x;
const add = (a, b) => a + b;
```

```javascript title="example.js (arrowParens: 'avoid')"
const square = (x) => x * x;
const add = (a, b) => a + b;
```

**bracketSpacing (기본값: true)**
객체 리터럴의 괄호 내부 공백을 설정합니다.

```javascript title="example.js (bracketSpacing: true)"
const obj = { foo: "bar" };
```

```javascript title="example.js (bracketSpacing: false)"
const obj = { foo: "bar" };
```

### 3.3 설정 파일 형식

Prettier는 다양한 형식의 설정 파일을 지원합니다.

```json title=".prettierrc"
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5"
}
```

```javascript title="prettier.config.js"
module.exports = {
  printWidth: 80,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  singleQuote: true,
  trailingComma: "es5",
};
```

```yaml title=".prettierrc.yml"
printWidth: 80
tabWidth: 2
useTabs: false
semi: true
singleQuote: true
trailingComma: "es5"
```

## 4. CLI 사용법

명령줄에서 Prettier를 사용하는 것은 스크립트 자동화와 CI/CD 파이프라인 구축에 핵심적인 역할을 합니다. 단순한 파일 포맷팅부터 복잡한 배치 처리까지, CLI의 다양한 옵션을 활용하면 개발 워크플로우를 크게 향상시킬 수 있습니다.

### 4.1 기본 명령어

**단일 파일 포맷팅**

```bash title="terminal"
prettier index.js
```

**여러 파일 동시 처리**

```bash title="terminal"
prettier src/components/*.js src/utils/*.js
```

**Glob 패턴 사용**

```bash title="terminal"
prettier "src/**/*.{js,ts,jsx,tsx}"
```

**제자리 수정 (실제 파일 변경)**

```bash title="terminal"
prettier --write "src/**/*.js"
```

### 4.2 주요 CLI 옵션

**--check 옵션**
포맷팅이 필요한 파일이 있는지 확인만 하고 실제로는 수정하지 않습니다.

```bash title="terminal"
prettier --check "src/**/*.js"
```

**--list-different 옵션**
포맷팅이 필요한 파일들의 목록을 출력합니다.

```bash title="terminal"
prettier --list-different "src/**/*.js"
```

**--config 옵션**
특정 설정 파일을 지정합니다.

```bash title="terminal"
prettier --config ./configs/.prettierrc --write "src/**/*.js"
```

**실용적인 CLI 활용 예시**
CI/CD 환경에서 코드 스타일 검증을 위해 자주 사용되는 명령어입니다.

```bash title="terminal"
# 포맷팅 체크 후 실패 시 프로세스 종료
prettier --check "src/**/*.{js,ts,jsx,tsx}" || exit 1

# 변경이 필요한 파일 목록 확인
prettier --list-different "**/*.{js,ts,json,md}"
```

### 4.3 Glob 패턴 활용

**기본 패턴**

```bash title="terminal"
# 모든 JavaScript 파일
prettier "**/*.js"

# 특정 디렉토리의 TypeScript 파일
prettier "src/**/*.ts"

# 여러 확장자 동시 처리
prettier "**/*.{js,ts,jsx,tsx,json}"
```

**따옴표 사용 주의사항**
셸에서 glob 패턴이 확장되는 것을 방지하기 위해 따옴표를 사용해야 합니다.

## 5. .prettierignore 파일

모든 파일을 포맷팅할 필요는 없습니다. 빌드 결과물이나 외부 라이브러리, 특정 형식을 유지해야 하는 파일들은 Prettier 처리에서 제외해야 합니다. .prettierignore 파일을 통해 이런 파일들을 효율적으로 관리할 수 있습니다.

### 5.1 무시할 파일 지정

.gitignore와 동일한 문법을 사용합니다.

```text title=".prettierignore"
# Dependencies
node_modules/
bower_components/

# Build outputs
dist/
build/
*.min.js
*.bundle.js

# Generated files
coverage/
.nyc_output/

# Config files (sometimes)
webpack.config.js
babel.config.js

# Documentation
docs/
*.md
```

**자동으로 무시되는 항목들**

- node_modules 디렉토리
- .git 디렉토리
- .prettierignore에 명시된 파일들

### 5.2 주석을 통한 부분 무시

특정 코드 블록만 Prettier 처리에서 제외할 수 있습니다.

```javascript title="example.js"
// prettier-ignore
const uglyArray = [
  1,    2,
    3,      4,
      5
];

// 일반적인 코드는 포맷팅됨
const niceArray = [1, 2, 3, 4, 5];
```

```html title="example.html"
<!-- prettier-ignore -->
<div    class="keep-formatting"
     style="color: red;    background: blue;">
  Spacing preserved
</div>

<!-- 일반적인 HTML은 포맷팅됨 -->
<div class="formatted" style="color: green; background: yellow;">
  Nicely formatted
</div>
```

```css title="example.css"
/* prettier-ignore */
.ugly-rule    {
    color:red;
        background:blue;
}

/* 일반적인 CSS는 포맷팅됨 */
.nice-rule {
  color: green;
  background: yellow;
}
```

## 6. 에디터 통합

에디터와의 완벽한 통합은 Prettier의 진정한 가치를 실현하는 핵심입니다. 저장할 때마다 자동으로 코드가 정리되고, 키보드 단축키로 즉시 포맷팅을 적용할 수 있다면 개발 경험이 크게 향상됩니다. VS Code를 중심으로 최적의 설정 방법을 알아보겠습니다.

### 6.1 VS Code 설정 심화

**언어별 포매터 세밀 설정**

```json title="settings.json"
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,

  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode",
    "editor.formatOnSave": true
  },
  "[typescript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[html]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[css]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[json]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  },
  "[markdown]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

**키보드 단축키 설정**

```json title="keybindings.json"
[
  {
    "key": "ctrl+shift+f",
    "command": "editor.action.formatDocument",
    "when": "editorHasDocumentFormattingProvider && !editorReadonly"
  },
  {
    "key": "ctrl+k ctrl+f",
    "command": "editor.action.formatSelection",
    "when": "editorHasDocumentSelectionFormattingProvider && !editorReadonly"
  }
]
```

### 6.2 Prettier 로그 확인

문제가 발생했을 때 디버깅을 위한 로그 확인 방법입니다.

**Output 패널에서 로그 확인**

1. VS Code에서 Ctrl+Shift+U (Output 패널 열기)
2. 드롭다운에서 "Prettier" 선택
3. 포맷팅 과정의 상세 로그 확인

**일반적인 에러 메시지와 해결법**

- "No parser could be inferred": 파일 형식 인식 실패
- "Configuration error": 설정 파일 문법 오류
- "Parsing error": 코드 문법 오류로 포맷팅 불가

### 6.3 다른 에디터 지원

**WebStorm/IntelliJ**

- Settings > Tools > File Watchers에서 Prettier 설정
- 또는 Settings > Languages & Frameworks > JavaScript > Prettier

**Sublime Text**

- Package Control을 통해 JsPrettier 설치
- Preferences > Package Settings에서 설정

## 7. 협업 환경 구성

팀 개발에서 Prettier의 진정한 가치는 모든 개발자가 동일한 코드 스타일을 자동으로 적용할 수 있다는 점입니다. 개인의 선호도와 관계없이 일관된 코드베이스를 유지하고, 코드 리뷰에서 스타일 관련 논의를 최소화하여 더 중요한 로직과 설계에 집중할 수 있게 해줍니다.

### 7.1 팀 설정 통일

**프로젝트 루트에 설정 파일 배치**

```json title=".prettierrc"
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "quoteProps": "as-needed",
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

**Git을 통한 설정 관리**

```text title=".gitignore"
# Prettier ignore 파일은 버전 관리에 포함
!.prettierrc
!.prettierignore

# 개인별 에디터 설정은 제외
.vscode/settings.json
```

**설정 우선순위 이해**

1. CLI 옵션
2. .prettierrc 파일 (프로젝트 루트)
3. package.json의 prettier 필드
4. 에디터 기본 설정

### 7.2 npm scripts 활용

팀원들이 쉽게 사용할 수 있도록 npm scripts를 정의합니다.

```json title="package.json"
{
  "scripts": {
    "format": "prettier --write \"src/**/*.{js,ts,jsx,tsx,json,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{js,ts,jsx,tsx,json,css,md}\"",
    "format:staged": "lint-staged"
  },
  "lint-staged": {
    "*.{js,ts,jsx,tsx,json,css,md}": ["prettier --write", "git add"]
  }
}
```

**사용법**

```bash title="terminal"
# 전체 프로젝트 포맷팅
npm run format

# 포맷팅 필요 여부만 확인
npm run format:check

# 스테이징된 파일만 포맷팅
npm run format:staged
```

### 7.3 설정 패키지 공유

여러 프로젝트에서 동일한 설정을 사용하려면 공통 설정 패키지를 만들 수 있습니다.

```json title="@company/prettier-config/package.json"
{
  "name": "@company/prettier-config",
  "version": "1.0.0",
  "main": "index.js"
}
```

```javascript title="@company/prettier-config/index.js"
module.exports = {
  printWidth: 100,
  tabWidth: 2,
  useTabs: false,
  semi: true,
  singleQuote: true,
  trailingComma: "es5",
  bracketSpacing: true,
  arrowParens: "avoid",
  endOfLine: "lf",
};
```

**프로젝트에서 사용**

```json title="package.json"
{
  "prettier": "@company/prettier-config",
  "devDependencies": {
    "@company/prettier-config": "^1.0.0",
    "prettier": "^2.8.0"
  }
}
```

## 8. Git Hooks 연동

커밋 전에 자동으로 코드를 포맷팅하는 것은 팀의 코드 품질을 보장하는 가장 효과적인 방법 중 하나입니다. Git Hooks와 Prettier를 연동하면 잘못된 형식의 코드가 저장소에 들어가는 것을 원천적으로 방지할 수 있습니다.

### 8.1 Husky 설정

Husky는 Git Hooks를 쉽게 관리할 수 있게 해주는 도구입니다.

**설치 과정**

```bash title="terminal"
npm install --save-dev husky lint-staged
npx husky install
npm set-script prepare "husky install"
```

**pre-commit 훅 설정**

```bash title="terminal"
npx husky add .husky/pre-commit "npx lint-staged"
```

**lint-staged 설정**

```json title="package.json"
{
  "lint-staged": {
    "*.{js,ts,jsx,tsx}": ["prettier --write", "eslint --fix"],
    "*.{json,css,md}": ["prettier --write"]
  }
}
```

**실제 동작 예시**
개발자가 git commit을 실행하면 자동으로 스테이징된 파일들이 포맷팅되고 다시 스테이징됩니다.

```bash title="terminal"
git add .
git commit -m "Add new feature"
# husky가 자동으로 prettier와 eslint 실행
# 포맷팅된 파일들이 다시 스테이징됨
# 모든 검사 통과 후 커밋 완료
```

### 8.2 CI/CD 통합

**GitHub Actions에서 Prettier 체크**

```yaml title=".github/workflows/code-quality.yml"
name: Code Quality

on: [push, pull_request]

jobs:
  prettier:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "npm"

      - run: npm ci
      - run: npm run format:check

      - name: Comment PR if formatting needed
        if: failure()
        uses: actions/github-script@v6
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: '🎨 Please run `npm run format` to fix formatting issues.'
            })
```

## 9. ESLint와의 통합

ESLint는 코드 품질을, Prettier는 코드 스타일을 담당합니다. 이 두 도구를 함께 사용할 때 발생할 수 있는 충돌을 해결하고 최적의 개발 환경을 구축하는 방법을 알아보겠습니다. 올바른 통합은 코드의 품질과 일관성을 동시에 보장하는 강력한 개발 워크플로우를 만들어줍니다.

### 9.1 ESLint와 Prettier 충돌 해결

**eslint-config-prettier 설치**

```bash title="terminal"
npm install --save-dev eslint-config-prettier
```

**ESLint 설정에서 Prettier 규칙 비활성화**

```json title=".eslintrc.json"
{
  "extends": [
    "eslint:recommended",
    "@typescript-eslint/recommended",
    "prettier"
  ],
  "rules": {
    // Prettier와 충돌하는 규칙들이 자동으로 비활성화됨
  }
}
```

**주요 충돌 규칙들**

- `indent`: 들여쓰기 관련 규칙
- `quotes`: 따옴표 관련 규칙
- `semi`: 세미콜론 관련 규칙
- `comma-dangle`: 후행 쉼표 관련 규칙

### 9.2 동시 사용 방법

**순차 실행 스크립트**

```json title="package.json"
{
  "scripts": {
    "lint": "eslint src/**/*.{js,ts,jsx,tsx}",
    "lint:fix": "eslint src/**/*.{js,ts,jsx,tsx} --fix",
    "format": "prettier --write src/**/*.{js,ts,jsx,tsx}",
    "check": "npm run lint && npm run format:check",
    "fix": "npm run lint:fix && npm run format"
  }
}
```

**VS Code에서 동시 사용 설정**

```json title="settings.json"
{
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.formatDocument": true
  },
  "editor.formatOnSave": true,
  "eslint.alwaysShowStatus": true
}
```

**실용적인 활용 예시**
ESLint가 논리적 오류를 잡고, Prettier가 스타일을 정리하는 협력 관계입니다.

```javascript title="example.js"
// ESLint가 잡는 문제들
const unusedVariable = "never used"; // no-unused-vars
if (true) {
  // no-constant-condition
  console.log("always true");
}

// Prettier가 정리하는 문제들
const obj = { name: "John", age: 30 }; // 공백과 형식
const arr = [1, 2, 3, 4, 5]; // 배열 형식
```

정리 후:

```javascript title="example.js (정리 후)"
// ESLint 경고는 여전히 남음 (논리적 문제)
const unusedVariable = "never used"; // 경고: unused variable
if (true) {
  // 경고: constant condition
  console.log("always true");
}

// Prettier가 자동으로 정리
const obj = { name: "John", age: 30 };
const arr = [1, 2, 3, 4, 5];
```

## 10. 고급 활용법

Prettier의 기본 기능을 넘어서 더욱 강력한 개발 환경을 구축하는 방법들을 알아보겠습니다. 플러그인 활용부터 성능 최적화까지, 전문적인 개발 워크플로우를 위한 고급 기법들을 소개합니다.

### 10.1 Prettier Playground 활용

온라인에서 설정을 시각적으로 테스트할 수 있는 공식 도구입니다.

**사용법 (https://prettier.io/playground/)**

1. 웹사이트 접속
2. 코드 입력 또는 샘플 코드 선택
3. 우측 옵션 패널에서 설정 조정
4. 실시간으로 포맷팅 결과 확인
5. "Copy config JSON" 버튼으로 설정 복사

**활용 팁**

- 팀 설정 논의 시 시각적 비교 자료로 활용
- 새로운 프로젝트 시작 전 스타일 가이드 결정
- 복잡한 코드 구조에서의 포맷팅 결과 미리 확인

### 10.2 플러그인 활용

**@trivago/prettier-plugin-sort-imports**
import 문을 자동으로 정렬하는 플러그인입니다.

```bash title="terminal"
npm install --save-dev @trivago/prettier-plugin-sort-imports
```

```json title=".prettierrc"
{
  "plugins": ["@trivago/prettier-plugin-sort-imports"],
  "importOrder": [
    "^react$",
    "^react-dom$",
    "<THIRD_PARTY_MODULES>",
    "^@/(.*)$",
    "^[./]"
  ],
  "importOrderSeparation": true,
  "importOrderSortSpecifiers": true
}
```

**정렬 전:**

```javascript title="example.js"
import { useState } from "react";
import lodash from "lodash";
import { Button } from "./Button";
import axios from "axios";
import React from "react";
import { utils } from "@/utils";
```

**정렬 후:**

```javascript title="example.js"
import React from "react";
import { useState } from "react";

import axios from "axios";
import lodash from "lodash";

import { utils } from "@/utils";

import { Button } from "./Button";
```

### 10.3 캐시 활용

대용량 프로젝트에서 성능을 향상시키는 방법입니다.

**캐시 사용법**

```bash title="terminal"
prettier --cache --write "src/**/*.js"
```

**npm scripts에서 캐시 활용**

```json title="package.json"
{
  "scripts": {
    "format": "prettier --cache --write \"src/**/*.{js,ts,jsx,tsx}\"",
    "format:check": "prettier --cache --check \"src/**/*.{js,ts,jsx,tsx}\""
  }
}
```

**캐시 파일 관리**

- `.prettierCache` 파일이 생성됨
- .gitignore에 추가 권장
- CI 환경에서는 캐시 비활성화 고려

## 11. 문제 해결

실제 개발 중 마주치는 다양한 Prettier 관련 문제들과 해결책을 알아보겠습니다. 설정이 적용되지 않는 상황부터 성능 이슈까지, 실무에서 유용한 트러블슈팅 가이드를 제공합니다.

### 11.1 일반적인 문제

**설정이 적용되지 않을 때**

문제 증상:

- 저장해도 포맷팅이 되지 않음
- 설정한 스타일과 다르게 적용됨
- VS Code에서 "Format Document" 작동 안함

해결 방법:

1. VS Code 재시작
2. 기본 포매터 확인
3. 설정 파일 위치 및 문법 검증
4. 프로젝트 루트 경로 확인

```json title="settings.json 재점검"
{
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[javascript]": {
    "editor.defaultFormatter": "esbenp.prettier-vscode"
  }
}
```

**설정 우선순위 문제**
VS Code 사용자 설정이 프로젝트 설정을 덮어쓰는 경우가 있습니다.

**해결책:**

- 프로젝트별 .vscode/settings.json 사용
- 설정 충돌 여부 확인
- Command Palette에서 "Reload Window" 실행

### 11.2 성능 최적화

**대용량 파일 처리**

```text title=".prettierignore"
# 큰 JSON 파일들 제외
data/*.json
logs/

# 압축된 파일들
*.min.js
*.bundle.js

# 자동 생성 파일들
dist/
build/
coverage/
```

**선택적 포맷팅**

```bash title="terminal"
# 변경된 파일만 포맷팅
prettier --write $(git diff --name-only --diff-filter=ACM | grep -E '\.(js|ts|jsx|tsx)$')
```

### 11.3 디버깅 방법

**--debug-check 옵션 활용**

```bash title="terminal"
prettier --debug-check src/problematic-file.js
```

**상세 로그 확인**

```bash title="terminal"
prettier --loglevel debug --write src/**/*.js
```

**설정 파일 검증**

```javascript title="validate-prettier-config.js"
const prettier = require("prettier");
const fs = require("fs");

try {
  const config = JSON.parse(fs.readFileSync(".prettierrc", "utf8"));
  const resolved = prettier.resolveConfig.sync(process.cwd());
  console.log("Config is valid:", resolved);
} catch (error) {
  console.error("Config error:", error.message);
}
```

## 12. 실습 프로젝트

이론을 실제로 적용해보는 실습 시간입니다. 새로운 프로젝트에 Prettier를 설정하고, 팀 환경에서 활용하는 전체 워크플로우를 단계별로 진행해보겠습니다. 실습을 통해 앞서 배운 모든 내용을 종합적으로 적용하고 실무 역량을 키워보세요.

### 12.1 기본 설정 실습

**1단계: 새 프로젝트 생성**

```bash title="terminal"
mkdir prettier-practice
cd prettier-practice
npm init -y
```

**2단계: Prettier 설치**

```bash title="terminal"
npm install --save-dev --save-exact prettier
```

**3단계: 설정 파일 작성**

```json title=".prettierrc"
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

**4단계: 무시 파일 설정**

```text title=".prettierignore"
node_modules/
dist/
build/
coverage/
*.min.js
```

**5단계: npm scripts 추가**

```json title="package.json"
{
  "scripts": {
    "format": "prettier --write \"src/**/*.js\"",
    "format:check": "prettier --check \"src/**/*.js\""
  }
}
```

**6단계: 테스트 파일 작성**

```javascript title="src/example.js"
const messy = {
  name: "John",
  age: 30,
  hobbies: ["reading", "coding", "gaming"],
};

function greet(name) {
  return `Hello ${name}!`;
}

const arrow = (x, y) => {
  return x + y;
};

console.log(greet("World"));
```

**7단계: 포맷팅 실행**

```bash title="terminal"
npm run format
```

### 12.2 팀 프로젝트 실습

기존 프로젝트에 Prettier를 도입하는 시나리오입니다.

**팀 설정 합의 과정**

1. 코딩 스타일 가이드라인 논의
2. Prettier Playground에서 옵션 테스트
3. 팀 전체 승인 후 설정 파일 생성

**점진적 도입 전략**

```bash title="terminal"
# 1단계: 새 파일만 포맷팅
prettier --write "src/new-feature/**/*.js"

# 2단계: 특정 디렉토리 전체
prettier --write "src/components/**/*.js"

# 3단계: 전체 프로젝트 (주의 필요)
prettier --write "src/**/*.js"
```

### 12.3 고급 워크플로우 구축

**완전한 개발 환경 설정**

```json title="package.json"
{
  "scripts": {
    "dev": "webpack serve --mode development",
    "build": "webpack --mode production",
    "lint": "eslint src/**/*.{js,ts}",
    "lint:fix": "eslint src/**/*.{js,ts} --fix",
    "format": "prettier --write \"src/**/*.{js,ts,css,md}\"",
    "format:check": "prettier --check \"src/**/*.{js,ts,css,md}\"",
    "pre-commit": "lint-staged",
    "quality": "npm run lint && npm run format:check"
  },
  "lint-staged": {
    "*.{js,ts}": ["eslint --fix", "prettier --write"],
    "*.{css,md,json}": ["prettier --write"]
  }
}
```

**CI/CD 파이프라인 구성**

```yaml title=".github/workflows/ci.yml"
name: CI

on: [push, pull_request]

jobs:
  quality:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"
          cache: "npm"

      - run: npm ci
      - run: npm run lint
      - run: npm run format:check
      - run: npm run build
```

## 부록

### A. 공식 문서 링크

- **Prettier 공식 사이트**: https://prettier.io/
- **설정 옵션 레퍼런스**: https://prettier.io/docs/en/options.html
- **CLI 명령어 전체 목록**: https://prettier.io/docs/en/cli.html
- **에디터 통합 가이드**: https://prettier.io/docs/en/editors.html
- **Prettier Playground**: https://prettier.io/playground/

### B. 추천 설정 예시

**일반적인 JavaScript 프로젝트**

```json title=".prettierrc"
{
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

**React 프로젝트**

```json title=".prettierrc"
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "jsxSingleQuote": false,
  "trailingComma": "es5",
  "bracketSpacing": true,
  "jsxBracketSameLine": false,
  "arrowParens": "avoid"
}
```

**TypeScript 프로젝트**

```json title=".prettierrc"
{
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "semi": true,
  "singleQuote": true,
  "trailingComma": "all",
  "bracketSpacing": true,
  "arrowParens": "avoid"
}
```

### C. 자주 묻는 질문

**Q: Prettier와 ESLint 중 어느 것을 먼저 실행해야 하나요?**
A: ESLint를 먼저 실행하고 Prettier를 나중에 실행하는 것을 권장합니다. ESLint가 코드 품질 문제를 해결한 후 Prettier가 스타일을 정리하는 순서입니다.

**Q: 팀원마다 다른 에디터를 사용하는데 설정을 통일할 수 있나요?**
A: 프로젝트 루트에 .prettierrc 파일을 두면 에디터와 관계없이 동일한 포맷팅이 적용됩니다. EditorConfig와 함께 사용하면 더욱 완벽한 통일이 가능합니다.

**Q: 기존 대용량 프로젝트에 Prettier를 도입하는 좋은 방법은?**
A: 한 번에 전체를 변경하기보다는 새로운 파일부터 시작하거나, 디렉토리별로 점진적으로 적용하는 것을 권장합니다. Git blame 기록을 보존하려면 별도 커밋으로 포맷팅만 수행하세요.

**Q: Prettier가 내 코드를 원하지 않는 방식으로 포맷팅합니다.**
A: Prettier는 의도적으로 설정 옵션을 제한하여 논쟁을 줄이는 것이 철학입니다. 정말 필요한 경우에만 `// prettier-ignore` 주석을 사용하고, 대부분의 경우 Prettier의 결정을 따르는 것을 권장합니다.

이제 Prettier의 모든 기능을 활용하여 일관되고 아름다운 코드베이스를 만들어보세요. 자동화된 코드 포맷팅은 개발 생산성 향상의 첫걸음입니다!
