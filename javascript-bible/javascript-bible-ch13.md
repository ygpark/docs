---
title: '13장: 웹 API와 데이터 통신'
slug: javascript-bible-chapter-13-web-api-data-communication
description: '모던 자바스크립트에서 웹 API를 활용한 데이터 통신 방법을 배워보세요. Fetch API, JSON 처리, async/await, RESTful API, 로컬 스토리지 등을 실습을 통해 익힐 수 있습니다.'
keywords:
  [
    '자바스크립트',
    'JavaScript',
    'Fetch API',
    'JSON',
    'async await',
    'RESTful API',
    '로컬 스토리지',
    '웹 API',
    'HTTP 통신',
    'AJAX',
    'Promise',
    '비동기 처리',
  ]
sidebar_position: 13
---

# 13장: 웹 API와 데이터 통신

웹 애플리케이션의 핵심은 데이터입니다. 사용자가 버튼을 클릭하면 서버에서 최신 정보를 가져오고, 입력한 내용을 저장하며, 실시간으로 변화하는 데이터를 화면에 반영해야 합니다. 이 장에서는 모던 자바스크립트의 강력한 웹 API들을 활용해 서버와 통신하고, 브라우저의 저장소를 활용하며, 사용자에게 매끄러운 경험을 제공하는 방법을 배워보겠습니다. 실제 개발에서 자주 사용하는 패턴들을 실습을 통해 익혀보세요.

---

## Fetch API 기초

Fetch API는 서버와 통신하는 모던한 방법입니다. 기존의 XMLHttpRequest보다 간단하고 Promise 기반으로 동작하여 비동기 처리가 훨씬 편리합니다.

기본적인 GET 요청으로 데이터를 가져오는 방법부터 살펴보겠습니다.

```javascript title="basic-fetch.js"
// 기본 GET 요청
async function fetchUserData() {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/users/1');

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const userData = await response.json();
    console.log('사용자 정보:', userData);

    return userData;
  } catch (error) {
    console.error('데이터를 가져오는데 실패했습니다:', error);
    throw error;
  }
}

// 함수 호출
fetchUserData();
```

Fetch API의 기본 옵션들을 활용해 다양한 HTTP 메서드로 요청하는 방법을 알아보겠습니다.

```javascript title="fetch-methods.js"
// POST 요청으로 데이터 생성
async function createPost(postData) {
  try {
    const response = await fetch('https://jsonplaceholder.typicode.com/posts', {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
      },
      body: JSON.stringify(postData),
    });

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const result = await response.json();
    console.log('생성된 포스트:', result);
    return result;
  } catch (error) {
    console.error('포스트 생성 실패:', error);
    throw error;
  }
}

// 사용 예시
createPost({
  title: '새로운 포스트',
  body: '포스트 내용입니다.',
  userId: 1,
});
```

---

## JSON 데이터 처리

JSON은 웹에서 가장 널리 사용되는 데이터 형식입니다. 자바스크립트 객체와 JSON 문자열 간의 변환을 안전하게 처리하는 방법을 배워보겠습니다.

서버에서 받은 JSON 데이터를 파싱하고 활용하는 실용적인 예제입니다.

```javascript title="json-processing.js"
// 안전한 JSON 파싱 함수
function safeJsonParse(jsonString, defaultValue = null) {
  try {
    return JSON.parse(jsonString);
  } catch (error) {
    console.error('JSON 파싱 오류:', error);
    return defaultValue;
  }
}

// 복잡한 JSON 데이터 처리
function processApiResponse(responseData) {
  const users = responseData.users || [];

  return users.map(user => ({
    id: user.id,
    name: user.name,
    email: user.email,
    isActive: user.status === 'active',
    profileUrl: user.avatar || '/default-avatar.png',
  }));
}

// 사용 예시
const apiResponse = {
  users: [
    { id: 1, name: '김철수', email: 'kim@example.com', status: 'active' },
    {
      id: 2,
      name: '이영희',
      email: 'lee@example.com',
      status: 'inactive',
      avatar: '/custom-avatar.png',
    },
  ],
};

const processedUsers = processApiResponse(apiResponse);
console.log('처리된 사용자 데이터:', processedUsers);
```

