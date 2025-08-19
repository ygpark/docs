---
title: '10장: 비동기 프로그래밍'
slug: javascript-bible-async-programming
description: '콜백부터 Promise, async/await까지 자바스크립트의 비동기 처리 방법을 초급자도 쉽게 이해할 수 있도록 설명합니다.'
keywords:
  [
    '자바스크립트',
    '비동기',
    'Promise',
    'async',
    'await',
    '콜백',
    '자바스크립트 비동기',
    '모던 자바스크립트',
  ]
sidebar_position: 10
---

# 10장: 비동기 프로그래밍

## 학습 목표

이 장에서는 자바스크립트의 비동기 프로그래밍을 마스터하게 됩니다. 콜백의 한계를 이해하고, Promise와 async/await를 활용하여 깔끔하고 읽기 쉬운 비동기 코드를 작성하는 방법을 배웁니다. 또한 여러 비동기 작업을 효율적으로 관리하는 최신 기법들도 함께 다룹니다.

자바스크립트는 기본적으로 **단일 스레드**로 동작하지만, 비동기 작업을 통해 여러 일을 동시에 처리할 수 있습니다. 웹 개발에서 서버 통신, 파일 로딩, 타이머 등은 모두 비동기로 처리되기 때문에, 이를 잘 다루는 것은 필수적입니다. 처음에는 복잡해 보일 수 있지만, 단계별로 차근차근 배워나가면 누구나 마스터할 수 있습니다.

---

## 콜백과 콜백 지옥

비동기 프로그래밍의 시작점은 **콜백 함수**입니다. 콜백은 다른 함수의 실행이 끝난 후 호출되는 함수로, 가장 기본적인 비동기 처리 방법입니다. 하지만 콜백이 중첩되면서 생기는 '콜백 지옥'은 코드를 읽기 어렵게 만듭니다.

### 기본 콜백 사용법

```javascript
// 기본적인 콜백 함수 예제
function fetchUserData(userId, callback) {
  // 실제로는 서버 통신을 시뮬레이션
  setTimeout(() => {
    const userData = {
      id: userId,
      name: '김철수',
      email: 'kim@example.com',
    };
    callback(userData);
  }, 1000);
}

// 콜백 함수 사용
fetchUserData(123, function (user) {
  console.log('사용자 정보:', user);
});

console.log('이 메시지가 먼저 출력됩니다!');
```

위 예제는 간단한 비동기 데이터 로딩을 보여줍니다. `setTimeout`으로 네트워크 요청을 시뮬레이션하고, 데이터가 준비되면 콜백 함수를 호출합니다.

### 콜백 지옥의 문제

```javascript
// 콜백 지옥 예제 - 피해야 할 패턴
function getUser(userId, callback) {
  setTimeout(() => {
    callback({ id: userId, name: '김철수' });
  }, 500);
}

function getPosts(userId, callback) {
  setTimeout(() => {
    callback([{ title: '첫 번째 글' }, { title: '두 번째 글' }]);
  }, 500);
}

function getComments(postId, callback) {
  setTimeout(() => {
    callback([{ text: '좋은 글이네요!' }, { text: '감사합니다!' }]);
  }, 500);
}

// 콜백이 계속 중첩되는 구조
getUser(1, function (user) {
  console.log('사용자:', user.name);

  getPosts(user.id, function (posts) {
    console.log('글 목록:', posts);

    getComments(posts[0].id, function (comments) {
      console.log('댓글:', comments);
      // 더 깊어질 수 있음... 😰
    });
  });
});
```

이런 패턴은 코드를 읽기 어렵게 만들고, 에러 처리도 복잡해집니다. 이를 해결하기 위해 Promise가 등장했습니다.

---

## Promise의 이해와 활용

**Promise**는 비동기 작업의 결과를 나타내는 객체입니다. 작업이 성공할지 실패할지 아직 모르지만, 언젠가 결과를 받을 것이라는 '약속'을 의미합니다. Promise는 콜백 지옥을 해결하고 더 깔끔한 코드를 작성할 수 있게 해줍니다.

### Promise 기본 사용법

```javascript
// Promise를 반환하는 함수
function fetchUserData(userId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (userId > 0) {
        const userData = {
          id: userId,
          name: '이영희',
          email: 'lee@example.com',
        };
        resolve(userData); // 성공 시 resolve 호출
      } else {
        reject(new Error('유효하지 않은 사용자 ID')); // 실패 시 reject 호출
      }
    }, 1000);
  });
}

// Promise 사용하기
fetchUserData(123)
  .then(user => {
    console.log('사용자 정보 로딩 완료:', user);
    return user.id; // 다음 then으로 전달
  })
  .then(userId => {
    console.log('사용자 ID:', userId);
  })
  .catch(error => {
    console.error('에러 발생:', error.message);
  })
  .finally(() => {
    console.log('작업 완료 (성공/실패 관계없이 실행)');
  });
```

