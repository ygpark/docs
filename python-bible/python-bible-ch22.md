---
title: '22. 미니 프로젝트 3: 개인 데이터 분석 도구'
slug: python-project-data-analysis-tool
description: '공개 API 활용 실시간 데이터 수집과 분석. 날씨, 주식 등 다양한 데이터를 자동 수집해 인사이트 도출'
keywords: [파이썬 데이터분석, 공개 API, 데이터 수집, requests, 자동화, 데이터 시각화]
sidebar_position: 22
---

# 22. 미니 프로젝트 3: 개인 데이터 분석 도구

데이터는 현대 사회의 새로운 석유라고 불립니다. 매일 우리 주변에서 생성되는 수많은 데이터를 수집하고 분석하여 의미 있는 인사이트를 도출하는 능력은 이제 모든 분야에서 중요한 역량이 되었습니다. 이번 프로젝트에서는 공개 API를 활용해 실시간 데이터를 수집하고, 이를 분석하여 시각화하는 완전한 데이터 분석 도구를 만들어보겠습니다. 단순히 데이터를 보여주는 것을 넘어서, 자동화된 수집과 인사이트 도출까지 포함한 실무급 프로젝트를 경험해보세요.

## 22-1 프로젝트 개요

이번 프로젝트에서는 개인이 관심 있는 데이터를 수집하고 분석하는 통합 도구를 만들어보겠습니다. 날씨 데이터, 주식 정보, 인기 검색어 등 다양한 공개 API를 활용하여 실시간 데이터를 수집하고, 이를 분석하여 의미 있는 패턴을 찾아내는 것이 목표입니다.

### 프로젝트 목표

- 공개 API를 활용한 데이터 수집 자동화
- 수집된 데이터의 정제 및 분석
- 직관적인 시각화 대시보드 구현
- 스케줄러를 통한 자동화 시스템 구축
- 테스트 코드를 포함한 안정적인 프로그램 작성

### 사용할 기술

- `requests`: API 호출
- `pandas`: 데이터 처리 및 분석
- `matplotlib`, `seaborn`: 데이터 시각화
- `schedule`: 작업 스케줄링
- `json`: 데이터 저장 및 로드
- `logging`: 로그 관리

```python title=requirements.txt
requests>=2.31.0
pandas>=2.0.0
matplotlib>=3.7.0
seaborn>=0.12.0
schedule>=1.2.0
```

## 22-2 공개 API에서 데이터 수집하기

실시간 데이터 수집은 모든 데이터 분석 프로젝트의 시작점입니다. 여기서는 무료로 사용할 수 있는 공개 API를 활용하여 안정적인 데이터 수집 시스템을 구축해보겠습니다.

### API 클라이언트 기본 구조

```python title=data_collector.py
import requests
import json
import time
from datetime import datetime
from typing import Dict, List, Optional
import logging

class DataCollector:
    """다양한 공개 API에서 데이터를 수집하는 클래스"""

    def __init__(self):
        self.session = requests.Session()
        self.session.headers.update({
            'User-Agent': 'Personal Data Analysis Tool 1.0'
        })

        # 로깅 설정
        logging.basicConfig(
            level=logging.INFO,
            format='%(asctime)s - %(levelname)s - %(message)s',
            handlers=[
                logging.FileHandler('data_collector.log'),
                logging.StreamHandler()
            ]
        )
        self.logger = logging.getLogger(__name__)

    def get_weather_data(self, city: str = "Seoul") -> Optional[Dict]:
        """OpenWeatherMap API를 사용한 날씨 데이터 수집"""
        # 무료 API 키 없이 사용 가능한 대안 API 사용
        url = f"https://api.open-meteo.com/v1/forecast"
        params = {
            'latitude': 37.5665,  # 서울 위도
            'longitude': 126.9780,  # 서울 경도
            'current_weather': True,
            'hourly': 'temperature_2m,humidity,windspeed_10m',
            'daily': 'temperature_2m_max,temperature_2m_min,precipitation_sum',
            'forecast_days': 7
        }

        try:
            response = self.session.get(url, params=params, timeout=10)
            response.raise_for_status()

            data = response.json()

            # 데이터 정제
            weather_info = {
                'timestamp': datetime.now().isoformat(),
                'city': city,
                'current_temp': data['current_weather']['temperature'],
                'wind_speed': data['current_weather']['windspeed'],
                'hourly_forecast': data['hourly'],
                'daily_forecast': data['daily']
            }

            self.logger.info(f"날씨 데이터 수집 완료: {city}")
            return weather_info

        except requests.RequestException as e:
            self.logger.error(f"날씨 데이터 수집 실패: {e}")
            return None

    def get_news_data(self) -> Optional[List[Dict]]:
        """NewsAPI를 사용한 뉴스 데이터 수집 (무료 대안 사용)"""
        # 실제로는 NewsAPI나 다른 뉴스 API를 사용할 수 있습니다
        # 여기서는 데모용 데이터를 생성합니다
        news_data = [
            {
                'title': '파이썬 프로그래밍의 새로운 트렌드',
                'description': '2025년 파이썬 개발 동향에 대한 분석',
                'published_at': datetime.now().isoformat(),
                'source': 'Tech News'
            },
            {
                'title': '데이터 사이언스 시장 성장',
                'description': '데이터 분석 분야의 급속한 성장세',
                'published_at': datetime.now().isoformat(),
                'source': 'Business Weekly'
            }
        ]

        self.logger.info("뉴스 데이터 수집 완료")
        return news_data
```

