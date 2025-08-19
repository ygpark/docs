---
sidebar_position: 13
---

# 다른 프레임워크와의 차이점

이번 장에서는 스벨트킷이 React, Vue, Angular 같은 기존 프레임워크들과 어떻게 다른지 비교해보겠습니다. 각 프레임워크의 근본적인 차이를 이해하면 왜 스벨트킷이 등장했는지, 그리고 어떤 문제를 해결하려 했는지 알 수 있습니다.

## 근본적인 차이: 컴파일러 vs 런타임 프레임워크

### 스벨트킷 - 컴파일 타임 접근

스벨트킷의 가장 큰 차이점은 **컴파일러**라는 점입니다. 여러분이 작성한 코드는 빌드 시점에 순수한 JavaScript로 변환됩니다.

```html
<!-- 개발자가 작성하는 스벨트 코드 -->
<script>
  let count = 0;
</script>

<button on:click="{()" ="">count++}> 클릭 횟수: {count}</button>
```

이 코드는 빌드 후 최적화된 바닐라 JavaScript가 됩니다:

- 가상 DOM 없음
- 프레임워크 런타임 없음
- 직접적인 DOM 조작 코드로 변환

### React/Vue/Angular - 런타임 접근

반면 다른 프레임워크들은 **런타임**에 작동합니다. 브라우저에서 프레임워크 코드가 계속 실행되어야 합니다.

```jsx
// React - 런타임에 가상 DOM 생성 및 비교
function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>클릭 횟수: {count}</button>;
}
// → React 라이브러리가 브라우저에서 계속 작동
```

### 이 차이가 만드는 결과

| 항목            | 스벨트킷 (컴파일러) | React/Vue (런타임)   |
| --------------- | ------------------- | -------------------- |
| **번들 크기**   | ~10KB               | 65KB~130KB           |
| **초기 로딩**   | 매우 빠름           | 프레임워크 로드 필요 |
| **메모리 사용** | 최소                | 가상 DOM 유지        |
| **실행 속도**   | 직접 DOM 조작       | 가상 DOM 비교 과정   |

## 문법과 개발 방식의 차이

### 상태 관리 비교

각 프레임워크가 상태를 다루는 방식을 비교해보겠습니다.

**스벨트킷 - 일반 변수가 반응형**

```html
<script>
  let name = '홍길동'; // 그냥 변수
  let age = 25;

  // $ 레이블로 반응형 선언
  $: greeting = `${name}님은 ${age}살입니다`;
</script>

<input bind:value="{name}" />
<input type="number" bind:value="{age}" />
<p>{greeting}</p>
```

**React - 명시적인 상태 관리**

```jsx
function Component() {
  const [name, setName] = useState('홍길동');
  const [age, setAge] = useState(25);

  const greeting = `${name}님은 ${age}살입니다`;

  return (
    <>
      <input value={name} onChange={e => setName(e.target.value)} />
      <input type='number' value={age} onChange={e => setAge(Number(e.target.value))} />
      <p>{greeting}</p>
    </>
  );
}
```

**Vue - Options API 또는 Composition API**

```vue
<template>
  <div>
    <input v-model="name" />
    <input type="number" v-model.number="age" />
    <p>{{ greeting }}</p>
  </div>
</template>

<script>
// Options API
export default {
  data() {
    return {
      name: '홍길동',
      age: 25,
    };
  },
  computed: {
    greeting() {
      return `${this.name}님은 ${this.age}살입니다`;
    },
  },
};
</script>
```

**Angular - 클래스 기반 + 데코레이터**

```typescript
@Component({
  selector: 'app-component',
  template: `
    <input [(ngModel)]="name" />
    <input type="number" [(ngModel)]="age" />
    <p>{{ greeting }}</p>
  `,
})
export class AppComponent {
  name = '홍길동';
  age = 25;

  get greeting() {
    return `${this.name}님은 ${this.age}살입니다`;
  }
}
```

### 컴포넌트 스타일링 비교

**스벨트킷 - 자동 스코핑**

