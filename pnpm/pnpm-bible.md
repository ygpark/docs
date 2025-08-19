# pnpm 바이블

## 학습 목표

이 가이드를 통해 JavaScript 패키지 매니저인 pnpm의 기본 개념부터 고급 기능까지 단계별로 학습하고, 실제 프로젝트에서 pnpm을 효과적으로 활용할 수 있는 능력을 기를 수 있습니다. 특히 npm이나 yarn에 익숙한 개발자들이 pnpm으로 전환할 때 필요한 모든 지식을 제공합니다.

## 1. pnpm 소개 및 기본 개념

JavaScript 생태계에서 패키지 관리는 프로젝트 성공의 핵심 요소입니다. npm과 yarn이 오랫동안 표준으로 사용되어 왔지만, pnpm은 혁신적인 접근 방식으로 디스크 공간 효율성과 설치 속도를 크게 개선했습니다. 이 섹션에서는 pnpm이 무엇인지, 왜 주목받고 있는지 알아보겠습니다.

### 1.1 pnpm이란?

pnpm(Performant Node Package Manager)은 Node.js용 빠르고 효율적인 패키지 매니저입니다. npm과 완벽하게 호환되면서도 독특한 저장 방식을 통해 기존 패키지 매니저의 한계를 극복했습니다.

### 1.2 pnpm의 주요 특징

- **디스크 공간 절약**: 하드링크와 심볼릭 링크를 사용하여 같은 패키지를 중복 저장하지 않음
- **빠른 설치 속도**: 효율적인 캐싱과 병렬 처리로 설치 시간 단축
- **엄격한 의존성 관리**: Phantom dependencies 문제 해결
- **모노레포 지원**: 워크스페이스 기능으로 대규모 프로젝트 관리 용이

### 1.3 왜 pnpm을 사용해야 할까?

```bash title="디스크 사용량 비교 예시"
# npm으로 여러 프로젝트에 React 설치 시
project1/node_modules/react (45MB)
project2/node_modules/react (45MB)
project3/node_modules/react (45MB)
총 사용량: 135MB

# pnpm으로 설치 시
~/.pnpm-store/react@18.2.0 (45MB)
project1/node_modules/react -> 하드링크
project2/node_modules/react -> 하드링크
project3/node_modules/react -> 하드링크
총 사용량: 45MB
```

## 2. pnpm 설치하기

pnpm을 사용하기 전에 시스템에 설치해야 합니다. 다양한 설치 방법이 있으며, 운영체제와 개발 환경에 따라 적절한 방법을 선택할 수 있습니다. 설치 과정은 간단하지만, 올바른 설치 확인까지 마쳐야 문제없이 사용할 수 있습니다.

### 2.1 설치 전 준비사항

- Node.js 16.14 이상 버전 필요
- npm이 이미 설치되어 있어야 함

### 2.2 설치 방법

```bash title="pnpm 설치 명령어들"
# npm을 통한 설치 (권장)
npm install -g pnpm

# 또는 corepack 사용 (Node.js 16.13+ 포함)
corepack enable
corepack prepare pnpm@latest --activate

# macOS/Linux용 설치 스크립트
curl -fsSL https://get.pnpm.io/install.sh | sh -

# Windows PowerShell
iwr https://get.pnpm.io/install.ps1 -useb | iex
```

### 2.3 설치 확인

```bash title="설치 확인 명령어"
# 버전 확인
pnpm --version

# 도움말 확인
pnpm help
```

## 3. pnpm 기본 사용법

pnpm의 기본 명령어들은 npm과 매우 유사하여 학습 곡선이 완만합니다. 하지만 내부 동작 방식의 차이로 인해 몇 가지 주의사항이 있습니다. 이 섹션에서는 프로젝트 생성부터 패키지 관리, 스크립트 실행까지 일상적인 개발 작업에 필요한 모든 명령어를 다룹니다.

### 3.1 프로젝트 초기화

```bash title="프로젝트 초기화"
# 새 프로젝트 생성
mkdir my-project
cd my-project
pnpm init

# 기존 npm 프로젝트에서 전환
pnpm import  # package-lock.json을 pnpm-lock.yaml로 변환
```

### 3.2 패키지 설치 및 관리

```bash title="패키지 관리 명령어"
# 의존성 설치
pnpm install           # 또는 pnpm i
pnpm add react         # 프로덕션 의존성 추가
pnpm add -D typescript # 개발 의존성 추가
pnpm add -g nodemon    # 전역 설치

# 패키지 제거
pnpm remove react      # 또는 pnpm rm, pnpm uninstall

# 패키지 업데이트
pnpm update           # 모든 패키지 업데이트
pnpm update react     # 특정 패키지 업데이트

# 설치된 패키지 확인
pnpm list             # 또는 pnpm ls
pnpm list --depth=0   # 최상위 의존성만 표시
```