### Promise 체이닝으로 콜백 지옥 해결

```javascript
// Promise로 개선한 버전
function getUser(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve({ id: userId, name: '박민수' });
    }, 500);
  });
}

function getPosts(userId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve([
        { id: 1, title: 'Promise 활용법' },
        { id: 2, title: 'async/await 가이드' },
      ]);
    }, 500);
  });
}

function getComments(postId) {
  return new Promise(resolve => {
    setTimeout(() => {
      resolve([{ text: '유용한 정보입니다!' }, { text: '잘 읽었어요!' }]);
    }, 500);
  });
}

// 깔끔한 체이닝
getUser(1)
  .then(user => {
    console.log('사용자:', user.name);
    return getPosts(user.id);
  })
  .then(posts => {
    console.log('글 목록:', posts);
    return getComments(posts[0].id);
  })
  .then(comments => {
    console.log('댓글:', comments);
  })
  .catch(error => {
    console.error('어딘가에서 에러 발생:', error);
  });
```

---

## async/await 문법

**async/await**는 Promise를 더욱 간단하고 읽기 쉽게 사용할 수 있게 해주는 ES2017의 문법입니다. 비동기 코드를 마치 동기 코드처럼 작성할 수 있어, 직관적이고 이해하기 쉽습니다.

### async/await 기본 사용법

```javascript
// async 함수 선언
async function loadUserData(userId) {
  try {
    // await로 Promise 결과를 기다림
    const user = await getUser(userId);
    console.log('사용자:', user.name);

    const posts = await getPosts(user.id);
    console.log('글 목록:', posts);

    const comments = await getComments(posts[0].id);
    console.log('댓글:', comments);

    return { user, posts, comments }; // 자동으로 Promise로 감싸짐
  } catch (error) {
    console.error('데이터 로딩 중 에러:', error);
    throw error; // 에러를 다시 던짐
  }
}

// async 함수 호출
loadUserData(1)
  .then(result => {
    console.log('모든 데이터 로딩 완료:', result);
  })
  .catch(error => {
    console.error('최종 에러 처리:', error);
  });
```

### 실용적인 API 호출 예제

```javascript
// 실제 API 호출을 시뮬레이션하는 함수
async function fetchWeatherData(city) {
  try {
    console.log(`${city}의 날씨 정보를 가져오는 중...`);

    // 실제로는 fetch() API를 사용
    const response = await new Promise(resolve => {
      setTimeout(() => {
        resolve({
          city: city,
          temperature: Math.floor(Math.random() * 30) + 5,
          condition: ['맑음', '흐림', '비', '눈'][Math.floor(Math.random() * 4)],
        });
      }, 1500);
    });

    return response;
  } catch (error) {
    console.error('날씨 데이터 조회 실패:', error);
    return null;
  }
}

// 여러 도시의 날씨를 순차적으로 조회
async function getMultipleCityWeather() {
  const cities = ['서울', '부산', '대구'];
  const weatherData = [];

  for (const city of cities) {
    const weather = await fetchWeatherData(city);
    if (weather) {
      weatherData.push(weather);
      console.log(`${weather.city}: ${weather.temperature}°C, ${weather.condition}`);
    }
  }

  return weatherData;
}

getMultipleCityWeather();
```

이 예제는 실제 웹 개발에서 자주 사용하는 패턴입니다. API 호출 중 로딩 메시지를 표시하고, 에러가 발생해도 프로그램이 중단되지 않도록 처리합니다.

---

## 에러 처리와 Promise 메서드들

비동기 작업에서는 다양한 에러가 발생할 수 있습니다. 네트워크 문제, 서버 오류, 잘못된 데이터 등을 적절히 처리하는 것이 중요합니다. 또한 여러 Promise를 효율적으로 관리하는 메서드들도 알아보겠습니다.

### Promise.all - 모든 작업이 완료될 때까지 대기