JSON 데이터를 로컬 스토리지에 저장하고 불러오는 유틸리티 함수를 만들어보겠습니다.

```javascript title="json-storage.js"
// JSON 로컬 스토리지 유틸리티
class JsonStorage {
  static save(key, data) {
    try {
      const jsonString = JSON.stringify(data);
      localStorage.setItem(key, jsonString);
      return true;
    } catch (error) {
      console.error('데이터 저장 실패:', error);
      return false;
    }
  }

  static load(key, defaultValue = null) {
    try {
      const jsonString = localStorage.getItem(key);
      return jsonString ? JSON.parse(jsonString) : defaultValue;
    } catch (error) {
      console.error('데이터 로드 실패:', error);
      return defaultValue;
    }
  }

  static remove(key) {
    localStorage.removeItem(key);
  }

  static clear() {
    localStorage.clear();
  }
}

// 사용 예시
const userData = { name: '홍길동', age: 30, hobbies: ['독서', '영화감상'] };
JsonStorage.save('user', userData);

const loadedUser = JsonStorage.load('user');
console.log('불러온 사용자:', loadedUser);
```

---

## async/await vs then 체이닝

비동기 처리의 두 가지 주요 방식을 비교해보고, 각각의 장단점과 적절한 사용 시점을 알아보겠습니다.

Promise.then() 체이닝 방식의 예제입니다.

```javascript title="promise-then.js"
// then 체이닝 방식
function fetchUserPostsWithThen(userId) {
  return fetch(`https://jsonplaceholder.typicode.com/users/${userId}`)
    .then(response => {
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }
      return response.json();
    })
    .then(user => {
      console.log('사용자 정보:', user.name);
      return fetch(`https://jsonplaceholder.typicode.com/posts?userId=${userId}`);
    })
    .then(response => response.json())
    .then(posts => {
      console.log(`${posts.length}개의 포스트를 찾았습니다.`);
      return posts;
    })
    .catch(error => {
      console.error('오류 발생:', error);
      throw error;
    });
}

// 사용 예시
fetchUserPostsWithThen(1)
  .then(posts => console.log('포스트 목록:', posts))
  .catch(error => console.error('최종 오류:', error));
```

동일한 기능을 async/await로 구현한 더 읽기 쉬운 버전입니다.

```javascript title="async-await.js"
// async/await 방식
async function fetchUserPostsWithAsync(userId) {
  try {
    // 사용자 정보 가져오기
    const userResponse = await fetch(`https://jsonplaceholder.typicode.com/users/${userId}`);
    if (!userResponse.ok) {
      throw new Error(`HTTP error! status: ${userResponse.status}`);
    }

    const user = await userResponse.json();
    console.log('사용자 정보:', user.name);

    // 사용자의 포스트 가져오기
    const postsResponse = await fetch(
      `https://jsonplaceholder.typicode.com/posts?userId=${userId}`
    );
    const posts = await postsResponse.json();

    console.log(`${posts.length}개의 포스트를 찾았습니다.`);
    return posts;
  } catch (error) {
    console.error('오류 발생:', error);
    throw error;
  }
}

// 사용 예시
async function main() {
  try {
    const posts = await fetchUserPostsWithAsync(1);
    console.log('포스트 목록:', posts);
  } catch (error) {
    console.error('최종 오류:', error);
  }
}

main();
```

---

## RESTful API 기초

REST API의 기본 원칙을 이해하고, 각 HTTP 메서드의 용도에 맞게 API를 호출하는 방법을 실습해보겠습니다.

CRUD 작업을 위한 API 클라이언트 클래스를 만들어보겠습니다.

```javascript title="api-client.js"
class ApiClient {
  constructor(baseUrl) {
    this.baseUrl = baseUrl;
  }

