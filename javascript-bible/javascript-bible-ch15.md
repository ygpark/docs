---
title: '15장: 현대적 웹 개발 도구'
slug: javascript-bible-chapter-15-modern-web-tools
description: '모던 자바스크립트 개발에 필수적인 개발자 도구, 번들러, 린터, Git 등의 활용법을 배우고 전문적인 개발 환경을 구축해보세요.'
keywords:
  [
    '자바스크립트 개발 도구',
    '브라우저 개발자 도구',
    'Vite',
    'Webpack',
    'ESLint',
    'Prettier',
    'Git',
    '번들러',
    '린터',
    '코드 포매터',
    '모던 자바스크립트',
    '개발 환경',
  ]
sidebar_position: 15
---

# 15장: 현대적 웹 개발 도구

## 학습 목표

- 브라우저 개발자 도구를 활용한 디버깅과 성능 분석
- 번들러(Vite, Webpack)를 이용한 프로젝트 빌드 시스템 구축
- ESLint와 Prettier를 활용한 코드 품질 관리
- Git을 이용한 효율적인 버전 관리 방법

---

## 브라우저 개발자 도구 심화 활용

모던 웹 개발에서 브라우저 개발자 도구는 필수적입니다. 단순히 콘솔에 로그를 출력하는 것을 넘어서, 성능 분석, 네트워크 모니터링, 메모리 사용량 추적 등 다양한 기능을 활용할 수 있습니다.

### 콘솔 고급 기능

개발자 도구의 콘솔은 단순한 로깅 도구가 아닙니다. 다양한 메서드를 활용하여 더 효율적으로 디버깅할 수 있습니다.

```javascript title="console-advanced.js"
// 기본 로깅
console.log('일반 메시지');
console.warn('경고 메시지');
console.error('에러 메시지');

// 객체를 테이블 형태로 출력
const users = [
  { name: '김철수', age: 25, city: '서울' },
  { name: '이영희', age: 30, city: '부산' },
  { name: '박민수', age: 28, city: '대구' },
];
console.table(users);

// 그룹화하여 출력
console.group('사용자 정보');
console.log('이름:', users[0].name);
console.log('나이:', users[0].age);
console.groupEnd();

// 조건부 로깅
const score = 85;
console.assert(score >= 90, '점수가 90점 미만입니다:', score);

// 실행 시간 측정
console.time('배열 처리');
const numbers = Array.from({ length: 1000000 }, (_, i) => i);
const doubled = numbers.map(n => n * 2);
console.timeEnd('배열 처리');

// 스타일링된 로그
console.log('%c성공!', 'color: green; font-size: 20px; font-weight: bold;');
```

### 네트워크 탭 활용

웹 애플리케이션의 성능을 분석하고 API 호출을 모니터링하는 데 네트워크 탭을 활용할 수 있습니다.

```javascript title="network-monitoring.js"
// API 호출 성능 모니터링
async function fetchUserData(userId) {
  console.time(`사용자 ${userId} 데이터 로드`);

  try {
    const response = await fetch(`/api/users/${userId}`);

    // 응답 시간과 상태 확인
    console.log('응답 상태:', response.status);
    console.log('응답 시간:', response.headers.get('x-response-time'));

    const userData = await response.json();
    console.timeEnd(`사용자 ${userId} 데이터 로드`);

    return userData;
  } catch (error) {
    console.error('API 호출 실패:', error);
    throw error;
  }
}

// 여러 API 호출 병렬 처리 성능 측정
async function loadDashboardData() {
  console.time('대시보드 로드');

  const promises = [
    fetch('/api/stats'),
    fetch('/api/notifications'),
    fetch('/api/recent-activity'),
  ];

  try {
    const responses = await Promise.all(promises);
    console.log('모든 API 호출 완료');
    console.timeEnd('대시보드 로드');

    return responses;
  } catch (error) {
    console.error('대시보드 로드 실패:', error);
  }
}
```

### 성능 분석 도구

