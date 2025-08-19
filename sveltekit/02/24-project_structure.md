---
sidebar_position: 24
---

# 프로젝트 구조 이해하기

스벨트킷 프로젝트의 폴더와 파일 구조를 알아보겠습니다.

## 전체 구조

```
my-sveltekit-app/
├── src/                 # 소스 코드
│   ├── routes/         # 페이지와 API
│   ├── lib/            # 재사용 가능한 코드
│   └── app.html        # HTML 템플릿
├── static/             # 정적 파일 (이미지, 아이콘 등)
├── package.json        # 프로젝트 정보
├── svelte.config.js    # 스벨트킷 설정
└── vite.config.js      # 빌드 도구 설정
```

## src/routes - 페이지 라우팅

폴더 구조가 곧 URL 구조가 됩니다.

```
src/routes/
├── +page.svelte           # / (홈페이지)
├── +layout.svelte         # 전체 레이아웃
├── about/
│   └── +page.svelte       # /about
├── blog/
│   ├── +page.svelte       # /blog
│   └── [slug]/
│       └── +page.svelte   # /blog/hello-world
└── api/
    └── posts/
        └── +server.ts     # /api/posts
```

### 특수 파일명

- `+page.svelte` - 페이지 컴포넌트
- `+layout.svelte` - 레이아웃 컴포넌트
- `+server.ts` - API 엔드포인트
- `+page.ts` - 데이터 로드 함수
- `+error.svelte` - 에러 페이지

#### 특수 파일명 구조 예시

```
src/routes/
├── +layout.svelte          # 모든 페이지에 적용되는 레이아웃
├── +page.svelte            # 홈페이지 (/)
├── +error.svelte           # 전역 에러 페이지
├── about/
│   ├── +page.svelte        # /about 페이지
│   └── +page.ts            # /about 페이지의 데이터 로드
├── dashboard/
│   ├── +layout.svelte      # 대시보드 섹션 전용 레이아웃
│   ├── +layout.ts          # 대시보드 레이아웃의 데이터 로드
│   ├── +page.svelte        # /dashboard 페이지
│   ├── +error.svelte       # 대시보드 섹션 에러 페이지
│   └── settings/
│       └── +page.svelte    # /dashboard/settings 페이지
└── api/
    ├── users/
    │   └── +server.ts      # GET/POST /api/users
    └── posts/
        ├── +server.ts      # GET/POST /api/posts
        └── [id]/
            └── +server.ts  # GET/PUT/DELETE /api/posts/123
```

각 파일의 역할:

- **+layout.svelte**: 하위 페이지들의 공통 UI (헤더, 사이드바 등)
- **+page.svelte**: 실제 페이지 내용
- **+page.ts**: 페이지 렌더링 전에 필요한 데이터 로드
- **+server.ts**: REST API 엔드포인트
- **+error.svelte**: 해당 섹션에서 에러 발생 시 표시할 페이지

### 동적 라우트

- `[id]` - 단일 매개변수 (`/blog/hello`)
- `[...rest]` - 나머지 경로 (`/files/docs/intro.md`)
- `(group)` - 라우트 그룹 (URL에 영향 없음)

#### 동적 라우트 구조 예시

```
src/routes/
├── blog/
│   ├── +page.svelte           # /blog (블로그 목록)
│   ├── [slug]/
│   │   ├── +page.svelte       # /blog/my-first-post
│   │   ├── +page.ts           # 개별 포스트 데이터 로드
│   │   └── edit/
│   │       └── +page.svelte   # /blog/my-first-post/edit
│   └── category/
│       └── [name]/
│           └── +page.svelte   # /blog/category/javascript
├── user/
│   └── [id]/
│       ├── +page.svelte       # /user/123
│       ├── profile/
│       │   └── +page.svelte   # /user/123/profile
│       └── settings/
│           └── +page.svelte   # /user/123/settings
├── docs/
│   └── [...path]/
│       └── +page.svelte       # /docs/guide/getting-started
└── (app)/                     # 라우트 그룹 (URL에 영향 없음)
    ├── dashboard/
    │   └── +page.svelte       # /dashboard
    └── settings/
        └── +page.svelte       # /settings
```

동적 라우트 설명:

- **[slug]**: 하나의 매개변수를 받습니다. `/blog/hello-world`에서 `slug`는 `"hello-world"`
- **[id]**: 숫자나 문자열 ID를 받습니다. `/user/123`에서 `id`는 `"123"`
- **[...path]**: 여러 경로 세그먼트를 받습니다. `/docs/guide/getting-started`에서 `path`는 `["guide", "getting-started"]`
- **(app)**: 라우트 그룹으로 URL에는 나타나지 않지만 레이아웃을 공유할 수 있습니다

## src/lib - 공통 코드

재사용 가능한 컴포넌트, 유틸리티, 상태 관리 코드를 저장합니다.

```
src/lib/
├── components/
│   ├── Button.svelte
│   ├── Header.svelte
│   └── index.ts
├── stores/
│   └── user.ts
├── utils/
│   └── helpers.ts
└── types/
    └── index.ts
```

`$lib` 별칭으로 접근 가능:

```jsx
import Button from '$lib/components/Button.svelte';
import { userStore } from '$lib/stores/user.ts';
```

## static - 정적 파일

웹서버 루트에서 직접 제공되는 파일들입니다.

```
static/
├── favicon.ico
├── robots.txt
├── images/
│   └── logo.png
└── fonts/
    └── custom.woff2
```

사용법:

```jsx
<img src='/images/logo.png' alt='로고' />
```

## 주요 설정 파일

### svelte.config.js

```javascript
import adapter from '@sveltejs/adapter-auto';

export default {
  kit: {
    adapter: adapter(),
    alias: {
      $components: 'src/lib/components',
    },
  },
};
```

### app.html

모든 페이지의 기본 HTML 템플릿:

```html
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" href="%sveltekit.assets%/favicon.ico" />
    <meta name="viewport" content="width=device-width" />
    %sveltekit.head%
  </head>
  <body>
    <div>%sveltekit.body%</div>
  </body>
</html>
```

## 확장된 구조 예시

대규모 프로젝트에서는 다음과 같이 구성할 수 있습니다:

```
src/
├── routes/
│   ├── (app)/              # 인증 필요한 페이지들
│   │   ├── dashboard/
│   │   └── settings/
│   ├── (marketing)/        # 마케팅 페이지들
│   │   ├── pricing/
│   │   └── about/
│   └── api/
├── lib/
│   ├── components/
│   │   ├── ui/            # 기본 UI 컴포넌트
│   │   ├── forms/         # 폼 컴포넌트
│   │   └── layout/        # 레이아웃 컴포넌트
│   ├── stores/            # 상태 관리
│   ├── utils/             # 유틸리티 함수
│   ├── types/             # TypeScript 타입
│   └── server/            # 서버 전용 코드
└── styles/
    └── global.css
```

:::tip 핵심 포인트

- **routes**: 폴더 구조 = URL 구조
- **lib**: `$lib`으로 어디서든 접근 가능
- **static**: 웹서버 루트에서 직접 제공
- **특수 파일**: `+`로 시작하는 파일들은 특별한 역할
  :::

이제 스벨트킷 프로젝트 구조의 기본을 이해했습니다!
