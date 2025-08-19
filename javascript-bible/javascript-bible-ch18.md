---
title: '18장: 프로젝트 2 - 날씨 앱'
slug: javascript-bible-weather-app-project
description: 'API 연동과 비동기 처리를 활용하여 실용적인 날씨 앱을 만들어보세요. OpenWeatherMap API를 사용하고 ES6+ 문법으로 깔끔한 코드를 작성합니다.'
keywords:
  [
    'JavaScript 날씨 앱',
    'API 연동',
    '비동기 처리',
    'fetch API',
    'OpenWeatherMap',
    'async await',
    'DOM 조작',
    'JavaScript 프로젝트',
    'ES6 문법',
    '에러 처리',
  ]
sidebar_position: 18
---

# 18장: 프로젝트 2 - 날씨 앱

이번 장에서는 실제 날씨 API를 연동하여 동작하는 날씨 앱을 만들어보겠습니다. OpenWeatherMap API를 사용해 현재 날씨 정보를 가져오고, 사용자 친화적인 인터페이스로 표시하는 완전한 웹 애플리케이션을 구축합니다. 이 프로젝트를 통해 API 연동, 비동기 처리, 에러 핸들링, 그리고 모던 자바스크립트 문법을 실전에서 활용하는 방법을 익힐 수 있습니다.

---

## 18.1 프로젝트 개요와 설계

날씨 앱 프로젝트의 전체적인 구조와 기능을 설계해보겠습니다. 사용자가 도시명을 입력하면 해당 지역의 현재 날씨 정보를 보여주는 간단하면서도 실용적인 앱을 만들 예정입니다.

### 주요 기능 정의

우리가 구현할 날씨 앱의 핵심 기능들을 먼저 정의해보겠습니다. 명확한 기능 정의는 개발 과정에서 방향성을 잃지 않게 도와줍니다.

```javascript title="기능 요구사항 정의"
// 날씨 앱 주요 기능 목록
const FEATURES = {
  // 기본 기능
  SEARCH_CITY: '도시명으로 날씨 검색',
  DISPLAY_CURRENT: '현재 날씨 정보 표시',
  SHOW_DETAILS: '상세 정보 (습도, 바람 등)',

  // 사용자 경험 개선
  LOADING_STATE: '로딩 상태 표시',
  ERROR_HANDLING: '에러 메시지 처리',
  RESPONSIVE_UI: '반응형 디자인',

  // 추가 기능 (선택사항)
  GEOLOCATION: '현재 위치 기반 날씨',
  FAVORITES: '즐겨찾는 도시 저장',
  UNITS_TOGGLE: '온도 단위 변경',
};

// API 응답 데이터 구조 예시
const SAMPLE_WEATHER_DATA = {
  name: 'Seoul',
  main: {
    temp: 15.5,
    feels_like: 14.2,
    humidity: 65,
    pressure: 1013,
  },
  weather: [
    {
      main: 'Clouds',
      description: 'few clouds',
      icon: '02d',
    },
  ],
  wind: {
    speed: 3.2,
    deg: 180,
  },
};
```

### 프로젝트 구조 설계

효율적인 개발을 위해 파일 구조와 모듈 분리 방법을 계획해보겠습니다.

```javascript title="프로젝트 구조"
// 파일 구조 계획
const PROJECT_STRUCTURE = {
  'index.html': 'HTML 마크업',
  'styles.css': 'TailwindCSS 스타일링',
  'scripts/': {
    'app.js': '메인 애플리케이션 로직',
    'api.js': 'API 호출 관련 함수',
    'ui.js': 'UI 조작 함수',
    'utils.js': '유틸리티 함수',
    'config.js': '설정 상수',
  },
};

// 모듈별 책임 분리
const MODULE_RESPONSIBILITIES = {
  API_MODULE: ['날씨 데이터 API 호출', '에러 응답 처리', '데이터 변환'],
  UI_MODULE: ['DOM 요소 조작', '사용자 인터랙션 처리', '화면 업데이트'],
  UTILS_MODULE: ['온도 단위 변환', '날짜 포매팅', '입력값 검증'],
};
```

---

## 18.2 OpenWeatherMap API 설정

실제 날씨 데이터를 가져오기 위해 OpenWeatherMap API를 설정하고 사용법을 익혀보겠습니다.

### API 키 발급과 기본 설정

OpenWeatherMap에서 API 키를 발급받고 기본적인 API 호출 구조를 만들어보겠습니다.