Performance 탭을 활용하여 애플리케이션의 성능 병목 지점을 찾아보겠습니다.

```javascript title="performance-monitoring.js"
// 성능 마크 설정
function performanceTracker() {
  // 시작점 마킹
  performance.mark('data-processing-start');

  // 무거운 작업 시뮬레이션
  const largeArray = Array.from({ length: 100000 }, (_, i) => ({
    id: i,
    value: Math.random() * 1000,
    category: i % 10,
  }));

  // 중간 지점 마킹
  performance.mark('array-created');

  // 데이터 처리
  const processed = largeArray
    .filter(item => item.value > 500)
    .map(item => ({
      ...item,
      processed: true,
      timestamp: Date.now(),
    }))
    .sort((a, b) => b.value - a.value);

  // 완료 지점 마킹
  performance.mark('data-processing-end');

  // 성능 측정
  performance.measure('전체 처리 시간', 'data-processing-start', 'data-processing-end');

  performance.measure('배열 생성 시간', 'data-processing-start', 'array-created');

  // 결과 출력
  const measures = performance.getEntriesByType('measure');
  measures.forEach(measure => {
    console.log(`${measure.name}: ${measure.duration.toFixed(2)}ms`);
  });
}
```

---

## 번들러와 빌드 도구

현대적인 웹 개발에서는 여러 파일을 하나로 묶고, 코드를 최적화하며, 개발 서버를 제공하는 번들러가 필수적입니다. Vite와 Webpack의 기본 사용법을 알아보겠습니다.

### Vite 프로젝트 설정

Vite는 빠른 개발 서버와 효율적인 빌드를 제공하는 차세대 번들러입니다.

```javascript title="vite.config.js"
import { defineConfig } from 'vite';

export default defineConfig({
  // 개발 서버 설정
  server: {
    port: 3000,
    open: true,
    host: true,
  },

  // 빌드 설정
  build: {
    outDir: 'dist',
    sourcemap: true,
    minify: 'terser',
    target: 'es2020',
  },

  // CSS 처리 설정
  css: {
    modules: {
      localsConvention: 'camelCase',
    },
    preprocessorOptions: {
      scss: {
        additionalData: '@import "./src/styles/variables.scss";',
      },
    },
  },

  // 별칭 설정
  resolve: {
    alias: {
      '@': '/src',
      '@components': '/src/components',
      '@utils': '/src/utils',
    },
  },
});
```

### 모듈 시스템 활용

모던 번들러를 활용하여 효율적인 모듈 구조를 만들어보겠습니다.

```javascript title="src/utils/api.js"
// API 유틸리티 모듈
const API_BASE_URL = import.meta.env.VITE_API_URL || 'http://localhost:3000';

class ApiClient {
  constructor(baseURL = API_BASE_URL) {
    this.baseURL = baseURL;
    this.defaultHeaders = {
      'Content-Type': 'application/json',
    };
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseURL}${endpoint}`;
    const config = {
      headers: this.defaultHeaders,
      ...options,
    };

    try {
      const response = await fetch(url, config);

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      return await response.json();
    } catch (error) {
      console.error('API 요청 실패:', error);
      throw error;
    }
  }

  get(endpoint) {
    return this.request(endpoint, { method: 'GET' });
  }

  post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: JSON.stringify(data),
    });
  }
}

export default new ApiClient();
```

```javascript title="src/components/UserList.js"
// 사용자 목록 컴포넌트
import apiClient from '@utils/api.js';

export class UserList {
  constructor(container) {
    this.container = container;
    this.users = [];
    this.init();
  }

  async init() {
    await this.loadUsers();
    this.render();
    this.attachEvents();
  }

  async loadUsers() {
    try {
      this.users = await apiClient.get('/users');
    } catch (error) {
      console.error('사용자 로드 실패:', error);
      this.users = [];
    }
  }

