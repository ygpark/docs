---
sidebar_position: 2
sidebar_label: 2. Runes 시스템
---

# Svelte 5 레퍼런스

# 2. Runes 시스템 (Svelte 5 핵심)

## 2.1 Runes 개념 이해

### Runes란 무엇인가?

Runes는 Svelte 5에서 도입된 새로운 반응성 시스템으로, `$` 기호로 시작하는 특별한 함수들입니다. 컴파일 타임에 처리되어 효율적인 반응성을 제공하며, 기존의 마법적인 컴파일러 변환 대신 명시적이고 예측 가능한 API를 제공합니다.

**주요 Runes:**

- `$state()` - 반응성 상태 생성
- `$derived()` - 파생 상태 생성
- `$effect()` - 사이드 이펙트 처리
- `$props()` - 컴포넌트 속성 정의
- `$bindable()` - 양방향 바인딩
- `$inspect()` - 디버깅 도구
- `$host()` - 호스트 요소 접근

### 기존 반응성 시스템과의 차이점

**Svelte 4 (기존):**

```javascript
// 반응성 선언
let count = 0;

// 파생 상태
$: doubled = count * 2;

// 사이드 이펙트
$: console.log('count changed:', count);

// Props
export let name;
```

**Svelte 5 (Runes):**

```javascript
// 반응성 상태
let count = $state(0);

// 파생 상태
let doubled = $derived(count * 2);

// 사이드 이펙트
$effect(() => {
  console.log('count changed:', count);
});

// Props
let { name } = $props();
```

### Runes 사용 시점과 이유

**사용해야 하는 경우:**

- Svelte 5 프로젝트
- 더 명확한 반응성 로직이 필요한 경우
- TypeScript와의 더 나은 통합이 필요한 경우
- 복잡한 상태 관리가 필요한 경우

**장점:**

- 명시적이고 예측 가능한 동작
- 더 나은 TypeScript 지원
- 성능 최적화
- 디버깅 용이성

---

## 2.2 $state - 상태 관리

### `$state()` 기본 사용법

```javascript
// 기본 상태 생성
let count = $state(0);
let name = $state('');
let isVisible = $state(true);

// 상태 업데이트
count = 10;
name = 'Svelte';
isVisible = !isVisible;
```

### 원시값 상태 관리

```javascript
// 숫자
let age = $state(25);
age += 1;

// 문자열
let message = $state('Hello');
message = 'Hello World';

// 불린
let loading = $state(false);
loading = true;

// null/undefined
let data = $state(null);
data = { id: 1, title: 'Item' };
```

### 객체와 배열 상태 관리

```javascript
// 객체 상태
let user = $state({
  name: 'John',
  email: 'john@example.com',
  age: 30,
});

// 객체 업데이트 (깊은 반응성)
user.name = 'Jane';
user.age += 1;

// 배열 상태
let items = $state([1, 2, 3]);

// 배열 조작
items.push(4);
items[0] = 10;
items = items.filter(item => item > 2);

// 중첩된 객체
let todo = $state({
  id: 1,
  title: 'Learn Svelte',
  completed: false,
  tags: ['frontend', 'svelte'],
});

todo.completed = true;
todo.tags.push('runes');
```

### `$state.frozen()` 불변 상태

```javascript
// 불변 객체 생성
let config = $state.frozen({
  apiUrl: 'https://api.example.com',
  timeout: 5000,
  retries: 3,
});

// 오류 발생 - 불변 객체는 수정 불가
// config.apiUrl = 'new-url'; // TypeError

// 새로운 객체로 교체해야 함
config = $state.frozen({
  ...config,
  apiUrl: 'https://new-api.example.com',
});

// 배열도 동일
let numbers = $state.frozen([1, 2, 3]);
// numbers.push(4); // TypeError
numbers = $state.frozen([...numbers, 4]);
```

### `$state.snapshot()` 스냅샷

```javascript
let user = $state({
  name: 'John',
  preferences: {
    theme: 'dark',
    language: 'en',
  },
});

// 현재 상태의 스냅샷 생성 (일반 JavaScript 객체)
let userSnapshot = $state.snapshot(user);

// 스냅샷은 반응성이 없는 일반 객체
console.log(userSnapshot); // { name: 'John', preferences: { theme: 'dark', language: 'en' } }

// 스냅샷을 JSON으로 변환
let jsonData = JSON.stringify($state.snapshot(user));

// API 전송용 데이터 준비
async function saveUser() {
  const userData = $state.snapshot(user);
  await fetch('/api/users', {
    method: 'POST',
    body: JSON.stringify(userData),
  });
}
```