### 3.3 스크립트 실행

```json title="package.json"
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "test": "jest"
  }
}
```

```bash title="스크립트 실행"
# 스크립트 실행
pnpm run dev          # 또는 pnpm dev
pnpm run build
pnpm test             # run 생략 가능한 스크립트

# 여러 스크립트 동시 실행
pnpm run -r build     # 워크스페이스의 모든 패키지에서 build 실행
```

## 4. pnpm의 작동 원리 이해하기

pnpm의 혁신적인 성능은 독특한 패키지 저장 방식에서 나옵니다. 기존의 flat한 node_modules 구조 대신 심볼릭 링크와 하드링크를 활용한 계층적 구조를 사용합니다. 이 방식을 이해하면 pnpm이 어떻게 디스크 공간을 절약하고 의존성 문제를 해결하는지 알 수 있습니다.

### 4.1 Content-addressable Store

pnpm은 모든 패키지를 전역 저장소(.pnpm-store)에 한 번만 저장하고, 프로젝트에서는 하드링크로 참조합니다.

```bash title="저장소 위치 확인"
# 저장소 위치 확인
pnpm store path

# 저장소 사용량 확인
pnpm store status

# 사용하지 않는 패키지 정리
pnpm store prune
```

### 4.2 node_modules 구조

```text title="pnpm node_modules 구조 예시"
node_modules/
├── .pnpm/
│   ├── react@18.2.0/
│   │   └── node_modules/
│   │       └── react/        # 실제 패키지 파일들
│   └── lodash@4.17.21/
│       └── node_modules/
│           └── lodash/
├── react -> .pnpm/react@18.2.0/node_modules/react
└── lodash -> .pnpm/lodash@4.17.21/node_modules/lodash
```

### 4.3 락파일 (pnpm-lock.yaml)

```yaml title="pnpm-lock.yaml 예시"
lockfileVersion: '6.0'

dependencies:
  react:
    specifier: ^18.2.0
    version: 18.2.0

packages:
  /react@18.2.0:
    resolution: { integrity: sha512-... }
    dependencies:
      loose-envify: 1.4.0
    dev: false
```

## 5. pnpm 워크스페이스 (Monorepo)

모노레포는 여러 관련 프로젝트를 하나의 저장소에서 관리하는 방식으로, 대규모 애플리케이션 개발에 필수적입니다. pnpm의 워크스페이스 기능은 패키지 간 의존성 관리, 공유 의존성 최적화, 일괄 명령 실행 등을 지원하여 모노레포 관리를 효율적으로 만들어줍니다.

### 5.1 워크스페이스 설정

```yaml title="pnpm-workspace.yaml"
packages:
  - 'packages/*'
  - 'apps/*'
  - '!**/test/**'
```

```json title="루트 package.json"
{
  "name": "my-monorepo",
  "private": true,
  "scripts": {
    "build": "pnpm -r run build",
    "test": "pnpm -r run test"
  }
}
```

실용적 예제: 모노레포 구조를 이용한 프론트엔드 프로젝트 설정입니다. 공통 UI 컴포넌트와 여러 애플리케이션을 효율적으로 관리할 수 있습니다.

```text title="모노레포 디렉토리 구조"
my-monorepo/
├── pnpm-workspace.yaml
├── package.json
├── packages/
│   ├── ui-components/
│   │   └── package.json
│   └── utils/
│       └── package.json
└── apps/
    ├── web/
    │   └── package.json
    └── mobile/
        └── package.json
```

### 5.2 워크스페이스 명령어

```bash title="워크스페이스 명령어"
# 모든 워크스페이스에 의존성 설치
pnpm install

# 특정 워크스페이스에 패키지 추가
pnpm add react --filter web

# 워크스페이스 간 의존성 추가
pnpm add ui-components --filter web --workspace

# 모든 워크스페이스에서 스크립트 실행
pnpm -r run build

# 특정 워크스페이스에서만 실행
pnpm --filter web run dev
```

### 5.3 필터링 옵션

```bash title="필터링 예시"
# 패턴 매칭
pnpm --filter "./packages/*" run build

# 의존성 기반 필터링
pnpm --filter "...ui-components" run build  # ui-components와 그것에 의존하는 모든 패키지

# 변경된 패키지만
pnpm --filter "...[HEAD~1]" run test  # 마지막 커밋 이후 변경된 패키지만
```

## 6. pnpm 설정 및 구성