  render() {
    const userListHTML = this.users
      .map(
        user => `
      <div class="user-card" data-user-id="${user.id}">
        <h3 class="user-name">${user.name}</h3>
        <p class="user-email">${user.email}</p>
        <button class="delete-btn" data-user-id="${user.id}">삭제</button>
      </div>
    `
      )
      .join('');

    this.container.innerHTML = `
      <div class="user-list">
        <h2>사용자 목록</h2>
        <div class="users-grid">
          ${userListHTML}
        </div>
      </div>
    `;
  }

  attachEvents() {
    this.container.addEventListener('click', e => {
      if (e.target.classList.contains('delete-btn')) {
        const userId = e.target.dataset.userId;
        this.deleteUser(userId);
      }
    });
  }
}
```

### 환경별 설정 관리

개발, 테스트, 프로덕션 환경에 따른 설정을 관리하는 방법을 알아보겠습니다.

```javascript title="src/config/environment.js"
// 환경별 설정 관리
const environments = {
  development: {
    API_URL: 'http://localhost:3000',
    DEBUG: true,
    LOG_LEVEL: 'debug',
  },

  production: {
    API_URL: 'https://api.myapp.com',
    DEBUG: false,
    LOG_LEVEL: 'error',
  },

  test: {
    API_URL: 'http://localhost:3001',
    DEBUG: true,
    LOG_LEVEL: 'warn',
  },
};

const currentEnv = import.meta.env.MODE || 'development';
const config = environments[currentEnv];

// 로거 설정
export const logger = {
  debug: (...args) => config.DEBUG && console.log('[DEBUG]', ...args),
  info: (...args) => console.info('[INFO]', ...args),
  warn: (...args) => console.warn('[WARN]', ...args),
  error: (...args) => console.error('[ERROR]', ...args),
};

export default config;
```

---

## 코드 포매터와 린터

코드 품질을 일관되게 유지하기 위해 Prettier와 ESLint를 활용하는 방법을 알아보겠습니다.

### ESLint 설정

ESLint는 자바스크립트 코드의 문제점을 찾아주고 일관된 코딩 스타일을 유지하도록 도와줍니다.

```javascript title=".eslintrc.js"
module.exports = {
  env: {
    browser: true,
    es2021: true,
    node: true,
  },

  extends: ['eslint:recommended', '@typescript-eslint/recommended'],

  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
  },

  rules: {
    // 코드 품질 관련
    'no-unused-vars': 'error',
    'no-console': 'warn',
    'prefer-const': 'error',
    'no-var': 'error',

    // 스타일 관련
    indent: ['error', 2],
    quotes: ['error', 'single'],
    semi: ['error', 'always'],
    'comma-dangle': ['error', 'never'],

    // 함수 관련
    'arrow-spacing': 'error',
    'prefer-arrow-callback': 'error',
    'func-style': ['error', 'expression'],
  },

  // 특정 파일에 대한 예외 설정
  overrides: [
    {
      files: ['*.test.js', '*.spec.js'],
      rules: {
        'no-console': 'off',
      },
    },
  ],
};
```

### Prettier 설정

Prettier는 코드 포매팅을 자동화하여 일관된 스타일을 유지하도록 도와줍니다.

```json title=".prettierrc"
{
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "avoid",
  "endOfLine": "lf"
}
```

### 코드 품질 자동화

개발 워크플로우에 코드 품질 검사를 자동화하는 방법을 알아보겠습니다.

```javascript title="scripts/code-quality.js"
// 코드 품질 검사 스크립트
import { execSync } from 'child_process';
import chalk from 'chalk';

class CodeQualityChecker {
  constructor() {
    this.errors = [];
    this.warnings = [];
  }

  async runLint() {
    console.log(chalk.blue('🔍 ESLint 검사 중...'));

    try {
      execSync('eslint src --ext .js,.ts --format=json > lint-results.json', {
        stdio: 'inherit',
      });
      console.log(chalk.green('✅ ESLint 검사 완료'));
    } catch (error) {
      this.errors.push('ESLint 오류 발견');
      console.log(chalk.red('❌ ESLint 오류 발견'));
    }
  }

