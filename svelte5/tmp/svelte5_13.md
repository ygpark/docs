# Svelte 5 레퍼런스

## 1. Svelte 기초 개념

### 1.1 Svelte 소개

- 컴파일러로서의 Svelte
- Virtual DOM vs 컴파일 타임 최적화
- Svelte의 철학과 특징
- Svelte 4 vs Svelte 5 주요 변화점

### 1.2 개발 환경 설정

- Svelte 프로젝트 생성
- 개발 도구 설정
- 빌드 시스템 이해

### 1.3 컴포넌트 파일 구조

- `.svelte` 파일의 3가지 블록
- `<script>`, `<style>`, HTML 영역
- `.svelte.js` / `.svelte.ts` 파일
- 컴포넌트 작성 기본 규칙

## 2. Runes 시스템 (Svelte 5 핵심)

### 2.1 Runes 개념 이해

- Runes란 무엇인가?
- 기존 반응성 시스템과의 차이점
- Runes 사용 시점과 이유

### 2.2 $state - 상태 관리

- `$state()` 기본 사용법
- 원시값 상태 관리
- 객체와 배열 상태 관리
- `$state.frozen()` 불변 상태
- `$state.snapshot()` 스냅샷

### 2.3 $derived - 파생 상태

- `$derived()` 기본 개념
- 계산된 값 생성
- `$derived.by()` 복잡한 파생 로직
- 기존 `$:` 문법과의 비교

### 2.4 $effect - 사이드 이펙트

- `$effect()` 기본 사용법
- DOM 조작과 외부 API 호출
- `$effect.pre()` DOM 업데이트 전 실행
- `$effect.tracking()` 의존성 확인
- `$effect.root()` 루트 이펙트
- 정리(cleanup) 함수

### 2.5 $props - 컴포넌트 속성

- `$props()` 새로운 Props 시스템
- Props 구조 분해
- 기본값 설정 패턴
- 타입 안전성 확보
- 기존 `export let`과의 비교

### 2.6 $bindable - 양방향 바인딩

- `$bindable()` 개념
- 컴포넌트 간 양방향 통신
- 바인딩 가능한 Props 생성

### 2.7 $inspect - 디버깅

- `$inspect()` 디버깅 도구
- 상태 변화 추적
- 개발 환경에서의 활용

### 2.8 $host - 호스트 요소 접근

- `$host()` 사용법
- 커스텀 요소에서의 활용

## 3. 템플릿 문법 (Modern)

### 3.1 기본 표현식

- `{expression}` 기본 문법
- JavaScript 표현식 사용
- 주석 `<!-- -->`와 `{/* */}`

### 3.2 HTML 속성 바인딩

- 속성 바인딩 `{value}`
- 불린 속성 `{disabled}`
- 속성 단축 문법 `{name}`
- 스프레드 속성 `{...props}`

### 3.3 조건부 렌더링

- `{#if condition}` 기본 조건문
- `{:else if condition}` 다중 조건
- `{:else}` else 블록
- `{/if}` 조건문 종료
- 중첩 조건문

### 3.4 반복 렌더링

- `{#each items as item}` 기본 반복
- `{#each items as item, index}` 인덱스 포함
- `{#each items as item (key)}` 키를 이용한 반복
- `{:else}` 빈 배열 처리
- `{/each}` 반복문 종료
- 중첩 반복문

### 3.5 대기 블록 (Await)

- `{#await promise}` 비동기 처리
- `{:then result}` 성공 시 처리
- `{:catch error}` 에러 처리
- `{/await}` 대기 블록 종료
- 단축 await 문법
- await 표현식

### 3.6 키 블록 (Key)

- `{#key expression}` 강제 재생성
- 컴포넌트 재마운트 제어

### 3.7 스니펫 시스템 (Svelte 5 신규)

- `{#snippet name}` 스니펫 정의
- `{@render snippet()}` 스니펫 렌더링
- 매개변수가 있는 스니펫
- 스니펫 재사용 패턴

### 3.8 특수 템플릿 태그

- `{@html htmlString}` 원시 HTML 삽입
- `{@attach ...}` 새로운 첨부 문법
- `{@const variable = value}` 템플릿 내 상수
- `{@debug variables}` 디버깅 도구

## 4. 컴포넌트 시스템 (Modern)

### 4.1 컴포넌트 기본 (Runes 방식)

- 컴포넌트 정의와 사용
- 컴포넌트 임포트/익스포트
- Runes 기반 컴포넌트 작성

