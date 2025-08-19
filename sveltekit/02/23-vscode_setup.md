---
sidebar_position: 23
---

# VS Code와 확장 프로그램

스벨트킷 개발에서 적절한 개발 도구 설정은 생산성을 크게 향상시킵니다. 이 섹션에서는 Visual Studio Code를 중심으로 스벨트킷 개발에 최적화된 환경을 구성하는 방법을 알아보겠습니다.

## 1. Visual Studio Code 설치 및 기본 설정

### VS Code 설치

```bash
# Windows (Chocolatey)
choco install vscode

# macOS (Homebrew)
brew install --cask visual-studio-code

# Ubuntu/Debian
wget -qO- https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > packages.microsoft.gpg
sudo install -o root -g root -m 644 packages.microsoft.gpg /etc/apt/trusted.gpg.d/
sudo sh -c 'echo "deb [arch=amd64,arm64,armhf signed-by=/etc/apt/trusted.gpg.d/packages.microsoft.gpg] https://packages.microsoft.com/repos/code stable main" > /etc/apt/sources.list.d/vscode.list'
sudo apt update
sudo apt install code
```

### 기본 설정 최적화

VS Code의 `settings.json` 파일을 다음과 같이 설정합니다:

```json
{
  // 에디터 기본 설정
  "editor.fontSize": 14,
  "editor.fontFamily": "'Fira Code', 'Cascadia Code', Consolas, monospace",
  "editor.fontLigatures": true,
  "editor.tabSize": 2,
  "editor.insertSpaces": true,
  "editor.wordWrap": "on",
  "editor.minimap.enabled": true,
  "editor.rulers": [80, 120],

  // 자동 저장 및 포맷팅
  "editor.formatOnSave": true,
  "editor.formatOnPaste": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true,
    "source.organizeImports": true
  },

  // 파일 탐색기
  "explorer.confirmDelete": false,
  "explorer.confirmDragAndDrop": false,
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,

  // 터미널 설정
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",

  // Svelte 특화 설정
  "svelte.enable-ts-plugin": true,
  "svelte.plugin.typescript.enable": true,
  "svelte.plugin.typescript.diagnostics.enable": true,
  "svelte.plugin.css.diagnostics.enable": true,
  "svelte.plugin.html.documentSymbols.enable": true,

  // 파일 연결
  "files.associations": {
    "*.svelte": "svelte"
  },

  // Emmet 설정
  "emmet.includeLanguages": {
    "svelte": "html"
  },

  // 검색 제외 파일
  "search.exclude": {
    "**/node_modules": true,
    "**/.svelte-kit": true,
    "**/build": true,
    "**/.env": true
  },

  // 파일 감시 제외
  "files.watcherExclude": {
    "**/.svelte-kit/**": true,
    "**/node_modules/**": true
  }
}
```

## 2. 필수 VS Code 확장 프로그램

### 스벨트 공식 확장 프로그램

#### 1. Svelte for VS Code (공식)

- **ID**: `svelte.svelte-vscode`
- **기능**:
  - 문법 하이라이팅
  - 자동완성 (IntelliSense)
  - 오류 검사 및 진단
  - 리팩토링 도구
  - 컴포넌트 미리보기

```json
// .vscode/extensions.json (팀 공유용)
{
  "recommendations": ["svelte.svelte-vscode"]
}
```

#### 설치 및 설정

```bash
# 명령줄에서 설치
code --install-extension svelte.svelte-vscode
```

확장 프로그램별 설정:

```json
{
  // Svelte 확장 프로그램 설정
  "svelte.enable-ts-plugin": true,
  "svelte.plugin.typescript.enable": true,
  "svelte.plugin.typescript.diagnostics.enable": true,
  "svelte.plugin.typescript.hover.enable": true,
  "svelte.plugin.typescript.completions.enable": true,
  "svelte.plugin.css.completions.enable": true,
  "svelte.plugin.css.hover.enable": true,
  "svelte.plugin.css.diagnostics.enable": true,
  "svelte.plugin.html.hover.enable": true,
  "svelte.plugin.html.completions.enable": true,
  "svelte.plugin.svelte.completions.enable": true,
  "svelte.plugin.svelte.hover.enable": true,
  "svelte.plugin.svelte.codeActions.enable": true
}
```

### 개발 생산성 확장 프로그램

#### 2. Auto Rename Tag

- **ID**: `formulahendry.auto-rename-tag`
- **기능**: HTML/SVG 태그 자동 이름 변경

```jsx
<!-- 시작 태그를 변경하면 종료 태그도 자동 변경 -->
<div>     →     <section>
  Content           Content
</div>     →     </section>
```

