---
sidebar_position: 21
---

# Node.js 설치

스벨트킷 개발을 시작하기 위해서는 먼저 Node.js를 설치해야 합니다. Node.js는 JavaScript 런타임 환경으로, 스벨트킷의 개발 도구들과 빌드 시스템을 실행하는 데 필요합니다. 이 섹션에서는 운영체제별 Node.js 설치 방법과 권장 설정을 알아보겠습니다.

## Node.js란?

Node.js는 Chrome V8 JavaScript 엔진으로 빌드된 JavaScript 런타임입니다. 브라우저 밖에서도 JavaScript를 실행할 수 있게 해주며, 다음과 같은 기능을 제공합니다:

- **패키지 관리**: npm(Node Package Manager)을 통한 라이브러리 설치 및 관리
- **빌드 도구**: 개발 서버, 번들러, 컴파일러 등의 실행 환경
- **스크립트 실행**: 빌드, 테스트, 배포 등의 자동화 스크립트 실행

## 스벨트킷의 Node.js 요구사항

스벨트킷은 다음 Node.js 버전을 지원합니다:

- **최소 요구 버전**: Node.js 16.14.0 이상
- **권장 버전**: Node.js 18.x 또는 20.x (LTS 버전)
- **최신 버전**: Node.js 21.x (최신 기능 사용 가능하지만 안정성 고려 필요)

```bash
# 현재 설치된 Node.js 버전 확인
node --version
# 또는
node -v

# npm 버전 확인
npm --version
```

## 설치 방법별 비교

### 1. 공식 웹사이트에서 다운로드 (권장)

**장점:**

- 가장 안정적이고 간단한 방법
- 공식 지원과 보안 업데이트 보장
- Windows, macOS, Linux 모든 플랫폼 지원

**단점:**

- 버전 관리가 어려움
- 여러 프로젝트에서 다른 Node.js 버전이 필요한 경우 불편

### 2. 버전 관리자 사용 (개발자 권장)

**장점:**

- 여러 Node.js 버전을 쉽게 설치하고 전환 가능
- 프로젝트별로 다른 버전 사용 가능
- 팀 개발 시 일관된 환경 구성 가능

**단점:**

- 초기 설정이 약간 복잡
- 추가 도구 학습 필요

### 3. 패키지 매니저 사용

**장점:**

- 운영체제의 패키지 매니저와 통합
- 시스템 전체 업데이트와 함께 관리

**단점:**

- 최신 버전이 늦게 제공될 수 있음
- 버전 관리 제한적

## 운영체제별 설치 가이드

### Windows

#### 방법 1: 공식 웹사이트에서 설치

1. **Node.js 공식 웹사이트 방문**
   - https://nodejs.org 접속
   - LTS 버전 다운로드 (권장)

2. **설치 파일 실행**

   ```
   node-v20.x.x-x64.msi 파일 다운로드 및 실행
   ```

3. **설치 마법사 진행**
   - "Add to PATH" 옵션 체크 (중요!)
   - "Automatically install the necessary tools" 체크 (Python, Visual Studio Build Tools 자동 설치)

4. **설치 확인**
   ```cmd
   # 명령 프롬프트 또는 PowerShell에서 실행
   node --version
   npm --version
   ```

#### 방법 2: Chocolatey 사용

```powershell
# PowerShell을 관리자 권한으로 실행
# Chocolatey 설치 (이미 설치된 경우 생략)
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))

# Node.js 설치
choco install nodejs

# 특정 버전 설치
choco install nodejs --version=20.10.0
```

#### 방법 3: winget 사용 (Windows 10/11)

```cmd
# LTS 버전 설치
winget install OpenJS.NodeJS

# 또는 최신 버전
winget install OpenJS.NodeJS.Current
```

#### 방법 4: nvm-windows 사용 (버전 관리)

1. **nvm-windows 설치**
   - https://github.com/coreybutler/nvm-windows/releases 에서 최신 버전 다운로드
   - `nvm-setup.zip` 다운로드 후 설치

2. **Node.js 설치 및 관리**

   ```cmd
   # 사용 가능한 Node.js 버전 목록 확인
   nvm list available

   # 특정 버전 설치
   nvm install 20.10.0
   nvm install 18.19.0

   # 설치된 버전 목록 확인
   nvm list

   # 사용할 버전 선택
   nvm use 20.10.0

   # 기본 버전 설정
   nvm alias default 20.10.0
   ```

### macOS

#### 방법 1: 공식 웹사이트에서 설치

1. **Node.js 공식 웹사이트에서 다운로드**
   - https://nodejs.org에서 macOS Installer 다운로드

2. **설치 파일 실행**

   ```
   node-v20.x.x.pkg 파일 다운로드 및 실행
   ```

3. **설치 확인**
   ```bash
   node --version
   npm --version
   ```

#### 방법 2: Homebrew 사용 (권장)

```bash
# Homebrew 설치 (이미 설치된 경우 생략)
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Node.js 설치
brew install node

# 특정 버전 설치
brew install node@18
brew install node@20

# 설치된 버전 확인
brew list | grep node
```

#### 방법 3: nvm 사용 (버전 관리, 개발자 권장)

```bash
# nvm 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 터미널 재시작 또는 다음 명령 실행
source ~/.bashrc
# 또는 zsh 사용 시
source ~/.zshrc

# 사용 가능한 Node.js 버전 확인
nvm ls-remote

# LTS 버전 설치
nvm install --lts
nvm install 20.10.0
nvm install 18.19.0

# 설치된 버전 확인
nvm list

# 사용할 버전 선택
nvm use 20.10.0

# 기본 버전 설정
nvm alias default 20.10.0

# 현재 버전 확인
nvm current
```