pnpm의 동작을 프로젝트나 시스템 전체에서 세밀하게 제어할 수 있는 다양한 설정 옵션들이 있습니다. 이러한 설정들을 적절히 활용하면 보안, 성능, 개발 경험을 크게 향상시킬 수 있습니다. 특히 팀 프로젝트에서는 일관된 설정을 통해 개발 환경을 통일할 수 있습니다.

### 6.1 .npmrc 파일 설정

```ini title=".npmrc 파일 예시"
# 레지스트리 설정
registry=https://registry.npmjs.org/

# 저장소 설정
store-dir=/path/to/custom/store

# 보안 설정
fund=false
audit-level=moderate

# 성능 설정
shamefully-hoist=true
strict-peer-dependencies=false

# 로깅
loglevel=warn
```

### 6.2 주요 설정 옵션

```bash title="설정 명령어"
# 현재 설정 확인
pnpm config list

# 특정 설정 확인
pnpm config get registry

# 설정 변경
pnpm config set store-dir ~/.pnpm-store
pnpm config set registry https://registry.npm.taobao.org/

# 설정 삭제
pnpm config delete registry
```

## 7. pnpm vs npm vs yarn 비교

패키지 매니저 선택은 프로젝트의 성능과 개발 경험에 큰 영향을 미칩니다. 각 도구는 고유한 장단점을 가지고 있으며, 프로젝트의 규모, 팀의 요구사항, 성능 기대치에 따라 최적의 선택이 달라집니다. 이 비교를 통해 pnpm이 언제, 왜 좋은 선택인지 명확히 이해할 수 있습니다.

### 7.1 성능 비교

| 항목          | npm  | yarn | pnpm      |
| ------------- | ---- | ---- | --------- |
| 설치 속도     | 보통 | 빠름 | 매우 빠름 |
| 디스크 사용량 | 높음 | 높음 | 낮음      |
| 메모리 사용량 | 보통 | 높음 | 낮음      |
| 캐시 효율성   | 보통 | 좋음 | 매우 좋음 |

### 7.2 기능 비교

```bash title="명령어 비교"
# 패키지 설치
npm install package-name
yarn add package-name
pnpm add package-name

# 개발 의존성 추가
npm install --save-dev package-name
yarn add --dev package-name
pnpm add --save-dev package-name

# 스크립트 실행
npm run script-name
yarn script-name
pnpm script-name
```

## 8. 고급 기능 및 팁

pnpm의 진정한 가치는 고급 기능들에서 나타납니다. 저장소 관리, Node.js 버전 제어, 캐시 최적화 등의 기능들은 대규모 프로젝트나 CI/CD 환경에서 특히 유용합니다. 이러한 기능들을 마스터하면 개발 워크플로우를 크게 개선할 수 있습니다.

### 8.1 저장소 관리

```bash title="저장소 관리 명령어"
# 저장소 정보 확인
pnpm store status

# 사용하지 않는 패키지 정리
pnpm store prune

# 저장소 위치 변경
pnpm config set store-dir /new/path/to/store

# 저장소 완전 정리
rm -rf $(pnpm store path)
```

### 8.2 Node.js 버전 관리

```bash title="Node.js 버전 관리"
# Node.js 설치 및 사용
pnpm env use --global 18
pnpm env use --global lts

# 사용 가능한 버전 확인
pnpm env list

# 특정 버전으로 스크립트 실행
pnpm --use-node-version=16 run build
```

### 8.3 캐시 관리

```bash title="캐시 관리"
# 캐시 위치 확인
pnpm config get cache-dir

# 캐시 정리
pnpm cache clean

# 메타데이터 캐시 정리
pnpm cache clean --metadata-only
```

## 9. 마이그레이션 가이드

기존 프로젝트를 pnpm으로 전환하는 것은 신중한 계획과 단계적 접근이 필요합니다. 잘못된 마이그레이션은 프로젝트의 안정성을 해칠 수 있으므로, 각 패키지 매니저별 특성을 이해하고 올바른 절차를 따라야 합니다. 이 가이드는 안전하고 효율적인 전환을 도와줍니다.

### 9.1 npm에서 pnpm으로 전환

실용적 예제: 기존 React 프로젝트를 npm에서 pnpm으로 안전하게 전환하는 과정입니다. 의존성 호환성 문제를 미리 확인하고 해결할 수 있습니다.

```bash title="npm에서 pnpm 전환"
# 1. 기존 node_modules와 lock 파일 제거
rm -rf node_modules package-lock.json

# 2. pnpm으로 의존성 설치
pnpm install

# 3. 스크립트 테스트
pnpm run build
pnpm run test

# 4. package.json scripts 업데이트 (필요한 경우)
```

### 9.2 yarn에서 pnpm으로 전환

