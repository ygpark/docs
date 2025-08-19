---
sidebar_position: 22
---

# 스벨트킷 프로젝트 생성

이 섹션에서는 Node.js가 설치된 환경에서 스벨트킷 프로젝트를 생성하는 다양한 방법을 알아보겠습니다. 프로젝트 생성부터 초기 설정, 개발 서버 실행까지의 전체 과정을 단계별로 설명합니다.

## 1. 프로젝트 생성 방법

### create-svelte 사용 (권장)

가장 공식적이고 권장되는 방법으로, 최신 템플릿과 설정을 제공합니다.

```bash
# 새 프로젝트 생성
npm create svelte@latest my-sveltekit-app

# 또는 특정 디렉토리에 생성
npm create svelte@latest ./my-project

# yarn 사용 시
yarn create svelte my-sveltekit-app

# pnpm 사용 시
pnpm create svelte my-sveltekit-app
```

### 대화형 프로젝트 설정

`create-svelte` 명령을 실행하면 다음과 같은 대화형 설정이 시작됩니다:

```bash
┌  Welcome to SvelteKit!
│
◇  Which Svelte app template?
│  ● SvelteKit demo app (추천 - 데모 포함)
│  ○ Skeleton project (최소 구성)
│  ○ Library project (라이브러리 개발용)
│
◇  Add type checking with TypeScript?
│  ● Yes, using TypeScript syntax (추천)
│  ○ Yes, using JavaScript with JSDoc comments
│  ○ No
│
◇  Select additional options (use arrow keys/space bar)
│  ◻ Add ESLint for code linting
│  ◻ Add Prettier for code formatting
│  ◻ Add Playwright for browser testing
│  ◻ Add Vitest for unit testing
│  ◻ Try the Svelte 5 preview (experimental)
│
└  Your project is ready!
```

### 각 옵션 설명

#### 1. 프로젝트 템플릿

**SvelteKit demo app:**

```
my-app/
├── src/
│   ├── routes/
│   │   ├── +layout.svelte
│   │   ├── +page.svelte
│   │   ├── about/
│   │   │   └── +page.svelte
│   │   └── sverdle/
│   │       └── +page.svelte
│   ├── lib/
│   └── app.html
├── static/
├── package.json
└── svelte.config.js
```

**Skeleton project:**

```
my-app/
├── src/
│   ├── routes/
│   │   └── +page.svelte
│   ├── app.html
│   └── lib/
├── static/
├── package.json
└── svelte.config.js
```

**Library project:**

```
my-lib/
├── src/
│   └── lib/
│       └── index.js
├── package.json
└── svelte.config.js
```

#### 2. TypeScript 설정

**TypeScript syntax (권장):**

```typescript
// src/routes/+page.svelte
<script lang="ts">
  interface User {
    name: string;
    email: string;
  }

  let user: User = {
    name: 'John Doe',
    email: 'john@example.com'
  };
</script>

<h1>Hello, {user.name}!</h1>
<p>Email: {user.email}</p>
```

**JavaScript with JSDoc:**

```javascript
// src/routes/+page.svelte
<script>
  /**
   * @typedef {Object} User
   * @property {string} name
   * @property {string} email
   */

  /** @type {User} */
  let user = {
    name: 'John Doe',
    email: 'john@example.com'
  };
</script>
```

#### 3. 추가 도구들

**ESLint:** 코드 품질 검사
**Prettier:** 코드 포맷팅
**Playwright:** E2E 테스트
**Vitest:** 단위 테스트

## 2. 프로젝트 초기화 및 실행

### 의존성 설치

```bash
# 프로젝트 디렉토리로 이동
cd my-sveltekit-app

# 의존성 설치
npm install

# 또는
yarn install
# 또는
pnpm install
```

### 개발 서버 실행

```bash
# 개발 서버 시작
npm run dev

# 특정 포트에서 실행
npm run dev -- --port 3000

# 네트워크 접근 허용 (모바일 테스트용)
npm run dev -- --host

# 자동으로 브라우저 열기
npm run dev -- --open
```

개발 서버가 성공적으로 시작되면 다음과 같은 메시지가 표시됩니다:

```bash
  VITE v4.4.2  ready in 500 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help
```

## 3. 프로젝트 구조 상세 분석

### 기본 디렉토리 구조

```
my-sveltekit-app/
├── .svelte-kit/           # 빌드 관련 임시 파일 (Git 무시)
├── node_modules/          # 의존성 패키지
├── src/                   # 소스 코드
│   ├── routes/           # 페이지 라우팅
│   ├── lib/              # 재사용 가능한 컴포넌트/유틸리티
│   ├── app.html          # HTML 템플릿
│   └── app.d.ts          # TypeScript 타입 정의
├── static/               # 정적 파일 (이미지, 폰트 등)
├── tests/                # 테스트 파일
├── package.json          # 프로젝트 설정 및 의존성
├── svelte.config.js      # SvelteKit 설정
├── vite.config.js        # Vite 빌드 도구 설정
└── tsconfig.json         # TypeScript 설정
```

### 핵심 파일들 분석