---

## 2.3 $derived - 파생 상태

### `$derived()` 기본 개념

```javascript
let count = $state(0);
let doubled = $derived(count * 2);
let isEven = $derived(count % 2 === 0);

// 여러 상태로부터 파생
let firstName = $state('John');
let lastName = $state('Doe');
let fullName = $derived(`${firstName} ${lastName}`);

// 조건부 파생
let age = $state(20);
let status = $derived(age >= 18 ? 'adult' : 'minor');
```

### 계산된 값 생성

```javascript
// 배열 처리
let items = $state([
  { name: 'Apple', price: 1.2, category: 'fruit' },
  { name: 'Bread', price: 2.5, category: 'bakery' },
  { name: 'Banana', price: 0.8, category: 'fruit' },
]);

let totalPrice = $derived(items.reduce((sum, item) => sum + item.price, 0));

let fruitItems = $derived(items.filter(item => item.category === 'fruit'));

let sortedItems = $derived([...items].sort((a, b) => a.price - b.price));

// 복잡한 계산
let users = $state([
  { name: 'Alice', age: 25, role: 'admin' },
  { name: 'Bob', age: 30, role: 'user' },
  { name: 'Charlie', age: 35, role: 'user' },
]);

let statistics = $derived({
  total: users.length,
  averageAge: users.reduce((sum, user) => sum + user.age, 0) / users.length,
  adminCount: users.filter(user => user.role === 'admin').length,
  oldestUser: users.reduce((oldest, user) => (user.age > oldest.age ? user : oldest), users[0]),
});
```

### `$derived.by()` 복잡한 파생 로직

```javascript
let searchTerm = $state('');
let products = $state([
  { id: 1, name: 'Laptop', price: 999, tags: ['electronics', 'computer'] },
  { id: 2, name: 'Phone', price: 599, tags: ['electronics', 'mobile'] },
  { id: 3, name: 'Book', price: 29, tags: ['education', 'reading'] },
]);

// 복잡한 검색 로직
let searchResults = $derived.by(() => {
  if (!searchTerm.trim()) {
    return products;
  }

  const term = searchTerm.toLowerCase();

  return products.filter(product => {
    const nameMatch = product.name.toLowerCase().includes(term);
    const tagMatch = product.tags.some(tag => tag.toLowerCase().includes(term));
    return nameMatch || tagMatch;
  });
});

// 비동기 작업과 함께 사용
let userId = $state(1);
let userProfile = $derived.by(async () => {
  if (!userId) return null;

  try {
    const response = await fetch(`/api/users/${userId}`);
    return await response.json();
  } catch (error) {
    console.error('Failed to fetch user:', error);
    return null;
  }
});
```

### 기존 `$:` 문법과의 비교

**Svelte 4:**

```javascript
let count = 0;
$: doubled = count * 2;
$: quadrupled = doubled * 2;
$: description = `Count is ${count}, doubled is ${doubled}`;
```

**Svelte 5:**

```javascript
let count = $state(0);
let doubled = $derived(count * 2);
let quadrupled = $derived(doubled * 2);
let description = $derived(`Count is ${count}, doubled is ${doubled}`);
```

---

## 2.4 $effect - 사이드 이펙트

### `$effect()` 기본 사용법

```javascript
let count = $state(0);

// 기본 이펙트
$effect(() => {
  console.log('Count changed:', count);
});

// 조건부 이펙트
$effect(() => {
  if (count > 10) {
    console.log('Count is high!');
  }
});

// 여러 상태 추적
let name = $state('');
let email = $state('');

$effect(() => {
  console.log('User data:', { name, email });
});
```

### DOM 조작과 외부 API 호출

```javascript
let theme = $state('light');

// DOM 조작
$effect(() => {
  document.body.className = theme;
});

// 로컬 스토리지 동기화
$effect(() => {
  localStorage.setItem('theme', theme);
});

// API 호출
let userId = $state(null);
let userData = $state(null);

$effect(() => {
  if (userId) {
    fetch(`/api/users/${userId}`)
      .then(response => response.json())
      .then(data => {
        userData = data;
      })
      .catch(error => {
        console.error('API Error:', error);
        userData = null;
      });
  }
});

// 외부 라이브러리 통합
let chartData = $state([]);

$effect(() => {
  const chart = new Chart(document.getElementById('myChart'), {
    type: 'bar',
    data: chartData,
  });

  return () => {
    chart.destroy();
  };
});
```

