---
sidebar_position: 3
sidebar_label: 3. Svelte 기초 개념
---

<!-- 레퍼런스 스타일로 아래 목차의 내용을 markdown으로 작성해서 아티팩트창으로 보여줘. -->

# Svelte 5 레퍼런스

# 3. 템플릿 문법 (Modern)

## 3.1 기본 표현식

### `{expression}` 기본 문법

Svelte 템플릿에서 JavaScript 표현식을 사용하려면 중괄호 `{}`로 감쌉니다.

```html
<script>
  let name = 'World';
  let count = 42;
</script>

<h1>Hello {name}!</h1>
<p>Count: {count}</p>
<p>Double: {count * 2}</p>
```

### JavaScript 표현식 사용

중괄호 안에는 유효한 JavaScript 표현식을 사용할 수 있습니다.

```html
<script>
  let user = { name: 'Alice', age: 30 };
  let items = ['apple', 'banana', 'cherry'];
</script>

<p>User: {user.name} ({user.age})</p>
<p>First item: {items[0]}</p>
<p>Uppercase: {user.name.toUpperCase()}</p>
<p>Conditional: {user.age >= 18 ? 'Adult' : 'Minor'}</p>
```

### 주석 `<!-- -->`와 `{/* */}`

HTML 주석과 JavaScript 스타일 주석을 모두 사용할 수 있습니다.

```html
<!-- HTML 주석 - 렌더링된 HTML에 포함됨 -->
{/* JavaScript 주석 - 컴파일 시 제거됨 */}

<p>Hello World</p>
{/* 이 주석은 최종 HTML에 포함되지 않음 */}
```

## 3.2 HTML 속성 바인딩

### 속성 바인딩 `{value}`

동적으로 속성 값을 설정할 수 있습니다.

```html
<script>
  let src = 'image.jpg';
  let alt = '이미지 설명';
  let className = 'highlight';
</script>

<img {src} {alt} class="{className}" />
<input type="text" value="{name}" />
```

### 불린 속성 `{disabled}`

불린 속성은 값이 truthy일 때만 포함됩니다.

```html
<script>
  let isDisabled = true;
  let isChecked = false;
</script>

<button disabled="{isDisabled}">버튼</button>
<input type="checkbox" checked="{isChecked}" />
```

### 속성 단축 문법 `{name}`

속성명과 변수명이 같을 때 단축 문법을 사용할 수 있습니다.

```html
<script>
  let src = 'image.jpg';
  let alt = '이미지';
  let disabled = true;
</script>

<!-- 일반 문법 -->
<img src="{src}" alt="{alt}" />
<button disabled="{disabled}">버튼</button>

<!-- 단축 문법 -->
<img {src} {alt} />
<button {disabled}>버튼</button>
```

### 스프레드 속성 `{...props}`

객체의 모든 속성을 한 번에 전달할 수 있습니다.

```html
<script>
  let props = {
    src: 'image.jpg',
    alt: '이미지',
    width: 200,
    height: 150,
  };
</script>

<img {...props} />
<!-- 다른 속성과 함께 사용 가능 -->
<img {...props} class="rounded" />
```

## 3.3 조건부 렌더링

### `{#if condition}` 기본 조건문

조건에 따라 콘텐츠를 조건부로 렌더링합니다.

```html
<script>
  let user = { name: 'Alice', loggedIn: true };
</script>

{#if user.loggedIn}
<p>환영합니다, {user.name}님!</p>
{/if}
```

### `{:else if condition}` 다중 조건

여러 조건을 체크할 수 있습니다.

```html
<script>
  let score = 85;
</script>

{#if score >= 90}
<p class="grade-a">A등급</p>
{:else if score >= 80}
<p class="grade-b">B등급</p>
{:else if score >= 70}
<p class="grade-c">C등급</p>
{:else}
<p class="grade-f">재시험</p>
{/if}
```

### `{:else}` else 블록

조건이 거짓일 때 실행되는 블록입니다.

```html
<script>
  let isLoggedIn = false;
</script>

{#if isLoggedIn}
<button>로그아웃</button>
{:else}
<button>로그인</button>
{/if}
```

### 중첩 조건문

조건문을 중첩하여 사용할 수 있습니다.

