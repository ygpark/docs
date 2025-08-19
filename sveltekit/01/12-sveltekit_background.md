---
sidebar_position: 12
---

# 스벨트킷 등장 배경

스벨트킷이 왜 만들어졌는지 이해하는 것은 매우 중요합니다. 스벨트라는 훌륭한 컴포넌트 라이브러리가 있었는데도 왜 스벨트킷이 필요했을까요?

스벨트가 2016년에 처음 등장했을 때, 컴포넌트를 작성하고 관리하는 혁신적인 방법을 제시했습니다. 하지만 실제 웹 애플리케이션을 구축하려면 컴포넌트 시스템 외에도 많은 것들이 필요했습니다.

## 스벨트만의 한계

초기 스벨트는 컴포넌트를 만드는 데는 최고였지만, 실제 웹사이트를 만들려면 여러 가지 추가 작업이 필요했습니다.

### 1. 라우팅 문제

스벨트는 단일 페이지를 만드는 데는 완벽했지만, `/about`, `/contact` 같은 여러 페이지가 있는 웹사이트를 만들려면 별도의 라우팅 라이브러리가 필요했습니다.

```javascript title="스벨트만으로는 라우팅이 어려웠습니다"
import router from 'page';
import Home from './Home.svelte';
import About from './About.svelte';

let currentPage;

router('/', () => (currentPage = Home));
router('/about', () => (currentPage = About));
router.start();
```

:::tip 왜 문제였을까요?
라우팅 설정이 복잡하고, 컴포넌트와 분리되어 있어서 관리가 어려웠습니다. 또한 SEO에 중요한 서버 사이드 렌더링도 직접 구현해야 했습니다.
:::

### 2. 서버 사이드 렌더링의 어려움

구글 검색에 잘 노출되려면 서버에서 HTML을 미리 만들어주는 SSR이 필요했는데, 이를 직접 구현하기는 너무 복잡했습니다.

### 3. 복잡한 빌드 설정

실제 웹사이트를 만들려면 Webpack이나 Rollup 같은 도구를 설정해야 했는데, 이는 스벨트의 "간단함"과 맞지 않았습니다.

### 4. API 서버 분리

데이터베이스와 연결하려면 별도의 백엔드 서버를 만들어야 했습니다. 프론트엔드와 백엔드를 따로 관리하는 것은 번거로웠습니다.

## Sapper: 첫 번째 해결책

2018년, 스벨트 팀은 "스벨트로 실제 웹사이트를 만들기 어렵다"는 문제를 해결하기 위해 **Sapper**를 만들었습니다.

### Sapper가 해결해준 것들

- ✅ **쉬운 라우팅**: 폴더 구조 = URL 구조
- ✅ **자동 SSR**: SEO를 위한 서버 사이드 렌더링
- ✅ **정적 사이트 생성**: 블로그 같은 사이트에 최적
- ✅ **코드 스플리팅**: 페이지별로 나눠서 로딩
- ✅ **서비스 워커**: 오프라인 지원

```title="Sapper의 직관적인 폴더 구조"
src/routes/
├── index.svelte        # / 홈페이지
├── about.svelte        # /about 페이지
└── blog/
    ├── index.svelte    # /blog 목록 페이지
    └── [slug].svelte   # /blog/my-post 개별 글
```

### 하지만 Sapper도 완벽하지 않았습니다

Sapper는 스벨트의 문제들을 많이 해결했지만, 여전히 아쉬운 점들이 있었습니다.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="problems" label="주요 문제들">

**1. 복잡한 데이터 불러오기**

```javascript
// 이해하기 어려운 preload 함수
export async function preload(page, session) {
  const { slug } = page.params;
  const res = await this.fetch(`api/posts/${slug}`);

  if (res.status === 200) {
    return { post: await res.json() };
  } else {
    this.error(res.status, 'Post not found');
  }
}
```

**2. 오래된 빌드 도구**

- Webpack만 사용 가능
- 새로운 도구들과 호환 안됨
- 설정 변경이 어려움

**3. TypeScript 지원 부족**

- 타입 안전성 부족
- 개발 도구 지원 미흡

</TabItem>
<TabItem value="result" label="결과">

이런 문제들 때문에:

- 😰 배우기 어려웠음
- 🐌 개발 속도가 느렸음
- 🏢 기업에서 도입하기 부담스러웠음
- 🌱 커뮤니티 성장이 제한적이었음

</TabItem>
</Tabs>

## 더 나은 해결책이 필요했습니다

Sapper를 사용하는 동안 웹 개발 세계에는 새로운 변화들이 일어나고 있었습니다.

### 다른 프레임워크들의 발전

**Next.js (React 기반)**가 보여준 훌륭한 개발 경험:

```javascript title="Next.js의 간단한 API 만들기"
// pages/api/users.js
export default function handler(req, res) {
  res.status(200).json({ name: 'John Doe' });
}
```

**Nuxt.js (Vue 기반)**도 비슷하게 쉬운 방법들을 제공했습니다.

### 새로운 빌드 도구의 등장

**Vite**라는 혁신적인 도구가 나타났습니다:

- ⚡ 개발 서버가 엄청나게 빨라짐
- 🔧 다양한 프레임워크 지원
- 📦 최신 기술 적극 활용