#### package.json

```json
{
  "name": "my-sveltekit-app",
  "version": "0.0.1",
  "private": true,
  "scripts": {
    "build": "vite build",
    "dev": "vite dev",
    "preview": "vite preview",
    "check": "svelte-kit sync && svelte-check --tsconfig ./tsconfig.json",
    "check:watch": "svelte-kit sync && svelte-check --tsconfig ./tsconfig.json --watch",
    "lint": "prettier --plugin-search-dir . --check . && eslint .",
    "format": "prettier --plugin-search-dir . --write .",
    "test": "vitest",
    "test:integration": "playwright test"
  },
  "devDependencies": {
    "@sveltejs/adapter-auto": "^2.0.0",
    "@sveltejs/kit": "^1.20.4",
    "@typescript-eslint/eslint-plugin": "^6.0.0",
    "@typescript-eslint/parser": "^6.0.0",
    "eslint": "^8.28.0",
    "eslint-config-prettier": "^8.5.0",
    "eslint-plugin-svelte": "^2.30.0",
    "prettier": "^2.8.0",
    "prettier-plugin-svelte": "^2.10.1",
    "svelte": "^4.0.5",
    "svelte-check": "^3.4.3",
    "tslib": "^2.4.1",
    "typescript": "^5.0.0",
    "vite": "^4.4.2",
    "vitest": "^0.34.0"
  },
  "type": "module"
}
```

#### svelte.config.js

```javascript
import adapter from '@sveltejs/adapter-auto';
import { vitePreprocess } from '@sveltejs/kit/vite';

/** @type {import('@sveltejs/kit').Config} */
const config = {
  // Svelte 전처리기 설정 (TypeScript, SCSS 등)
  preprocess: vitePreprocess(),

  kit: {
    // 배포 환경에 맞는 어댑터 자동 선택
    adapter: adapter(),

    // 별칭 설정
    alias: {
      $components: 'src/lib/components',
      $utils: 'src/lib/utils',
    },

    // CSP (Content Security Policy) 설정
    csp: {
      mode: 'auto',
    },
  },
};

export default config;
```

#### vite.config.js

```javascript
import { sveltekit } from '@sveltejs/kit/vite';
import { defineConfig } from 'vite';

export default defineConfig({
  plugins: [sveltekit()],

  // 개발 서버 설정
  server: {
    port: 5173,
    strictPort: false,
    host: true, // 네트워크 접근 허용
  },

  // 빌드 설정
  build: {
    target: 'esnext',
  },

  // 테스트 설정
  test: {
    include: ['src/**/*.{test,spec}.{js,ts}'],
  },
});
```

#### app.html

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%sveltekit.assets%/favicon.png" />
    <meta name="viewport" content="width=device-width" />
    %sveltekit.head%
  </head>
  <body data-sveltekit-preload-data="hover">
    <div style="display: contents">%sveltekit.body%</div>
  </body>
</html>
```

## 4. 다양한 프로젝트 설정 예시

### 최소 설정 프로젝트

```bash
# 최소 구성으로 프로젝트 생성
npm create svelte@latest minimal-app
# Skeleton project 선택
# TypeScript: No
# 추가 옵션: 모두 체크 해제
```

### 풀스택 개발 환경

```bash
# 완전한 개발 환경으로 설정
npm create svelte@latest fullstack-app
# SvelteKit demo app 선택
# TypeScript: Yes, using TypeScript syntax
# 추가 옵션: 모두 체크
```

### 라이브러리 개발용

```bash
# 컴포넌트 라이브러리 개발용
npm create svelte@latest my-component-lib
# Library project 선택
# TypeScript: Yes
# ESLint, Prettier 선택
```

## 5. 프로젝트 설정 커스터마이징

### 커스텀 템플릿 사용

특정 GitHub 리포지토리를 템플릿으로 사용할 수 있습니다:

```bash
# GitHub 템플릿 사용
npx degit sveltejs/template my-svelte-project
npx degit your-username/your-template my-custom-project

# 특정 브랜치 사용
npx degit sveltejs/template#main my-project
```

### 수동 프로젝트 설정

완전히 처음부터 프로젝트를 설정하려면:

```bash
# 새 디렉토리 생성
mkdir my-manual-project
cd my-manual-project

# package.json 초기화
npm init -y

# SvelteKit 설치
npm install -D @sveltejs/kit @sveltejs/adapter-auto vite
npm install svelte

# 기본 디렉토리 구조 생성
mkdir -p src/routes src/lib static
```

```javascript
// svelte.config.js 생성
import adapter from '@sveltejs/adapter-auto';

export default {
  kit: {
    adapter: adapter(),
  },
};
```

```javascript
// vite.config.js 생성
import { sveltekit } from '@sveltejs/kit/vite';

export default {
  plugins: [sveltekit()],
};
```

```html
<!-- src/app.html 생성 -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width" />
    %sveltekit.head%
  </head>
  <body>
    %sveltekit.body%
  </body>
