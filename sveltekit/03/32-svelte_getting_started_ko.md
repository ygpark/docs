# 시작하기

[거의 모든 것을 구축](https://svelte.dev/docs/kit/project-types)할 수 있는 SvelteKit 사용을 권장합니다. SvelteKit은 Svelte 팀의 공식 애플리케이션 프레임워크이며 [Vite](https://vite.dev/)로 구동됩니다. 다음 명령어로 새 프로젝트를 생성하세요:

```sh
npx sv create myapp
cd myapp
npm install
npm run dev
```

아직 Svelte를 모르더라도 걱정하지 마세요! 지금은 SvelteKit이 제공하는 모든 멋진 기능들을 무시하고 나중에 자세히 알아보셔도 됩니다.

## SvelteKit 대안

`npm create vite@latest`를 실행하고 `svelte` 옵션을 선택하여 Vite와 함께 Svelte를 직접 사용할 수도 있습니다. 이 경우 `npm run build`는 [vite-plugin-svelte](https://github.com/sveltejs/vite-plugin-svelte)를 사용하여 `dist` 디렉토리에 HTML, JS, CSS 파일을 생성합니다. 대부분의 경우 라우팅 라이브러리 선택도 필요할 것입니다.

:::note
Vite는 종종 독립 실행 모드로 단일 페이지 앱(SPA)을 구축하는 데 사용되며, SvelteKit으로도 구축할 수 있습니다.
:::

[Rollup](https://github.com/sveltejs/rollup-plugin-svelte), [Webpack](https://github.com/sveltejs/svelte-loader) [및 기타 몇 가지](https://sveltesociety.dev/packages?category=build-plugins) 플러그인도 있지만 Vite를 권장합니다.

## 편집기 도구

Svelte 팀은 [VS Code 확장](https://marketplace.visualstudio.com/items?itemName=svelte.svelte-vscode)을 유지 관리하고 있으며, 다양한 다른 [편집기](https://sveltesociety.dev/resources#editor-support)와 도구와의 통합도 제공됩니다.

[sv check](https://github.com/sveltejs/cli)를 사용하여 명령줄에서 코드를 확인할 수도 있습니다.

## 도움 받기

[Discord 채팅방](https://discord.com/invite/svelte)에서 도움을 요청하는 것을 주저하지 마세요! [Stack Overflow](https://stackoverflow.com/questions/tagged/svelte)에서도 답변을 찾을 수 있습니다.