#### 3. Bracket Pair Colorizer 2

- **ID**: `CoenraadS.bracket-pair-colorizer-2`
- **기능**: 중괄호, 대괄호 색상 구분

#### 4. Indent Rainbow

- **ID**: `oderwat.indent-rainbow`
- **기능**: 들여쓰기 수준별 색상 표시

#### 5. Path Intellisense

- **ID**: `christian-kohler.path-intellisense`
- **기능**: 파일 경로 자동완성

```jsx
<script>
  // 자동완성으로 경로 제안 import Component from '../components/MyComponent.svelte'; import{' '}
  {myStore} from '$lib/stores/myStore.js';
</script>
```

### 코드 품질 관리

#### 6. ESLint

- **ID**: `dbaeumer.vscode-eslint`
- **기능**: JavaScript/TypeScript 코드 린팅

```json
// .eslintrc.js
module.exports = {
  root: true,
  extends: [
    'eslint:recommended',
    '@typescript-eslint/recommended',
    'plugin:svelte/recommended',
    'prettier'
  ],
  parser: '@typescript-eslint/parser',
  plugins: ['@typescript-eslint'],
  parserOptions: {
    sourceType: 'module',
    ecmaVersion: 2020,
    extraFileExtensions: ['.svelte']
  },
  env: {
    browser: true,
    es2017: true,
    node: true
  },
  overrides: [
    {
      files: ['*.svelte'],
      parser: 'svelte-eslint-parser',
      parserOptions: {
        parser: '@typescript-eslint/parser'
      }
    }
  ]
};
```

#### 7. Prettier - Code formatter

- **ID**: `esbenp.prettier-vscode`
- **기능**: 코드 자동 포맷팅

```json
// .prettierrc
{
  "useTabs": false,
  "singleQuote": true,
  "trailingComma": "none",
  "printWidth": 100,
  "plugins": ["prettier-plugin-svelte"],
  "pluginSearchDirs": ["."],
  "overrides": [
    {
      "files": "*.svelte",
      "options": {
        "parser": "svelte"
      }
    }
  ]
}
```

#### 8. Code Spell Checker

- **ID**: `streetsidesoftware.code-spell-checker`
- **기능**: 영어 철자 검사

```json
// cSpell 설정
{
  "cSpell.words": ["sveltekit", "svelte", "vite", "typescript", "eslint", "prettier"]
}
```

### Git 관리

#### 9. GitLens

- **ID**: `eamodio.gitlens`
- **기능**: Git 히스토리, blame 정보 표시

#### 10. Git Graph

- **ID**: `mhutchie.git-graph`
- **기능**: Git 브랜치 시각화

### 테스트 및 디버깅

#### 11. Vitest

- **ID**: `ZixuanChen.vitest-explorer`
- **기능**: Vitest 테스트 실행 및 디버깅

#### 12. Playwright Test for VSCode

- **ID**: `ms-playwright.playwright`
- **기능**: Playwright E2E 테스트 실행

```json
// .vscode/launch.json (디버깅 설정)
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch SvelteKit",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/@sveltejs/kit/src/runtime/client/start.js",
      "args": ["dev"],
      "env": {
        "NODE_ENV": "development"
      },
      "console": "integratedTerminal",
      "internalConsoleOptions": "neverOpen"
    }
  ]
}
```

### 유틸리티 확장 프로그램

#### 13. Thunder Client

- **ID**: `rangav.vscode-thunder-client`
- **기능**: REST API 테스트 (Postman 대안)

#### 14. Live Server

- **ID**: `ritwickdey.liveserver`
- **기능**: 정적 파일 라이브 서버 (빌드된 파일 미리보기용)

#### 15. Color Highlight

- **ID**: `naumovs.color-highlight`
- **기능**: CSS 색상 값 시각화

## 3. 스벨트킷 특화 설정

### 워크스페이스 설정

프로젝트별 설정을 위한 `.vscode/settings.json`:

```json
{
  // 스벨트킷 프로젝트 특화 설정
  "typescript.preferences.importModuleSpecifier": "relative",
  "typescript.suggest.includeCompletionsForImportStatements": true,
  "typescript.suggest.autoImports": true,

  // 파일 탐색기에서 숨길 파일들
  "files.exclude": {
    "**/.git": true,
    "**/.svelte-kit": true,
    "**/node_modules": true,
    "**/.env": true,
    "**/package-lock.json": true
  },

  // 검색에서 제외할 폴더
  "search.exclude": {
    "**/node_modules": true,
    "**/.svelte-kit": true,
    "**/build": true,
    "**/.vercel": true,
    "**/.netlify": true
  },

  // Emmet 설정
  "emmet.includeLanguages": {
    "svelte": "html",
    "postcss": "css"
  },

  // 자동 import 설정
  "typescript.preferences.includePackageJsonAutoImports": "auto",
  "javascript.preferences.includePackageJsonAutoImports": "auto"
}
```