### `$effect.pre()` DOM 업데이트 전 실행

```javascript
let items = $state([1, 2, 3]);
let scrollPosition = 0;

// DOM 업데이트 전에 스크롤 위치 저장
$effect.pre(() => {
  const container = document.getElementById('list-container');
  if (container) {
    scrollPosition = container.scrollTop;
  }
});

// DOM 업데이트 후에 스크롤 위치 복원
$effect(() => {
  const container = document.getElementById('list-container');
  if (container) {
    container.scrollTop = scrollPosition;
  }
});
```

### `$effect.tracking()` 의존성 확인

```javascript
let a = $state(1);
let b = $state(2);
let c = $state(3);

$effect(() => {
  console.log('Effect running');

  // 현재 추적 중인 의존성 확인
  if ($effect.tracking()) {
    console.log('Currently tracking dependencies');
  }

  const sum = a + b; // a, b만 의존성으로 추가됨

  if (sum > 5) {
    console.log('Sum with c:', sum + c); // 조건부로 c 추가
  }
});
```

### `$effect.root()` 루트 이펙트

```javascript
// 컴포넌트 라이프사이클과 독립적인 이펙트
function createGlobalListener() {
  let eventCount = $state(0);

  const cleanup = $effect.root(() => {
    const handleClick = () => {
      eventCount++;
    };

    document.addEventListener('click', handleClick);

    $effect(() => {
      console.log('Global clicks:', eventCount);
    });

    return () => {
      document.removeEventListener('click', handleClick);
    };
  });

  return cleanup;
}

// 사용 예
const cleanupGlobalListener = createGlobalListener();

// 필요할 때 정리
onDestroy(() => {
  cleanupGlobalListener();
});
```

### 정리(cleanup) 함수

```javascript
let isOnline = $state(navigator.onLine);

$effect(() => {
  const handleOnline = () => (isOnline = true);
  const handleOffline = () => (isOnline = false);

  window.addEventListener('online', handleOnline);
  window.addEventListener('offline', handleOffline);

  // 정리 함수 반환
  return () => {
    window.removeEventListener('online', handleOnline);
    window.removeEventListener('offline', handleOffline);
  };
});

// 타이머 정리
let seconds = $state(0);

$effect(() => {
  const interval = setInterval(() => {
    seconds++;
  }, 1000);

  return () => {
    clearInterval(interval);
  };
});

// 비동기 작업 취소
let query = $state('');
let results = $state([]);

$effect(() => {
  if (!query.trim()) {
    results = [];
    return;
  }

  const controller = new AbortController();

  fetch(`/api/search?q=${query}`, {
    signal: controller.signal,
  })
    .then(response => response.json())
    .then(data => {
      results = data;
    })
    .catch(error => {
      if (error.name !== 'AbortError') {
        console.error('Search error:', error);
      }
    });

  return () => {
    controller.abort();
  };
});
```

---

## 2.5 $props - 컴포넌트 속성

### `$props()` 새로운 Props 시스템

```javascript
<!-- Child.svelte -->
<script>
  // 모든 props 받기
  let props = $props();
  console.log(props); // { name: 'John', age: 30, ... }

  // 특정 props만 구조 분해
  let { name, age } = $props();

  // 나머지 props
  let { name, age, ...rest } = $props();
</script>

<div>
  <h1>Hello {name}</h1>
  <p>Age: {age}</p>
</div>
```

### Props 구조 분해

```javascript
<!-- UserCard.svelte -->
<script>
  // 기본 구조 분해
  let {
    name,
    email,
    avatar,
    isOnline
  } = $props();

  // 중첩된 객체 구조 분해
  let {
    user: {
      profile: { name, bio },
      settings: { theme, language }
    }
  } = $props();

  // 배열 구조 분해
  let {
    coordinates: [x, y],
    colors: [primary, secondary, ...otherColors]
  } = $props();
</script>

<div class="user-card" class:online={isOnline}>
  <img src={avatar} alt={name} />
  <h2>{name}</h2>
  <p>{email}</p>
</div>
```

