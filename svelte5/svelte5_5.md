---
sidebar_position: 5
sidebar_label: 5. 이벤트 처리
---

<!-- 레퍼런스 스타일로 아래 목차의 내용을 markdown으로 작성해서 아티팩트창으로 보여줘. -->

# Svelte 5 레퍼런스

# 5. 이벤트 처리

## 5.1 기본 이벤트 핸들링

### `on:event={handler}` 기본 문법

Svelte 5에서 이벤트 핸들링의 가장 기본적인 형태입니다.

```html
<script>
  let count = $state(0);

  function handleClick() {
    count++;
  }
</script>

<button on:click="{handleClick}">클릭 횟수: {count}</button>
```

### 인라인 이벤트 핸들러

간단한 로직은 인라인으로 처리할 수 있습니다.

```html
<script>
  let name = $state('');
  let isVisible = $state(true);
</script>

<!-- 인라인 함수 -->
<button on:click="{()" ="">isVisible = !isVisible}> 토글</button>

<!-- 인라인 표현식 -->
<input type="text" bind:value="{name}" on:input="{()" ="" /> console.log('입력:', name)} />
```

### 이벤트 객체 접근

이벤트 핸들러는 자동으로 이벤트 객체를 첫 번째 매개변수로 받습니다.

```html
<script>
  let mousePosition = $state({ x: 0, y: 0 });
  let keyPressed = $state('');

  function handleMouseMove(event) {
    mousePosition = { x: event.clientX, y: event.clientY };
  }

  function handleKeyDown(event) {
    keyPressed = event.key;
  }
</script>

<div on:mousemove="{handleMouseMove}">마우스 위치: {mousePosition.x}, {mousePosition.y}</div>

<input type="text" on:keydown="{handleKeyDown}" placeholder="키를 눌러보세요" />
<p>눌린 키: {keyPressed}</p>
```

## 5.2 이벤트 수정자

### `preventDefault` - 기본 동작 방지

브라우저의 기본 동작을 방지합니다.

```html
<script>
  let formData = $state({ email: '', password: '' });

  function handleSubmit(event) {
    console.log('폼 제출:', formData);
    // 페이지 새로고침이 방지됩니다
  }
</script>

<form on:submit|preventDefault="{handleSubmit}">
  <input bind:value="{formData.email}" type="email" placeholder="이메일" />
  <input bind:value="{formData.password}" type="password" placeholder="비밀번호" />
  <button type="submit">로그인</button>
</form>

<!-- 링크 클릭 방지 -->
<a href="/somewhere" on:click|preventDefault="{()" ="">
  console.log('클릭됨')}> 클릭해도 이동하지 않음
</a>
```

### `stopPropagation` - 버블링 중단

이벤트 버블링을 중단합니다.

```html
<script>
  function parentClick() {
    console.log('부모 클릭');
  }

  function childClick() {
    console.log('자식 클릭');
    // 부모로 이벤트가 전파되지 않습니다
  }
</script>

<div on:click="{parentClick}" class="parent">
  부모 영역
  <button on:click|stopPropagation="{childClick}">자식 버튼 (버블링 중단)</button>
  <button on:click="{childClick}">자식 버튼 (버블링 발생)</button>
</div>
```

### `once` - 한 번만 실행

이벤트 핸들러가 한 번만 실행됩니다.

```html
<script>
  let clickCount = $state(0);

  function handleOnce() {
    console.log('한 번만 실행됩니다');
  }

  function handleNormal() {
    clickCount++;
  }
</script>

<button on:click|once="{handleOnce}">한 번만 클릭 가능</button>

<button on:click="{handleNormal}">일반 버튼 (클릭 횟수: {clickCount})</button>
```

### `self` - 자기 자신에서만

이벤트 타겟이 요소 자신일 때만 실행됩니다.

```html
<script>
  function handleSelfOnly() {
    console.log('div 자체를 클릭했을 때만 실행');
  }
</script>

<div on:click|self="{handleSelfOnly}" class="container">
  이 영역을 클릭하세요 (자식 요소 제외)
  <button>이 버튼을 클릭해도 핸들러가 실행되지 않음</button>
  <span>이 텍스트를 클릭해도 핸들러가 실행되지 않음</span>
</div>
```

### `trusted` - 신뢰할 수 있는 이벤트