  async runPrettier() {
    console.log(chalk.blue('💅 Prettier 포매팅 검사 중...'));

    try {
      execSync('prettier --check src/**/*.{js,ts,css,html}', {
        stdio: 'inherit',
      });
      console.log(chalk.green('✅ 코드 포매팅 검사 완료'));
    } catch (error) {
      this.warnings.push('포매팅 불일치 발견');
      console.log(chalk.yellow('⚠️ 포매팅 불일치 발견'));
    }
  }

  async runTests() {
    console.log(chalk.blue('🧪 테스트 실행 중...'));

    try {
      execSync('npm test', { stdio: 'inherit' });
      console.log(chalk.green('✅ 모든 테스트 통과'));
    } catch (error) {
      this.errors.push('테스트 실패');
      console.log(chalk.red('❌ 테스트 실패'));
    }
  }
}
```

---

## Git과 버전 관리

효율적인 Git 사용법과 협업을 위한 브랜치 전략을 알아보겠습니다.

### Git 기본 워크플로우

프로젝트의 버전 관리를 위한 기본적인 Git 명령어들을 실습해보겠습니다.

```bash title="git-workflow.sh"
# 새 프로젝트 시작
git init
git add .
git commit -m "Initial commit"

# 원격 저장소 연결
git remote add origin https://github.com/username/project.git
git push -u origin main

# 새 기능 개발 브랜치 생성
git checkout -b feature/user-authentication
git add .
git commit -m "Add user authentication feature"

# 메인 브랜치에 병합
git checkout main
git merge feature/user-authentication

# 브랜치 정리
git branch -d feature/user-authentication
```

### 커밋 메시지 컨벤션

일관된 커밋 메시지를 작성하기 위한 스크립트를 만들어보겠습니다.

```javascript title="scripts/commit-helper.js"
// 커밋 메시지 도우미
import readline from 'readline';
import { execSync } from 'child_process';

const COMMIT_TYPES = {
  feat: '새로운 기능',
  fix: '버그 수정',
  docs: '문서 변경',
  style: '코드 포매팅',
  refactor: '코드 리팩토링',
  test: '테스트 추가/수정',
  chore: '빌드 도구, 라이브러리 설정',
};

class CommitHelper {
  constructor() {
    this.rl = readline.createInterface({
      input: process.stdin,
      output: process.stdout,
    });
  }

  async getCommitType() {
    console.log('\n커밋 타입을 선택하세요:');
    Object.entries(COMMIT_TYPES).forEach(([key, desc], index) => {
      console.log(`${index + 1}. ${key}: ${desc}`);
    });

    return new Promise(resolve => {
      this.rl.question('\n번호를 선택하세요: ', answer => {
        const types = Object.keys(COMMIT_TYPES);
        const selectedType = types[parseInt(answer) - 1];
        resolve(selectedType || 'feat');
      });
    });
  }

  async getCommitScope() {
    return new Promise(resolve => {
      this.rl.question('스코프 (선택사항, 예: auth, ui): ', resolve);
    });
  }

  async getCommitMessage() {
    return new Promise(resolve => {
      this.rl.question('커밋 메시지: ', resolve);
    });
  }

  async generateCommit() {
    const type = await this.getCommitType();
    const scope = await this.getCommitScope();
    const message = await this.getCommitMessage();

    const scopeText = scope ? `(${scope})` : '';
    const commitMessage = `${type}${scopeText}: ${message}`;

    console.log(`\n생성된 커밋 메시지: ${commitMessage}`);

    this.rl.question('\n커밋하시겠습니까? (y/n): ', answer => {
      if (answer.toLowerCase() === 'y') {
        try {
          execSync(`git commit -m "${commitMessage}"`, { stdio: 'inherit' });
          console.log('커밋이 완료되었습니다!');
        } catch (error) {
          console.error('커밋 실패:', error.message);
        }
      }
      this.rl.close();
    });
  }
}
```

### 브랜치 전략

효율적인 협업을 위한 Git Flow 전략을 구현해보겠습니다.

```javascript title="scripts/branch-manager.js"
// 브랜치 관리 도우미
import { execSync } from 'child_process';
import chalk from 'chalk';