### 기본값 설정 패턴

```javascript
<!-- Button.svelte -->
<script>
  // 기본값과 함께 구조 분해
  let {
    variant = 'primary',
    size = 'medium',
    disabled = false,
    loading = false,
    children,
    onclick = () => {}
  } = $props();

  // 함수형 기본값
  let {
    id = crypto.randomUUID(),
    timestamp = Date.now(),
    className = '',
    style = {}
  } = $props();

  // 조건부 기본값
  let {
    theme = 'light',
    color = theme === 'dark' ? '#ffffff' : '#000000'
  } = $props();
</script>

<button
  {id}
  class="{variant} {size} {className}"
  {style}
  {disabled}
  onclick={onclick}
  class:loading
>
  {#if loading}
    <Spinner />
  {/if}
  {@render children?.()}
</button>
```

### 타입 안전성 확보

```typescript
<!-- TypeScript에서 Props 타입 정의 -->
<script lang="ts">
  interface Props {
    name: string;
    age?: number;
    emails: string[];
    onSubmit: (data: FormData) => void;
    user: {
      id: number;
      profile: {
        avatar: string;
        bio?: string;
      };
    };
  }

  let {
    name,
    age = 0,
    emails,
    onSubmit,
    user
  }: Props = $props();

  // 런타임 검증
  $effect(() => {
    if (age < 0) {
      console.warn('Age cannot be negative');
    }

    if (!emails.length) {
      console.warn('At least one email is required');
    }
  });
</script>

<!-- JSDoc을 사용한 타입 정의 -->
<script>
  /**
   * @typedef {Object} UserProps
   * @property {string} name - 사용자 이름
   * @property {number} [age] - 사용자 나이
   * @property {string[]} emails - 이메일 목록
   * @property {(data: FormData) => void} onSubmit - 제출 핸들러
   */

  /** @type {UserProps} */
  let { name, age = 0, emails, onSubmit } = $props();
</script>
```

### 기존 `export let`과의 비교

**Svelte 4:**

```javascript
<!-- Child.svelte -->
<script>
  export let name;
  export let age = 0;
  export let emails = [];
  export let onSubmit = () => {};

  // Props 변경 감지
  $: console.log('name changed:', name);
</script>
```

**Svelte 5:**

```javascript
<!-- Child.svelte -->
<script>
  let {
    name,
    age = 0,
    emails = [],
    onSubmit = () => {}
  } = $props();

  // Props 변경 감지
  $effect(() => {
    console.log('name changed:', name);
  });
</script>
```

---

## 2.6 $bindable - 양방향 바인딩

### `$bindable()` 개념

```javascript
<!-- Input.svelte -->
<script>
  let { value = $bindable('') } = $props();
</script>

<input bind:value />

<!-- Parent.svelte -->
<script>
  let inputValue = $state('');
</script>

<Input bind:value={inputValue} />
<p>Current value: {inputValue}</p>
```

### 컴포넌트 간 양방향 통신

```javascript
<!-- Counter.svelte -->
<script>
  let { count = $bindable(0) } = $props();

  function increment() {
    count++;
  }

  function decrement() {
    count--;
  }
</script>

<div class="counter">
  <button onclick={decrement}>-</button>
  <span>{count}</span>
  <button onclick={increment}>+</button>
</div>

<!-- App.svelte -->
<script>
  let counterValue = $state(5);
  let anotherValue = $state(10);
</script>

<Counter bind:count={counterValue} />
<Counter bind:count={anotherValue} />

<p>Total: {counterValue + anotherValue}</p>
```

### 바인딩 가능한 Props 생성