### Linux (Ubuntu/Debian)

#### 방법 1: NodeSource 저장소 사용 (권장)

```bash
# 시스템 업데이트
sudo apt update

# NodeSource GPG 키 추가 및 저장소 설정
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# Node.js 설치
sudo apt-get install -y nodejs

# 설치 확인
node --version
npm --version
```

#### 방법 2: snap 패키지 사용

```bash
# LTS 버전 설치
sudo snap install node --classic

# 특정 버전 설치
sudo snap install node --channel=18/stable --classic
```

#### 방법 3: nvm 사용 (버전 관리)

```bash
# nvm 설치
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.0/install.sh | bash

# 터미널 재시작 또는 다음 명령 실행
source ~/.bashrc

# Node.js 설치 및 관리 (macOS와 동일)
nvm install --lts
nvm use --lts
nvm alias default node
```

### Linux (CentOS/RHEL/Fedora)

#### 방법 1: NodeSource 저장소 사용

```bash
# CentOS/RHEL
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
sudo yum install -y nodejs

# Fedora
curl -fsSL https://rpm.nodesource.com/setup_20.x | sudo bash -
sudo dnf install -y nodejs
```

#### 방법 2: dnf/yum 기본 저장소 사용

```bash
# Fedora
sudo dnf install npm nodejs

# CentOS/RHEL (EPEL 저장소 필요)
sudo yum install epel-release
sudo yum install npm nodejs
```

## 설치 후 설정

### npm 설정

```bash
# npm 최신 버전으로 업데이트
npm install -g npm@latest

# 글로벌 패키지 설치 경로 확인
npm config get prefix

# 사용자별 글로벌 패키지 경로 설정 (권한 문제 방지)
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'

# 환경 변수 설정 (.bashrc 또는 .zshrc에 추가)
echo 'export PATH=~/.npm-global/bin:$PATH' >> ~/.bashrc
source ~/.bashrc
```

### 필수 개발 도구 설치

```bash
# 자주 사용되는 글로벌 패키지들
npm install -g @sveltejs/kit
npm install -g create-svelte
npm install -g vite
npm install -g typescript
npm install -g prettier
npm install -g eslint
```

### 대안 패키지 매니저

Node.js와 함께 다른 패키지 매니저도 고려해볼 수 있습니다:

#### pnpm (성능 개선)

```bash
# pnpm 설치
npm install -g pnpm

# 사용법 (npm과 거의 동일)
pnpm install
pnpm add svelte
pnpm run dev
```

#### Yarn (페이스북 개발)

```bash
# Yarn 설치
npm install -g yarn

# 사용법
yarn install
yarn add svelte
yarn dev
```

## 문제 해결

### 일반적인 문제들

#### 1. 권한 오류 (Permission Denied)

**Windows:**

```cmd
# PowerShell을 관리자 권한으로 실행
# 또는 nvm-windows 사용
```

**macOS/Linux:**

```bash
# sudo 없이 글로벌 패키지 설치하기
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
```

#### 2. PATH 환경 변수 문제

```bash
# Node.js가 설치되어 있지만 명령을 찾을 수 없는 경우
echo $PATH  # 현재 PATH 확인

# 수동으로 PATH 추가 (예시)
export PATH="/usr/local/bin:$PATH"  # macOS
export PATH="/usr/bin:$PATH"        # Linux
```

#### 3. 오래된 npm 버전

```bash
# npm 업데이트
npm install -g npm@latest

# 캐시 정리
npm cache clean --force
```

#### 4. 네트워크 관련 문제

```bash
# 프록시 설정 (회사 환경)
npm config set proxy http://proxy.company.com:8080
npm config set https-proxy http://proxy.company.com:8080

# 레지스트리 변경 (중국 등 일부 지역)
npm config set registry https://registry.npmmirror.com
```

### 버전 확인 및 테스트

설치가 완료되면 다음 명령들로 정상 설치를 확인하세요:

```bash
# 버전 정보 확인
node --version    # v20.10.0
npm --version     # 10.2.3

# Node.js 기본 테스트
node -e "console.log('Node.js 설치 완료!')"

# npm 기본 테스트
npm --help

# 간단한 패키지 설치 테스트
mkdir test-node
cd test-node
npm init -y
npm install lodash
node -e "console.log(require('lodash').VERSION)"
```

## 다음 단계

Node.js 설치가 완료되면:

1. **스벨트킷 프로젝트 생성** (2.2장에서 다룰 예정)
2. **개발 도구 설정** (2.3장에서 다룰 예정)
3. **프로젝트 구조 이해** (2.4장에서 다룰 예정)

Node.js는 스벨트킷 개발의 기초가 되는 중요한 도구입니다. 안정적인 LTS 버전을 사용하고, 개발 팀이 있다면 모든 팀원이 동일한 버전을 사용하는 것을 권장합니다.

`.nvmrc` 파일을 프로젝트 루트에 생성하여 팀 전체가 동일한 Node.js 버전을 사용하도록 할 수 있습니다:

```bash
# .nvmrc 파일 생성
echo "20.10.0" > .nvmrc

# 팀원들은 다음 명령으로 올바른 버전 사용
nvm use
```

다음 섹션에서는 Node.js를 이용해 실제로 스벨트킷 프로젝트를 생성하는 방법을 알아보겠습니다.