class BranchManager {
  getCurrentBranch() {
    try {
      const branch = execSync('git rev-parse --abbrev-ref HEAD', {
        encoding: 'utf8',
      }).trim();
      return branch;
    } catch (error) {
      console.error('현재 브랜치 확인 실패:', error.message);
      return null;
    }
  }

  createFeatureBranch(featureName) {
    const branchName = `feature/${featureName}`;

    try {
      // 메인 브랜치로 이동
      execSync('git checkout main', { stdio: 'inherit' });

      // 최신 변경사항 가져오기
      execSync('git pull origin main', { stdio: 'inherit' });

      // 새 브랜치 생성 및 이동
      execSync(`git checkout -b ${branchName}`, { stdio: 'inherit' });

      console.log(chalk.green(`✅ ${branchName} 브랜치가 생성되었습니다.`));
    } catch (error) {
      console.error(chalk.red('브랜치 생성 실패:'), error.message);
    }
  }

  createReleaseBranch(version) {
    const branchName = `release/${version}`;

    try {
      execSync('git checkout develop', { stdio: 'inherit' });
      execSync('git pull origin develop', { stdio: 'inherit' });
      execSync(`git checkout -b ${branchName}`, { stdio: 'inherit' });

      console.log(chalk.green(`✅ ${branchName} 릴리즈 브랜치가 생성되었습니다.`));
    } catch (error) {
      console.error(chalk.red('릴리즈 브랜치 생성 실패:'), error.message);
    }
  }

  mergeToDevelop() {
    const currentBranch = this.getCurrentBranch();

    if (!currentBranch.startsWith('feature/')) {
      console.log(chalk.yellow('⚠️ feature 브랜치에서만 실행 가능합니다.'));
      return;
    }

    try {
      execSync('git checkout develop', { stdio: 'inherit' });
      execSync('git pull origin develop', { stdio: 'inherit' });
      execSync(`git merge ${currentBranch}`, { stdio: 'inherit' });
      execSync('git push origin develop', { stdio: 'inherit' });

      console.log(chalk.green(`✅ ${currentBranch}가 develop에 병합되었습니다.`));

      // 브랜치 삭제 여부 확인
      console.log('브랜치를 삭제하시겠습니까?');
    } catch (error) {
      console.error(chalk.red('병합 실패:'), error.message);
    }
  }
}
```

---

## 실습: 개발 환경 구축 실전

실제 프로젝트에서 사용할 수 있는 완전한 개발 환경을 구축해보겠습니다.

### 프로젝트 초기 설정

새로운 프로젝트를 위한 완전한 설정을 자동화하는 스크립트를 만들어보겠습니다.

```javascript title="scripts/setup-project.js"
// 프로젝트 초기 설정 스크립트
import fs from 'fs/promises';
import { execSync } from 'child_process';
import chalk from 'chalk';

class ProjectSetup {
  constructor(projectName) {
    this.projectName = projectName;
    this.projectPath = `./${projectName}`;
  }

  async createProjectStructure() {
    console.log(chalk.blue('📁 프로젝트 구조 생성 중...'));

    const directories = [
      'src',
      'src/components',
      'src/utils',
      'src/styles',
      'src/assets',
      'tests',
      'docs',
      'scripts',
    ];

    try {
      await fs.mkdir(this.projectPath);

      for (const dir of directories) {
        await fs.mkdir(`${this.projectPath}/${dir}`, { recursive: true });
      }

      console.log(chalk.green('✅ 프로젝트 구조 생성 완료'));
    } catch (error) {
      console.error(chalk.red('디렉토리 생성 실패:'), error.message);
    }
  }