```javascript
<!-- Form.svelte -->
<script>
  let {
    formData = $bindable({
      name: '',
      email: '',
      age: 0
    }),
    errors = $bindable({})
  } = $props();

  function validateField(field, value) {
    const newErrors = { ...errors };

    switch (field) {
      case 'email':
        if (!value.includes('@')) {
          newErrors.email = 'Invalid email format';
        } else {
          delete newErrors.email;
        }
        break;

      case 'age':
        if (value < 0) {
          newErrors.age = 'Age must be positive';
        } else {
          delete newErrors.age;
        }
        break;
    }

    errors = newErrors;
  }

  function handleInput(field) {
    return (event) => {
      formData[field] = event.target.value;
      validateField(field, event.target.value);
    };
  }
</script>

<form>
  <div>
    <label>
      Name:
      <input
        value={formData.name}
        oninput={handleInput('name')}
      />
    </label>
    {#if errors.name}
      <span class="error">{errors.name}</span>
    {/if}
  </div>

  <div>
    <label>
      Email:
      <input
        type="email"
        value={formData.email}
        oninput={handleInput('email')}
      />
    </label>
    {#if errors.email}
      <span class="error">{errors.email}</span>
    {/if}
  </div>

  <div>
    <label>
      Age:
      <input
        type="number"
        value={formData.age}
        oninput={handleInput('age')}
      />
    </label>
    {#if errors.age}
      <span class="error">{errors.age}</span>
    {/if}
  </div>
</form>

<!-- Parent 컴포넌트 -->
<script>
  let userData = $state({
    name: 'John',
    email: 'john@example.com',
    age: 30
  });

  let formErrors = $state({});

  $effect(() => {
    console.log('Form data:', userData);
    console.log('Form errors:', formErrors);
  });
</script>

<Form
  bind:formData={userData}
  bind:errors={formErrors}
/>

<pre>{JSON.stringify(userData, null, 2)}</pre>
```

---

## 2.7 $inspect - 디버깅

### `$inspect()` 디버깅 도구

```javascript
let count = $state(0);
let doubled = $derived(count * 2);

// 단일 값 검사
$inspect(count);

// 여러 값 검사
$inspect(count, doubled);

// 라벨과 함께 검사
$inspect('Counter State:', { count, doubled });

function increment() {
  count++;
  $inspect('After increment:', count);
}
```

### 상태 변화 추적

```javascript
let user = $state({
  name: 'John',
  email: 'john@example.com',
  preferences: {
    theme: 'light',
    language: 'en',
  },
});

// 객체 전체 추적
$inspect('User:', user);

// 특정 속성만 추적
$inspect('User Theme:', user.preferences.theme);

// 조건부 검사
$inspect(user.name.length > 10 ? 'Long name' : 'Short name', user.name);

// 함수에서의 검사
function updateUserTheme(newTheme) {
  $inspect('Before theme change:', user.preferences.theme);
  user.preferences.theme = newTheme;
  $inspect('After theme change:', user.preferences.theme);
}
```

### 개발 환경에서의 활용

```javascript
// 개발 모드에서만 활성화
let debugMode = $state(true);
let apiCalls = $state([]);

$effect(() => {
  if (debugMode) {
    $inspect('API Calls:', apiCalls);
  }
});

// 성능 측정
let renderTime = $state(0);

$effect.pre(() => {
  if (debugMode) {
    performance.mark('render-start');
  }
});

$effect(() => {
  if (debugMode) {
    performance.mark('render-end');
    performance.measure('render-time', 'render-start', 'render-end');
    const measure = performance.getEntriesByName('render-time')[0];
    renderTime = measure.duration;
    $inspect('Render Time (ms):', renderTime);
  }
});

// 복잡한 상태 추적
let shoppingCart = $state({
  items: [],
  total: 0,
  discounts: [],
});

$derived(() => {
  const calculatedTotal = shoppingCart.items.reduce(
    (sum, item) => sum + item.price * item.quantity,
    0
  );

  $inspect('Cart Calculation:', {
    itemCount: shoppingCart.items.length,
    subtotal: calculatedTotal,
    discounts: shoppingCart.discounts,
    final: shoppingCart.total,
  });

  return calculatedTotal;
});
```

---

## 2.8 $host - 호스트 요소 접근

### `$host()` 사용법

```javascript
<!-- CustomElement.svelte -->
<script>
  import { tick } from 'svelte';

  // 호스트 요소 참조
  const host = $host();

  let isVisible = $state(false);

  // 호스트 요소 조작
  $effect(() => {
    if (host) {
      host.style.opacity = isVisible ? '1' : '0';
      host.style.transition = 'opacity 0.3s ease';
    }
  });

  // 호스트 이벤트 리스너
  $effect(() => {
    if (host) {
      const handleClick = (event) => {
        console.log('Host clicked:', event);
        isVisible = !isVisible;
      };

      host.addEventListener('click', handleClick);

      return () => {
        host.removeEventListener('click', handleClick);
      };
    }
  });

  // 호스트 속성 설정
  $effect(() => {
    if (host) {
      host.setAttribute('data-visible', isVisible.toString());
      host.classList.toggle('visible', isVisible);
    }
  });
</script>

<div class="content">
  <button onclick={() => isVisible = !isVisible}>
    Toggle Visibility
  </button>

  {#if isVisible}
    <p>This content is visible!</p>
  {/if}
</div>

<style>
  .content {
    padding: 1rem;
    border: 1px solid #ccc;
    border-radius: 4px;
  }

  :host(.visible) {
    background-color: #e8f5e8;
  }

  :host([data-visible="true"]) {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  }
</style>
```