  async request(endpoint, options = {}) {
    const url = `${this.baseUrl}${endpoint}`;
    const config = {
      headers: {
        'Content-Type': 'application/json',
        ...options.headers,
      },
      ...options,
    };

    try {
      const response = await fetch(url, config);

      if (!response.ok) {
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      return await response.json();
    } catch (error) {
      console.error(`API 요청 실패 [${options.method || 'GET'}] ${url}:`, error);
      throw error;
    }
  }

  // GET - 데이터 조회
  async get(endpoint) {
    return this.request(endpoint);
  }

  // POST - 데이터 생성
  async post(endpoint, data) {
    return this.request(endpoint, {
      method: 'POST',
      body: JSON.stringify(data),
    });
  }
}

// 사용 예시
const api = new ApiClient('https://jsonplaceholder.typicode.com');

async function demonstrateApi() {
  try {
    // 포스트 목록 조회
    const posts = await api.get('/posts');
    console.log(`${posts.length}개의 포스트를 조회했습니다.`);

    // 새 포스트 생성
    const newPost = await api.post('/posts', {
      title: 'REST API 테스트',
      body: '새로운 포스트입니다.',
      userId: 1,
    });
    console.log('생성된 포스트:', newPost);
  } catch (error) {
    console.error('API 데모 실패:', error);
  }
}

demonstrateApi();
```

에러 처리와 재시도 로직이 포함된 고급 API 클라이언트입니다.

```javascript title="advanced-api-client.js"
class AdvancedApiClient {
  constructor(baseUrl, maxRetries = 3) {
    this.baseUrl = baseUrl;
    this.maxRetries = maxRetries;
  }

  async requestWithRetry(endpoint, options = {}, retryCount = 0) {
    try {
      const response = await fetch(`${this.baseUrl}${endpoint}`, {
        headers: { 'Content-Type': 'application/json' },
        ...options,
      });

      if (!response.ok) {
        if (response.status >= 500 && retryCount < this.maxRetries) {
          console.log(`서버 오류로 재시도 중... (${retryCount + 1}/${this.maxRetries})`);
          await this.delay(1000 * (retryCount + 1)); // 지수 백오프
          return this.requestWithRetry(endpoint, options, retryCount + 1);
        }
        throw new Error(`HTTP ${response.status}: ${response.statusText}`);
      }

      return await response.json();
    } catch (error) {
      if (retryCount < this.maxRetries && error.name === 'TypeError') {
        console.log(`네트워크 오류로 재시도 중... (${retryCount + 1}/${this.maxRetries})`);
        await this.delay(1000 * (retryCount + 1));
        return this.requestWithRetry(endpoint, options, retryCount + 1);
      }
      throw error;
    }
  }

  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }

  async get(endpoint) {
    return this.requestWithRetry(endpoint);
  }
}

// 사용 예시
const advancedApi = new AdvancedApiClient('https://jsonplaceholder.typicode.com');
```

---

## 로컬 스토리지와 세션 스토리지

브라우저의 스토리지를 활용해 사용자 데이터를 보관하고 관리하는 방법을 알아보겠습니다.

사용자 설정을 저장하고 관리하는 실용적인 예제입니다.

```javascript title="storage-manager.js"
class StorageManager {
  constructor(storageType = 'localStorage') {
    this.storage = storageType === 'sessionStorage' ? sessionStorage : localStorage;
  }

  // 설정 저장
  saveSettings(settings) {
    try {
      const existingSettings = this.getSettings();
      const updatedSettings = { ...existingSettings, ...settings };
      this.storage.setItem('userSettings', JSON.stringify(updatedSettings));
      console.log('설정이 저장되었습니다:', updatedSettings);
      return true;
    } catch (error) {
      console.error('설정 저장 실패:', error);
      return false;
    }
  }

  // 설정 불러오기
  getSettings() {
    try {
      const settings = this.storage.getItem('userSettings');
      return settings ? JSON.parse(settings) : this.getDefaultSettings();
    } catch (error) {
      console.error('설정 불러오기 실패:', error);
      return this.getDefaultSettings();
    }
  }

  // 기본 설정
  getDefaultSettings() {
    return {
      theme: 'light',
      language: 'ko',
      notifications: true,
      autoSave: true,
    };
  }

  // 설정 초기화
  resetSettings() {
    this.storage.removeItem('userSettings');
    console.log('설정이 초기화되었습니다.');
  }
}

// 사용 예시
const settingsManager = new StorageManager();

// 테마 변경
settingsManager.saveSettings({ theme: 'dark' });