```javascript title="config.js"
// API 설정 상수
const API_CONFIG = {
  BASE_URL: 'https://api.openweathermap.org/data/2.5',
  API_KEY: 'YOUR_API_KEY_HERE', // 실제 키로 교체 필요
  DEFAULT_UNITS: 'metric', // metric, imperial, kelvin
  DEFAULT_LANG: 'kr',
};

// API 엔드포인트 정의
const API_ENDPOINTS = {
  CURRENT_WEATHER: '/weather',
  FORECAST: '/forecast',
  GEOCODING: '/geo/1.0/direct',
};

// 요청 옵션 기본값
const DEFAULT_REQUEST_OPTIONS = {
  method: 'GET',
  headers: {
    'Content-Type': 'application/json',
  },
};

// 지원하는 온도 단위
const TEMPERATURE_UNITS = {
  CELSIUS: { key: 'metric', symbol: '°C' },
  FAHRENHEIT: { key: 'imperial', symbol: '°F' },
  KELVIN: { key: 'kelvin', symbol: 'K' },
};
```

### API 호출 함수 구현

실제 API를 호출하고 응답을 처리하는 함수들을 만들어보겠습니다.

```javascript title="api.js"
// 기본 API 호출 함수
async function fetchWeatherData(city) {
  const url = buildWeatherURL(city);

  try {
    const response = await fetch(url);

    if (!response.ok) {
      throw new Error(`HTTP error! status: ${response.status}`);
    }

    const data = await response.json();
    return transformWeatherData(data);
  } catch (error) {
    console.error('Weather API Error:', error);
    throw error;
  }
}

// URL 빌더 함수
function buildWeatherURL(city) {
  const params = new URLSearchParams({
    q: city,
    appid: API_CONFIG.API_KEY,
    units: API_CONFIG.DEFAULT_UNITS,
    lang: API_CONFIG.DEFAULT_LANG,
  });

  return `${API_CONFIG.BASE_URL}${API_ENDPOINTS.CURRENT_WEATHER}?${params}`;
}

// 데이터 변환 함수
function transformWeatherData(rawData) {
  return {
    city: rawData.name,
    country: rawData.sys.country,
    temperature: Math.round(rawData.main.temp),
    description: rawData.weather[0].description,
    icon: rawData.weather[0].icon,
    humidity: rawData.main.humidity,
    windSpeed: rawData.wind.speed,
    pressure: rawData.main.pressure,
  };
}
```

---

## 18.3 HTML 구조와 TailwindCSS 스타일링

사용자 인터페이스의 HTML 구조를 만들고 TailwindCSS로 스타일링해보겠습니다.

### 기본 HTML 마크업

날씨 앱의 전체적인 레이아웃과 주요 UI 요소들을 구성해보겠습니다.

```html title="index.html"
<!DOCTYPE html>
<html lang="ko">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>날씨 앱</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body class="bg-gradient-to-br from-blue-400 via-blue-500 to-blue-600 min-h-screen">
    <div class="container mx-auto px-4 py-8">
      <!-- 헤더 -->
      <header class="text-center mb-8">
        <h1 class="text-4xl font-bold text-white mb-2">날씨 앱</h1>
        <p class="text-blue-100">전 세계 날씨를 확인하세요</p>
      </header>

      <!-- 검색 영역 -->
      <div class="max-w-md mx-auto mb-8">
        <div class="bg-white rounded-lg shadow-lg p-6">
          <form id="search-form" class="space-y-4">
            <input
              type="text"
              id="city-input"
              placeholder="도시명을 입력하세요"
              class="w-full px-4 py-2 border rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500"
              required
            />
            <button
              type="submit"
              class="w-full bg-blue-500 text-white py-2 rounded-lg hover:bg-blue-600 transition-colors"
            >
              날씨 검색
            </button>
          </form>
        </div>
      </div>

      <!-- 날씨 정보 표시 영역 -->
      <div id="weather-display" class="max-w-lg mx-auto">
        <!-- 동적으로 생성될 내용 -->
      </div>
    </div>
  </body>
</html>
```

### 동적 UI 컴포넌트 생성

날씨 정보를 표시하기 위한 동적 UI 컴포넌트들을 만들어보겠습니다.