  async createConfigFiles() {
    console.log(chalk.blue('⚙️ 설정 파일 생성 중...'));

    const packageJson = {
      name: this.projectName,
      version: '1.0.0',
      type: 'module',
      scripts: {
        dev: 'vite',
        build: 'vite build',
        preview: 'vite preview',
        lint: 'eslint src --ext .js,.ts',
        format: 'prettier --write src/**/*.{js,ts,css,html}',
        test: 'vitest',
      },
      devDependencies: {
        vite: '^4.0.0',
        eslint: '^8.0.0',
        prettier: '^2.8.0',
        vitest: '^0.28.0',
      },
    };

    try {
      await fs.writeFile(`${this.projectPath}/package.json`, JSON.stringify(packageJson, null, 2));

      console.log(chalk.green('✅ package.json 생성 완료'));
    } catch (error) {
      console.error(chalk.red('설정 파일 생성 실패:'), error.message);
    }
  }

  async initializeGit() {
    console.log(chalk.blue('🔧 Git 저장소 초기화 중...'));

    try {
      process.chdir(this.projectPath);
      execSync('git init', { stdio: 'inherit' });

      const gitignore = `
node_modules/
dist/
.env
.env.local
*.log
.DS_Store
coverage/
      `.trim();

      await fs.writeFile('.gitignore', gitignore);

      execSync('git add .', { stdio: 'inherit' });
      execSync('git commit -m "Initial commit"', { stdio: 'inherit' });

      console.log(chalk.green('✅ Git 저장소 초기화 완료'));
    } catch (error) {
      console.error(chalk.red('Git 초기화 실패:'), error.message);
    }
  }
}
```

### 개발 서버 설정

개발 중 실시간으로 코드 변경사항을 확인할 수 있는 개발 서버를 설정해보겠습니다.

```javascript title="scripts/dev-server.js"
// 개발 서버 관리
import { createServer } from 'vite';
import chalk from 'chalk';

class DevServer {
  constructor() {
    this.server = null;
  }

  async start() {
    try {
      this.server = await createServer({
        server: {
          port: 3000,
          open: true,
          cors: true,
        },

        // 핫 리로드 설정
        plugins: [
          {
            name: 'reload-on-change',
            handleHotUpdate(ctx) {
              console.log(chalk.blue(`🔄 ${ctx.file} 파일이 변경되었습니다.`));
              ctx.server.ws.send({
                type: 'full-reload',
              });
            },
          },
        ],
      });

      await this.server.listen();
      console.log(chalk.green('🚀 개발 서버가 시작되었습니다!'));
      console.log(chalk.blue('📝 http://localhost:3000'));
    } catch (error) {
      console.error(chalk.red('개발 서버 시작 실패:'), error.message);
    }
  }

  async stop() {
    if (this.server) {
      await this.server.close();
      console.log(chalk.yellow('⏹️ 개발 서버가 중지되었습니다.'));
    }
  }

  async restart() {
    console.log(chalk.blue('🔄 개발 서버 재시작 중...'));
    await this.stop();
    await this.start();
  }
}

// 프로세스 종료 시 서버 정리
process.on('SIGINT', async () => {
  console.log(chalk.yellow('\n🛑 서버를 종료합니다...'));
  process.exit(0);
});
```

---

## 실습: 코드 품질 자동화 설정

지속적인 코드 품질 관리를 위한 자동화 시스템을 구축해보겠습니다.

### 사전 커밋 훅 설정

커밋 전에 자동으로 코드 품질을 검사하는 시스템을 만들어보겠습니다.

```javascript title="scripts/pre-commit-hook.js"
// 사전 커밋 훅
import { execSync } from 'child_process';
import chalk from 'chalk';

class PreCommitHook {
  constructor() {
    this.hasErrors = false;
    this.stagedFiles = this.getStagedFiles();
  }

  getStagedFiles() {
    try {
      const files = execSync('git diff --cached --name-only --diff-filter=ACM', {
        encoding: 'utf8',
      });
      return files.split('\n').filter(file => file.endsWith('.js') || file.endsWith('.ts'));
    } catch (error) {
      return [];
    }
  }

