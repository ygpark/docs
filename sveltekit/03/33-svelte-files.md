# .svelte 파일

컴포넌트는 Svelte 애플리케이션의 구성 요소입니다. HTML의 상위 집합을 사용하여 `.svelte` 파일로 작성됩니다.

script, styles, markup 세 섹션 모두 선택 사항입니다.

<!-- prettier-ignore -->
```html
/// file: MyComponent.svelte
<script module>
	// 모듈 레벨 로직이 여기에 위치합니다
	// (거의 사용하지 않을 것입니다)
</script>

<script>
	// 인스턴스 레벨 로직이 여기에 위치합니다
</script>

<!-- 마크업 (0개 이상의 항목)이 여기에 위치합니다 -->

<style>
	/* 스타일이 여기에 위치합니다 */
</style>
```

## `<script>`

`<script>` 블록에는 컴포넌트 인스턴스가 생성될 때 실행되는 JavaScript(또는 `lang="ts"` 속성을 추가할 때 TypeScript)가 포함됩니다. 최상위 레벨에서 선언(또는 가져온) 변수는 컴포넌트의 마크업에서 참조할 수 있습니다.

일반적인 JavaScript 외에도 *룬(runes)*을 사용하여 컴포넌트 props를 선언하고 컴포넌트에 반응성을 추가할 수 있습니다. 룬은 다음 섹션에서 다룹니다.

<!-- TODO describe behaviour of `export` -->

## `<script module>`

`module` 속성이 있는 `<script>` 태그는 각 컴포넌트 인스턴스마다가 아니라 모듈이 처음 평가될 때 한 번 실행됩니다. 이 블록에서 선언된 변수는 컴포넌트의 다른 곳에서 참조할 수 있지만 그 반대는 불가능합니다.

```html
<script module>
  let total = 0;
</script>

<script>
  total += 1;
  console.log(`instantiated ${total} times`);
</script>
```

이 블록에서 바인딩을 `export`할 수 있으며, 컴파일된 모듈의 내보내기가 됩니다. 기본 내보내기는 컴포넌트 자체이므로 `export default`는 사용할 수 없습니다.

:::note
TypeScript를 사용하고 있고 `module` 블록에서 이러한 내보내기를 `.ts` 파일로 가져오는 경우, TypeScript가 이를 인식할 수 있도록 편집기 설정이 되어 있는지 확인하세요. 이는 VS Code 확장과 IntelliJ 플러그인에는 적용되지만, 다른 경우에는 [TypeScript 편집기 플러그인](https://www.npmjs.com/package/typescript-svelte-plugin) 설정이 필요할 수 있습니다.
:::

:::info[레거시]
Svelte 4에서는 이 스크립트 태그가 `<script context="module">`을 사용하여 생성되었습니다.
:::

## `<style>`

`<style>` 블록 내부의 CSS는 해당 컴포넌트로 범위가 지정됩니다.

```html
<style>
  p {
    /* 이는 이 컴포넌트의 <p> 요소에만 영향을 줍니다 */
    color: burlywood;
  }
</style>
```

자세한 정보는 스타일링 섹션을 참조하세요.