```javascript
// 여러 API를 동시에 호출하여 성능 향상
async function loadDashboardData() {
  try {
    console.log('대시보드 데이터 로딩 시작...');

    // 여러 작업을 병렬로 실행
    const [userInfo, recentPosts, notifications] = await Promise.all([
      fetchUserData(1),
      getPosts(1),
      getNotifications(1),
    ]);

    console.log('모든 데이터 로딩 완료!');
    return { userInfo, recentPosts, notifications };
  } catch (error) {
    console.error('하나라도 실패하면 전체 실패:', error);
    throw error;
  }
}

// 알림 데이터를 가져오는 함수 (예제용)
function getNotifications(userId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      resolve([{ message: '새 댓글이 달렸습니다.' }, { message: '프로필이 업데이트되었습니다.' }]);
    }, 800);
  });
}
```

### Promise.allSettled - 모든 결과를 받기 (실패해도 계속)

```javascript
// 일부 실패해도 가능한 데이터는 모두 수집
async function loadOptionalData() {
  const dataPromises = [
    fetchUserData(1),
    fetchUserData(-1), // 이것은 실패할 것
    getPosts(1),
    getNotifications(1),
  ];

  const results = await Promise.allSettled(dataPromises);

  results.forEach((result, index) => {
    if (result.status === 'fulfilled') {
      console.log(`작업 ${index + 1} 성공:`, result.value);
    } else {
      console.log(`작업 ${index + 1} 실패:`, result.reason.message);
    }
  });

  // 성공한 결과만 필터링
  const successfulResults = results
    .filter(result => result.status === 'fulfilled')
    .map(result => result.value);

  return successfulResults;
}

loadOptionalData();
```

### Promise.race와 타임아웃 구현

```javascript
// 타임아웃 기능이 있는 데이터 로딩
function createTimeoutPromise(ms) {
  return new Promise((_, reject) => {
    setTimeout(() => {
      reject(new Error(`${ms}ms 타임아웃`));
    }, ms);
  });
}

async function fetchWithTimeout(userId, timeoutMs = 3000) {
  try {
    // 데이터 로딩과 타임아웃 중 먼저 완료되는 것을 선택
    const result = await Promise.race([fetchUserData(userId), createTimeoutPromise(timeoutMs)]);

    console.log('데이터 로딩 성공:', result);
    return result;
  } catch (error) {
    if (error.message.includes('타임아웃')) {
      console.error('요청 시간이 너무 오래 걸립니다.');
    } else {
      console.error('데이터 로딩 실패:', error.message);
    }
    throw error;
  }
}

fetchWithTimeout(1, 2000); // 2초 타임아웃
```

---

## 최신 Promise 메서드들

ES2021에서 추가된 새로운 Promise 메서드들을 살펴보겠습니다. 이들은 더 세밀한 비동기 작업 제어를 가능하게 합니다.

### Promise.any - 첫 번째 성공 결과 사용

```javascript
// 여러 서버 중 가장 빠른 응답을 사용
async function fetchFromFastestServer(data) {
  const servers = ['https://api-server1.com', 'https://api-server2.com', 'https://api-server3.com'];

  // 실제 fetch 대신 시뮬레이션
  const requests = servers.map(server => simulateServerRequest(server, data));

  try {
    // 가장 먼저 성공하는 요청의 결과를 반환
    const fastest = await Promise.any(requests);
    console.log('가장 빠른 서버 응답:', fastest);
    return fastest;
  } catch (error) {
    console.error('모든 서버가 실패했습니다:', error);
    throw new Error('서버 연결 불가');
  }
}

// 서버 요청 시뮬레이션
function simulateServerRequest(serverUrl, data) {
  return new Promise((resolve, reject) => {
    const delay = Math.random() * 2000 + 500; // 0.5~2.5초 랜덤 지연
    const shouldFail = Math.random() < 0.3; // 30% 확률로 실패

    setTimeout(() => {
      if (shouldFail) {
        reject(new Error(`${serverUrl} 서버 오류`));
      } else {
        resolve({
          server: serverUrl,
          data: `${data} 처리 완료`,
          timestamp: new Date().toISOString(),
        });
      }
    }, delay);
  });
}

fetchFromFastestServer('사용자 정보 조회');
```

---

## 실습: 순차/병렬 API 호출 시스템

실제 웹 애플리케이션에서 자주 사용되는 패턴을 구현해보겠습니다. 사용자 프로필 페이지에 필요한 다양한 데이터를 효율적으로 로딩하는 시스템입니다.

