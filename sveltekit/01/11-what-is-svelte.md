---
sidebar_position: 11
---

# 스벨트란?

스벨트(Svelte)는 2016년 Rich Harris가 개발한 웹 프론트엔드 프레임워크입니다. 하지만 스벨트를 단순한 "프레임워크"라고 부르는 것은 정확하지 않습니다.

:::info 핵심 개념
스벨트는 더 정확히 말하면 **컴파일러**입니다.
:::

스벨트킷(SvelteKit)을 이해하기 위해서는 먼저 스벨트 자체가 무엇인지 알아야 합니다. 스벨트는 스벨트킷의 기반이 되는 컴포넌트 라이브러리이며, 스벨트킷은 이를 활용해 풀스택 웹 애플리케이션을 구축할 수 있게 해주는 프레임워크입니다.

## 기존 프레임워크와의 차이점

스벨트가 다른 프레임워크들과 어떻게 다른지 이해하는 것은 매우 중요합니다. 이 차이점이야말로 스벨트가 혁신적인 이유이기 때문입니다.

### 런타임 vs 컴파일 타임

이 개념이 스벨트를 이해하는 핵심입니다. 기존 프레임워크와 스벨트의 근본적인 차이점을 보여줍니다.

**기존 프레임워크 (React, Vue):**

- 브라우저에서 실행되는 런타임 라이브러리 포함
- 가상 DOM 생성 및 관리
- 실행 시점에 컴포넌트 상태 관리

**스벨트:**

- 빌드 타임에 컴포넌트를 바닐라 JavaScript로 컴파일
- 런타임 라이브러리 불필요
- 최적화된 코드 자동 생성

### 간단한 예시

실제 코드를 통해 스벨트의 직관적인 문법을 확인해보겠습니다. 스벨트 컴포넌트는 하나의 파일에 로직, 마크업, 스타일을 모두 포함할 수 있습니다.

```html title="Svelte 컴포넌트"
<script>
  let count = 0;

  function increment() {
    count += 1;
  }
</script>

<button on:click="{increment}">클릭 횟수: {count}</button>

<style>
  button {
    background: #ff3e00;
    color: white;
    border: none;
    padding: 8px 16px;
    border-radius: 4px;
  }
</style>
```

위 컴포넌트는 컴파일 과정을 거쳐 효율적인 바닐라 JavaScript로 변환됩니다.

## 스벨트의 핵심 철학

스벨트가 추구하는 세 가지 핵심 가치는 다른 프레임워크들과 차별화되는 스벨트만의 특징을 보여줍니다.

### 1. Write Less, Do More

개발자의 생산성을 높이는 것이 스벨트의 최우선 목표입니다. 보일러플레이트 코드를 최소화하여 개발자가 핵심 로직에 집중할 수 있게 합니다.

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
<TabItem value="react" label="React">

```jsx
import React, { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return <button onClick={() => setCount(count + 1)}>클릭 횟수: {count}</button>;
}
```

</TabItem>
<TabItem value="svelte" label="Svelte">

```html
<script>
  let count = 0;
</script>

<button on:click="{()" ="">count++}> 클릭 횟수: {count}</button>
```

</TabItem>
</Tabs>

스벨트가 훨씬 간결하고 직관적입니다.

### 2. No Virtual DOM

가상 DOM은 성능 최적화를 위한 좋은 해결책이었지만, 스벨트는 컴파일 타임 분석을 통해 이를 완전히 우회하는 혁신적인 방법을 제시했습니다.

:::tip 가상 DOM 없음의 장점

- **더 작은 번들 크기**: 가상 DOM 라이브러리 불필요
- **더 빠른 실행 속도**: 가상 DOM 비교 과정 생략
- **메모리 효율성**: 가상 DOM 트리 유지 불필요
  :::

### 3. 진정한 반응성

스벨트의 반응성 시스템은 매우 직관적입니다. 복잡한 상태 관리 패턴이나 훅을 배울 필요 없이, JavaScript의 기본 할당 연산자만으로 반응성을 구현할 수 있습니다.

```html
<script>
  let name = '세계';

  // name이 변경되면 자동으로 업데이트
  $: greeting = `안녕하세요, ${name}!`;
</script>

<input bind:value="{name}" />
<h1>{greeting}</h1>
```

