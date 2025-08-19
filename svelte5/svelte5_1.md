---
sidebar_position: 1
sidebar_label: 1. Svelte 기초 개념
---

# Svelte 5 레퍼런스

# 1. Svelte 기초 개념

## 1.1 Svelte 소개

### 컴파일러로서의 Svelte

Svelte는 런타임 프레임워크가 아닌 **컴파일 타임 도구**입니다. 빌드 시점에 Svelte 컴포넌트를 효율적인 바닐라 JavaScript로 변환하여 런타임 오버헤드를 최소화합니다.

```javascript
// 컴파일 전 (.svelte)
<script>
  let count = 0;
</script>

<button on:click={() => count++}>
  {count}
</button>

// 컴파일 후 (간소화된 예시)
function create_fragment(ctx) {
  let button;
  let t;

  return {
    c() {
      button = element("button");
      t = text(ctx[0]);
    },
    m(target, anchor) {
      insert(target, button, anchor);
      append(button, t);
    },
    p(ctx, dirty) {
      if (dirty & 1) set_data(t, ctx[0]);
    }
  };
}
```

### Virtual DOM vs 컴파일 타임 최적화

**Virtual DOM 방식 (React, Vue):**

- 런타임에 가상 DOM 트리를 생성하고 비교
- 메모리 사용량과 CPU 오버헤드 발생
- 동적이지만 성능 비용 존재

**Svelte의 컴파일 타임 최적화:**

- 컴파일 시점에 변경 사항을 정확히 추적
- 런타임에 최소한의 DOM 조작만 수행
- 더 작은 번들 크기와 빠른 실행 속도

```html
<!-- Svelte는 컴파일 시점에 name 변수가 변경될 때
     span 요소만 업데이트하는 코드를 생성합니다 -->
<script>
  let name = 'world';
</script>

<div>
  <h1>Hello!</h1>
  <span>{name}</span>
</div>
```

### Svelte의 철학과 특징

**핵심 철학:**

- **Write less, do more** - 간결한 문법으로 더 많은 기능 구현
- **No boilerplate** - 보일러플레이트 코드 최소화
- **Truly reactive** - 진정한 반응성을 통한 직관적인 상태 관리

**주요 특징:**

- **Zero-runtime overhead** - 런타임 프레임워크 코드 없음
- **Scoped CSS** - 컴포넌트별 CSS 스코핑 자동 처리
- **Built-in state management** - 별도 라이브러리 없이 상태 관리
- **Progressive enhancement** - 기존 HTML에 점진적 향상 가능

### Svelte 4 vs Svelte 5 주요 변화점

**반응성 시스템 개선:**

```javascript
// Svelte 4
let count = 0;
$: doubled = count * 2;

// Svelte 5 - Runes 시스템
let count = $state(0);
let doubled = $derived(count * 2);
```

**이벤트 핸들링 변화:**

```html
<!-- Svelte 4 -->
<button on:click="{handleClick}">Click me</button>

<!-- Svelte 5 -->
<button onclick="{handleClick}">Click me</button>
```

**Props 정의 방식:**

```javascript
// Svelte 4
export let title;
export let count = 0;

// Svelte 5
let { title, count = 0 } = $props();
```

**주요 신기능:**

- **Runes** - 새로운 반응성 프리미티브
- **Snippets** - 재사용 가능한 마크업 조각
- **Universal reactivity** - 컴포넌트 외부에서도 반응성 사용 가능
- **Better TypeScript support** - 향상된 타입 추론

## 1.2 개발 환경 설정

### Svelte 프로젝트 생성

**SvelteKit을 사용한 프로젝트 생성 (권장):**

```bash
npm create svelte@latest my-app
cd my-app
npm install
npm run dev
```

**Vite를 사용한 Svelte 프로젝트:**

```bash
npm create vite@latest my-svelte-app -- --template svelte
cd my-svelte-app
npm install
npm run dev
```

**수동 설정:**

```bash
npm init
npm install -D svelte @sveltejs/vite-plugin-svelte vite
```

```javascript
// vite.config.js
import { defineConfig } from 'vite';
import { svelte } from '@sveltejs/vite-plugin-svelte';

export default defineConfig({
  plugins: [svelte()],
});
```

### 개발 도구 설정

**VS Code 확장 프로그램:**

- **Svelte for VS Code** - 공식 Svelte 지원
- **Svelte Intellisense** - 자동 완성 및 문법 하이라이팅

**기본 설정 파일:**

```json
// .vscode/settings.json
{
  "svelte.enable-ts-plugin": true,
  "emmet.includeLanguages": {
    "svelte": "html"
  }
}
```

**ESLint 설정:**

```javascript
// eslint.config.js
import js from '@eslint/js';
import svelte from 'eslint-plugin-svelte';
import globals from 'globals';

export default [
  js.configs.recommended,
  ...svelte.configs['flat/recommended'],
  {
    languageOptions: {
      globals: {
        ...globals.browser,
        ...globals.node,
      },
    },
  },
];
```

**Prettier 설정:**

```json
// .prettierrc
{
  "useTabs": true,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100,
  "plugins": ["prettier-plugin-svelte"],
  "overrides": [
    {
      "files": "*.svelte",
      "options": {
        "parser": "svelte"
      }
    }
  ]
}
```