### 4.2 컴포넌트 Props (Modern)

- `$props()` 활용 패턴
- Props 검증과 타입 안전성
- 조건부 Props 처리

### 4.3 스니펫 기반 컴포넌트 조합

- 스니펫을 이용한 컴포넌트 설계
- 재사용 가능한 템플릿 패턴
- 기존 슬롯과의 차이점

### 4.4 컴포넌트 컨텍스트 (Modern)

- `setContext()` / `getContext()` with Runes
- 컨텍스트와 Runes 통합 패턴

### 4.5 컴포넌트 라이프사이클 (Modern)

- `$effect()` 기반 라이프사이클
- 마운트/언마운트 처리
- 업데이트 감지 패턴

## 5. 이벤트 처리

### 5.1 기본 이벤트 핸들링

- `on:event={handler}` 기본 문법
- 인라인 이벤트 핸들러
- 이벤트 객체 접근

### 5.2 이벤트 수정자 (Event Modifiers)

- `preventDefault` 기본 동작 방지
- `stopPropagation` 버블링 중단
- `once` 한 번만 실행
- `self` 자기 자신에서만
- `trusted` 신뢰할 수 있는 이벤트
- `passive` 패시브 리스너

### 5.3 커스텀 이벤트 (Modern)

- Runes와 커스텀 이벤트 통합
- 이벤트 디스패칭 패턴
- 타입 안전한 이벤트 시스템

## 6. 양방향 데이터 바인딩

### 6.1 기본 바인딩

- `bind:value` 입력값 바인딩
- `bind:checked` 체크박스 바인딩
- `bind:group` 라디오/체크박스 그룹

### 6.2 고급 바인딩

- `bind:this` 요소 참조
- `bind:innerHTML` HTML 내용 바인딩
- `bind:textContent` 텍스트 내용 바인딩

### 6.3 미디어 바인딩

- `bind:currentTime` 비디오/오디오 시간
- `bind:duration` 재생 시간
- `bind:paused` 일시정지 상태
- `bind:volume` 볼륨

### 6.4 블록 레벨 바인딩

- `bind:offsetWidth` 요소 너비
- `bind:offsetHeight` 요소 높이
- `bind:clientWidth` 클라이언트 너비
- `bind:clientHeight` 클라이언트 높이
- `bind:scrollX` 스크롤 X 위치
- `bind:scrollY` 스크롤 Y 위치

### 6.5 컴포넌트 바인딩 (Modern)

- `$bindable()` 활용 패턴
- 양방향 컴포넌트 통신

## 7. 액션 (Actions)

### 7.1 액션 기본 개념

- `use:action` 기본 문법
- 액션 함수 정의
- 액션의 용도와 활용

### 7.2 액션 매개변수

- 매개변수가 있는 액션
- 매개변수 업데이트 처리

### 7.3 액션 반환값

- `destroy` 함수 반환
- `update` 함수 반환
- 액션 라이프사이클

### 7.4 액션과 Runes 통합

- `$effect()` 기반 액션 패턴
- 상태 기반 액션 제어

## 8. 스토어 (Stores)

### 8.1 기본 스토어 타입

- `writable(value)` 쓰기 가능 스토어
- `readable(value, start)` 읽기 전용 스토어
- `derived(stores, fn)` 파생 스토어

### 8.2 스토어 사용법

- `$store` 자동 구독 문법
- `store.subscribe()` 수동 구독
- `store.set(value)` 값 설정
- `store.update(fn)` 값 업데이트

### 8.3 Runes vs Stores

- 언제 Runes를 사용할지
- 언제 Stores를 사용할지
- 마이그레이션 전략

### 8.4 스토어와 Runes 통합

- Stores를 Runes로 래핑
- 하이브리드 상태 관리 패턴

## 9. 스타일링

### 9.1 컴포넌트 스타일

- `<style>` 블록 기본 사용법
- 스타일 스코핑 (Scoped Styles)
- CSS 선택자 사용
- 중첩된 `<style>` 요소

### 9.2 동적 스타일링

- `class:name={condition}` 조건부 클래스
- `style:property={value}` 동적 스타일
- CSS 커스텀 속성 (CSS Variables)
- Runes와 동적 스타일링

### 9.3 글로벌 스타일

- `:global()` 선택자
- 글로벌 스타일 적용
- 스타일 상속과 재정의

## 10. 특수 요소와 컴포넌트

### 10.1 특수 요소