`$:` 구문은 반응성 선언문으로, 의존하는 변수가 변경될 때마다 자동 실행됩니다.

## 주요 특징

스벨트의 주요 특징들은 모두 개발자 경험과 성능 최적화를 위해 설계되었습니다. 각 특징이 어떤 문제를 해결하는지 살펴보겠습니다.

### 컴포넌트 기반 아키텍처

현대적인 웹 개발의 기본인 컴포넌트 기반 아키텍처를 지원합니다. 각 컴포넌트는 독립적이고 재사용 가능한 UI 블록입니다.

### 스코프드 CSS

CSS 충돌 문제를 자동으로 해결해주는 혁신적인 기능입니다. 별도의 CSS-in-JS 라이브러리나 복잡한 설정 없이도 컴포넌트별 스타일 격리가 가능합니다.

```html
<style>
  /* 이 스타일은 현재 컴포넌트에만 적용 */
  p {
    color: purple;
    font-size: 18px;
  }
</style>

<p>보라색 텍스트</p>
```

### 내장 상태 관리

Redux나 MobX 같은 복잡한 상태 관리 라이브러리 없이도 효과적으로 전역 상태를 관리할 수 있습니다. 스벨트의 stores는 간단하면서도 강력합니다.

```javascript title="stores.js"
import { writable } from 'svelte/store';

export const count = writable(0);
```

```html title="App.svelte"
<script>
  import { count } from './stores.js';
</script>

<button on:click="{()" ="">$count++}> 카운트: {$count}</button>
```

### 내장 애니메이션

웹 애니메이션을 구현하는 것은 전통적으로 복잡한 작업이었습니다. 스벨트는 이를 간단하고 직관적으로 만들어주는 강력한 애니메이션 시스템을 내장하고 있습니다.

```html
<script>
  import { fade } from 'svelte/transition';
  let visible = true;
</script>

<button on:click={() => visible = !visible}>토글</button>

{#if visible}
  <div transition:fade={{ duration: 300 }}>
    부드러운 페이드 효과
  </div>
{/if}
```

## 스벨트가 해결하는 문제들

스벨트는 기존 웹 개발에서 개발자들이 겪었던 주요 문제점들을 해결하기 위해 설계되었습니다. 이러한 문제 해결 능력이 스벨트의 가치를 보여줍니다.

### 번들 크기 최적화

모바일 환경과 느린 인터넷 연결에서 웹 애플리케이션의 로딩 속도는 매우 중요합니다. 스벨트는 컴파일 타임 최적화를 통해 이 문제를 근본적으로 해결합니다.

### 학습 곡선 단축

새로운 프레임워크를 배우는 것은 시간이 많이 걸리는 일입니다. 스벨트는 기존 웹 표준 지식을 최대한 활용할 수 있도록 설계되어 학습 비용을 크게 줄여줍니다.

### 자동 성능 최적화

React에서 성능 최적화를 위해 `useMemo`, `useCallback` 등을 신경써서 사용해야 하는 번거로움이 있습니다. 스벨트는 이런 최적화를 컴파일러가 자동으로 처리합니다.

## 언제 스벨트를 사용할까?

프로젝트의 특성과 요구사항에 따라 적절한 기술을 선택하는 것이 중요합니다. 스벨트가 특히 유리한 상황들을 알아보겠습니다.

:::tip 스벨트가 적합한 경우

- **성능 중시**: 작은 번들과 빠른 실행이 필요한 앱
- **새 프로젝트**: 레거시 제약이 없는 경우
- **빠른 개발**: 프로토타이핑이나 MVP 개발
- **SEO 중요**: 정적 사이트나 블로그
- **모바일 웹**: 제한된 리소스 환경
  :::

## 정리

스벨트는 단순히 새로운 프레임워크가 아닙니다. 웹 개발 방식 자체를 재정의하는 패러다임의 전환을 제시합니다. 컴파일러 기반의 혁신적 접근 방식으로 다음을 실현합니다:

- 웹 개발의 복잡성 감소
- 개발자 경험 개선
- 최종 사용자 성능 향상

이는 웹 개발의 미래를 제시하는 중요한 패러다임 전환입니다.

다음에는 스벨트킷이 등장한 배경을 알아보겠습니다.