```bash title="yarn에서 pnpm 전환"
# 1. yarn 관련 파일 제거
rm -rf node_modules yarn.lock .yarn/

# 2. yarn.lock을 pnpm-lock.yaml로 변환
pnpm import

# 3. 의존성 재설치
pnpm install

# 4. 워크스페이스 설정 변환 (yarn workspaces 사용 시)
# package.json의 workspaces를 pnpm-workspace.yaml로 이동
```

## 10. 문제 해결 및 FAQ

pnpm 사용 중 발생할 수 있는 다양한 문제들과 해결 방법을 다룹니다. 특히 기존 npm/yarn 사용자들이 자주 겪는 혼란스러운 상황들을 중심으로 실용적인 해결책을 제시합니다. 이 섹션의 내용을 숙지하면 대부분의 문제를 스스로 해결할 수 있습니다.

### 10.1 자주 발생하는 문제들

```bash title="일반적인 문제 해결"
# 1. 의존성 해결 실패
pnpm install --shamefully-hoist

# 2. peer dependencies 경고
pnpm install --strict-peer-dependencies=false

# 3. 권한 문제 (전역 설치)
sudo pnpm install -g package-name
# 또는 권한 없이 설치
pnpm config set prefix ~/.local
```

### 10.2 디버깅 방법

```bash title="디버깅 명령어"
# 상세 로그 출력
pnpm install --loglevel=debug

# 의존성 트리 확인
pnpm list --depth=infinity

# 중복 의존성 확인
pnpm dedupe

# 저장소 상태 확인
pnpm store status --json
```

## 11. 실전 예제

이론적 지식을 실제 프로젝트에 적용하는 것이 중요합니다. 이 섹션에서는 인기 있는 프레임워크들과 모노레포 구조에서 pnpm을 효과적으로 활용하는 구체적인 예제들을 제공합니다. 각 예제는 실제 개발 환경에서 바로 적용할 수 있도록 구성되었습니다.

### 11.1 React 프로젝트 예제

실용적 예제: Create React App 대신 Vite와 pnpm을 사용한 최신 React 개발 환경 구성입니다. 빠른 개발 서버와 효율적인 패키지 관리를 경험할 수 있습니다.

```bash title="React 프로젝트 설정"
# 프로젝트 생성
pnpm create react-app my-react-app --template typescript
cd my-react-app

# 추가 의존성 설치
pnpm add @emotion/react @emotion/styled
pnpm add -D @types/react @types/react-dom

# 개발 서버 실행
pnpm start
```

```json title="package.json"
{
  "name": "my-react-app",
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  }
}
```

### 11.2 Next.js 프로젝트 예제

```bash title="Next.js 프로젝트 설정"
# Next.js 프로젝트 생성
pnpm create next-app@latest my-next-app --typescript --tailwind --eslint

# 추가 패키지 설치
pnpm add next-auth prisma
pnpm add -D @types/node

# 개발 서버 실행
pnpm dev
```

### 11.3 모노레포 프로젝트 예제

```yaml title="pnpm-workspace.yaml"
packages:
  - 'packages/*'
  - 'apps/*'
```

```bash title="모노레포 설정"
# 프로젝트 구조 생성
mkdir -p packages/ui apps/web apps/api

# UI 패키지 생성
cd packages/ui
pnpm init
pnpm add react typescript

# 웹 앱에서 UI 패키지 사용
cd ../../apps/web
pnpm init
pnpm add ui --workspace
```

## 12. 유용한 리소스

pnpm 학습과 문제 해결에 도움이 되는 공식 문서, 커뮤니티 리소스, 도구들을 정리했습니다. 지속적인 학습과 최신 정보 습득을 위해 이러한 리소스들을 활용하시기 바랍니다.

### 공식 문서 및 가이드

- [pnpm 공식 문서](https://pnpm.io/)
- [pnpm GitHub 저장소](https://github.com/pnpm/pnpm)
- [pnpm 블로그](https://pnpm.io/blog)

### 커뮤니티 및 지원

- [pnpm Discord](https://discord.gg/pnpm)
- [Stack Overflow](https://stackoverflow.com/questions/tagged/pnpm)
- [Reddit r/nodejs](https://reddit.com/r/nodejs)

### 유용한 도구

- [npm-to-pnpm](https://github.com/pnpm/npm-to-pnpm) - 마이그레이션 도구
- [pnpm-lock-export](https://github.com/pnpm/pnpm-lock-export) - 락파일 변환 도구
- [size-limit](https://github.com/ai/size-limit) - 번들 크기 관리

---

이 가이드를 통해 pnpm의 모든 기능을 활용하여 더 효율적이고 빠른 JavaScript 개발 환경을 구축하시기 바랍니다. pnpm은 단순한 패키지 매니저를 넘어서 현대적인 개발 워크플로우의 핵심 도구가 될 것입니다.