```javascript title="ui.js"
// 날씨 카드 생성 함수
function createWeatherCard(weatherData) {
  return `
    <div class="bg-white rounded-lg shadow-lg p-6 text-center">
      <div class="mb-4">
        <h2 class="text-2xl font-bold text-gray-800">${weatherData.city}</h2>
        <p class="text-gray-600">${weatherData.country}</p>
      </div>
      
      <div class="mb-6">
        <img 
          src="https://openweathermap.org/img/wn/${weatherData.icon}@2x.png"
          alt="${weatherData.description}"
          class="mx-auto mb-2"
        >
        <div class="text-4xl font-bold text-gray-800 mb-2">
          ${weatherData.temperature}°C
        </div>
        <p class="text-gray-600 capitalize">${weatherData.description}</p>
      </div>
      
      <div class="grid grid-cols-3 gap-4 text-sm">
        <div class="bg-blue-50 p-3 rounded">
          <div class="text-blue-600 font-semibold">습도</div>
          <div class="text-gray-800">${weatherData.humidity}%</div>
        </div>
        <div class="bg-blue-50 p-3 rounded">
          <div class="text-blue-600 font-semibold">바람</div>
          <div class="text-gray-800">${weatherData.windSpeed} m/s</div>
        </div>
        <div class="bg-blue-50 p-3 rounded">
          <div class="text-blue-600 font-semibold">기압</div>
          <div class="text-gray-800">${weatherData.pressure} hPa</div>
        </div>
      </div>
    </div>
  `;
}

// 로딩 스피너 생성
function createLoadingSpinner() {
  return `
    <div class="bg-white rounded-lg shadow-lg p-8 text-center">
      <div class="animate-spin rounded-full h-12 w-12 border-b-2 border-blue-500 mx-auto mb-4"></div>
      <p class="text-gray-600">날씨 정보를 가져오는 중...</p>
    </div>
  `;
}
```

---

## 18.4 비동기 처리와 상태 관리

앱의 상태를 관리하고 비동기 작업을 효율적으로 처리하는 방법을 알아보겠습니다.

### 앱 상태 관리 시스템

애플리케이션의 다양한 상태를 체계적으로 관리하는 시스템을 구축해보겠습니다.

```javascript title="state.js"
// 앱 상태 관리 객체
const AppState = {
  // 현재 상태
  current: {
    isLoading: false,
    currentWeather: null,
    error: null,
    searchHistory: [],
  },

  // 상태 변경 함수들
  setLoading(isLoading) {
    this.current.isLoading = isLoading;
    this.notify('loading', isLoading);
  },

  setWeatherData(weatherData) {
    this.current.currentWeather = weatherData;
    this.current.error = null;
    this.addToHistory(weatherData.city);
    this.notify('weather', weatherData);
  },

  setError(error) {
    this.current.error = error;
    this.current.currentWeather = null;
    this.notify('error', error);
  },

  addToHistory(city) {
    const history = this.current.searchHistory;
    if (!history.includes(city)) {
      history.unshift(city);
      if (history.length > 5) history.pop();
    }
  },

  // 상태 변경 알림
  listeners: [],

  subscribe(listener) {
    this.listeners.push(listener);
  },

  notify(type, data) {
    this.listeners.forEach(listener => listener(type, data));
  },
};
```

### 비동기 작업 처리 패턴

Promise와 async/await를 활용한 효율적인 비동기 처리 패턴을 구현해보겠습니다.

```javascript title="async-handler.js"
// 비동기 작업 래퍼 함수
async function handleAsyncOperation(operation, errorMessage = '작업 실패') {
  try {
    AppState.setLoading(true);
    const result = await operation();
    return result;
  } catch (error) {
    console.error(errorMessage, error);
    AppState.setError(error.message || errorMessage);
    throw error;
  } finally {
    AppState.setLoading(false);
  }
}

// 날씨 검색 핸들러
async function searchWeather(city) {
  if (!city || !city.trim()) {
    AppState.setError('도시명을 입력해주세요');
    return;
  }

  const trimmedCity = city.trim();

  await handleAsyncOperation(
    () => fetchWeatherData(trimmedCity),
    '날씨 정보를 가져올 수 없습니다'
  ).then(weatherData => {
    AppState.setWeatherData(weatherData);
  });
}

// 재시도 메커니즘
async function retryOperation(operation, maxRetries = 3, delay = 1000) {
  for (let i = 0; i < maxRetries; i++) {
    try {
      return await operation();
    } catch (error) {
      if (i === maxRetries - 1) throw error;
      await new Promise(resolve => setTimeout(resolve, delay));
    }
  }
}
```

---

## 18.5 에러 처리와 사용자 피드백

다양한 에러 상황을 처리하고 사용자에게 적절한 피드백을 제공하는 시스템을 구축해보겠습니다.

### 에러 타입별 처리 시스템

각각의 에러 상황에 맞는 처리 방법과 사용자 메시지를 정의해보겠습니다.