### 코드 스니펫

`.vscode/svelte.code-snippets`:

```json
{
  "Svelte Component": {
    "prefix": "sveltecomp",
    "body": [
      "<script${1: lang=\"ts\"}>",
      "\t${2:// Component logic}",
      "</script>",
      "",
      "<${3:div}>",
      "\t${4:<!-- Component content -->}",
      "</${3:div}>",
      "",
      "<style>",
      "\t${5:/* Component styles */}",
      "</style>"
    ],
    "description": "Create a basic Svelte component"
  },

  "SvelteKit Page": {
    "prefix": "skpage",
    "body": [
      "<script${1: lang=\"ts\"}>",
      "\texport let data;",
      "\t${2:// Page logic}",
      "</script>",
      "",
      "<svelte:head>",
      "\t<title>${3:Page Title}</title>",
      "</svelte:head>",
      "",
      "<main>",
      "\t<h1>${4:Welcome}</h1>",
      "\t${5:<!-- Page content -->}",
      "</main>",
      "",
      "<style>",
      "\tmain {",
      "\t\tmax-width: 800px;",
      "\t\tmargin: 0 auto;",
      "\t\tpadding: 2rem;",
      "\t}",
      "</style>"
    ],
    "description": "Create a SvelteKit page component"
  },

  "Load Function": {
    "prefix": "skload",
    "body": [
      "import type { PageLoad } from './$types';",
      "",
      "export const load: PageLoad = async ({ fetch, params }) => {",
      "\t${1:// Load data}",
      "\treturn {",
      "\t\t${2:data: {}}",
      "\t};",
      "};"
    ],
    "description": "Create a SvelteKit load function"
  },

  "Server Load Function": {
    "prefix": "skserverload",
    "body": [
      "import type { PageServerLoad } from './$types';",
      "",
      "export const load: PageServerLoad = async ({ fetch, params }) => {",
      "\t${1:// Server-side load data}",
      "\treturn {",
      "\t\t${2:data: {}}",
      "\t};",
      "};"
    ],
    "description": "Create a SvelteKit server load function"
  },

  "Form Action": {
    "prefix": "skaction",
    "body": [
      "import type { Actions } from './$types';",
      "",
      "export const actions: Actions = {",
      "\t${1:default}: async ({ request }) => {",
      "\t\tconst data = await request.formData();",
      "\t\t${2:// Handle form submission}",
      "\t\treturn {",
      "\t\t\tsuccess: true",
      "\t\t};",
      "\t}",
      "};"
    ],
    "description": "Create a SvelteKit form action"
  }
}
```

### 태스크 설정

`.vscode/tasks.json`:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "dev",
      "type": "npm",
      "script": "dev",
      "group": {
        "kind": "build",
        "isDefault": true
      },
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      },
      "problemMatcher": []
    },
    {
      "label": "build",
      "type": "npm",
      "script": "build",
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      }
    },
    {
      "label": "check",
      "type": "npm",
      "script": "check",
      "group": "test",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      }
    },
    {
      "label": "lint",
      "type": "npm",
      "script": "lint",
      "group": "test",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "shared"
      }
    }
  ]
}
```

## 4. 키보드 단축키 설정

`.vscode/keybindings.json`:

```json
[
  {
    "key": "ctrl+shift+s",
    "command": "workbench.action.files.saveAll"
  },
  {
    "key": "ctrl+shift+`",
    "command": "workbench.action.terminal.new"
  },
  {
    "key": "alt+shift+f",
    "command": "editor.action.formatDocument"
  },
  {
    "key": "ctrl+shift+p",
    "command": "workbench.action.showCommands"
  },
  {
    "key": "ctrl+shift+e",
    "command": "workbench.view.explorer"
  },
  {
    "key": "ctrl+shift+g",
    "command": "workbench.view.scm"
  },
  {
    "key": "ctrl+shift+x",
    "command": "workbench.view.extensions"
  },
  {
    "key": "f5",
    "command": "workbench.action.debug.start"
  }
]
```

## 5. 다른 에디터 설정

### WebStorm/IntelliJ IDEA

JetBrains IDE 사용자를 위한 설정:

```javascript
// .idea/modules.xml (자동 생성됨)
// Svelte 플러그인 설치 필요
```

Svelte 플러그인 설치:

- File → Settings → Plugins → "Svelte" 검색 후 설치

### Vim/Neovim

Vim 사용자를 위한 플러그인:

```vim
" .vimrc 또는 init.vim
Plug 'evanleck/vim-svelte'
Plug 'pangloss/vim-javascript'
Plug 'HerringtonDarkholme/yats.vim'