날씨 API는 실제 공개 서비스인 Open-Meteo를 사용하여 무료로 데이터를 수집할 수 있습니다. API 키 없이도 사용 가능하며, 전 세계 날씨 정보를 제공합니다.

## 22-3 데이터 정제 및 분석

수집된 원시 데이터는 분석하기 전에 정제 과정을 거쳐야 합니다. pandas를 활용하여 데이터를 구조화하고 분석에 필요한 형태로 변환해보겠습니다.

### 데이터 처리 클래스

```python title=data_processor.py
import pandas as pd
import numpy as np
from datetime import datetime, timedelta
from typing import Dict, List, Any
import json

class DataProcessor:
    """수집된 데이터를 정제하고 분석하는 클래스"""

    def __init__(self):
        self.weather_df = pd.DataFrame()
        self.news_df = pd.DataFrame()

    def process_weather_data(self, weather_data: Dict) -> pd.DataFrame:
        """날씨 데이터를 pandas DataFrame으로 변환 및 정제"""
        if not weather_data:
            return pd.DataFrame()

        # 시간별 데이터 처리
        hourly_data = weather_data['hourly']
        df = pd.DataFrame({
            'datetime': pd.to_datetime(hourly_data['time']),
            'temperature': hourly_data['temperature_2m'],
            'humidity': hourly_data['humidity'],
            'wind_speed': hourly_data['windspeed_10m']
        })

        # 결측값 처리
        df = df.dropna()

        # 추가 분석 컬럼 생성
        df['hour'] = df['datetime'].dt.hour
        df['day'] = df['datetime'].dt.day
        df['temp_category'] = pd.cut(
            df['temperature'],
            bins=[-np.inf, 0, 10, 20, 30, np.inf],
            labels=['매우추움', '추움', '선선함', '따뜻함', '더움']
        )

        return df

    def calculate_weather_statistics(self, df: pd.DataFrame) -> Dict[str, Any]:
        """날씨 데이터 통계 계산"""
        if df.empty:
            return {}

        stats = {
            'avg_temperature': df['temperature'].mean(),
            'max_temperature': df['temperature'].max(),
            'min_temperature': df['temperature'].min(),
            'avg_humidity': df['humidity'].mean(),
            'temp_trend': self._calculate_trend(df['temperature']),
            'hourly_patterns': df.groupby('hour')['temperature'].mean().to_dict()
        }

        return stats

    def _calculate_trend(self, series: pd.Series) -> str:
        """시계열 데이터의 트렌드 계산"""
        if len(series) < 2:
            return "데이터 부족"

        # 선형 회귀를 통한 트렌드 분석
        x = np.arange(len(series))
        slope = np.polyfit(x, series, 1)[0]

        if slope > 0.1:
            return "상승"
        elif slope < -0.1:
            return "하강"
        else:
            return "안정"

    def process_news_data(self, news_data: List[Dict]) -> pd.DataFrame:
        """뉴스 데이터 처리 및 분석"""
        if not news_data:
            return pd.DataFrame()

        df = pd.DataFrame(news_data)
        df['published_at'] = pd.to_datetime(df['published_at'])

        # 텍스트 분석
        df['title_length'] = df['title'].str.len()
        df['word_count'] = df['title'].str.split().str.len()

        return df
```