```javascript
// 실습: 사용자 프로필 데이터 로딩 시스템
class ProfileDataLoader {
  constructor() {
    this.cache = new Map(); // 간단한 캐시 시스템
  }

  // 캐시된 데이터가 있으면 사용, 없으면 새로 로딩
  async getCachedData(key, loadFunction) {
    if (this.cache.has(key)) {
      console.log(`캐시에서 ${key} 데이터 반환`);
      return this.cache.get(key);
    }

    const data = await loadFunction();
    this.cache.set(key, data);
    return data;
  }

  // 기본 사용자 정보 (필수 - 먼저 로딩)
  async loadBasicUserInfo(userId) {
    return this.getCachedData(`user-${userId}`, async () => {
      console.log('기본 사용자 정보 로딩 중...');
      await this.delay(800);
      return {
        id: userId,
        name: '김개발자',
        email: 'dev@example.com',
        avatar: 'https://example.com/avatar.jpg',
      };
    });
  }

  // 추가 데이터들 (병렬 로딩 가능)
  async loadUserPosts(userId) {
    return this.getCachedData(`posts-${userId}`, async () => {
      console.log('사용자 게시글 로딩 중...');
      await this.delay(1200);
      return [
        { id: 1, title: 'React 훅 완전 정복', likes: 45 },
        { id: 2, title: 'TypeScript 타입 가드', likes: 32 },
      ];
    });
  }

  async loadUserFollowers(userId) {
    return this.getCachedData(`followers-${userId}`, async () => {
      console.log('팔로워 정보 로딩 중...');
      await this.delay(600);
      return { count: 256, recent: ['user1', 'user2', 'user3'] };
    });
  }

  async loadUserStats(userId) {
    return this.getCachedData(`stats-${userId}`, async () => {
      console.log('사용자 통계 로딩 중...');
      await this.delay(900);
      return {
        totalPosts: 24,
        totalLikes: 892,
        joinDate: '2023-01-15',
      };
    });
  }

  // 메인 로딩 함수 - 순차와 병렬을 적절히 조합
  async loadFullProfile(userId) {
    const startTime = Date.now();

    try {
      // 1단계: 기본 정보 먼저 로딩 (필수)
      console.log('=== 프로필 로딩 시작 ===');
      const basicInfo = await this.loadBasicUserInfo(userId);
      console.log('기본 정보 완료:', basicInfo.name);

      // 2단계: 나머지 데이터를 병렬로 로딩
      console.log('추가 데이터 병렬 로딩 시작...');
      const [posts, followers, stats] = await Promise.allSettled([
        this.loadUserPosts(userId),
        this.loadUserFollowers(userId),
        this.loadUserStats(userId),
      ]);

      // 3단계: 결과 조합
      const profile = {
        basic: basicInfo,
        posts: posts.status === 'fulfilled' ? posts.value : [],
        followers: followers.status === 'fulfilled' ? followers.value : { count: 0 },
        stats: stats.status === 'fulfilled' ? stats.value : {},
      };

      const loadTime = Date.now() - startTime;
      console.log(`=== 프로필 로딩 완료 (${loadTime}ms) ===`);

      return profile;
    } catch (error) {
      console.error('프로필 로딩 실패:', error);
      throw error;
    }
  }

  // 유틸리티 함수
  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}

// 사용 예제
async function demonstrateProfileLoading() {
  const loader = new ProfileDataLoader();

  try {
    const profile = await loader.loadFullProfile(123);

    console.log('\n=== 최종 프로필 데이터 ===');
    console.log('사용자:', profile.basic.name);
    console.log('게시글 수:', profile.posts.length);
    console.log('팔로워 수:', profile.followers.count);
    console.log('총 좋아요:', profile.stats.totalLikes);
  } catch (error) {
    console.error('프로필 로딩 중 오류:', error);
  }
}

demonstrateProfileLoading();
```

---

## 실습: 타임아웃이 있는 데이터 로더

네트워크가 불안정한 환경에서도 안정적으로 동작하는 데이터 로더를 만들어보겠습니다. 재시도 기능과 점진적 타임아웃을 포함합니다.

