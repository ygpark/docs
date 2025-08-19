---
sidebar_position: 31
---

# 소개

Svelte는 웹에서 사용자 인터페이스를 구축하기 위한 프레임워크입니다. 컴파일러를 사용하여 HTML, CSS, JavaScript로 작성된 선언적 컴포넌트를...

```html
<!--- file: App.svelte --->
<script>
  function greet() {
    alert('Svelte에 오신 것을 환영합니다!');
  }
</script>

<button onclick="{greet}">클릭하세요</button>

<style>
  button {
    font-size: 2em;
  }
</style>
```

...가볍고 고도로 최적화된 JavaScript로 변환합니다.

웹에서 독립형 컴포넌트부터 야심찬 풀스택 앱(Svelte의 동반 애플리케이션 프레임워크인 [SvelteKit](https://svelte.dev/docs/kit/) 사용) 그리고 그 사이의 모든 것까지 무엇이든 구축할 수 있습니다.

이 페이지들은 참조 문서 역할을 합니다. Svelte를 처음 접하신다면 [인터랙티브 튜토리얼](https://svelte.dev/tutorial)부터 시작하고 궁금한 점이 있을 때 여기로 돌아오시는 것을 권합니다.

[플레이그라운드](https://svelte.dev/playground)에서 Svelte를 온라인으로 시도해보거나, 더 완전한 기능을 갖춘 환경이 필요하다면 [StackBlitz](https://sveltekit.new)를 이용하실 수도 있습니다.