이 데이터 처리 클래스는 원시 데이터를 분석 가능한 형태로 변환하고, 기본적인 통계 분석을 수행합니다. 실제 프로젝트에서는 더 복잡한 전처리 과정이 필요할 수 있습니다.

## 22-4 시각화 대시보드 구현

데이터 분석의 결과를 효과적으로 전달하기 위해서는 직관적인 시각화가 필수입니다. matplotlib과 seaborn을 활용하여 인터랙티브한 대시보드를 구현해보겠습니다.

### 시각화 클래스

```python title=visualizer.py
import matplotlib.pyplot as plt
import seaborn as sns
import pandas as pd
from datetime import datetime
import matplotlib.dates as mdates
from typing import Dict, Any

# 한글 폰트 설정
plt.rcParams['font.family'] = 'DejaVu Sans'
sns.set_style("whitegrid")
sns.set_palette("husl")

class DataVisualizer:
    """데이터 시각화를 담당하는 클래스"""

    def __init__(self):
        self.fig_size = (12, 8)

    def create_weather_dashboard(self, df: pd.DataFrame, stats: Dict[str, Any]):
        """날씨 데이터 대시보드 생성"""
        fig, axes = plt.subplots(2, 2, figsize=(15, 10))
        fig.suptitle('날씨 데이터 분석 대시보드', fontsize=16, fontweight='bold')

        # 1. 시간별 온도 변화
        axes[0, 0].plot(df['datetime'], df['temperature'],
                       color='red', linewidth=2, marker='o', markersize=4)
        axes[0, 0].set_title('시간별 온도 변화')
        axes[0, 0].set_xlabel('시간')
        axes[0, 0].set_ylabel('온도 (°C)')
        axes[0, 0].grid(True, alpha=0.3)

        # 2. 온도 분포 히스토그램
        axes[0, 1].hist(df['temperature'], bins=20, alpha=0.7, color='skyblue', edgecolor='black')
        axes[0, 1].axvline(stats['avg_temperature'], color='red', linestyle='--',
                          label=f'평균: {stats["avg_temperature"]:.1f}°C')
        axes[0, 1].set_title('온도 분포')
        axes[0, 1].set_xlabel('온도 (°C)')
        axes[0, 1].set_ylabel('빈도')
        axes[0, 1].legend()

        # 3. 습도 vs 온도 산점도
        scatter = axes[1, 0].scatter(df['temperature'], df['humidity'],
                                   alpha=0.6, c=df['wind_speed'], cmap='viridis')
        axes[1, 0].set_title('온도 vs 습도 (풍속별)')
        axes[1, 0].set_xlabel('온도 (°C)')
        axes[1, 0].set_ylabel('습도 (%)')
        plt.colorbar(scatter, ax=axes[1, 0], label='풍속 (m/s)')

        # 4. 시간대별 평균 온도
        if 'hourly_patterns' in stats:
            hours = list(stats['hourly_patterns'].keys())
            temps = list(stats['hourly_patterns'].values())
            axes[1, 1].bar(hours, temps, alpha=0.7, color='orange')
            axes[1, 1].set_title('시간대별 평균 온도')
            axes[1, 1].set_xlabel('시간')
            axes[1, 1].set_ylabel('평균 온도 (°C)')

        plt.tight_layout()

        # 파일로 저장
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        plt.savefig(f'weather_dashboard_{timestamp}.png', dpi=300, bbox_inches='tight')
        plt.show()

    def create_summary_report(self, weather_stats: Dict[str, Any], news_count: int):
        """요약 리포트 생성"""
        fig, ax = plt.subplots(figsize=(10, 6))

        # 텍스트 기반 요약 정보
        summary_text = f"""
        데이터 분석 요약 리포트
        생성 시간: {datetime.now().strftime('%Y-%m-%d %H:%M:%S')}

        날씨 정보:
        • 평균 온도: {weather_stats.get('avg_temperature', 0):.1f}°C
        • 최고 온도: {weather_stats.get('max_temperature', 0):.1f}°C
        • 최저 온도: {weather_stats.get('min_temperature', 0):.1f}°C
        • 평균 습도: {weather_stats.get('avg_humidity', 0):.1f}%
        • 온도 트렌드: {weather_stats.get('temp_trend', '알 수 없음')}

        뉴스 정보:
        • 수집된 뉴스 개수: {news_count}개
        """

        ax.text(0.1, 0.5, summary_text, fontsize=12, verticalalignment='center',
                bbox=dict(boxstyle="round,pad=0.5", facecolor="lightblue", alpha=0.7))
        ax.set_xlim(0, 1)
        ax.set_ylim(0, 1)
        ax.axis('off')

        plt.title('일일 데이터 분석 요약', fontsize=16, fontweight='bold', pad=20)
        plt.tight_layout()

        # 저장
        timestamp = datetime.now().strftime("%Y%m%d_%H%M%S")
        plt.savefig(f'summary_report_{timestamp}.png', dpi=300, bbox_inches='tight')
        plt.show()
```