  async runLint() {
    if (this.stagedFiles.length === 0) {
      console.log(chalk.blue('📝 검사할 파일이 없습니다.'));
      return;
    }

    console.log(chalk.blue('🔍 코드 린팅 검사 중...'));

    try {
      const filesArg = this.stagedFiles.join(' ');
      execSync(`eslint ${filesArg}`, { stdio: 'inherit' });
      console.log(chalk.green('✅ 린팅 검사 통과'));
    } catch (error) {
      console.log(chalk.red('❌ 린팅 오류 발견'));
      this.hasErrors = true;
    }
  }

  async runFormat() {
    console.log(chalk.blue('💅 코드 포매팅 검사 중...'));

    try {
      const filesArg = this.stagedFiles.join(' ');
      execSync(`prettier --check ${filesArg}`, { stdio: 'inherit' });
      console.log(chalk.green('✅ 포매팅 검사 통과'));
    } catch (error) {
      console.log(chalk.yellow('⚠️ 포매팅 불일치 - 자동 수정 중...'));

      try {
        execSync(`prettier --write ${filesArg}`, { stdio: 'inherit' });
        execSync(`git add ${filesArg}`, { stdio: 'inherit' });
        console.log(chalk.green('✅ 포매팅 자동 수정 완료'));
      } catch (fixError) {
        console.log(chalk.red('❌ 포매팅 수정 실패'));
        this.hasErrors = true;
      }
    }
  }

  async runTests() {
    console.log(chalk.blue('🧪 관련 테스트 실행 중...'));

    try {
      execSync('npm test -- --passWithNoTests', { stdio: 'inherit' });
      console.log(chalk.green('✅ 테스트 통과'));
    } catch (error) {
      console.log(chalk.red('❌ 테스트 실패'));
      this.hasErrors = true;
    }
  }

  async execute() {
    console.log(chalk.cyan('🚀 사전 커밋 검사 시작\n'));

    await this.runLint();
    await this.runFormat();
    await this.runTests();

    if (this.hasErrors) {
      console.log(chalk.red('\n❌ 커밋이 중단되었습니다. 오류를 수정해주세요.'));
      process.exit(1);
    } else {
      console.log(chalk.green('\n✅ 모든 검사 통과! 커밋을 진행합니다.'));
      process.exit(0);
    }
  }
}

// 훅 실행
const hook = new PreCommitHook();
hook.execute();
```

### CI/CD 파이프라인 기초

지속적 통합을 위한 기본적인 파이프라인을 구성해보겠습니다.

```yaml title=".github/workflows/ci.yml"
name: CI Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  test:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [16, 18, 20]

    steps:
      - uses: actions/checkout@v3

      - name: Node.js 설정
        uses: actions/setup-node@v3
        with:
          node-version: ${{ matrix.node-version }}
          cache: 'npm'

      - name: 의존성 설치
        run: npm ci

      - name: 린팅 검사
        run: npm run lint

      - name: 포매팅 검사
        run: npm run format:check

      - name: 테스트 실행
        run: npm test

      - name: 빌드 테스트
        run: npm run build
```

## 마무리

이번 장에서는 현대적인 웹 개발에 필수적인 도구들을 살펴보았습니다. 브라우저 개발자 도구를 활용한 디버깅과 성능 분석, Vite와 같은 번들러를 이용한 효율적인 개발 환경 구축, ESLint와 Prettier를 통한 코드 품질 관리, 그리고 Git을 활용한 체계적인 버전 관리 방법을 학습했습니다.

이러한 도구들을 프로젝트에 적용하면 개발 생산성이 크게 향상되고, 팀 협업 시에도 일관된 코드 품질을 유지할 수 있습니다. 특히 자동화된 코드 품질 검사와 사전 커밋 훅을 설정하면 실수를 미연에 방지하고 안정적인 코드베이스를 구축할 수 있습니다.

다음 장에서는 이러한 도구들을 활용하여 실제 프로젝트를 진행하면서 더 실무적인 개발 경험을 쌓아보겠습니다.
