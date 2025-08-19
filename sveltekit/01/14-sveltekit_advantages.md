---
sidebar_position: 14
---

# 장점과 특징

스벨트킷은 스벨트의 컴파일러 기반 철학을 풀스택 웹 개발 영역으로 확장한 혁신적인 프레임워크입니다. 이 섹션에서는 스벨트킷만의 독특한 장점과 특징들을 살펴보겠습니다.

## 제로 설정 개발 환경

스벨트킷의 가장 인상적인 특징 중 하나는 **복잡한 설정 없이 바로 시작할 수 있다**는 점입니다. 과거에는 웹팩 설정, 라우팅 설정, SSR 설정 등을 모두 개발자가 직접 해야 했지만, 스벨트킷은 이 모든 것을 추상화했습니다.

```bash
# 단 한 줄로 프로젝트 시작
npm create svelte@latest my-app
cd my-app
npm install
npm run dev
```

이것만으로 파일 기반 라우팅, SSR, 핫 모듈 교체가 모두 포함된 완전한 개발 환경이 준비됩니다.

## 직관적인 파일 시스템 규칙

스벨트킷은 **convention over configuration** 원칙을 따라 파일 이름과 위치만으로 기능을 정의할 수 있습니다.

```
src/routes/
├── +page.svelte              # GET / (홈페이지)
├── +layout.svelte            # 모든 하위 페이지의 공통 레이아웃
├── +error.svelte             # 에러 페이지
├── about/
│   └── +page.svelte          # GET /about
├── blog/
│   ├── +page.svelte          # GET /blog
│   ├── +page.server.js       # 서버에서 데이터 로드
│   └── [slug]/
│       ├── +page.svelte      # GET /blog/[slug]
│       └── +page.js          # 클라이언트/서버 공통 로드
└── api/
    └── posts/
        └── +server.js        # API 엔드포인트 /api/posts
```

이런 명명 규칙을 통해 코드를 보지 않고도 애플리케이션의 구조를 파악할 수 있습니다.

## 유연한 렌더링 전략

스벨트킷은 **페이지별로 다른 렌더링 전략**을 선택할 수 있는 유연성을 제공합니다. 하나의 애플리케이션에서도 필요에 따라 다양한 방식을 혼용할 수 있습니다.

```javascript
// +page.js - 특정 페이지의 렌더링 옵션 설정
export const ssr = false; // 클라이언트 사이드 렌더링
export const prerender = true; // 빌드 타임에 정적 생성
export const csr = true; // 클라이언트 사이드 라우팅 활성화
```

| 페이지 유형   | SSR | SSG | CSR | 최적 사용 사례            |
| ------------- | --- | --- | --- | ------------------------- |
| 블로그 포스트 | ✗   | ✓   | ✓   | SEO 중요, 정적 콘텐츠     |
| 대시보드      | ✓   | ✗   | ✓   | 실시간 데이터, 인증 필요  |
| 랜딩 페이지   | ✗   | ✓   | ✓   | 빠른 로딩, 마케팅         |
| 관리자 패널   | ✗   | ✗   | ✓   | SPA 경험, 복잡한 상호작용 |

## 통합된 데이터 로딩 시스템

스벨트킷의 `load` 함수는 **서버와 클라이언트에서 일관된 방식**으로 데이터를 처리할 수 있게 해줍니다.

```javascript
// +page.server.js - 서버에서만 실행
export async function load({ params, url, cookies }) {
  // 민감한 데이터 처리 (API 키, 데이터베이스 접근)
  const secret = process.env.SECRET_KEY;
  const user = await getUserFromDatabase(cookies.get('session'));

  return {
    user: {
      name: user.name,
      email: user.email,
      // 민감한 정보는 제외하고 반환
    },
  };
}
```

```javascript
// +page.js - 서버와 클라이언트 모두에서 실행
export async function load({ params, fetch, parent }) {
  // 부모 레이아웃의 데이터 활용
  const { user } = await parent();

  // 공개 API 호출
  const posts = await fetch(`/api/posts?author=${user.id}`).then(r => r.json());

  return {
    posts,
  };
}
```

## 어댑터를 통한 플랫폼 독립성

스벨트킷의 **어댑터 시스템**은 동일한 코드베이스를 다양한 플랫폼에 배포할 수 있게 해줍니다.

```javascript title="svelte.config.js"
import adapter from '@sveltejs/adapter-auto';
// import adapter from '@sveltejs/adapter-static';
// import adapter from '@sveltejs/adapter-node';
// import adapter from '@sveltejs/adapter-vercel';

export default {
  kit: {
    adapter: adapter(),
  },
};
```

:::tip 어댑터 자동 선택
`@sveltejs/adapter-auto`는 배포 환경을 자동으로 감지하여 최적의 어댑터를 선택합니다. Vercel에서는 Vercel 어댑터를, Netlify에서는 Netlify 어댑터를 자동으로 사용합니다.
:::

## 진보적 향상과 웹 표준 준수

스벨트킷은 **JavaScript가 비활성화되어도 작동하는 웹사이트**를 만들 수 있도록 진보적 향상을 지원합니다.

```html
<!-- 폼은 JavaScript 없이도 작동 -->
<form method="POST" action="?/login">
  <input name="email" type="email" required />
  <input name="password" type="password" required />
  <button type="submit">로그인</button>
</form>
```

