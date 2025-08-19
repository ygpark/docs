---
sidebar_position: 6
sidebar_label: 6. 양방향 데이터 바인딩
---

<!-- 레퍼런스 스타일로 아래 목차의 내용을 markdown으로 작성해서 아티팩트창으로 보여줘. -->

# Svelte 5 레퍼런스

# 6. 양방향 데이터 바인딩

## 6.1 기본 바인딩

### `bind:value` 입력값 바인딩

입력 요소의 값을 변수와 양방향으로 연결합니다.

```html
<script>
  let name = '';
  let message = '';
  let number = 0;
</script>

<!-- 텍스트 입력 -->
<input bind:value="{name}" placeholder="이름을 입력하세요" />
<p>안녕하세요, {name}!</p>

<!-- 텍스트 영역 -->
<textarea bind:value="{message}"></textarea>

<!-- 숫자 입력 -->
<input type="number" bind:value="{number}" />
<p>숫자: {number}</p>
```

### `bind:checked` 체크박스 바인딩

체크박스의 체크 상태를 boolean 변수와 연결합니다.

```html
<script>
  let agreed = false;
  let newsletter = true;
</script>

<label>
  <input type="checkbox" bind:checked="{agreed}" />
  약관에 동의합니다
</label>

<label>
  <input type="checkbox" bind:checked="{newsletter}" />
  뉴스레터 구독
</label>

<p>약관 동의: {agreed}</p>
<p>뉴스레터: {newsletter}</p>
```

### `bind:group` 라디오/체크박스 그룹

같은 그룹의 라디오 버튼이나 체크박스를 하나의 변수로 관리합니다.

```html
<script>
  let selectedColor = '';
  let selectedFruits = [];
</script>

<!-- 라디오 그룹 -->
<fieldset>
  <legend>색상 선택:</legend>
  <label><input type="radio" bind:group="{selectedColor}" value="red" />빨강</label>
  <label><input type="radio" bind:group="{selectedColor}" value="blue" />파랑</label>
  <label><input type="radio" bind:group="{selectedColor}" value="green" />초록</label>
</fieldset>

<!-- 체크박스 그룹 -->
<fieldset>
  <legend>과일 선택:</legend>
  <label><input type="checkbox" bind:group="{selectedFruits}" value="apple" />사과</label>
  <label><input type="checkbox" bind:group="{selectedFruits}" value="banana" />바나나</label>
  <label><input type="checkbox" bind:group="{selectedFruits}" value="orange" />오렌지</label>
</fieldset>

<p>선택된 색상: {selectedColor}</p>
<p>선택된 과일: {selectedFruits.join(', ')}</p>
```

## 6.2 고급 바인딩

### `bind:this` 요소 참조

DOM 요소에 대한 직접 참조를 얻습니다.

```html
<script>
  let canvas;
  let input;

  function drawOnCanvas() {
    const ctx = canvas.getContext('2d');
    ctx.fillStyle = 'red';
    ctx.fillRect(10, 10, 100, 100);
  }

  function focusInput() {
    input.focus();
  }
</script>

<canvas bind:this="{canvas}" width="200" height="200"></canvas>
<button onclick="{drawOnCanvas}">그리기</button>

<input bind:this="{input}" placeholder="포커스될 입력" />
<button onclick="{focusInput}">포커스</button>
```

### `bind:innerHTML` HTML 내용 바인딩

요소의 innerHTML을 변수와 연결합니다.

```html
<script>
  let htmlContent = '<strong>굵은 텍스트</strong>';
</script>

<div bind:innerHTML="{htmlContent}"></div>

<textarea bind:value="{htmlContent}"></textarea>
```

### `bind:textContent` 텍스트 내용 바인딩

요소의 텍스트 내용만을 변수와 연결합니다.

```html
<script>
  let textContent = '일반 텍스트';
</script>

<div bind:textContent="{textContent}"></div>

<input bind:value="{textContent}" />
```

## 6.3 미디어 바인딩

### `bind:currentTime` 비디오/오디오 시간

미디어의 현재 재생 시간을 제어합니다.

```html
<script>
  let currentTime = 0;
  let duration = 0;
</script>

<video bind:currentTime bind:duration src="video.mp4" controls></video>

<p>현재 시간: {Math.floor(currentTime)}초</p>
<p>전체 시간: {Math.floor(duration)}초</p>

<input type="range" min="0" max="{duration}" bind:value="{currentTime}" />
```

### `bind:duration` 재생 시간

미디어의 전체 재생 시간을 가져옵니다.

```html
<script>
  let duration = 0;
</script>

<audio bind:duration src="audio.mp3" controls></audio>
<p>오디오 길이: {Math.floor(duration)}초</p>
```