```html
<script>
  let user = { role: 'admin', active: true };
</script>

{#if user} {#if user.role === 'admin'} {#if user.active}
<p>활성 관리자</p>
{:else}
<p>비활성 관리자</p>
{/if} {:else}
<p>일반 사용자</p>
{/if} {/if}
```

## 3.4 반복 렌더링

### `{#each items as item}` 기본 반복

배열의 각 항목에 대해 콘텐츠를 반복 렌더링합니다.

```html
<script>
  let fruits = ['사과', '바나나', '체리'];
</script>

<ul>
  {#each fruits as fruit}
  <li>{fruit}</li>
  {/each}
</ul>
```

### `{#each items as item, index}` 인덱스 포함

인덱스도 함께 사용할 수 있습니다.

```html
<script>
  let colors = ['빨강', '초록', '파랑'];
</script>

<ol>
  {#each colors as color, i}
  <li>{i + 1}. {color}</li>
  {/each}
</ol>
```

### `{#each items as item (key)}` 키를 이용한 반복

키를 사용하여 성능을 최적화하고 상태를 유지합니다.

```html
<script>
  let users = [
    { id: 1, name: 'Alice' },
    { id: 2, name: 'Bob' },
    { id: 3, name: 'Charlie' },
  ];
</script>

<ul>
  {#each users as user (user.id)}
  <li>{user.name}</li>
  {/each}
</ul>
```

### `{:else}` 빈 배열 처리

배열이 비어있을 때 표시할 콘텐츠를 정의합니다.

```html
<script>
  let items = [];
</script>

<ul>
  {#each items as item}
  <li>{item}</li>
  {:else}
  <li>항목이 없습니다</li>
  {/each}
</ul>
```

### 중첩 반복문

반복문을 중첩하여 사용할 수 있습니다.

```html
<script>
  let categories = [
    { name: '과일', items: ['사과', '바나나'] },
    { name: '채소', items: ['당근', '브로콜리'] },
  ];
</script>

{#each categories as category}
<h3>{category.name}</h3>
<ul>
  {#each category.items as item}
  <li>{item}</li>
  {/each}
</ul>
{/each}
```

## 3.5 대기 블록 (Await)

### `{#await promise}` 비동기 처리

Promise의 상태에 따라 다른 콘텐츠를 렌더링합니다.

```html
<script>
  async function fetchUser() {
    const response = await fetch('/api/user');
    return response.json();
  }

  let userPromise = fetchUser();
</script>

{#await userPromise}
<p>사용자 정보를 불러오는 중...</p>
{:then user}
<p>안녕하세요, {user.name}님!</p>
{:catch error}
<p>오류가 발생했습니다: {error.message}</p>
{/await}
```

### `{:then result}` 성공 시 처리

Promise가 성공적으로 resolve되었을 때의 처리입니다.

```html
<script>
  let dataPromise = fetch('/api/data').then(r => r.json());
</script>

{#await dataPromise}
<div>로딩 중...</div>
{:then data}
<div>데이터: {JSON.stringify(data)}</div>
{/await}
```

### `{:catch error}` 에러 처리

Promise가 reject되었을 때의 에러 처리입니다.

```html
<script>
  let riskyOperation = new Promise((resolve, reject) => {
    setTimeout(() => reject(new Error('작업 실패')), 1000);
  });
</script>

{#await riskyOperation}
<p>처리 중...</p>
{:then result}
<p>성공: {result}</p>
{:catch error}
<p style="color: red;">에러: {error.message}</p>
{/await}
```

### 단축 await 문법

로딩 상태나 에러 처리가 필요 없는 경우 단축 문법을 사용할 수 있습니다.

```html
<script>
  let promise = fetch('/api/data').then(r => r.json());
</script>

<!-- 성공 시만 처리 -->
{#await promise then data}
<p>데이터: {data.value}</p>
{/await}

<!-- 에러 시만 처리 -->
{#await promise catch error}
<p>에러: {error.message}</p>
{/await}
```

## 3.6 키 블록 (Key)

### `{#key expression}` 강제 재생성

키 값이 변경될 때 내부 콘텐츠를 강제로 재생성합니다.

```html
<script>
  let count = 0;
  let reset = () => (count = 0);
</script>

{#key count}
<div>현재 카운트: {count}</div>
<AnimatedComponent value="{count}" />
{/key}

<button on:click="{()" ="">count++}>증가</button>
<button on:click="{reset}">리셋</button>
```