```javascript
// 실습: 고급 데이터 로더 (재시도 + 타임아웃 + 회로 차단기)
class RobustDataLoader {
  constructor(options = {}) {
    this.maxRetries = options.maxRetries || 3;
    this.baseTimeout = options.baseTimeout || 2000;
    this.retryDelay = options.retryDelay || 1000;
    this.circuitBreakerThreshold = options.circuitBreakerThreshold || 5;
    this.failureCount = 0;
    this.isCircuitOpen = false;
    this.lastFailureTime = null;
  }

  // 회로 차단기 패턴 구현
  checkCircuitBreaker() {
    if (this.isCircuitOpen) {
      const timeSinceLastFailure = Date.now() - this.lastFailureTime;
      if (timeSinceLastFailure > 30000) {
        // 30초 후 재시도 허용
        console.log('회로 차단기 재시도 허용');
        this.isCircuitOpen = false;
        this.failureCount = 0;
      } else {
        throw new Error('회로 차단기 열림 - 서비스 일시 중단');
      }
    }
  }

  // 재시도가 있는 안전한 로딩
  async loadWithRetry(loadFunction, description = '데이터') {
    this.checkCircuitBreaker();

    for (let attempt = 1; attempt <= this.maxRetries; attempt++) {
      try {
        console.log(`${description} 로딩 시도 ${attempt}/${this.maxRetries}`);

        // 시도할 때마다 타임아웃 증가
        const timeout = this.baseTimeout * attempt;
        const result = await this.withTimeout(loadFunction(), timeout);

        // 성공 시 실패 카운트 리셋
        this.failureCount = 0;
        console.log(`${description} 로딩 성공!`);
        return result;
      } catch (error) {
        console.log(`시도 ${attempt} 실패:`, error.message);

        if (attempt === this.maxRetries) {
          this.handleFailure();
          throw new Error(`${description} 로딩 최종 실패: ${error.message}`);
        }

        // 재시도 전 대기
        if (attempt < this.maxRetries) {
          console.log(`${this.retryDelay * attempt}ms 후 재시도...`);
          await this.delay(this.retryDelay * attempt);
        }
      }
    }
  }

  // 타임아웃 래퍼
  withTimeout(promise, timeoutMs) {
    return Promise.race([
      promise,
      new Promise((_, reject) => {
        setTimeout(() => {
          reject(new Error(`${timeoutMs}ms 타임아웃`));
        }, timeoutMs);
      }),
    ]);
  }

  // 실패 처리
  handleFailure() {
    this.failureCount++;
    this.lastFailureTime = Date.now();

    if (this.failureCount >= this.circuitBreakerThreshold) {
      console.log('회로 차단기 활성화 - 서비스 일시 중단');
      this.isCircuitOpen = true;
    }
  }

  // 불안정한 네트워크 시뮬레이션
  simulateUnstableAPI(data, failureRate = 0.3) {
    return new Promise((resolve, reject) => {
      const delay = Math.random() * 3000 + 500; // 0.5~3.5초
      const shouldFail = Math.random() < failureRate;

      setTimeout(() => {
        if (shouldFail) {
          reject(new Error('네트워크 연결 불안정'));
        } else {
          resolve({
            data: data,
            timestamp: new Date().toISOString(),
            processingTime: delay,
          });
        }
      }, delay);
    });
  }

  // 공개 메서드들
  async loadUserProfile(userId) {
    return this.loadWithRetry(
      () => this.simulateUnstableAPI(`사용자 ${userId} 프로필`),
      '사용자 프로필'
    );
  }

  async loadUserSettings(userId) {
    return this.loadWithRetry(
      () => this.simulateUnstableAPI(`사용자 ${userId} 설정`, 0.4), // 40% 실패율
      '사용자 설정'
    );
  }

  delay(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
  }
}

// 사용 예제
async function demonstrateRobustLoading() {
  const loader = new RobustDataLoader({
    maxRetries: 3,
    baseTimeout: 1500,
    retryDelay: 800,
  });

  try {
    console.log('=== 안정적인 데이터 로딩 테스트 ===\n');

    // 여러 데이터를 순차적으로 로딩
    const profile = await loader.loadUserProfile(456);
    console.log('프로필 로딩 완료:', profile.data);

    const settings = await loader.loadUserSettings(456);
    console.log('설정 로딩 완료:', settings.data);

    console.log('\n모든 데이터 로딩 성공!');
  } catch (error) {
    console.error('\n최종 오류:', error.message);
  }
}

demonstrateRobustLoading();
```

## 마무리

비동기 프로그래밍은 현대 웹 개발의 핵심입니다. 이 장에서 배운 내용을 정리하면:

**핵심 개념들:**

- 콜백의 한계와 Promise의 등장 배경
- async/await를 통한 직관적인 비동기 코드 작성
- Promise.all, Promise.allSettled 등을 활용한 효율적인 작업 관리
- 에러 처리와 타임아웃 구현

**실전 적용:**

- 순차 로딩과 병렬 로딩의 적절한 조합
- 재시도 로직과 회로 차단기 패턴
- 사용자 경험을 고려한 로딩 상태 관리

다음 장에서는 ES6 모듈 시스템을 배우며, 코드를 더 체계적으로 구조화하는 방법을 알아보겠습니다. 비동기 프로그래밍과 모듈 시스템을 함께 활용하면 대규모 애플리케이션도 효율적으로 개발할 수 있습니다!