```html
<style>
  /* 자동으로 이 컴포넌트에만 적용 */
  button {
    background: blue;
    color: white;
  }
</style>

<button>클릭</button>
```

**React - 별도 솔루션 필요**

```jsx
// CSS Modules, styled-components, emotion 등 선택 필요
import styles from './Button.module.css';
// 또는
import styled from 'styled-components';
```

**Vue - Scoped 속성 사용**

```vue
<style scoped>
button {
  background: blue;
  color: white;
}
</style>
```

**Angular - ViewEncapsulation**

```typescript
@Component({
  styles: [`
    button {
      background: blue;
      color: white;
    }
  `],
  encapsulation: ViewEncapsulation.Emulated
})
```

## 아키텍처와 철학의 차이

### 프레임워크별 핵심 철학

| 프레임워크   | 핵심 철학                    | 접근 방식                |
| ------------ | ---------------------------- | ------------------------ |
| **스벨트킷** | "Write less, do more"        | 컴파일 타임 최적화       |
| **React**    | "Learn Once, Write Anywhere" | 컴포넌트 기반, 선언적    |
| **Vue**      | "Progressive Framework"      | 점진적 채택 가능         |
| **Angular**  | "Platform for Building"      | 풀 프레임워크, 규약 중심 |

### 프로젝트 구조 비교

**스벨트킷 - 파일 기반 라우팅**

```
src/routes/
  +page.svelte          # /
  +layout.svelte        # 공통 레이아웃
  about/+page.svelte    # /about
  blog/
    +page.svelte        # /blog
    [id]/+page.svelte   # /blog/:id
```

**Next.js (React) - Pages 또는 App 디렉토리**

```
pages/  (또는 app/)
  index.js              # /
  about.js              # /about
  blog/
    index.js            # /blog
    [id].js             # /blog/:id
```

**Nuxt (Vue) - 자동 라우팅**

```
pages/
  index.vue             # /
  about.vue             # /about
  blog/
    index.vue           # /blog
    _id.vue             # /blog/:id
```

**Angular - 명시적 라우팅 설정**

```typescript
// app-routing.module.ts
const routes: Routes = [
  { path: '', component: HomeComponent },
  { path: 'about', component: AboutComponent },
  { path: 'blog/:id', component: BlogDetailComponent },
];
```

## 데이터 페칭과 SSR 비교

### 서버사이드 렌더링 접근 방식

**스벨트킷 - load 함수**

```javascript
// +page.js
export async function load({ params, fetch }) {
  const res = await fetch(`/api/post/${params.id}`);
  const post = await res.json();

  return {
    post, // 자동으로 컴포넌트에 전달
  };
}
```

**Next.js - getServerSideProps**

```javascript
export async function getServerSideProps({ params }) {
  const res = await fetch(`/api/post/${params.id}`);
  const post = await res.json();

  return {
    props: { post },
  };
}
```

**Nuxt - asyncData**

```javascript
export default {
  async asyncData({ params, $fetch }) {
    const post = await $fetch(`/api/post/${params.id}`);
    return { post };
  },
};
```

**Angular Universal - 복잡한 설정**

```typescript
// 별도의 서버 설정과 TransferState 필요
constructor(
  private http: HttpClient,
  private transferState: TransferState
) {}
```

## 생태계와 도구 비교

### 개발 도구와 생태계

| 항목             | 스벨트킷          | React                 | Vue                | Angular            |
| ---------------- | ----------------- | --------------------- | ------------------ | ------------------ |
| **CLI 도구**     | npm create svelte | create-react-app/Vite | Vue CLI/create-vue | Angular CLI        |
| **상태 관리**    | 내장 Stores       | Redux/MobX/Zustand    | Vuex/Pinia         | NgRx/Akita         |
| **라우터**       | 내장              | React Router          | Vue Router         | 내장               |
| **폼 처리**      | 내장 액션         | React Hook Form       | VeeValidate        | Reactive Forms     |
| **애니메이션**   | 내장              | Framer Motion         | Vue Transitions    | Angular Animations |
| **타입스크립트** | 선택적            | 선택적                | 선택적             | 필수               |