사용자가 직접 발생시킨 신뢰할 수 있는 이벤트만 처리합니다.

```html
<script>
  function handleTrustedOnly() {
    console.log('신뢰할 수 있는 이벤트만 처리');
  }

  function simulateClick() {
    // 프로그래밍 방식으로 생성된 이벤트는 처리되지 않습니다
    document.querySelector('#trusted-button').click();
  }
</script>

<button id="trusted-button" on:click|trusted="{handleTrustedOnly}">신뢰할 수 있는 클릭만</button>

<button on:click="{simulateClick}">프로그래밍 방식 클릭 시뮬레이션</button>
```

### `passive` - 패시브 리스너

패시브 이벤트 리스너로 등록하여 성능을 향상시킵니다.

```html
<script>
  function handleScroll(event) {
    // preventDefault()를 호출할 수 없습니다
    console.log('스크롤 위치:', event.target.scrollTop);
  }

  function handleTouch(event) {
    // 터치 이벤트 처리 (패시브)
    console.log('터치 이벤트');
  }
</script>

<div
  on:scroll|passive="{handleScroll}"
  on:touchstart|passive="{handleTouch}"
  style="height: 200px; overflow-y: scroll;"
>
  <div style="height: 1000px;">스크롤할 수 있는 긴 콘텐츠...</div>
</div>
```

## 5.3 커스텀 이벤트

### Runes와 커스텀 이벤트 통합

Svelte 5에서는 runes와 함께 더 강력한 커스텀 이벤트 시스템을 제공합니다.

```html
<!-- CustomButton.svelte -->
<script>
  import { createEventDispatcher } from 'svelte';

  const dispatch = createEventDispatcher();

  let { variant = 'primary', disabled = false } = $props();

  function handleClick(event) {
    if (disabled) return;

    dispatch('customClick', {
      timestamp: Date.now(),
      variant,
      originalEvent: event,
    });
  }
</script>

<button class="btn btn-{variant}" {disabled} on:click="{handleClick}">
  <slot />
</button>
```

### 이벤트 디스패칭 패턴

```html
<!-- App.svelte -->
<script>
  import CustomButton from './CustomButton.svelte';

  let events = $state([]);

  function handleCustomClick(event) {
    events = [...events, event.detail];
  }

  function handleFormSubmit(event) {
    dispatch('formSubmit', {
      formData: event.detail,
      isValid: validateForm(event.detail),
    });
  }
</script>

<CustomButton variant="success" on:customClick="{handleCustomClick}"> 커스텀 버튼 </CustomButton>

<div class="events">
  <h3>이벤트 로그:</h3>
  {#each events as event}
  <div class="event-item">{event.timestamp}: {event.variant} 버튼 클릭</div>
  {/each}
</div>
```

### 타입 안전한 이벤트 시스템

TypeScript와 함께 사용할 때 타입 안전성을 보장할 수 있습니다.

```typescript
// types.ts
export interface CustomClickEvent {
  timestamp: number;
  variant: string;
  originalEvent: MouseEvent;
}

export interface FormSubmitEvent {
  formData: Record<string, any>;
  isValid: boolean;
}
```

```html
<!-- TypedComponent.svelte -->
<script lang="ts">
  import type { CustomClickEvent } from './types';
  import { createEventDispatcher } from 'svelte';

  interface Events {
    customClick: CustomClickEvent;
    formSubmit: FormSubmitEvent;
  }

  const dispatch = createEventDispatcher<Events>();

  function handleClick(event: MouseEvent) {
    dispatch('customClick', {
      timestamp: Date.now(),
      variant: 'typed',
      originalEvent: event,
    });
  }
</script>

<button on:click="{handleClick}">타입 안전한 이벤트</button>
```

### 고급 이벤트 패턴

```html
<script>
  // 이벤트 체이닝
  function createEventChain() {
    return {
      step1: data => dispatch('step1', data),
      step2: data => dispatch('step2', data),
      complete: data => dispatch('complete', data),
    };
  }

  // 조건부 이벤트
  function conditionalDispatch(condition, eventName, data) {
    if (condition) {
      dispatch(eventName, data);
    }
  }

  // 이벤트 디바운싱
  let timeoutId;
  function debouncedDispatch(eventName, data, delay = 300) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      dispatch(eventName, data);
    }, delay);
  }
</script>
```