- `<svelte:boundary>` 에러 경계 (Svelte 5 신규)
- `<svelte:window>` 윈도우 이벤트
- `<svelte:document>` 문서 이벤트
- `<svelte:body>` body 이벤트
- `<svelte:head>` head 요소 조작
- `<svelte:element>` 동적 요소

### 10.2 특수 요소 바인딩

- 윈도우 크기 바인딩
- 스크롤 위치 바인딩
- 온라인/오프라인 상태

### 10.3 옵션 태그

- `<svelte:options>` 컴포넌트 옵션
- `immutable` 불변성 옵션
- `accessors` 접근자 생성

## 11. 트랜지션과 애니메이션

### 11.1 기본 트랜지션

- `transition:fade` 페이드 효과
- `transition:fly` 플라이 효과
- `transition:slide` 슬라이드 효과
- `transition:scale` 스케일 효과

### 11.2 인/아웃 트랜지션

- `in:transition` 등장 트랜지션
- `out:transition` 사라짐 트랜지션
- 비대칭 트랜지션

### 11.3 커스텀 트랜지션

- 트랜지션 함수 생성
- CSS 트랜지션 제어
- JavaScript 애니메이션

### 11.4 애니메이션

- `animate:flip` 리스트 애니메이션
- 커스텀 애니메이션 함수
- 애니메이션 매개변수

### 11.5 모션 (Motion)

- `tweened` 값 트위닝
- `spring` 스프링 애니메이션
- 이징 함수와 보간

## 12. 웹 API 통합

### 12.1 네트워크 요청 (Runes 패턴)

- `$effect()` 기반 데이터 페칭
- 비동기 상태 관리
- 에러 처리 및 재시도 로직
- 로딩 상태 관리

### 12.2 브라우저 API 활용

- Local Storage / Session Storage
- Geolocation API
- Web Workers 통합
- Service Worker 연동
- File API 사용

### 12.3 실시간 통신

- WebSocket 연결
- Server-Sent Events (SSE)
- 실시간 데이터 업데이트

## 13. 폼 처리 및 검증 (Modern)

### 13.1 폼 상태 관리 (Runes)

- `$state()` 기반 폼 관리
- 폼 필드 바인딩
- 동적 폼 생성

### 13.2 검증 시스템

- `$derived()` 기반 검증
- 실시간 검증 패턴
- 에러 상태 관리

## 14. 타입스크립트 지원

### 14.1 기본 타입 설정

- TypeScript 설정
- 컴포넌트 타입 정의
- Runes 타입 안전성

### 14.2 고급 타입 패턴

- 제네릭 컴포넌트
- Props 타입 추론
- 이벤트 타입 정의

## 15. 마이그레이션 및 Legacy

### 15.1 Svelte 4에서 5로 마이그레이션

- 자동 마이그레이션 도구
- 단계별 마이그레이션 전략
- 호환성 모드

### 15.2 Legacy APIs 이해

- `export let` (Legacy Props)
- `$:` 반응형 구문 (Legacy)
- `<slot>` 시스템 (Legacy)
- `$$props` / `$$restProps` (Legacy)
- `on:` 이벤트 (Legacy)

### 15.3 하이브리드 개발

- Legacy와 Modern 코드 혼용
- 점진적 업그레이드 패턴

## 16. 테스팅 및 디버깅

### 16.1 단위 테스트 (Modern)

- Runes 기반 컴포넌트 테스트
- 상태 변화 테스트
- 이벤트 시뮬레이션

### 16.2 디버깅 기법

- `$inspect()` 활용
- 브라우저 개발자 도구
- Svelte DevTools

### 16.3 에러 처리

- `<svelte:boundary>` 활용
- 에러 상태 관리
- 사용자 친화적 에러 페이지

## 17. 성능 최적화 (Modern)

### 17.1 Runes 최적화

- `$state.frozen()` 활용
- 불필요한 재계산 방지
- 메모이제이션 패턴

### 17.2 번들 크기 최적화

- 트리 셰이킹
- 코드 분할 전략
- 의존성 최적화

## 18. 고급 패턴 및 모범 사례

### 18.1 Runes 기반 아키텍처

- 상태 관리 패턴
- 컴포넌트 설계 원칙
- 데이터 플로우 설계

### 18.2 재사용 가능한 로직

- 커스텀 Runes 패턴
- 상태 로직 추상화
- 컴포저블 함수

### 18.3 대규모 애플리케이션

- 모듈화 전략
- 상태 관리 스케일링
- 성능 고려사항