이 시각화 클래스는 다양한 차트 유형을 조합하여 데이터의 전체적인 패턴과 세부 정보를 모두 보여주는 종합적인 대시보드를 생성합니다.

## 22-5 자동화 스케줄러 추가

데이터 분석 도구의 진정한 가치는 자동화에 있습니다. 사용자가 직접 실행하지 않아도 정기적으로 데이터를 수집하고 분석하는 스케줄러를 구현해보겠습니다.

### 메인 애플리케이션

```python title=main.py
import schedule
import time
import json
from datetime import datetime
from data_collector import DataCollector
from data_processor import DataProcessor
from visualizer import DataVisualizer
import logging

class PersonalDataAnalyzer:
    """개인 데이터 분석 도구 메인 클래스"""

    def __init__(self):
        self.collector = DataCollector()
        self.processor = DataProcessor()
        self.visualizer = DataVisualizer()
        self.data_storage = "collected_data.json"

        # 로깅 설정
        logging.basicConfig(level=logging.INFO)
        self.logger = logging.getLogger(__name__)

    def collect_and_analyze(self):
        """데이터 수집 및 분석 실행"""
        self.logger.info("데이터 수집 및 분석 시작")

        try:
            # 1. 데이터 수집
            weather_data = self.collector.get_weather_data()
            news_data = self.collector.get_news_data()

            if not weather_data:
                self.logger.warning("날씨 데이터 수집 실패")
                return

            # 2. 데이터 처리
            weather_df = self.processor.process_weather_data(weather_data)
            news_df = self.processor.process_news_data(news_data)

            # 3. 통계 계산
            weather_stats = self.processor.calculate_weather_statistics(weather_df)

            # 4. 시각화
            if not weather_df.empty:
                self.visualizer.create_weather_dashboard(weather_df, weather_stats)
                self.visualizer.create_summary_report(weather_stats, len(news_df))

            # 5. 데이터 저장
            self.save_data({
                'timestamp': datetime.now().isoformat(),
                'weather_stats': weather_stats,
                'news_count': len(news_df)
            })

            self.logger.info("데이터 분석 완료")

        except Exception as e:
            self.logger.error(f"분석 중 오류 발생: {e}")

    def save_data(self, data):
        """분석 결과를 파일에 저장"""
        try:
            # 기존 데이터 로드
            try:
                with open(self.data_storage, 'r', encoding='utf-8') as f:
                    stored_data = json.load(f)
            except FileNotFoundError:
                stored_data = []

            # 새 데이터 추가
            stored_data.append(data)

            # 파일에 저장 (최근 100개만 유지)
            with open(self.data_storage, 'w', encoding='utf-8') as f:
                json.dump(stored_data[-100:], f, ensure_ascii=False, indent=2)

        except Exception as e:
            self.logger.error(f"데이터 저장 중 오류: {e}")

    def start_scheduler(self):
        """스케줄러 시작"""
        # 매 시간마다 실행
        schedule.every().hour.do(self.collect_and_analyze)

        # 매일 오전 9시에 실행
        schedule.every().day.at("09:00").do(self.collect_and_analyze)

        self.logger.info("스케줄러 시작됨")
        self.logger.info("매시간 데이터 수집 및 분석 실행")
        self.logger.info("매일 오전 9시 종합 분석 실행")

        # 첫 번째 실행
        self.collect_and_analyze()

        # 스케줄러 실행
        while True:
            schedule.run_pending()
            time.sleep(60)  # 1분마다 스케줄 체크

if __name__ == "__main__":
    analyzer = PersonalDataAnalyzer()

    print("개인 데이터 분석 도구를 시작합니다...")
    print("프로그램을 종료하려면 Ctrl+C를 누르세요.")

    try:
        analyzer.start_scheduler()
    except KeyboardInterrupt:
        print("\n프로그램이 종료되었습니다.")
```