:::info 개발자들의 요구
스벨트 개발자들도 다른 프레임워크만큼 쉽고 빠른 개발 경험을 원했습니다.
:::

## 스벨트킷의 탄생

2021년, 스벨트 팀은 결국 완전히 새로운 프레임워크를 만들기로 결정했습니다. Sapper의 문제들을 해결하고, 다른 프레임워크들의 장점도 가져오는 **스벨트킷**을 개발하기 시작했습니다.

### 스벨트킷이 추구한 목표들

**1. 정말 쉬운 개발**

- 설정 없이 바로 시작 가능
- 직관적이고 이해하기 쉬운 방법들
- 완벽한 TypeScript 지원

**2. 유연함과 성능 모두**

- 필요에 따라 SSR, SSG, SPA 선택 가능
- 페이지마다 다른 설정 적용 가능
- 자동으로 최적화된 코드 생성

**3. 웹 표준 따르기**

- 브라우저 기본 기능 최대 활용
- JavaScript 없이도 작동하는 기능들
- 미래에도 계속 사용할 수 있는 코드

### 스벨트킷의 혁신적인 특징들

#### 1. 한 줄로 시작하기

```bash title="정말 간단한 프로젝트 시작"
npm create svelte@latest my-app
```

더 이상 복잡한 설정은 필요 없습니다!

#### 2. 파일 이름으로 모든 것 표현

```title="폴더 구조만 보면 웹사이트 구조가 보입니다"
src/routes/
├── +page.svelte              # / 홈페이지
├── about/+page.svelte        # /about 페이지
├── blog/
│   ├── +page.svelte          # /blog 목록
│   └── [slug]/+page.svelte   # /blog/my-post 개별 글
└── api/
    └── posts/+server.js      # /api/posts API
```

#### 3. 간단한 데이터 가져오기

<Tabs>
<TabItem value="sapper" label="Sapper (복잡했음)">

```javascript
// 복잡하고 이해하기 어려웠음
export async function preload(page, session) {
  const { slug } = page.params;
  const res = await this.fetch(`api/posts/${slug}`);

  if (res.status === 200) {
    return { post: await res.json() };
  } else {
    this.error(res.status, 'Post not found');
  }
}
```

</TabItem>
<TabItem value="sveltekit" label="SvelteKit (간단함)">

```javascript
// 간단하고 직관적임
export async function load({ params, fetch }) {
  const response = await fetch(`/api/posts/${params.slug}`);
  const post = await response.json();

  return { post };
}
```

</TabItem>
</Tabs>

#### 4. 어디든 배포 가능

하나의 코드로 여러 곳에 배포할 수 있습니다:

- 🚀 Vercel, Netlify 같은 클라우드 서비스
- 🖥️ 일반 웹 서버
- 📄 정적 사이트
- ⚡ 서버리스 함수

## 스벨트킷이 모든 것을 해결했습니다

### 1. 설정의 복잡성 → 간단함

<Tabs>
<TabItem value="before" label="예전 (너무 복잡했음)">

```javascript title="Webpack 설정이 수십 줄..."
module.exports = {
  entry: './src/main.js',
  resolve: {
    alias: { svelte: path.resolve('node_modules', 'svelte') },
    extensions: ['.mjs', '.js', '.svelte'],
    mainFields: ['svelte', 'browser', 'module', 'main'],
  },
  module: {
    rules: [
      {
        test: /\.svelte$/,
        use: {
          loader: 'svelte-loader',
          options: { emitCss: true, hotReload: true },
        },
      },
    ],
  },
  // ... 계속 계속...
};
```

</TabItem>
<TabItem value="after" label="지금 (매우 간단함)">

```javascript title="설정이 한 줄!"
import { sveltekit } from '@sveltejs/kit/vite';

export default {
  plugins: [sveltekit()],
};
```

</TabItem>
</Tabs>

### 2. 일관된 방법 제공

이제 모든 작업이 비슷한 패턴으로 이루어집니다:

```javascript title="모든 것이 일관됩니다"
// 데이터 가져오기
export async function load() {
  /* ... */
}

// 폼 처리하기
export const actions = {
  default: async ({ request }) => {
    /* ... */
  },
};

// 에러 처리하기
export function handleError({ error, event }) {
  /* ... */
}
```

### 3. 자동으로 최적화

개발자가 신경 쓸 필요 없이 자동으로:

- 🔄 코드를 페이지별로 나눠서 로딩
- 🖼️ 이미지를 자동으로 최적화
- 📦 번들 크기를 최소화
- 💾 똑똑한 캐싱 적용

## 정리

스벨트킷이 탄생한 이유를 한 줄로 요약하면: **스벨트로 실제 웹사이트를 쉽게 만들고 싶었기 때문**입니다.

1. **스벨트** - 훌륭한 컴포넌트 만들기 도구
2. **Sapper** - 첫 번째 시도, 하지만 아직 부족함
3. **스벨트킷** - 드디어 완성된 해답

:::tip 스벨트킷의 가치
스벨트킷은 복잡한 설정은 숨기면서도, 필요할 때는 세밀하게 제어할 수 있는 완벽한 균형을 제공합니다. 이제 개발자는 설정 대신 실제 기능 구현에 집중할 수 있습니다.
:::

다음에는 스벨트킷이 다른 프레임워크들과 어떻게 다른지 자세히 알아보겠습니다.