### `bind:paused` 일시정지 상태

미디어의 일시정지 상태를 제어합니다.

```html
<script>
  let paused = true;
</script>

<video bind:paused src="video.mp4"></video>

<button onclick="{()" ="">paused = !paused}> {paused ? '재생' : '일시정지'}</button>
```

### `bind:volume` 볼륨

미디어의 볼륨을 제어합니다 (0-1 사이의 값).

```html
<script>
  let volume = 0.5;
</script>

<audio bind:volume src="audio.mp3" controls></audio>

<label>
  볼륨: {Math.round(volume * 100)}%
  <input type="range" min="0" max="1" step="0.1" bind:value="{volume}" />
</label>
```

## 6.4 블록 레벨 바인딩

### `bind:offsetWidth`, `bind:offsetHeight` 요소 크기

요소의 실제 크기를 가져옵니다 (border 포함).

```html
<script>
  let width = 0;
  let height = 0;
</script>

<div
  bind:offsetWidth="{width}"
  bind:offsetHeight="{height}"
  style="width: 300px; height: 200px; border: 5px solid blue; padding: 20px;"
>
  크기 측정 영역
</div>

<p>너비: {width}px, 높이: {height}px</p>
```

### `bind:clientWidth`, `bind:clientHeight` 클라이언트 크기

요소의 내부 크기를 가져옵니다 (padding 포함, border 제외).

```html
<script>
  let clientWidth = 0;
  let clientHeight = 0;
</script>

<div
  bind:clientWidth
  bind:clientHeight
  style="width: 300px; height: 200px; border: 10px solid red; padding: 15px;"
>
  클라이언트 영역
</div>

<p>클라이언트 너비: {clientWidth}px, 높이: {clientHeight}px</p>
```

### `bind:scrollX`, `bind:scrollY` 스크롤 위치

요소의 스크롤 위치를 제어합니다.

```html
<script>
  let scrollX = 0;
  let scrollY = 0;
</script>

<div
  bind:scrollX
  bind:scrollY
  style="width: 200px; height: 200px; overflow: auto; border: 1px solid black;"
>
  <div style="width: 500px; height: 500px; background: linear-gradient(45deg, #f0f0f0, #ccc);">
    스크롤 가능한 큰 콘텐츠
    <p>스크롤 X: {scrollX}</p>
    <p>스크롤 Y: {scrollY}</p>
  </div>
</div>

<button onclick="{()" ="">scrollX = 100}>X 100으로 스크롤</button>
<button onclick="{()" ="">scrollY = 100}>Y 100으로 스크롤</button>
```

## 6.5 컴포넌트 바인딩 (Modern)

### `$bindable()` 활용 패턴

Svelte 5의 새로운 방식으로 컴포넌트 간 양방향 바인딩을 구현합니다.

**부모 컴포넌트:**

```html
<script>
  import Counter from './Counter.svelte';

  let count = 0;
</script>

<Counter bind:value="{count}" />
<p>부모에서 본 값: {count}</p>

<button onclick="{()" ="">count += 10}>부모에서 +10</button>
```

**자식 컴포넌트 (Counter.svelte):**

```html
<script>
  let { value = $bindable(0) } = $props();
</script>

<div>
  <p>카운터: {value}</p>
  <button onclick="{()" ="">value++}>+1</button>
  <button onclick="{()" ="">value--}>-1</button>
</div>
```

### 양방향 컴포넌트 통신

복잡한 상태를 가진 컴포넌트 간의 양방향 통신 예제입니다.

**부모 컴포넌트:**

```html
<script>
  import UserForm from './UserForm.svelte';

  let user = {
    name: '',
    email: '',
    age: 0,
  };
</script>

<UserForm bind:user />

<pre>{JSON.stringify(user, null, 2)}</pre>
```

**자식 컴포넌트 (UserForm.svelte):**

```html
<script>
  let { user = $bindable({ name: '', email: '', age: 0 }) } = $props();
</script>

<form>
  <label>
    이름:
    <input bind:value="{user.name}" />
  </label>

  <label>
    이메일:
    <input type="email" bind:value="{user.email}" />
  </label>

  <label>
    나이:
    <input type="number" bind:value="{user.age}" />
  </label>
</form>
```

# 바인딩 사용 시 주의사항

1. **성능**: 너무 많은 바인딩은 성능에 영향을 줄 수 있습니다.
2. **단방향 vs 양방향**: 필요한 경우에만 양방향 바인딩을 사용하세요.
3. **타입 안정성**: TypeScript 사용 시 바인딩되는 값의 타입을 명확히 하세요.
4. **생명주기**: 컴포넌트가 언마운트될 때 바인딩도 자동으로 정리됩니다.