// 현재 설정 확인
const currentSettings = settingsManager.getSettings();
console.log('현재 설정:', currentSettings);
```

오프라인 상태에서도 작동하는 데이터 캐시 시스템을 구현해보겠습니다.

```javascript title="offline-cache.js"
class OfflineCache {
  constructor(cacheName = 'appData') {
    this.cacheName = cacheName;
    this.maxAge = 24 * 60 * 60 * 1000; // 24시간
  }

  // 데이터 캐시에 저장
  cacheData(key, data) {
    const cacheItem = {
      data: data,
      timestamp: Date.now(),
      version: '1.0',
    };

    try {
      localStorage.setItem(`${this.cacheName}_${key}`, JSON.stringify(cacheItem));
      console.log(`데이터가 캐시되었습니다: ${key}`);
    } catch (error) {
      console.error('캐시 저장 실패:', error);
    }
  }

  // 캐시에서 데이터 가져오기
  getCachedData(key) {
    try {
      const cached = localStorage.getItem(`${this.cacheName}_${key}`);
      if (!cached) return null;

      const cacheItem = JSON.parse(cached);
      const isExpired = Date.now() - cacheItem.timestamp > this.maxAge;

      if (isExpired) {
        this.removeCachedData(key);
        console.log(`만료된 캐시가 삭제되었습니다: ${key}`);
        return null;
      }

      console.log(`캐시에서 데이터를 불러왔습니다: ${key}`);
      return cacheItem.data;
    } catch (error) {
      console.error('캐시 로드 실패:', error);
      return null;
    }
  }

  // 캐시 삭제
  removeCachedData(key) {
    localStorage.removeItem(`${this.cacheName}_${key}`);
  }

  // 모든 캐시 삭제
  clearCache() {
    const keys = Object.keys(localStorage);
    keys.forEach(key => {
      if (key.startsWith(this.cacheName)) {
        localStorage.removeItem(key);
      }
    });
    console.log('모든 캐시가 삭제되었습니다.');
  }
}

// 사용 예시
const cache = new OfflineCache();

// API 응답 캐시
cache.cacheData('userProfile', { name: '김철수', email: 'kim@example.com' });

// 캐시된 데이터 사용
const userProfile = cache.getCachedData('userProfile');
if (userProfile) {
  console.log('캐시된 프로필:', userProfile);
} else {
  console.log('캐시가 없습니다. API 호출이 필요합니다.');
}
```

---

## 최신 웹 API: Intersection Observer

스크롤 이벤트를 효율적으로 처리하고 성능을 개선하는 Intersection Observer API를 활용해보겠습니다.

무한 스크롤 기능을 구현하는 실용적인 예제입니다.

```javascript title="intersection-observer.js"
class InfiniteScroll {
  constructor(containerSelector, loadMoreCallback) {
    this.container = document.querySelector(containerSelector);
    this.loadMoreCallback = loadMoreCallback;
    this.isLoading = false;
    this.hasMore = true;

    this.setupObserver();
    this.createSentinel();
  }

  setupObserver() {
    this.observer = new IntersectionObserver(
      entries => {
        const [entry] = entries;
        if (entry.isIntersecting && !this.isLoading && this.hasMore) {
          this.loadMore();
        }
      },
      {
        rootMargin: '100px', // 100px 전에 미리 로딩
        threshold: 0.1,
      }
    );
  }

  createSentinel() {
    this.sentinel = document.createElement('div');
    this.sentinel.className = 'scroll-sentinel';
    this.sentinel.style.cssText = 'height: 1px; margin: 20px 0;';

    this.container.appendChild(this.sentinel);
    this.observer.observe(this.sentinel);
  }

  async loadMore() {
    if (this.isLoading) return;

    this.isLoading = true;
    this.showLoading();

    try {
      const hasMoreData = await this.loadMoreCallback();
      this.hasMore = hasMoreData !== false;

      if (!this.hasMore) {
        this.showEnd();
      }
    } catch (error) {
      console.error('데이터 로딩 실패:', error);
      this.showError();
    } finally {
      this.isLoading = false;
      this.hideLoading();
    }
  }

  showLoading() {
    this.sentinel.innerHTML = '<div class="loading">로딩 중...</div>';
  }

  hideLoading() {
    this.sentinel.innerHTML = '';
  }