```javascript title="error-handler.js"
// 에러 타입 정의
const ERROR_TYPES = {
  NETWORK_ERROR: 'network',
  API_ERROR: 'api',
  VALIDATION_ERROR: 'validation',
  NOT_FOUND: 'not_found',
  RATE_LIMIT: 'rate_limit',
};

// 에러 분류 함수
function classifyError(error) {
  if (!navigator.onLine) {
    return ERROR_TYPES.NETWORK_ERROR;
  }

  if (error.message.includes('404')) {
    return ERROR_TYPES.NOT_FOUND;
  }

  if (error.message.includes('429')) {
    return ERROR_TYPES.RATE_LIMIT;
  }

  return ERROR_TYPES.API_ERROR;
}

// 사용자 친화적 에러 메시지
const ERROR_MESSAGES = {
  [ERROR_TYPES.NETWORK_ERROR]: '인터넷 연결을 확인해주세요',
  [ERROR_TYPES.API_ERROR]: '서버에서 응답이 없습니다',
  [ERROR_TYPES.VALIDATION_ERROR]: '입력값을 확인해주세요',
  [ERROR_TYPES.NOT_FOUND]: '해당 도시를 찾을 수 없습니다',
  [ERROR_TYPES.RATE_LIMIT]: '요청이 너무 많습니다. 잠시 후 다시 시도해주세요',
};

// 에러 처리 함수
function handleError(error) {
  const errorType = classifyError(error);
  const message = ERROR_MESSAGES[errorType] || '알 수 없는 오류가 발생했습니다';

  showErrorMessage(message);
  logError(error, errorType);
}
```

### 사용자 피드백 시스템

에러 메시지와 성공 메시지를 효과적으로 표시하는 시스템을 만들어보겠습니다.

```javascript title="feedback.js"
// 에러 메시지 표시 함수
function showErrorMessage(message) {
  const errorHTML = `
    <div class="bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded-lg mb-4">
      <div class="flex items-center">
        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7 4a1 1 0 11-2 0 1 1 0 012 0zm-1-9a1 1 0 00-1 1v4a1 1 0 102 0V6a1 1 0 00-1-1z" clip-rule="evenodd"></path>
        </svg>
        <span>${message}</span>
      </div>
    </div>
  `;

  updateWeatherDisplay(errorHTML);

  // 5초 후 자동 제거
  setTimeout(() => {
    const errorElement = document.querySelector('.bg-red-100');
    if (errorElement) {
      errorElement.remove();
    }
  }, 5000);
}

// 성공 메시지 표시 함수
function showSuccessMessage(message) {
  const successHTML = `
    <div class="bg-green-100 border border-green-400 text-green-700 px-4 py-3 rounded-lg mb-4">
      <div class="flex items-center">
        <svg class="w-5 h-5 mr-2" fill="currentColor" viewBox="0 0 20 20">
          <path fill-rule="evenodd" d="M16.707 5.293a1 1 0 010 1.414l-8 8a1 1 0 01-1.414 0l-4-4a1 1 0 011.414-1.414L8 12.586l7.293-7.293a1 1 0 011.414 0z" clip-rule="evenodd"></path>
        </svg>
        <span>${message}</span>
      </div>
    </div>
  `;

  const displayElement = document.getElementById('weather-display');
  displayElement.insertAdjacentHTML('afterbegin', successHTML);
}

// 로딩 상태 표시 함수
function showLoadingState() {
  updateWeatherDisplay(createLoadingSpinner());
}

// 화면 업데이트 통합 함수
function updateWeatherDisplay(content) {
  const displayElement = document.getElementById('weather-display');
  displayElement.innerHTML = content;
}
```

---

## 18.6 메인 애플리케이션 로직

모든 모듈을 통합하여 완전한 날씨 앱을 구성하는 메인 로직을 구현해보겠습니다.

### 애플리케이션 초기화

앱이 시작될 때 필요한 초기 설정과 이벤트 리스너를 등록해보겠습니다.

```javascript title="app.js"
// 메인 애플리케이션 클래스
class WeatherApp {
  constructor() {
    this.init();
  }

  // 앱 초기화
  init() {
    this.setupEventListeners();
    this.setupStateSubscriptions();
    this.checkAPIKey();
    this.loadFromLocalStorage();
  }

  // 이벤트 리스너 설정
  setupEventListeners() {
    const searchForm = document.getElementById('search-form');
    const cityInput = document.getElementById('city-input');

    searchForm.addEventListener('submit', e => {
      e.preventDefault();
      const city = cityInput.value.trim();
      if (city) {
        this.searchWeather(city);
        cityInput.value = '';
      }
    });

    // 엔터 키 처리
    cityInput.addEventListener('keypress', e => {
      if (e.key === 'Enter') {
        searchForm.dispatchEvent(new Event('submit'));
      }
    });
  }

  // 상태 변경 구독 설정
  setupStateSubscriptions() {
    AppState.subscribe((type, data) => {
      switch (type) {
        case 'loading':
          if (data) {
            showLoadingState();
          }
          break;
        case 'weather':
          this.displayWeather(data);
          this.saveToLocalStorage(data);
          break;
        case 'error':
          handleError(new Error(data));
          break;
      }
    });
  }
}
```

### 핵심 기능 구현

날씨 검색과 데이터 표시 등 앱의 핵심 기능들을 구현해보겠습니다.