### 커스텀 요소에서의 활용

```javascript
<!-- WebComponent.svelte -->
<script>
  const host = $host();

  let { title = 'Default Title' } = $props();
  let expanded = $state(false);

  // 커스텀 요소 속성 동기화
  $effect(() => {
    if (host) {
      // 속성을 DOM에 반영
      host.setAttribute('title', title);
      host.setAttribute('expanded', expanded.toString());

      // CSS 커스텀 속성 설정
      host.style.setProperty('--expansion-height',
        expanded ? 'auto' : '60px');
    }
  });

  // 외부에서 속성 변경 감지
  $effect(() => {
    if (host) {
      const observer = new MutationObserver((mutations) => {
        mutations.forEach((mutation) => {
          if (mutation.type === 'attributes') {
            const attrName = mutation.attributeName;
            const newValue = host.getAttribute(attrName);

            if (attrName === 'expanded') {
              expanded = newValue === 'true';
            }
          }
        });
      });

      observer.observe(host, { attributes: true });

      return () => {
        observer.disconnect();
      };
    }
  });

  // 커스텀 이벤트 발생
  function toggleExpand() {
    expanded = !expanded;

    if (host) {
      host.dispatchEvent(new CustomEvent('expand-change', {
        detail: { expanded },
        bubbles: true
      }));
    }
  }

  // 슬롯 내용 감지
  $effect(() => {
    if (host) {
      const checkSlotContent = () => {
        const hasContent = host.innerHTML.trim().length > 0;
        host.classList.toggle('has-content', hasContent);
      };

      checkSlotContent();

      const observer = new MutationObserver(checkSlotContent);
      observer.observe(host, {
        childList: true,
        subtree: true,
        characterData: true
      });

      return () => {
        observer.disconnect();
      };
    }
  });
</script>

<div class="header" onclick={toggleExpand}>
  <h3>{title}</h3>
  <span class="toggle" class:expanded>▼</span>
</div>

{#if expanded}
  <div class="content">
    <slot />
  </div>
{/if}

<style>
  :host {
    display: block;
    border: 1px solid #ddd;
    border-radius: 4px;
    overflow: hidden;
    transition: all 0.3s ease;
  }

  :host(.has-content) {
    border-color: #007bff;
  }

  .header {
    padding: 1rem;
    background: #f8f9fa;
    cursor: pointer;
    display: flex;
    justify-content: space-between;
    align-items: center;
  }

  .toggle {
    transition: transform 0.3s ease;
  }

  .toggle.expanded {
    transform: rotate(180deg);
  }

  .content {
    padding: 1rem;
    max-height: var(--expansion-height, auto);
    overflow: hidden;
    transition: max-height 0.3s ease;
  }
</style>
```

---

## 마무리

Svelte 5의 Runes 시스템은 명시적이고 예측 가능한 반응성을 제공하여, 더 강력하고 유지보수 가능한 애플리케이션을 구축할 수 있게 해줍니다. 각 Rune은 특정한 목적을 가지고 있으며, 적절히 조합하여 사용하면 복잡한 상태 관리와 사이드 이펙트를 효율적으로 처리할 수 있습니다.

**핵심 포인트:**

- `$state()`로 반응성 상태 생성
- `$derived()`로 계산된 값 정의
- `$effect()`로 사이드 이펙트 관리
- `$props()`로 컴포넌트 속성 처리
- `$bindable()`로 양방향 바인딩 구현
- `$inspect()`로 디버깅 지원
- `$host()`로 호스트 요소 제어

이러한 도구들을 적절히 활용하면 Svelte 5에서 더욱 견고하고 성능 좋은 웹 애플리케이션을 개발할 수 있습니다.