  showEnd() {
    this.sentinel.innerHTML = '<div class="end-message">모든 데이터를 불러왔습니다.</div>';
    this.observer.unobserve(this.sentinel);
  }

  showError() {
    this.sentinel.innerHTML = '<div class="error">데이터 로딩에 실패했습니다.</div>';
  }
}

// 사용 예시
const infiniteScroll = new InfiniteScroll('#content-list', async () => {
  // 실제 API 호출 시뮬레이션
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?_page=${page}&_limit=10`
  );
  const posts = await response.json();

  // DOM에 추가
  posts.forEach(post => {
    const element = document.createElement('div');
    element.innerHTML = `<h3>${post.title}</h3><p>${post.body}</p>`;
    document.querySelector('#content-list').appendChild(element);
  });

  page++;
  return posts.length > 0; // 더 불러올 데이터가 있는지 반환
});
```

---

## 최신 웹 API: Resize Observer

요소의 크기 변화를 감지하고 반응형 레이아웃을 구현하는 Resize Observer API를 활용해보겠습니다.

```javascript title="resize-observer.js"
class ResponsiveManager {
  constructor() {
    this.observers = new Map();
    this.setupResizeObserver();
  }

  setupResizeObserver() {
    this.resizeObserver = new ResizeObserver(entries => {
      entries.forEach(entry => {
        const element = entry.target;
        const callback = this.observers.get(element);

        if (callback) {
          const { width, height } = entry.contentRect;
          callback({ element, width, height });
        }
      });
    });
  }

  // 요소 관찰 시작
  observe(element, callback) {
    this.observers.set(element, callback);
    this.resizeObserver.observe(element);
  }

  // 요소 관찰 중지
  unobserve(element) {
    this.observers.delete(element);
    this.resizeObserver.unobserve(element);
  }

  // 모든 관찰 중지
  disconnect() {
    this.observers.clear();
    this.resizeObserver.disconnect();
  }
}

// 사용 예시
const responsiveManager = new ResponsiveManager();

// 카드 레이아웃 자동 조정
const cardContainer = document.querySelector('.card-container');
responsiveManager.observe(cardContainer, ({ width }) => {
  const cardWidth = 300;
  const gap = 20;
  const columns = Math.floor((width + gap) / (cardWidth + gap));

  cardContainer.style.gridTemplateColumns = `repeat(${columns}, 1fr)`;
  console.log(`컨테이너 너비: ${width}px, 컬럼 수: ${columns}`);
});

// 텍스트 크기 자동 조정
const titleElement = document.querySelector('.responsive-title');
responsiveManager.observe(titleElement, ({ width }) => {
  const fontSize = Math.max(16, Math.min(48, width / 20));
  titleElement.style.fontSize = `${fontSize}px`;
});
```

---

## 실습: 무한 스크롤 구현

지금까지 배운 내용을 종합해 실제 무한 스크롤 기능이 있는 포스트 목록을 구현해보겠습니다.

```javascript title="infinite-scroll-app.js"
class PostListApp {
  constructor() {
    this.currentPage = 1;
    this.isLoading = false;
    this.hasMore = true;
    this.posts = [];

    this.initializeApp();
  }

  initializeApp() {
    this.createUI();
    this.setupInfiniteScroll();
    this.loadInitialPosts();
  }

  createUI() {
    document.body.innerHTML = `
      <div class="max-w-4xl mx-auto p-6">
        <h1 class="text-3xl font-bold mb-6">포스트 목록</h1>
        <div id="posts-container" class="space-y-4">
          <!-- 포스트들이 여기에 추가됩니다 -->
        </div>
        <div id="loading" class="text-center py-4 hidden">
          <div class="text-gray-600">로딩 중...</div>
        </div>
        <div id="error" class="text-center py-4 hidden">
          <div class="text-red-600">오류가 발생했습니다.</div>
          <button id="retry-btn" class="mt-2 px-4 py-2 bg-blue-500 text-white rounded">
            다시 시도
          </button>
        </div>
      </div>
    `;

    // 재시도 버튼 이벤트
    document.getElementById('retry-btn').addEventListener('click', () => {
      this.retryLoading();
    });
  }

  setupInfiniteScroll() {
    this.observer = new IntersectionObserver(
      entries => {
        const [entry] = entries;
        if (entry.isIntersecting && !this.isLoading && this.hasMore) {
          this.loadMorePosts();
        }
      },
      { rootMargin: '100px' }
    );

    // 감시할 요소 생성
    this.createSentinel();
  }

  createSentinel() {
    this.sentinel = document.createElement('div');
    this.sentinel.id = 'scroll-sentinel';
    this.sentinel.style.height = '1px';
    document.getElementById('posts-container').appendChild(this.sentinel);
    this.observer.observe(this.sentinel);
  }
}

// 앱 시작
const app = new PostListApp();
```

포스트 로딩과 렌더링 기능을 추가해보겠습니다.

```javascript title="infinite-scroll-app-2.js"
// PostListApp 클래스에 추가할 메서드들

async loadInitialPosts() {
  await this.loadMorePosts();
}

async loadMorePosts() {
  if (this.isLoading) return;

  this.isLoading = true;
  this.showLoading();
  this.hideError();

  try {
    const posts = await this.fetchPosts(this.currentPage);

    if (posts.length === 0) {
      this.hasMore = false;
      this.showEndMessage();
    } else {
      this.posts.push(...posts);
      this.renderPosts(posts);
      this.currentPage++;
    }
  } catch (error) {
    console.error('포스트 로딩 실패:', error);
    this.showError();
  } finally {
    this.isLoading = false;
    this.hideLoading();
  }
}

async fetchPosts(page) {
  const response = await fetch(
    `https://jsonplaceholder.typicode.com/posts?_page=${page}&_limit=10`
  );

  if (!response.ok) {
    throw new Error(`HTTP error! status: ${response.status}`);
  }

  return await response.json();
}