```javascript title="app-core.js"
// WeatherApp 클래스 확장
class WeatherApp {
  // 날씨 검색 메인 함수
  async searchWeather(city) {
    try {
      // 입력값 검증
      if (!this.validateCityInput(city)) {
        AppState.setError('올바른 도시명을 입력해주세요');
        return;
      }

      // API 키 확인
      if (!this.isAPIKeyValid()) {
        AppState.setError('API 키가 설정되지 않았습니다');
        return;
      }

      // 날씨 데이터 가져오기
      await searchWeather(city);
    } catch (error) {
      console.error('Weather search failed:', error);
    }
  }

  // 날씨 데이터 표시
  displayWeather(weatherData) {
    const weatherHTML = createWeatherCard(weatherData);
    updateWeatherDisplay(weatherHTML);

    // 성공 메시지 표시
    showSuccessMessage(`${weatherData.city}의 날씨 정보를 가져왔습니다`);
  }

  // 입력값 검증
  validateCityInput(city) {
    const trimmed = city.trim();
    return trimmed.length >= 2 && /^[a-zA-Z\s가-힣]+$/.test(trimmed);
  }

  // API 키 유효성 검사
  isAPIKeyValid() {
    return API_CONFIG.API_KEY && API_CONFIG.API_KEY !== 'YOUR_API_KEY_HERE';
  }

  // API 키 확인 및 안내
  checkAPIKey() {
    if (!this.isAPIKeyValid()) {
      const message = 'OpenWeatherMap API 키를 설정해주세요';
      showErrorMessage(message);
    }
  }
}

// 앱 시작
document.addEventListener('DOMContentLoaded', () => {
  new WeatherApp();
});
```

---

## 18.7 로컬 스토리지와 데이터 지속성

사용자 데이터를 브라우저에 저장하여 앱 재방문 시에도 정보를 유지하는 기능을 구현해보겠습니다.

### 로컬 스토리지 관리

사용자의 검색 기록과 즐겨찾는 도시 정보를 저장하고 관리하는 시스템을 만들어보겠습니다.

```javascript title="storage.js"
// 로컬 스토리지 키 상수
const STORAGE_KEYS = {
  SEARCH_HISTORY: 'weather_search_history',
  FAVORITE_CITIES: 'weather_favorite_cities',
  LAST_SEARCH: 'weather_last_search',
  USER_SETTINGS: 'weather_user_settings',
};

// 로컬 스토리지 유틸리티
const Storage = {
  // 데이터 저장
  save(key, data) {
    try {
      const jsonData = JSON.stringify(data);
      localStorage.setItem(key, jsonData);
      return true;
    } catch (error) {
      console.error('Storage save error:', error);
      return false;
    }
  },

  // 데이터 로드
  load(key, defaultValue = null) {
    try {
      const jsonData = localStorage.getItem(key);
      return jsonData ? JSON.parse(jsonData) : defaultValue;
    } catch (error) {
      console.error('Storage load error:', error);
      return defaultValue;
    }
  },

  // 데이터 삭제
  remove(key) {
    try {
      localStorage.removeItem(key);
      return true;
    } catch (error) {
      console.error('Storage remove error:', error);
      return false;
    }
  },

  // 모든 데이터 삭제
  clear() {
    try {
      Object.values(STORAGE_KEYS).forEach(key => {
        localStorage.removeItem(key);
      });
      return true;
    } catch (error) {
      console.error('Storage clear error:', error);
      return false;
    }
  },
};
```

### 검색 기록 관리

사용자의 검색 기록을 저장하고 빠른 재검색을 위한 UI를 제공해보겠습니다.

```javascript title="history.js"
// 검색 기록 관리 클래스
class SearchHistory {
  constructor(maxItems = 5) {
    this.maxItems = maxItems;
    this.history = this.loadHistory();
  }

  // 기록 추가
  addSearch(city) {
    // 중복 제거
    this.history = this.history.filter(item => item !== city);

    // 최상단에 추가
    this.history.unshift(city);

    // 최대 개수 제한
    if (this.history.length > this.maxItems) {
      this.history = this.history.slice(0, this.maxItems);
    }

    this.saveHistory();
    this.updateHistoryUI();
  }

  // 기록 로드
  loadHistory() {
    return Storage.load(STORAGE_KEYS.SEARCH_HISTORY, []);
  }

  // 기록 저장
  saveHistory() {
    Storage.save(STORAGE_KEYS.SEARCH_HISTORY, this.history);
  }

  // 기록 UI 업데이트
  updateHistoryUI() {
    const historyContainer = document.getElementById('search-history');
    if (!historyContainer || this.history.length === 0) return;

    const historyHTML = this.history
      .map(
        city => `
      <button 
        class="px-3 py-1 bg-blue-100 text-blue-800 rounded-full text-sm hover:bg-blue-200 transition-colors"
        onclick="weatherApp.searchWeather('${city}')"
      >
        ${city}
      </button>
    `
      )
      .join('');

    historyContainer.innerHTML = `
      <div class="mb-4">
        <h3 class="text-sm font-medium text-gray-700 mb-2">최근 검색</h3>
        <div class="flex flex-wrap gap-2">
          ${historyHTML}
        </div>
      </div>
    `;
  }
}
```