</html>
```

```jsx
<!-- src/routes/+page.svelte 생성 -->
<h1>Hello SvelteKit!</h1>
<p>This is a manually created project.</p>
```

## 6. 환경별 설정

### 개발 환경 설정

```javascript
// .env.local (개발 환경용)
VITE_API_URL=http://localhost:3001
VITE_DEBUG=true

// .env.production (프로덕션 환경용)
VITE_API_URL=https://api.myapp.com
VITE_DEBUG=false
```

```jsx
<!-- 환경 변수 사용 -->
<script>
  import { env } from '$env/dynamic/public';

  const apiUrl = env.VITE_API_URL;
  const isDebug = env.VITE_DEBUG === 'true';
</script>

{#if isDebug}
  <div class="debug">Debug mode: API URL is {apiUrl}</div>
{/if}
```

### Docker 설정

```dockerfile
# Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY . .
RUN npm run build

EXPOSE 3000
CMD ["node", "build"]
```

```yaml
# docker-compose.yml
version: '3.8'
services:
  sveltekit-app:
    build: .
    ports:
      - '3000:3000'
    environment:
      - NODE_ENV=production
    volumes:
      - ./static:/app/static
```

## 7. 프로젝트 검증 및 테스트

### 프로젝트 정상 작동 확인

```bash
# 의존성 체크
npm run check

# 린트 검사
npm run lint

# 포맷팅 확인
npm run format

# 타입 체크
npm run check

# 테스트 실행
npm run test
```

### 첫 번째 페이지 수정

기본 생성된 프로젝트에서 간단한 수정을 해보겠습니다:

```jsx
<!-- src/routes/+page.svelte -->
<script>
  let name = 'SvelteKit';
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<svelte:head>
  <title>My First SvelteKit App</title>
</svelte:head>

<main>
  <h1>Welcome to {name}!</h1>

  <div class="counter">
    <button on:click={increment}>
      Count: {count}
    </button>
  </div>

  <p>
    Visit <a href="/about">about page</a> to learn more.
  </p>
</main>

<style>
  main {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
    text-align: center;
  }

  h1 {
    color: #ff3e00;
    font-size: 3rem;
    margin-bottom: 2rem;
  }

  .counter button {
    background: #ff3e00;
    color: white;
    border: none;
    padding: 1rem 2rem;
    border-radius: 8px;
    font-size: 1.2rem;
    cursor: pointer;
    transition: background 0.2s;
  }

  .counter button:hover {
    background: #e63400;
  }

  a {
    color: #ff3e00;
    text-decoration: none;
  }

  a:hover {
    text-decoration: underline;
  }
</style>
```

### About 페이지 추가

```jsx
<!-- src/routes/about/+page.svelte -->
<script>
  const features = [
    'Fast builds with Vite',
    'Server-side rendering',
    'File-based routing',
    'TypeScript support',
    'Hot module replacement'
  ];
</script>

<svelte:head>
  <title>About - My SvelteKit App</title>
</svelte:head>

<main>
  <h1>About SvelteKit</h1>

  <p>
    SvelteKit is a framework for building web applications with Svelte.
    It provides many features out of the box:
  </p>

  <ul>
    {#each features as feature}
      <li>{feature}</li>
    {/each}
  </ul>

  <a href="/">← Back to home</a>
</main>

<style>
  main {
    max-width: 800px;
    margin: 0 auto;
    padding: 2rem;
  }

  h1 {
    color: #ff3e00;
    margin-bottom: 1rem;
  }

  ul {
    margin: 2rem 0;
  }

  li {
    margin: 0.5rem 0;
  }

  a {
    color: #ff3e00;
    text-decoration: none;
  }

  a:hover {
    text-decoration: underline;
  }
</style>
```

## 8. 문제 해결

### 일반적인 오류들

#### 포트 충돌

```bash
# 다른 포트 사용
npm run dev -- --port 3000

# 또는 환경 변수로 설정
PORT=3000 npm run dev
```

#### 모듈 찾기 오류

```bash
# 노드 모듈 재설치
rm -rf node_modules package-lock.json
npm install

# 캐시 클리어
npm cache clean --force
```

#### TypeScript 오류

```bash
# 타입 체크
npm run check

# svelte-kit sync 실행
npx svelte-kit sync
```

### 개발 서버 최적화

```javascript
// vite.config.js
export default defineConfig({
  plugins: [sveltekit()],
  server: {
    fs: {
      // 프로젝트 루트 외부 파일 접근 허용
      allow: ['..'],
    },
  },
  optimizeDeps: {
    // 특정 패키지 사전 번들링 제외
    exclude: ['@sveltejs/kit'],
  },
});
```

## 마무리

스벨트킷 프로젝트 생성은 매우 간단하지만, 올바른 설정을 통해 향후 개발 과정을 크게 개선할 수 있습니다.

**핵심 포인트:**

- `create-svelte`를 사용한 표준 프로젝트 생성
- TypeScript와 개발 도구 설정
- 프로젝트 구조 이해
- 환경별 설정 구분
- 기본 페이지 작성과 테스트

다음 섹션에서는 개발 도구 설정과 VS Code 환경 최적화에 대해 알아보겠습니다.