### 번들러와 빌드 도구

**스벨트킷**

- Vite 기반 (기본)
- 매우 빠른 HMR
- 자동 코드 스플리팅

**React**

- Webpack (CRA)
- Vite (최신 트렌드)
- Next.js (Turbopack)

**Vue**

- Vite (Vue 3)
- Webpack (Vue 2)
- Nuxt 자체 빌드 시스템

**Angular**

- Webpack 기반
- Angular CLI 통합
- 복잡한 최적화 옵션

## 학습 곡선과 생산성

### 초기 학습 난이도

```
쉬움 ←────────────────────────→ 어려움

스벨트킷    Vue    React    Angular
   ★        ★★     ★★★      ★★★★
```

### 프레임워크별 학습 요구사항

**스벨트킷**

- HTML, CSS, JavaScript 기본 지식
- 반응형 프로그래밍 개념 (간단)
- 스벨트 문법 (1-2주)

**Vue**

- HTML, CSS, JavaScript
- 템플릿 디렉티브
- Options/Composition API
- 생명주기 이해

**React**

- JavaScript ES6+ 필수
- JSX 문법
- Hooks 패러다임
- 함수형 프로그래밍 개념

**Angular**

- TypeScript 필수
- RxJS/Observable
- 의존성 주입
- 데코레이터
- 모듈 시스템

## 성능 특성 비교

### 런타임 성능 지표

| 지표                  | 스벨트킷 | React | Vue  | Angular |
| --------------------- | -------- | ----- | ---- | ------- |
| **초기 번들**         | 10KB     | 65KB  | 50KB | 130KB   |
| **메모리 사용**       | 1.6x     | 2.3x  | 1.9x | 4.2x    |
| **컴포넌트 업데이트** | 0.5ms    | 2ms   | 1ms  | 3ms     |
| **1000개 행 렌더링**  | 45ms     | 85ms  | 65ms | 110ms   |

\*측정 기준: JavaScript Framework Benchmark

### 개발 경험 비교

| 항목               | 스벨트킷 | React | Vue  | Angular |
| ------------------ | -------- | ----- | ---- | ------- |
| **HMR 속도**       | 즉시     | 빠름  | 빠름 | 느림    |
| **에러 메시지**    | 명확     | 보통  | 명확 | 복잡    |
| **디버깅**         | 쉬움     | 보통  | 쉬움 | 어려움  |
| **보일러플레이트** | 최소     | 중간  | 적음 | 많음    |

## 사용 사례별 적합성

### 프로젝트 유형별 추천

**스벨트킷이 적합한 경우**

- 정적 사이트, 블로그
- 성능 중심 웹앱
- 인터랙티브 대시보드
- 소규모 팀 프로젝트

**React가 적합한 경우**

- 대규모 엔터프라이즈 앱
- React Native와 코드 공유
- 복잡한 상태 관리 필요
- 큰 개발팀

**Vue가 적합한 경우**

- 점진적 마이그레이션
- 중간 규모 프로젝트
- 빠른 프로토타이핑
- Laravel/Django 통합

**Angular가 적합한 경우**

- 엔터프라이즈 솔루션
- 엄격한 타입 안정성
- 장기 유지보수 프로젝트
- 대기업 환경

## 정리

스벨트킷은 컴파일러 기반 접근으로 다른 프레임워크들과 근본적으로 다른 방식을 택했습니다. 이는 더 작은 번들, 더 빠른 실행, 더 간단한 코드를 가능하게 합니다.

각 프레임워크는 고유한 강점이 있으며, 프로젝트의 요구사항에 따라 선택해야 합니다. 스벨트킷은 특히 성능과 개발자 경험을 중시하는 프로젝트에 탁월한 선택입니다.

다음 장에서는 이러한 차이점을 바탕으로 스벨트킷만의 구체적인 장점과 특징을 더 자세히 살펴보겠습니다.