---

## 18.8 추가 기능과 최적화

사용자 경험을 향상시키는 추가 기능들과 성능 최적화 방법을 알아보겠습니다.

### 지도 연동 기능

사용자가 선택한 도시의 위치를 지도에서 확인할 수 있는 기능을 추가해보겠습니다.

```javascript title="geolocation.js"
// 지리적 위치 관리 클래스
class LocationManager {
  constructor() {
    this.currentPosition = null;
  }

  // 사용자 현재 위치 가져오기
  async getCurrentLocation() {
    return new Promise((resolve, reject) => {
      if (!navigator.geolocation) {
        reject(new Error('Geolocation is not supported'));
        return;
      }

      const options = {
        enableHighAccuracy: true,
        timeout: 5000,
        maximumAge: 0,
      };

      navigator.geolocation.getCurrentPosition(
        position => {
          this.currentPosition = {
            lat: position.coords.latitude,
            lon: position.coords.longitude,
          };
          resolve(this.currentPosition);
        },
        error => {
          reject(error);
        },
        options
      );
    });
  }

  // 좌표로 날씨 정보 가져오기
  async getWeatherByCoordinates(lat, lon) {
    const url = this.buildCoordinateURL(lat, lon);

    try {
      const response = await fetch(url);
      if (!response.ok) {
        throw new Error(`HTTP error! status: ${response.status}`);
      }

      const data = await response.json();
      return transformWeatherData(data);
    } catch (error) {
      console.error('Coordinate weather fetch error:', error);
      throw error;
    }
  }

  // 좌표 기반 URL 생성
  buildCoordinateURL(lat, lon) {
    const params = new URLSearchParams({
      lat: lat,
      lon: lon,
      appid: API_CONFIG.API_KEY,
      units: API_CONFIG.DEFAULT_UNITS,
      lang: API_CONFIG.DEFAULT_LANG,
    });

    return `${API_CONFIG.BASE_URL}${API_ENDPOINTS.CURRENT_WEATHER}?${params}`;
  }
}
```

### 성능 최적화

API 호출 최적화와 사용자 경험 개선을 위한 다양한 기법들을 적용해보겠습니다.

```javascript title="optimization.js"
// API 호출 캐싱 시스템
class WeatherCache {
  constructor(maxAge = 10 * 60 * 1000) {
    // 10분
    this.cache = new Map();
    this.maxAge = maxAge;
  }

  // 캐시 키 생성
  createKey(city) {
    return city.toLowerCase().trim();
  }

  // 캐시에서 데이터 가져오기
  get(city) {
    const key = this.createKey(city);
    const cached = this.cache.get(key);

    if (!cached) return null;

    // 만료 시간 체크
    if (Date.now() - cached.timestamp > this.maxAge) {
      this.cache.delete(key);
      return null;
    }

    return cached.data;
  }

  // 캐시에 데이터 저장
  set(city, data) {
    const key = this.createKey(city);
    this.cache.set(key, {
      data: data,
      timestamp: Date.now(),
    });
  }

  // 캐시 정리
  clear() {
    this.cache.clear();
  }
}

// 디바운스 유틸리티
function debounce(func, wait) {
  let timeout;
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// 최적화된 검색 함수
const optimizedSearch = debounce(async city => {
  // 캐시 확인
  const cached = weatherCache.get(city);
  if (cached) {
    AppState.setWeatherData(cached);
    return;
  }

  // API 호출
  await searchWeather(city);
}, 300);
```

---

## 18.9 테스트와 디버깅

코드의 품질을 보장하고 버그를 찾아 수정하는 방법을 알아보겠습니다.

### 기본 테스트 코드

핵심 함수들의 동작을 검증하는 간단한 테스트를 작성해보겠습니다.