### 컴포넌트 재마운트 제어

컴포넌트의 상태를 완전히 초기화하고 싶을 때 사용합니다.

```html
<script>
  let userId = 1;
  let refreshKey = 0;
</script>

{#key refreshKey}
<UserProfile {userId} />
{/key}

<button on:click="{()" ="">refreshKey++}> 프로필 새로고침</button>
```

## 3.7 스니펫 시스템 (Svelte 5 신규)

### `{#snippet name}` 스니펫 정의

재사용 가능한 템플릿 조각을 정의합니다.

```html
{#snippet greeting(name)}
<div class="greeting">
  <h2>안녕하세요, {name}님!</h2>
  <p>오늘도 좋은 하루 되세요.</p>
</div>
{/snippet}

<!-- 스니펫 사용 -->
{@render greeting('김철수')} {@render greeting('이영희')}
```

### `{@render snippet()}` 스니펫 렌더링

정의된 스니펫을 렌더링합니다.

```html
{#snippet button(text, variant = 'primary')}
<button class="btn btn-{variant}">{text}</button>
{/snippet} {@render button('저장')} {@render button('취소', 'secondary')} {@render button('삭제',
'danger')}
```

### 매개변수가 있는 스니펫

스니펫에 매개변수를 전달하여 동적으로 사용할 수 있습니다.

```html
{#snippet card(title, content, actions)}
<div class="card">
  <div class="card-header">
    <h3>{title}</h3>
  </div>
  <div class="card-body">
    <p>{content}</p>
  </div>
  <div class="card-footer">{@render actions}</div>
</div>
{/snippet} {#snippet cardActions}
<button>수정</button>
<button>삭제</button>
{/snippet} {@render card('게시글 제목', '게시글 내용입니다.', cardActions)}
```

### 스니펫 재사용 패턴

스니펫을 활용한 일반적인 재사용 패턴입니다.

```html
{#snippet listItem(item, index)}
<li class="list-item" class:even="{index" % 2="" ="" ="0}">
  <span class="item-name">{item.name}</span>
  <span class="item-value">{item.value}</span>
</li>
{/snippet}

<script>
  let items = [
    { name: '항목 1', value: 100 },
    { name: '항목 2', value: 200 },
    { name: '항목 3', value: 300 },
  ];
</script>

<ul>
  {#each items as item, i} {@render listItem(item, i)} {/each}
</ul>
```

## 3.8 특수 템플릿 태그

### `{@html htmlString}` 원시 HTML 삽입

문자열을 HTML로 직접 삽입합니다. (XSS 주의)

```html
<script>
  let htmlContent = '<strong>굵은 텍스트</strong>와 <em>기울임 텍스트</em>';
  let userInput = '<script>alert("XSS")</script>'; // 위험!
</script>

<div>{@html htmlContent}</div>

<!-- 사용자 입력은 항상 검증 후 사용 -->
<!-- <div>{@html userInput}</div> -->
```

### `{@const variable = value}` 템플릿 내 상수

템플릿 내에서 상수를 정의하여 사용합니다.

```html
<script>
  let user = { firstName: '김', lastName: '철수', age: 30 };
</script>

{@const fullName = user.firstName + user.lastName} {@const isAdult = user.age >= 18}

<h1>{fullName}</h1>
{#if isAdult}
<p>성인입니다.</p>
{/if}
```

### `{@debug variables}` 디버깅 도구

개발 중 변수를 콘솔에 출력하여 디버깅할 수 있습니다.

```html
<script>
  let count = 0;
  let user = { name: 'Alice', role: 'admin' };
</script>

{@debug count, user}

<button on:click="{()" ="">count++}> 카운트 증가: {count}</button>
```

### `{@attach ...}` 새로운 첨부 문법 (Svelte 5)

DOM 요소에 특별한 동작을 첨부합니다.

```html
<script>
  import { tooltip } from './actions.js';
</script>

<!-- 액션을 요소에 첨부 -->
<button {@attach tooltip('도움말 텍스트')}>
  도움말
</button>

<!-- 여러 액션 첨부 -->
<input
  {@attach focus}
  {@attach validator(emailSchema)}
  type="email"
/>
```
