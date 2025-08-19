---
sidebar_position: 4
sidebar_label: 4. 컴포넌트 시스템
---

<!-- 레퍼런스 스타일로 아래 목차의 내용을 markdown으로 작성해서 아티팩트창으로 보여줘. -->

# Svelte 5 레퍼런스

# 4. 컴포넌트 시스템

## 4.1 컴포넌트 기본 (Runes 방식)

### 컴포넌트 정의와 사용

**기본 컴포넌트 정의**

```html
<!-- Button.svelte -->
<script>
  let { children, variant = 'primary', ...props } = $props();
</script>

<button class="btn btn-{variant}" {...props}>{@render children()}</button>

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

**컴포넌트 사용**

```html
<!-- App.svelte -->
<script>
  import Button from './Button.svelte';
</script>

<button variant="primary" onclick="{()" ="">console.log('클릭됨')}> 클릭하세요</button>
```

### 컴포넌트 임포트/익스포트

**기본 익스포트**

```javascript
// components/index.js
export { default as Button } from './Button.svelte';
export { default as Modal } from './Modal.svelte';
export { default as Card } from './Card.svelte';
```

**네임드 익스포트와 함께 사용**

```html
<!-- Utils.svelte -->
<script context="module">
  export function formatDate(date) {
    return new Intl.DateTimeFormat('ko-KR').format(date);
  }

  export const THEME_COLORS = {
    primary: '#007bff',
    secondary: '#6c757d',
  };
</script>

<script>
  let { date } = $props();
</script>

<span>{formatDate(date)}</span>
```

### Runes 기반 컴포넌트 작성

**상태 관리를 포함한 컴포넌트**

```html
<!-- Counter.svelte -->
<script>
  let { initialValue = 0 } = $props();
  let count = $state(initialValue);

  let doubled = $derived(count * 2);

  function increment() {
    count++;
  }

  function decrement() {
    count--;
  }
</script>

<div class="counter">
  <button onclick="{decrement}">-</button>
  <span>카운트: {count}</span>
  <span>더블: {doubled}</span>
  <button onclick="{increment}">+</button>
</div>
```

## 4.2 컴포넌트 Props

### `$props()` 활용 패턴

**기본 Props 패턴**

```html
<script>
  // 기본값과 함께 props 정의
  let { title = '제목 없음', description, isVisible = true, items = [], ...restProps } = $props();
</script>