```javascript title="tests.js"
// 간단한 테스트 프레임워크
class SimpleTest {
  constructor() {
    this.tests = [];
    this.results = [];
  }

  // 테스트 추가
  test(name, testFunction) {
    this.tests.push({ name, testFunction });
  }

  // 어설션 함수
  assert(condition, message) {
    if (!condition) {
      throw new Error(message || 'Assertion failed');
    }
  }

  assertEqual(actual, expected, message) {
    this.assert(actual === expected, message || `Expected ${expected}, but got ${actual}`);
  }

  // 모든 테스트 실행
  runAll() {
    console.log('Running tests...');
    this.results = [];

    this.tests.forEach(({ name, testFunction }) => {
      try {
        testFunction();
        this.results.push({ name, status: 'PASS' });
        console.log(`✅ ${name}`);
      } catch (error) {
        this.results.push({ name, status: 'FAIL', error: error.message });
        console.log(`❌ ${name}: ${error.message}`);
      }
    });

    this.printSummary();
  }

  // 결과 요약 출력
  printSummary() {
    const passed = this.results.filter(r => r.status === 'PASS').length;
    const total = this.results.length;
    console.log(`\nTest Summary: ${passed}/${total} passed`);
  }
}

// 실제 테스트 작성
const tester = new SimpleTest();

// API URL 빌드 테스트
tester.test('buildWeatherURL creates correct URL', () => {
  const url = buildWeatherURL('Seoul');
  tester.assert(url.includes('q=Seoul'), 'URL should contain city name');
  tester.assert(url.includes('appid='), 'URL should contain API key');
});

// 데이터 변환 테스트
tester.test('transformWeatherData formats data correctly', () => {
  const mockData = {
    name: 'Seoul',
    sys: { country: 'KR' },
    main: { temp: 15.7, humidity: 65, pressure: 1013 },
    weather: [{ description: 'clear sky', icon: '01d' }],
    wind: { speed: 3.2 },
  };

  const result = transformWeatherData(mockData);
  tester.assertEqual(result.city, 'Seoul');
  tester.assertEqual(result.temperature, 16); // rounded
  tester.assertEqual(result.humidity, 65);
});

// 테스트 실행
// tester.runAll();
```

### 디버깅 도구

개발 과정에서 유용한 디버깅 도구와 로깅 시스템을 구현해보겠습니다.

```javascript title="debug.js"
// 디버그 로거 클래스
class DebugLogger {
  constructor(enabled = false) {
    this.enabled = enabled;
    this.logs = [];
  }

  // 로그 기록
  log(level, message, data = null) {
    if (!this.enabled) return;

    const logEntry = {
      timestamp: new Date().toISOString(),
      level,
      message,
      data,
    };

    this.logs.push(logEntry);

    // 콘솔 출력
    const styles = {
      INFO: 'color: blue',
      WARN: 'color: orange',
      ERROR: 'color: red',
      DEBUG: 'color: gray',
    };

    console.log(`%c[${level}] ${message}`, styles[level] || 'color: black', data || '');
  }

  info(message, data) {
    this.log('INFO', message, data);
  }
  warn(message, data) {
    this.log('WARN', message, data);
  }
  error(message, data) {
    this.log('ERROR', message, data);
  }
  debug(message, data) {
    this.log('DEBUG', message, data);
  }

  // 로그 내보내기
  exportLogs() {
    return JSON.stringify(this.logs, null, 2);
  }

  // 로그 지우기
  clearLogs() {
    this.logs = [];
  }
}

// 전역 디버거 인스턴스
const debugLogger = new DebugLogger(true); // 개발 중에는 true

// 네트워크 요청 모니터링
function monitorNetworkRequests() {
  const originalFetch = window.fetch;

  window.fetch = async function (...args) {
    const startTime = Date.now();
    debugLogger.info('API Request started', { url: args[0] });

    try {
      const response = await originalFetch.apply(this, args);
      const endTime = Date.now();

      debugLogger.info('API Request completed', {
        url: args[0],
        status: response.status,
        duration: `${endTime - startTime}ms`,
      });

      return response;
    } catch (error) {
      debugLogger.error('API Request failed', {
        url: args[0],
        error: error.message,
      });
      throw error;
    }
  };
}

// 개발 모드에서 모니터링 활성화
if (window.location.hostname === 'localhost') {
  monitorNetworkRequests();
}
```

---

## 18.10 프로젝트 완성과 배포 준비

완성된 날씨 앱을 최적화하고 배포를 위한 준비 작업을 진행해보겠습니다.

### 코드 리팩토링과 최종 정리

전체 코드를 검토하고 개선할 부분들을 정리해보겠습니다.