### 빌드 시스템 이해

**개발 모드 vs 프로덕션 빌드:**

```javascript
// vite.config.js
export default defineConfig({
  plugins: [
    svelte({
      compilerOptions: {
        // 개발 모드에서만 활성화
        dev: !process.env.NODE_ENV === 'production',
      },
    }),
  ],
  build: {
    // 프로덕션 빌드 최적화
    minify: 'terser',
    rollupOptions: {
      output: {
        manualChunks: {
          vendor: ['svelte'],
        },
      },
    },
  },
});
```

**컴파일러 옵션:**

```javascript
// svelte.config.js
export default {
  compilerOptions: {
    runes: true, // Svelte 5 runes 활성화
    dev: false, // 개발 모드 (기본값: NODE_ENV !== 'production')
    generate: 'dom', // 'dom' | 'ssr' | false
    hydratable: false, // SSR 하이드레이션 지원
    legacy: false, // Svelte 3/4 호환성
    css: 'injected', // 'injected' | 'external' | 'none'
    immutable: false, // 불변 데이터 가정
    accessors: false, // getter/setter 생성
  },
};
```

## 1.3 컴포넌트 파일 구조

### `.svelte` 파일의 3가지 블록

Svelte 컴포넌트는 세 가지 최상위 블록으로 구성됩니다:

```html
<!-- Component.svelte -->
<script>
  // JavaScript/TypeScript 로직
</script>

<style>
  /* CSS 스타일 */
</style>

<!-- HTML 마크업 -->
```

### `<script>`, `<style>`, HTML 영역

**`<script>` 블록:**

```html
<script>
  // Svelte 5 문법
  let { title, items = [] } = $props();
  let count = $state(0);
  let doubled = $derived(count * 2);

  function increment() {
    count++;
  }

  // 컴포넌트 라이프사이클
  import { onMount } from 'svelte';

  onMount(() => {
    console.log('컴포넌트가 마운트되었습니다');
  });
</script>
```

**`<style>` 블록:**

```html
<style>
  /* 컴포넌트 스코프 CSS (기본값) */
  h1 {
    color: blue;
    font-size: 2rem;
  }

  /* 전역 CSS */
  :global(body) {
    margin: 0;
    padding: 0;
  }

  /* CSS 변수 활용 */
  .container {
    --primary-color: #ff3e00;
    background: var(--primary-color);
  }

  /* 조건부 클래스 */
  .button {
    background: white;
  }

  .button.active {
    background: var(--primary-color);
  }
</style>
```

**HTML 마크업 영역:**

```html
<div class="container">
  <h1>{title}</h1>
  <p>Count: {count}</p>
  <p>Doubled: {doubled}</p>

  <button onclick="{increment}" class:active="{count">5} > Increment</button>

  {#if items.length > 0}
  <ul>
    {#each items as item, index}
    <li>{index}: {item}</li>
    {/each}
  </ul>
  {:else}
  <p>No items found</p>
  {/if}
</div>
```

### `.svelte.js` / `.svelte.ts` 파일

Svelte 5에서는 컴포넌트 로직을 별도 파일로 분리할 수 있습니다:

**utils.svelte.js:**

```javascript
// 반응성 상태를 모듈에서 내보내기
export const globalCount = $state(0);

export function createCounter(initial = 0) {
  let count = $state(initial);

  return {
    get count() {
      return count;
    },
    increment: () => count++,
    decrement: () => count--,
    reset: () => (count = initial),
  };
}

// 파생 상태
export const doubledGlobalCount = $derived(globalCount * 2);
```

**컴포넌트에서 사용:**

```html
<script>
  import { globalCount, createCounter } from './utils.svelte.js';

  const counter = createCounter(10);
</script>

<div>
  <p>Global: {globalCount}</p>
  <p>Local: {counter.count}</p>

  <button onclick="{()" ="">globalCount++}>Global +</button>
  <button onclick="{counter.increment}">Local +</button>
</div>
```

### 컴포넌트 작성 기본 규칙

**파일명 규칙:**

- PascalCase 사용: `MyComponent.svelte`
- 폴더 구조: `src/components/`, `src/routes/`

**Props 타입 정의 (TypeScript):**

```html
<script lang="ts">
  interface Props {
    title: string;
    count?: number;
    items: string[];
    onItemClick?: (item: string) => void;
  }

  let { title, count = 0, items, onItemClick }: Props = $props();
</script>
```

**이벤트 핸들링 규칙:**

```html
<script>
  let { onCustomEvent } = $props();

  function handleClick(event) {
    // 기본 이벤트 처리
    console.log('Button clicked', event);

    // 커스텀 이벤트 발생
    onCustomEvent?.('some data');
  }
</script>

<button onclick="{handleClick}">Click me</button>
```

**컴포넌트 내보내기:**

```html
<!-- Button.svelte -->
<script>
  let { variant = 'primary', children, ...restProps } = $props();
</script>

<button class="btn btn-{variant}" {...restProps}>{@render children()}</button>

<style>
  .btn {
    padding: 0.5rem 1rem;
    border: none;
    border-radius: 4px;
    cursor: pointer;
  }

  .btn-primary {
    background: #007bff;
    color: white;
  }

  .btn-secondary {
    background: #6c757d;
    color: white;
  }
</style>
```