renderPosts(posts) {
  const container = document.getElementById('posts-container');

  posts.forEach(post => {
    const postElement = this.createPostElement(post);
    container.insertBefore(postElement, this.sentinel);
  });
}

createPostElement(post) {
  const element = document.createElement('article');
  element.className = 'bg-white p-6 rounded-lg shadow-md border';
  element.innerHTML = `
    <h2 class="text-xl font-semibold mb-2">${post.title}</h2>
    <p class="text-gray-700">${post.body}</p>
    <div class="mt-4 text-sm text-gray-500">포스트 #${post.id}</div>
  `;
  return element;
}

showLoading() {
  document.getElementById('loading').classList.remove('hidden');
}

hideLoading() {
  document.getElementById('loading').classList.add('hidden');
}

showError() {
  document.getElementById('error').classList.remove('hidden');
}

hideError() {
  document.getElementById('error').classList.add('hidden');
}

showEndMessage() {
  this.sentinel.innerHTML = '<div class="text-center py-4 text-gray-500">모든 포스트를 불러왔습니다.</div>';
}

retryLoading() {
  this.hideError();
  this.loadMorePosts();
}
```

---

## 실습: 오프라인 지원 메모 앱

오프라인에서도 작동하는 간단한 메모 애플리케이션을 만들어보겠습니다.

```javascript title="offline-memo-app.js"
class OfflineMemoApp {
  constructor() {
    this.storageKey = 'memoApp_notes';
    this.notes = this.loadNotes();
    this.nextId = this.getNextId();

    this.initializeApp();
  }

  initializeApp() {
    this.createUI();
    this.bindEvents();
    this.renderNotes();
    this.checkOnlineStatus();
  }

  createUI() {
    document.body.innerHTML = `
      <div class="max-w-2xl mx-auto p-6">
        <div class="mb-6">
          <h1 class="text-3xl font-bold mb-2">오프라인 메모</h1>
          <div id="online-status" class="text-sm"></div>
        </div>
        
        <div class="mb-6">
          <textarea 
            id="memo-input" 
            placeholder="새 메모를 입력하세요..."
            class="w-full p-3 border rounded-lg resize-none h-24"
          ></textarea>
          <button 
            id="add-memo-btn"
            class="mt-2 px-4 py-2 bg-blue-500 text-white rounded hover:bg-blue-600"
          >
            메모 추가
          </button>
        </div>
        
        <div id="memo-list" class="space-y-3">
          <!-- 메모들이 여기에 표시됩니다 -->
        </div>
      </div>
    `;
  }