```javascript title="final-app.js"
// 최종 통합된 날씨 앱
class FinalWeatherApp {
  constructor() {
    this.cache = new WeatherCache();
    this.history = new SearchHistory();
    this.locationManager = new LocationManager();
    this.init();
  }

  async init() {
    // 기본 초기화
    this.setupEventListeners();
    this.setupStateSubscriptions();

    // 개발 도구 초기화
    if (this.isDevelopment()) {
      this.enableDebugMode();
    }

    // 저장된 데이터 복원
    this.restoreUserData();

    // API 키 검증
    await this.validateSetup();
  }

  // 개발 환경 확인
  isDevelopment() {
    return window.location.hostname === 'localhost' || window.location.hostname === '127.0.0.1';
  }

  // 디버그 모드 활성화
  enableDebugMode() {
    debugLogger.enabled = true;
    window.weatherApp = this; // 글로벌 접근을 위해
    console.log('🌤️ Weather App Debug Mode Enabled');
  }

  // 사용자 데이터 복원
  restoreUserData() {
    this.history.updateHistoryUI();

    const lastSearch = Storage.load(STORAGE_KEYS.LAST_SEARCH);
    if (lastSearch) {
      debugLogger.info('Restored last search', lastSearch);
    }
  }

  // 설정 검증
  async validateSetup() {
    if (!this.isAPIKeyValid()) {
      this.showSetupInstructions();
      return false;
    }

    // 초기 위치 기반 날씨 시도
    try {
      await this.loadCurrentLocationWeather();
    } catch (error) {
      debugLogger.warn('Initial location weather failed', error.message);
    }

    return true;
  }

  // 설정 안내 표시
  showSetupInstructions() {
    const instructionsHTML = `
      <div class="bg-yellow-100 border border-yellow-400 text-yellow-800 px-4 py-3 rounded-lg">
        <h3 class="font-bold mb-2">API 키 설정이 필요합니다</h3>
        <ol class="list-decimal list-inside space-y-1 text-sm">
          <li><a href="https://openweathermap.org/api" target="_blank" class="underline">OpenWeatherMap</a>에서 무료 API 키를 발급받으세요</li>
          <li>config.js 파일의 API_KEY 값을 교체하세요</li>
          <li>페이지를 새로고침하세요</li>
        </ol>
      </div>
    `;
    updateWeatherDisplay(instructionsHTML);
  }
}

// 앱 시작
document.addEventListener('DOMContentLoaded', () => {
  window.weatherApp = new FinalWeatherApp();
});
```

### 성능 최적화 체크리스트

배포 전 확인해야 할 성능 최적화 항목들을 정리해보겠습니다.

```javascript title="performance-checklist.js"
// 성능 최적화 체크리스트
const PERFORMANCE_CHECKLIST = {
  // 네트워크 최적화
  NETWORK: [
    'API 호출 캐싱 구현',
    'debounce로 과도한 요청 방지',
    'Error retry 메커니즘',
    'Request timeout 설정',
  ],

  // UI 최적화
  UI: ['DOM 조작 최소화', '이미지 lazy loading', 'CSS 애니메이션 최적화', '불필요한 리렌더링 방지'],

  // 메모리 최적화
  MEMORY: ['이벤트 리스너 정리', '캐시 크기 제한', '메모리 누수 체크', '전역 변수 최소화'],

  // 접근성
  ACCESSIBILITY: ['ARIA 라벨 추가', '키보드 네비게이션', '색상 대비 확인', '스크린 리더 지원'],
};

// 성능 측정 도구
class PerformanceMonitor {
  static measureAPICall(apiFunction) {
    return async function (...args) {
      const startTime = performance.now();

      try {
        const result = await apiFunction.apply(this, args);
        const endTime = performance.now();

        debugLogger.info('API Performance', {
          function: apiFunction.name,
          duration: `${(endTime - startTime).toFixed(2)}ms`,
          args: args,
        });

        return result;
      } catch (error) {
        const endTime = performance.now();
        debugLogger.error('API Error Performance', {
          function: apiFunction.name,
          duration: `${(endTime - startTime).toFixed(2)}ms`,
          error: error.message,
        });
        throw error;
      }
    };
  }

  static checkMemoryUsage() {
    if (performance.memory) {
      const memInfo = {
        used: (performance.memory.usedJSHeapSize / 1024 / 1024).toFixed(2),
        total: (performance.memory.totalJSHeapSize / 1024 / 1024).toFixed(2),
        limit: (performance.memory.jsHeapSizeLimit / 1024 / 1024).toFixed(2),
      };

      debugLogger.info('Memory Usage', memInfo);
      return memInfo;
    }
  }
}
```

---

## 마무리

이번 장에서는 OpenWeatherMap API를 활용한 완전한 날씨 앱을 구축해보았습니다. API 연동부터 시작해서 비동기 처리, 에러 핸들링, 사용자 인터페이스 구성, 그리고 성능 최적화까지 실제 웹 애플리케이션 개발에 필요한 모든 과정을 경험했습니다.

특히 모던 자바스크립트의 핵심 기능들인 async/await, fetch API, 모듈 시스템, 그리고 ES6+ 문법들을 실전에서 활용하는 방법을 익혔습니다. 또한 사용자 경험을 고려한 로딩 상태 관리, 에러 메시지 표시, 로컬 스토리지 활용 등 실제 서비스에서 중요한 요소들도 함께 구현했습니다.

다음 장에서는 현대 자바스크립트 생태계와 더 발전된 도구들에 대해 알아보겠습니다. TypeScript, 테스팅 프레임워크, 그리고 실무에서 사용되는 다양한 도구들을 소개하며, 지속적인 학습을 위한 방향을 제시할 예정입니다.