<div {...restProps}>
  {#if isVisible}
  <h2>{title}</h2>
  {#if description}
  <p>{description}</p>
  {/if}
  <ul>
    {#each items as item}
    <li>{item}</li>
    {/each}
  </ul>
  {/if}
</div>
```

**함수형 Props**

```html
<!-- DataTable.svelte -->
<script>
  let { data = [], onRowClick, onSort, renderCell = value => value } = $props();
</script>

<table>
  <tbody>
    {#each data as row, index}
    <tr onclick="{()" ="">
      onRowClick?.(row, index)}> {#each Object.entries(row) as [key, value]}
      <td>{@html renderCell(value, key, row)}</td>
      {/each}
    </tr>
    {/each}
  </tbody>
</table>
```

### Props 검증과 타입 안전성

**TypeScript와 함께 사용**

```html
<!-- Card.svelte -->
<script lang="ts">
  interface CardProps {
    title: string;
    subtitle?: string;
    image?: string;
    actions?: Array<{ label: string; onClick: () => void }>;
    variant?: 'default' | 'elevated' | 'outlined';
  }

  let { title, subtitle, image, actions = [], variant = 'default' }: CardProps = $props();
</script>

<div class="card card--{variant}">
  {#if image}
  <img src="{image}" alt="{title}" />
  {/if}

  <div class="card__content">
    <h3>{title}</h3>
    {#if subtitle}
    <p class="subtitle">{subtitle}</p>
    {/if}
  </div>

  {#if actions.length > 0}
  <div class="card__actions">
    {#each actions as action}
    <button onclick="{action.onClick}">{action.label}</button>
    {/each}
  </div>
  {/if}
</div>
```

### 조건부 Props 처리

**동적 Props 패턴**

```html
<!-- FormField.svelte -->
<script>
  let {
    type = 'text',
    label,
    value = $bindable(),
    error,
    required = false,
    disabled = false,
    ...inputProps
  } = $props();

  // 타입별 특별한 처리
  let computedProps = $derived(() => {
    const base = { ...inputProps, disabled };

    if (type === 'number') {
      return { ...base, step: inputProps.step || 'any' };
    }

    if (type === 'email') {
      return { ...base, pattern: inputProps.pattern || '[^@]+@[^@]+\\.[^@]+' };
    }

    return base;
  });
</script>

<div class="field" class:field--error="{error}">
  <label> {label} {#if required}<span class="required">*</span>{/if} </label>

  <input
    {type}
    bind:value
    {...computedProps}
    aria-invalid="{!!error}"
    aria-describedby="{error"
    ?
    `${label}-error`
    :
    undefined}
  />

  {#if error}
  <span id="{label}-error" class="error">{error}</span>
  {/if}
</div>
```

## 4.3 스니펫 기반 컴포넌트 조합

### 스니펫을 이용한 컴포넌트 설계

**기본 스니펫 패턴**

```html
<!-- Modal.svelte -->
<script>
  let { isOpen = $bindable(false), title, children, actions } = $props();
</script>

{#if isOpen}
<div class="modal-backdrop" onclick="{()" ="">
  isOpen = false}>
  <div class="modal" onclick|stopPropagation>
    <header class="modal__header">
      <h2>{title}</h2>
      <button onclick="{()" ="">isOpen = false}>×</button>
    </header>

    <main class="modal__body">{@render children()}</main>

    {#if actions}
    <footer class="modal__footer">{@render actions()}</footer>
    {/if}
  </div>
</div>
{/if}
```

**스니펫 사용 예시**

```html
<script>
  import Modal from './Modal.svelte';
  let showModal = $state(false);
  let username = $state('');
</script>

<Modal bind:isOpen="{showModal}" title="사용자 등록">
  {#snippet children()}
  <form>
    <input bind:value="{username}" placeholder="사용자명" />
  </form>
  {/snippet} {#snippet actions()}
  <button onclick="{()" ="">showModal = false}>취소</button>
  <button onclick="{()" ="">console.log('저장:', username)}>저장</button>
  {/snippet}
</Modal>
```

### 재사용 가능한 템플릿 패턴

**리스트 컴포넌트 스니펫**

```html
<!-- DataList.svelte -->
<script>
  let {
    items = [],
    itemTemplate,
    headerTemplate,
    emptyTemplate,
    loadingTemplate,
    isLoading = false,
  } = $props();
</script>

<div class="data-list">
  {#if headerTemplate}
  <header class="data-list__header">{@render headerTemplate()}</header>
  {/if}

  <main class="data-list__content">
    {#if isLoading && loadingTemplate} {@render loadingTemplate()} {:else if items.length === 0 &&
    emptyTemplate} {@render emptyTemplate()} {:else} {#each items as item, index}
    <div class="data-list__item">{@render itemTemplate(item, index)}</div>
    {/each} {/if}
  </main>
</div>
```

**스니펫 기반 사용**

```html
<script>
  import DataList from './DataList.svelte';
  let users = $state([
    { id: 1, name: '김철수', email: 'kim@example.com' },
    { id: 2, name: '이영희', email: 'lee@example.com' },
  ]);
  let loading = $state(false);
</script>

<datalist items="{users}" isLoading="{loading}">
  {#snippet headerTemplate()}
  <h2>사용자 목록</h2>
  <button onclick="{()" ="">loading = !loading}>새로고침</button>
  {/snippet} {#snippet itemTemplate(user, index)}
  <div class="user-card">
    <h3>{user.name}</h3>
    <p>{user.email}</p>
    <small>#{index + 1}</small>
  </div>
  {/snippet} {#snippet emptyTemplate()}
  <p>등록된 사용자가 없습니다.</p>
  {/snippet} {#snippet loadingTemplate()}
  <div class="spinner">로딩 중...</div>
  {/snippet}
</datalist>
```

### 기존 슬롯과의 차이점

**기존 슬롯 방식 (Svelte 4)**

```html
<!-- Svelte 4 방식 -->
<div class="card">
  <slot name="header" />
  <slot />
  <slot name="footer" />
</div>
```

**새로운 스니펫 방식 (Svelte 5)**

```html
<!-- Svelte 5 방식 -->
<script>
  let { header, children, footer } = $props();
</script>

<div class="card">
  {#if header} {@render header()} {/if} {@render children()} {#if footer} {@render footer()} {/if}
</div>
```

**스니펫의 장점**

- 매개변수 전달 가능
- 타입 안전성 향상
- 더 명시적인 API
- 조건부 렌더링 제어 개선

## 4.4 컴포넌트 컨텍스트

### `setContext()` / `getContext()` with Runes

**컨텍스트 제공자**

```html
<!-- ThemeProvider.svelte -->
<script>
  import { setContext } from 'svelte';

  let { theme = 'light', children } = $props();
  let currentTheme = $state(theme);

  // 컨텍스트에 반응형 상태 제공
  setContext('theme', {
    get current() {
      return currentTheme;
    },
    set: newTheme => {
      currentTheme = newTheme;
    },
    toggle: () => {
      currentTheme = currentTheme === 'light' ? 'dark' : 'light';
    },
  });

  $effect(() => {
    document.documentElement.setAttribute('data-theme', currentTheme);
  });
</script>

<div class="theme-provider" data-theme="{currentTheme}">{@render children()}</div>
```

**컨텍스트 소비자**

```html
<!-- ThemedButton.svelte -->
<script>
  import { getContext } from 'svelte';

  let { children, ...props } = $props();

  const theme = getContext('theme');

  // 컨텍스트 상태에 반응
  let buttonClass = $derived(`btn btn--${theme.current}`);
</script>

<button class="{buttonClass}" {...props}>{@render children()}</button>

<style>
  .btn--light {
    background: white;
    color: black;
    border: 1px solid #ccc;
  }

  .btn--dark {
    background: #333;
    color: white;
    border: 1px solid #666;
  }
</style>
```

### 컨텍스트와 Runes 통합 패턴

**복잡한 상태 관리 컨텍스트**

```html
<!-- AppStateProvider.svelte -->
<script>
  import { setContext } from 'svelte';

  let { children } = $props();

  // 복합 상태 관리
  let user = $state(null);
  let notifications = $state([]);
  let settings = $state({
    language: 'ko',
    theme: 'light',
  });

  // Derived 상태
  let isAuthenticated = $derived(!!user);
  let unreadCount = $derived(notifications.filter(n => !n.read).length);

  // 액션들
  const appState = {
    // 상태 접근자
    get user() {
      return user;
    },
    get notifications() {
      return notifications;
    },
    get settings() {
      return settings;
    },
    get isAuthenticated() {
      return isAuthenticated;
    },
    get unreadCount() {
      return unreadCount;
    },

    // 액션들
    login: userData => {
      user = userData;
    },

    logout: () => {
      user = null;
      notifications = [];
    },

    addNotification: notification => {
      notifications = [
        ...notifications,
        {
          ...notification,
          id: Date.now(),
          read: false,
        },
      ];
    },

    markAsRead: id => {
      notifications = notifications.map(n => (n.id === id ? { ...n, read: true } : n));
    },

    updateSettings: newSettings => {
      settings = { ...settings, ...newSettings };
    },
  };

  setContext('appState', appState);
</script>

{@render children()}
```

**컨텍스트 커스텀 훅 패턴**

```javascript
// useAppState.js
import { getContext } from 'svelte';

export function useAppState() {
  const context = getContext('appState');
  if (!context) {
    throw new Error('useAppState must be used within AppStateProvider');
  }
  return context;
}
```

## 4.5 컴포넌트 라이프사이클

### `$effect()` 기반 라이프사이클

**기본 라이프사이클 패턴**

```html
<script>
  let { data = [] } = $props();
  let loading = $state(true);
  let error = $state(null);

  // 마운트 시 실행 (onMount 대체)
  $effect(() => {
    console.log('컴포넌트 마운트됨');

    // 클린업 함수 반환 (onDestroy 대체)
    return () => {
      console.log('컴포넌트 언마운트됨');
    };
  });

  // 특정 값 변경 감지
  $effect(() => {
    if (data.length > 0) {
      loading = false;
    }
  });

  // 비동기 작업 처리
  $effect(() => {
    async function fetchData() {
      try {
        loading = true;
        const response = await fetch('/api/data');
        data = await response.json();
      } catch (err) {
        error = err.message;
      } finally {
        loading = false;
      }
    }

    fetchData();
  });
</script>
```

### 마운트/언마운트 처리

**리소스 관리 패턴**

```html
<!-- EventListener.svelte -->
<script>
  let { target = window, event, handler } = $props();

  $effect(() => {
    // 이벤트 리스너 등록
    target.addEventListener(event, handler);

    // 클린업: 이벤트 리스너 제거
    return () => {
      target.removeEventListener(event, handler);
    };
  });
</script>
```

**타이머 관리**

```html
<!-- Timer.svelte -->
<script>
  let { interval = 1000, onTick } = $props();
  let time = $state(new Date());

  $effect(() => {
    const timer = setInterval(() => {
      time = new Date();
      onTick?.(time);
    }, interval);

    // 클린업: 타이머 정리
    return () => clearInterval(timer);
  });
</script>

<div>현재 시간: {time.toLocaleTimeString()}</div>
```

### 업데이트 감지 패턴

**Props 변경 감지**

```html
<script>
  let { userId } = $props();
  let userData = $state(null);
  let loading = $state(false);

  // userId 변경 시 사용자 데이터 다시 로드
  $effect(() => {
    if (!userId) return;

    async function loadUser() {
      loading = true;
      try {
        const response = await fetch(`/api/users/${userId}`);
        userData = await response.json();
      } catch (error) {
        console.error('사용자 로드 실패:', error);
        userData = null;
      } finally {
        loading = false;
      }
    }

    loadUser();
  });

  // 이전 값과 비교하여 변경 감지
  let previousUserId = $state();
  $effect(() => {
    if (previousUserId && previousUserId !== userId) {
      console.log(`사용자 변경: ${previousUserId} → ${userId}`);
    }
    previousUserId = userId;
  });
</script>

{#if loading}
<div>사용자 정보 로딩 중...</div>
{:else if userData}
<div>
  <h2>{userData.name}</h2>
  <p>{userData.email}</p>
</div>
{:else}
<div>사용자를 찾을 수 없습니다.</div>
{/if}
```

**복잡한 상태 동기화**

```html
<script>
  let { initialValues = {} } = $props();
  let formData = $state({ ...initialValues });
  let isDirty = $state(false);
  let hasChanges = $derived(JSON.stringify(formData) !== JSON.stringify(initialValues));

  // Props 변경 시 폼 초기화
  $effect(() => {
    formData = { ...initialValues };
    isDirty = false;
  });

  // 폼 데이터 변경 감지
  $effect(() => {
    isDirty = hasChanges;
  });

  // 페이지 이탈 시 경고
  $effect(() => {
    function handleBeforeUnload(event) {
      if (isDirty) {
        event.preventDefault();
        event.returnValue = '저장되지 않은 변경사항이 있습니다.';
      }
    }

    if (isDirty) {
      window.addEventListener('beforeunload', handleBeforeUnload);
      return () => {
        window.removeEventListener('beforeunload', handleBeforeUnload);
      };
    }
  });
</script>
```