  bindEvents() {
    // 메모 추가 버튼
    document.getElementById('add-memo-btn').addEventListener('click', () => {
      this.addMemo();
    });

    // 엔터 키로 메모 추가 (Ctrl + Enter)
    document.getElementById('memo-input').addEventListener('keydown', e => {
      if (e.ctrlKey && e.key === 'Enter') {
        this.addMemo();
      }
    });

    // 온라인/오프라인 상태 감지
    window.addEventListener('online', () => this.checkOnlineStatus());
    window.addEventListener('offline', () => this.checkOnlineStatus());
  }

  addMemo() {
    const input = document.getElementById('memo-input');
    const content = input.value.trim();

    if (!content) return;

    const memo = {
      id: this.nextId++,
      content,
      createdAt: new Date().toISOString(),
      updatedAt: new Date().toISOString(),
    };

    this.notes.unshift(memo);
    this.saveNotes();
    this.renderNotes();

    input.value = '';
    input.focus();
  }
}

// 앱 시작
const memoApp = new OfflineMemoApp();
```

메모 렌더링과 삭제 기능을 추가해보겠습니다.

```javascript title="offline-memo-app-2.js"
// OfflineMemoApp 클래스에 추가할 메서드들

renderNotes() {
  const container = document.getElementById('memo-list');

  if (this.notes.length === 0) {
    container.innerHTML = `
      <div class="text-center text-gray-500 py-8">
        <p>아직 메모가 없습니다.</p>
        <p class="text-sm">첫 번째 메모를 작성해보세요!</p>
      </div>
    `;
    return;
  }

  container.innerHTML = this.notes.map(note => `
    <div class="bg-white border rounded-lg p-4 shadow-sm">
      <div class="mb-2">
        <p class="text-gray-800">${this.escapeHtml(note.content)}</p>
      </div>
      <div class="flex justify-between items-center text-sm text-gray-500">
        <span>${this.formatDate(note.createdAt)}</span>
        <button
          onclick="memoApp.deleteMemo(${note.id})"
          class="text-red-500 hover:text-red-700"
        >
          삭제
        </button>
      </div>
    </div>
  `).join('');
}

deleteMemo(id) {
  if (confirm('이 메모를 삭제하시겠습니까?')) {
    this.notes = this.notes.filter(note => note.id !== id);
    this.saveNotes();
    this.renderNotes();
  }
}

loadNotes() {
  try {
    const stored = localStorage.getItem(this.storageKey);
    return stored ? JSON.parse(stored) : [];
  } catch (error) {
    console.error('메모 로드 실패:', error);
    return [];
  }
}

saveNotes() {
  try {
    localStorage.setItem(this.storageKey, JSON.stringify(this.notes));
  } catch (error) {
    console.error('메모 저장 실패:', error);
    alert('메모 저장에 실패했습니다.');
  }
}

getNextId() {
  return this.notes.length > 0 ? Math.max(...this.notes.map(n => n.id)) + 1 : 1;
}

checkOnlineStatus() {
  const statusElement = document.getElementById('online-status');
  const isOnline = navigator.onLine;

  statusElement.innerHTML = isOnline
    ? '<span class="text-green-600">온라인</span>'
    : '<span class="text-red-600">오프라인 (로컬 저장만 가능)</span>';
}

formatDate(isoString) {
  const date = new Date(isoString);
  return date.toLocaleString('ko-KR');
}

escapeHtml(text) {
  const div = document.createElement('div');
  div.textContent = text;
  return div.innerHTML;
}
```

이 장에서는 모던 웹 개발의 핵심인 데이터 통신과 브라우저 API 활용법을 배웠습니다. Fetch API로 서버와 통신하고, JSON 데이터를 안전하게 처리하며, async/await로 비동기 코드를 깔끔하게 작성하는 방법을 익혔습니다. 또한 RESTful API 원칙과 브라우저 스토리지 활용법, 최신 웹 API들까지 실습을 통해 경험해보았습니다. 이제 실제 웹 애플리케이션에서 데이터를 효율적으로 다루고 사용자에게 훌륭한 경험을 제공할 수 있는 기반을 갖추었습니다.