```javascript
// +page.server.js - 서버에서 폼 처리
export const actions = {
  login: async ({ request, cookies }) => {
    const data = await request.formData();
    const email = data.get('email');
    const password = data.get('password');

    const user = await authenticate(email, password);

    if (user) {
      cookies.set('session', user.sessionId, {
        path: '/',
        httpOnly: true,
        secure: true,
        sameSite: 'strict',
      });

      throw redirect(303, '/dashboard');
    } else {
      return fail(400, { email, incorrect: true });
    }
  },
};
```

## 타입 안전성과 개발자 경험

스벨트킷은 **타입스크립트를 일급 시민으로 지원**하며, 뛰어난 타입 추론을 제공합니다.

```typescript
// app.d.ts - 전역 타입 정의
declare global {
  namespace App {
    interface Error {
      code?: string;
      id?: string;
    }

    interface Locals {
      user?: {
        id: string;
        name: string;
        role: 'admin' | 'user';
      };
    }

    interface PageData {
      flash?: { type: 'success' | 'error'; message: string };
    }
  }
}
```

```typescript
// +page.ts - 타입 안전한 데이터 로딩
import type { PageLoad } from './$types';

export const load: PageLoad = async ({ params, fetch }) => {
  const post = await fetch(`/api/posts/${params.slug}`).then(r => r.json());

  return {
    post, // 자동으로 타입이 추론됨
  };
};
```

## 내장된 성능 최적화

스벨트킷은 **개발자가 신경 쓰지 않아도 자동으로 성능을 최적화**합니다.

## 자동 코드 스플리팅

```javascript
// 각 라우트는 자동으로 별도의 청크로 분리
src/routes/
├── +page.svelte        # chunk-home.js
├── about/+page.svelte  # chunk-about.js
└── blog/+page.svelte   # chunk-blog.js
```

## 인텔리전트 프리로딩

```html
<!-- 링크에 마우스를 올리면 자동으로 프리로드 -->
<a href="/about" data-sveltekit-preload-data="hover"> 소개 페이지 </a>

<!-- 뷰포트에 들어오면 프리로드 -->
<a href="/contact" data-sveltekit-preload-data="viewport"> 연락처 </a>
```

## 이미지 최적화

```html
<script>
  import { enhanced } from '$app/forms';
</script>

<!-- 개발자는 일반 img 태그만 작성 -->
<img src="/images/hero.jpg" alt="히어로 이미지" />

<!-- 빌드 시 자동으로 최적화:
     - WebP/AVIF 포맷 생성
     - 반응형 크기별 이미지 생성
     - 지연 로딩 적용
-->
```

## 간편한 상태 관리

스벨트킷은 **복잡한 상태 관리 라이브러리 없이도 효과적인 상태 관리**가 가능합니다.

```javascript
// lib/stores.js - 전역 상태 정의
import { writable, derived } from 'svelte/store';
import { browser } from '$app/environment';

// 로컬스토리지와 동기화되는 스토어
function createPersistedStore(key, initialValue) {
  const store = writable(initialValue);

  if (browser) {
    const stored = localStorage.getItem(key);
    if (stored) store.set(JSON.parse(stored));

    store.subscribe(value => {
      localStorage.setItem(key, JSON.stringify(value));
    });
  }

  return store;
}

export const user = createPersistedStore('user', null);
export const cart = createPersistedStore('cart', []);

// 파생된 상태
export const cartTotal = derived(cart, $cart =>
  $cart.reduce((total, item) => total + item.price * item.quantity, 0)
);
```

```html
<!-- 컴포넌트에서 간단하게 사용 -->
<script>
  import { cart, cartTotal } from '$lib/stores.js';
</script>

<p>장바구니 상품 수: {$cart.length}</p>
<p>총 금액: {$cartTotal.toLocaleString()}원</p>

<button on:click="{()" ="">cart.update(items => [...items, newItem])}> 장바구니에 추가</button>
```

## 개발 도구와 디버깅

스벨트킷은 **뛰어난 개발자 도구**를 제공합니다.

```bash
# 개발 서버 - 매우 빠른 HMR
npm run dev

# 빌드 분석
npm run build
npm run preview

# 타입 체크
npm run check

# 린팅 및 포맷팅
npm run lint
npm run format
```

:::note 에러 메시지의 질
스벨트킷은 초보자도 이해할 수 있는 친화적인 에러 메시지를 제공합니다. 문제가 발생한 정확한 위치와 해결 방법을 명확하게 알려줍니다.
:::

## 정리

스벨트킷의 주요 장점들을 요약하면:

- 🚀 **제로 설정**: 복잡한 설정 없이 바로 시작
- 📁 **파일 기반 규칙**: 직관적이고 예측 가능한 구조
- 🔄 **유연한 렌더링**: 페이지별 최적화된 렌더링 전략
- 🌐 **플랫폼 독립성**: 어디든 배포 가능한 어댑터 시스템
- 📈 **진보적 향상**: 웹 표준을 따르는 점진적 개선
- 🛡️ **타입 안전성**: 완벽한 타입스크립트 지원
- ⚡ **자동 최적화**: 개발자 개입 없는 성능 최적화
- 🎯 **간편한 상태 관리**: 복잡한 라이브러리 불필요

이러한 특징들이 결합되어 스벨트킷은 **개발자 경험과 최종 사용자 성능을 모두 만족시키는** 현대적인 웹 개발 솔루션을 제공합니다.

다음 장에서는 스벨트킷을 실제로 설치하고 첫 번째 프로젝트를 만들어보겠습니다.