" 설정
autocmd BufNewFile,BufRead *.svelte set filetype=svelte
```

### Sublime Text

Package Control을 통한 패키지 설치:

- Svelte (공식 패키지)
- LSP (Language Server Protocol 지원)

## 6. 통합 개발 환경 최적화

### 터미널 통합

VS Code 내장 터미널 설정:

```json
{
  "terminal.integrated.defaultProfile.windows": "Git Bash",
  "terminal.integrated.defaultProfile.osx": "zsh",
  "terminal.integrated.defaultProfile.linux": "bash",
  "terminal.integrated.fontSize": 13,
  "terminal.integrated.fontFamily": "FiraCode Nerd Font",
  "terminal.integrated.cursorBlinking": true,
  "terminal.integrated.cursorStyle": "line"
}
```

자주 사용하는 터미널 명령어를 위한 태스크:

```json
{
  "version": "2.0.0",
  "tasks": [
    {
      "label": "SvelteKit Dev Server",
      "type": "shell",
      "command": "npm run dev",
      "group": "build",
      "presentation": {
        "echo": true,
        "reveal": "always",
        "focus": false,
        "panel": "new"
      },
      "runOptions": {
        "runOn": "folderOpen"
      }
    }
  ]
}
```

### 디버깅 설정

고급 디버깅을 위한 설정:

```json
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Launch Chrome against localhost",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:5173",
      "webRoot": "${workspaceFolder}/src",
      "sourceMaps": true
    },
    {
      "name": "Launch SvelteKit (SSR)",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/build/index.js",
      "args": [],
      "env": {
        "NODE_ENV": "development"
      },
      "sourceMaps": true,
      "outFiles": ["${workspaceFolder}/build/**/*.js"]
    }
  ]
}
```

## 7. 팀 개발을 위한 설정 공유

### EditorConfig

`.editorconfig`:

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
indent_style = space
indent_size = 2

[*.{js,ts,svelte}]
indent_size = 2

[*.md]
trim_trailing_whitespace = false
```

### 프로젝트 설정 템플릿

새 프로젝트용 설정 템플릿:

```bash
# 설정 파일들을 한 번에 생성하는 스크립트
mkdir -p .vscode
cat > .vscode/settings.json << 'EOF'
{
  "svelte.enable-ts-plugin": true,
  "editor.formatOnSave": true,
  "editor.codeActionsOnSave": {
    "source.fixAll.eslint": true
  }
}
EOF

cat > .vscode/extensions.json << 'EOF'
{
  "recommendations": [
    "svelte.svelte-vscode",
    "esbenp.prettier-vscode",
    "dbaeumer.vscode-eslint"
  ]
}
EOF
```

## 8. 성능 최적화

VS Code 성능 향상을 위한 설정:

```json
{
  // 대용량 파일 처리
  "files.maxMemoryForLargeFilesMB": 4096,

  // 파일 감시 최적화
  "files.watcherExclude": {
    "**/.git/objects/**": true,
    "**/.git/subtree-cache/**": true,
    "**/node_modules/*/**": true,
    "**/.hg/store/**": true,
    "**/.svelte-kit/**": true
  },

  // 검색 성능 향상
  "search.useRipgrep": true,
  "search.maintainFileSearchCache": true,

  // TypeScript 성능 최적화
  "typescript.disableAutomaticTypeAcquisition": true,
  "typescript.updateImportsOnFileMove.enabled": "always"
}
```

## 마무리

적절한 개발 도구 설정은 스벨트킷 개발 경험을 크게 향상시킵니다.

**핵심 포인트:**

- Svelte 공식 확장 프로그램 필수 설치
- ESLint, Prettier로 코드 품질 관리
- 프로젝트별 워크스페이스 설정
- 팀 개발을 위한 설정 공유
- 성능 최적화 설정

**권장 확장 프로그램 목록:**

1. Svelte for VS Code (필수)
2. ESLint
3. Prettier
4. Auto Rename Tag
5. GitLens
6. Thunder Client
7. Path Intellisense

다음 섹션에서는 스벨트킷 프로젝트의 구조를 자세히 분석해보겠습니다.