스케줄러는 사용자가 설정한 주기에 따라 자동으로 데이터를 수집하고 분석합니다. 이를 통해 지속적인 모니터링과 트렌드 분석이 가능해집니다.

## 22-6 전체 프로젝트 테스트

안정적인 프로그램을 위해서는 체계적인 테스트가 필수입니다. pytest를 사용하여 각 구성요소별로 단위 테스트를 작성해보겠습니다.

### 테스트 코드

```python title=test_data_analyzer.py
import pytest
import pandas as pd
from datetime import datetime
from data_processor import DataProcessor
from data_collector import DataCollector

class TestDataProcessor:
    """데이터 처리 클래스 테스트"""

    def setup_method(self):
        """각 테스트 전에 실행"""
        self.processor = DataProcessor()

    def test_weather_data_processing(self):
        """날씨 데이터 처리 테스트"""
        # 테스트용 데이터
        sample_weather_data = {
            'hourly': {
                'time': ['2025-08-19T00:00', '2025-08-19T01:00'],
                'temperature_2m': [25.0, 26.5],
                'humidity': [60, 65],
                'windspeed_10m': [5.0, 4.5]
            }
        }

        df = self.processor.process_weather_data(sample_weather_data)

        assert not df.empty
        assert len(df) == 2
        assert 'temperature' in df.columns
        assert 'humidity' in df.columns
        assert df['temperature'].iloc[0] == 25.0

    def test_weather_statistics(self):
        """날씨 통계 계산 테스트"""
        # 테스트용 DataFrame
        test_df = pd.DataFrame({
            'temperature': [20, 25, 30, 22, 28],
            'humidity': [50, 60, 70, 55, 65],
            'datetime': pd.date_range('2025-08-19', periods=5, freq='H')
        })
        test_df['hour'] = test_df['datetime'].dt.hour

        stats = self.processor.calculate_weather_statistics(test_df)

        assert 'avg_temperature' in stats
        assert 'max_temperature' in stats
        assert 'min_temperature' in stats
        assert stats['avg_temperature'] == 25.0
        assert stats['max_temperature'] == 30
        assert stats['min_temperature'] == 20

class TestDataCollector:
    """데이터 수집 클래스 테스트"""

    def setup_method(self):
        self.collector = DataCollector()

    def test_weather_api_response_structure(self):
        """날씨 API 응답 구조 테스트"""
        weather_data = self.collector.get_weather_data()

        if weather_data:  # API 응답이 있는 경우에만 테스트
            assert 'timestamp' in weather_data
            assert 'current_temp' in weather_data
            assert 'city' in weather_data

# 테스트 실행 함수
def run_tests():
    """모든 테스트 실행"""
    print("테스트를 실행합니다...")
    pytest.main([__file__, '-v'])

if __name__ == "__main__":
    run_tests()
```

테스트 코드는 각 기능이 예상대로 동작하는지 검증하여 프로그램의 안정성을 보장합니다. 실제 API 호출이 실패할 수 있는 상황도 고려하여 작성되었습니다.

---

## 학습 마무리

이번 프로젝트를 통해 실제 데이터 분석 파이프라인의 전체 과정을 경험해보았습니다. API 데이터 수집부터 시각화, 자동화까지 포함한 완전한 시스템을 구축하면서 파이썬의 다양한 라이브러리와 프로그래밍 패턴을 활용했습니다. 이러한 경험은 실무에서 데이터 관련 프로젝트를 진행할 때 큰 도움이 될 것입니다.

더 나아가 이 프로젝트를 확장하여 더 많은 데이터 소스를 추가하거나, 머신러닝 모델을 통한 예측 기능을 구현해볼 수도 있습니다. 중요한 것은 체계적인 설계와 테스트를 통해 안정적이고 확장 가능한 코드를 작성하는 것입니다.
