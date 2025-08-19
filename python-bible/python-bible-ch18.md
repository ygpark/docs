---
title: '18. 데이터 시각화 기초'
slug: python-data-visualization
description: 'matplotlib 라이브러리를 활용한 기본 그래프부터 전문가 수준의 시각화 차트 제작 기술'
keywords: [파이썬 데이터 시각화, matplotlib, 차트 생성, 그래프, 데이터 분석, 대시보드]
sidebar_position: 18
---

# 18. 데이터 시각화 기초

데이터 시각화는 복잡한 숫자 정보를 그래프와 차트로 변환하여 한눈에 이해할 수 있게 만드는 기술입니다. 파이썬의 matplotlib 라이브러리를 사용하면 간단한 코드만으로도 전문가 수준의 시각화를 만들 수 있어요. 이번 챕터에서는 기본적인 그래프부터 실무에서 활용할 수 있는 고급 차트까지 단계별로 배워보겠습니다.

## 학습 목표

이번 챕터를 완료하면 다음과 같은 능력을 갖게 됩니다:

- matplotlib 라이브러리의 기본 사용법 이해
- 선 그래프, 막대 그래프, 산점도 등 기본 차트 생성
- 차트의 제목, 축 레이블, 범례 등 요소 커스터마이징
- 실제 데이터를 활용한 분석 시각화 구현
- 여러 개의 서브플롯을 조합한 대시보드 제작

---

## 18-1. matplotlib 라이브러리 소개

matplotlib는 파이썬에서 가장 널리 사용되는 데이터 시각화 라이브러리입니다. MATLAB의 그래프 기능을 파이썬에서 구현한 것으로, 강력하면서도 직관적인 API를 제공합니다. 과학 연구부터 비즈니스 분석까지 다양한 분야에서 활용되고 있어요.

### matplotlib 설치 및 한글 폰트 설정

먼저 matplotlib를 설치해보겠습니다:

```bash title=terminal
pip install matplotlib
```

또는 uv를 사용하는 경우:

```bash title=terminal
uv add matplotlib
```

**한글 폰트 설정:**

```python title=font_setup.py
import matplotlib.pyplot as plt

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False
```

### 첫 번째 그래프 그리기

가장 간단한 선 그래프부터 시작해보겠습니다:

```python title=basic_plot.py
import matplotlib.pyplot as plt

# 데이터 준비
x = [1, 2, 3, 4, 5]
y = [2, 4, 6, 8, 10]

# 그래프 그리기
plt.plot(x, y)
plt.title('첫 번째 그래프')
plt.xlabel('X축')
plt.ylabel('Y축')
plt.show()
```

이 코드는 간단한 직선 그래프를 생성합니다. `plt.plot()`이 실제 그래프를 그리는 함수이고, 나머지는 제목과 축 레이블을 설정하는 부분입니다.

---

## 18-2. 기본 그래프 그리기

데이터의 특성에 따라 적절한 그래프 유형을 선택하는 것이 중요합니다. 시간의 변화를 보려면 선 그래프, 카테고리별 비교는 막대 그래프, 두 변수의 관계는 산점도가 효과적이에요.

### 선 그래프 (Line Plot)

시간에 따른 변화를 표현할 때 가장 적합한 그래프입니다:

```python title=line_plot.py
import matplotlib.pyplot as plt

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False

# 한 달간 온도 변화 데이터
days = list(range(1, 31))
temperature = [15, 18, 22, 25, 28, 30, 32, 31, 29, 26,
               23, 20, 18, 16, 19, 22, 26, 29, 31, 28,
               25, 22, 19, 17, 20, 24, 27, 29, 26, 23]

plt.figure(figsize=(10, 6))
plt.plot(days, temperature, marker='o', linewidth=2, color='red')
plt.title('한 달간 일일 기온 변화', fontsize=16)
plt.xlabel('날짜', fontsize=12)
plt.ylabel('온도 (°C)', fontsize=12)
plt.grid(True, alpha=0.3)
plt.show()
```

### 막대 그래프 (Bar Chart)

카테고리별 데이터 비교에 최적화된 그래프입니다:

```python title=bar_chart.py
import matplotlib.pyplot as plt

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False

# 과목별 점수 데이터
subjects = ['수학', '영어', '과학', '국어', '사회']
scores = [88, 92, 85, 90, 87]

plt.figure(figsize=(8, 6))
bars = plt.bar(subjects, scores, color=['skyblue', 'lightgreen', 'coral', 'gold', 'lightpink'])
plt.title('과목별 성적', fontsize=16)
plt.xlabel('과목', fontsize=12)
plt.ylabel('점수', fontsize=12)
plt.ylim(80, 95)

# 막대 위에 점수 표시
for bar, score in zip(bars, scores):
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 0.5,
             str(score), ha='center', va='bottom')

plt.show()
```

### 산점도 (Scatter Plot)

두 변수 간의 상관관계를 시각화하는 데 사용됩니다:

```python title=scatter_plot.py
import matplotlib.pyplot as plt
import random

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False

# 공부 시간과 성적 데이터 생성
study_hours = [random.uniform(1, 10) for _ in range(30)]
test_scores = [h * 8 + random.uniform(-10, 10) for h in study_hours]

plt.figure(figsize=(8, 6))
plt.scatter(study_hours, test_scores, alpha=0.7, s=50, color='purple')
plt.title('공부 시간과 시험 점수의 관계', fontsize=16)
plt.xlabel('공부 시간 (시간)', fontsize=12)
plt.ylabel('시험 점수', fontsize=12)
plt.grid(True, alpha=0.3)
plt.show()
```

---

## 18-3. 데이터 분석과 시각화

실제 데이터를 활용한 분석에서는 여러 개의 그래프를 함께 사용하여 데이터의 다양한 측면을 보여주는 것이 효과적입니다. 서브플롯을 활용하면 하나의 화면에 여러 그래프를 배치할 수 있어요.

### 여러 그래프 조합하기

```python title=multiple_plots.py
import matplotlib.pyplot as plt
import random

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False

# 가상의 판매 데이터 생성
months = ['1월', '2월', '3월', '4월', '5월', '6월']
product_a = [120, 135, 148, 162, 180, 195]
product_b = [80, 88, 95, 110, 125, 140]
product_c = [200, 190, 185, 175, 170, 165]

# 2x2 서브플롯 생성
fig, ((ax1, ax2), (ax3, ax4)) = plt.subplots(2, 2, figsize=(12, 10))

# 선 그래프 - 제품별 판매 추이
ax1.plot(months, product_a, marker='o', label='제품 A')
ax1.plot(months, product_b, marker='s', label='제품 B')
ax1.plot(months, product_c, marker='^', label='제품 C')
ax1.set_title('월별 제품 판매 추이')
ax1.set_ylabel('판매량')
ax1.legend()
ax1.grid(True, alpha=0.3)

# 막대 그래프 - 6월 제품별 판매량
june_sales = [195, 140, 165]
products = ['제품 A', '제품 B', '제품 C']
ax2.bar(products, june_sales, color=['blue', 'green', 'red'])
ax2.set_title('6월 제품별 판매량')
ax2.set_ylabel('판매량')

# 파이 차트 - 전체 판매 비율
total_sales = [sum(product_a), sum(product_b), sum(product_c)]
ax3.pie(total_sales, labels=products, autopct='%1.1f%%', startangle=90)
ax3.set_title('전체 판매 비율')

# 히스토그램 - 일일 판매량 분포
daily_sales = [random.randint(50, 200) for _ in range(100)]
ax4.hist(daily_sales, bins=15, alpha=0.7, color='orange')
ax4.set_title('일일 판매량 분포')
ax4.set_xlabel('판매량')
ax4.set_ylabel('빈도')

plt.tight_layout()
plt.show()
```

---

## 18-4. 차트 커스터마이징

전문적이고 보기 좋은 차트를 만들기 위해서는 색상, 폰트, 스타일 등을 세심하게 조정해야 합니다. matplotlib는 다양한 커스터마이징 옵션을 제공하여 원하는 스타일의 차트를 만들 수 있어요.

### 고급 스타일링

```python title=advanced_styling.py
import matplotlib.pyplot as plt
import numpy as np

# 한글 폰트 설정 (macOS)
plt.rc('font', family='Apple SD Gothic Neo')
plt.rcParams['axes.unicode_minus'] = False

# 데이터 준비
categories = ['마케팅', '개발', '영업', '디자인', '기획']
budget_2023 = [300, 500, 250, 200, 150]
budget_2024 = [350, 600, 280, 250, 180]

# 스타일 설정
plt.style.use('seaborn-v0_8')
fig, ax = plt.subplots(figsize=(10, 6))

# 막대 위치 설정
x = np.arange(len(categories))
width = 0.35

# 막대 그래프 생성
bars1 = ax.bar(x - width/2, budget_2023, width, label='2023년',
               color='#3498db', alpha=0.8)
bars2 = ax.bar(x + width/2, budget_2024, width, label='2024년',
               color='#e74c3c', alpha=0.8)

# 차트 꾸미기
ax.set_title('부서별 예산 비교', fontsize=18, fontweight='bold', pad=20)
ax.set_xlabel('부서', fontsize=14)
ax.set_ylabel('예산 (만원)', fontsize=14)
ax.set_xticks(x)
ax.set_xticklabels(categories)
ax.legend(fontsize=12)

# 격자 설정
ax.grid(True, axis='y', alpha=0.3, linestyle='--')
ax.set_axisbelow(True)

# 막대 위에 값 표시
def add_value_labels(bars):
    for bar in bars:
        height = bar.get_height()
        ax.text(bar.get_x() + bar.get_width()/2., height + 5,
                f'{int(height)}', ha='center', va='bottom', fontsize=10)

add_value_labels(bars1)
add_value_labels(bars2)

plt.tight_layout()
plt.show()
```

---

## 실습: 성적 분석 차트 만들기

학생들의 성적 데이터를 종합적으로 분석하는 시각화 프로그램을 만들어보겠습니다. 이 실습에서는 여러 과목의 성적을 다양한 관점에서 시각화해볼 거예요.

```python title=grade_analysis.py
import matplotlib.pyplot as plt
import numpy as np
import random

# 운영체제별 폰트 깨짐 방지 설정
# plt.rc('font', family='Apple SD Gothic Neo') # macOS
# plt.rc('font', family='DejaVu Sans') # Linux
plt.rc('font', family='Malgun Gothic') # Windows
plt.rcParams['axes.unicode_minus'] = False

class GradeAnalyzer:
    def __init__(self):
        self.subjects = ['수학', '영어', '과학', '국어', '사회']
        self.students = [f'학생{i+1}' for i in range(20)]
        self.grades = self.generate_sample_data()

    def generate_sample_data(self):
        """샘플 성적 데이터 생성"""
        grades = {}
        for subject in self.subjects:
            grades[subject] = [random.randint(60, 100) for _ in range(20)]
        return grades

    def create_dashboard(self):
        """성적 분석 대시보드 생성"""
        fig = plt.figure(figsize=(15, 10))

        # 1. 과목별 평균 점수 막대 그래프
        ax1 = plt.subplot(2, 3, 1)
        avg_scores = [np.mean(self.grades[subject]) for subject in self.subjects]
        bars = ax1.bar(self.subjects, avg_scores, color='skyblue', alpha=0.8)
        ax1.set_title('과목별 평균 점수', fontsize=14, fontweight='bold')
        ax1.set_ylabel('평균 점수')
        ax1.set_ylim(0, 100)

        # 막대 위에 평균값 표시
        for bar, avg in zip(bars, avg_scores):
            ax1.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 1,
                    f'{avg:.1f}', ha='center', va='bottom')

        # 2. 성적 분포 히스토그램
        ax2 = plt.subplot(2, 3, 2)
        all_scores = []
        for scores in self.grades.values():
            all_scores.extend(scores)
        ax2.hist(all_scores, bins=15, alpha=0.7, color='lightgreen', edgecolor='black')
        ax2.set_title('전체 성적 분포', fontsize=14, fontweight='bold')
        ax2.set_xlabel('점수')
        ax2.set_ylabel('빈도')

        # 3. 과목별 성적 상자 그림
        ax3 = plt.subplot(2, 3, 3)
        data_for_boxplot = [self.grades[subject] for subject in self.subjects]
        bp = ax3.boxplot(data_for_boxplot, labels=self.subjects, patch_artist=True)
        ax3.set_title('과목별 성적 분포', fontsize=14, fontweight='bold')
        ax3.set_ylabel('점수')

        # 상자 그림 색상 설정
        colors = ['lightblue', 'lightgreen', 'lightcoral', 'lightyellow', 'lightpink']
        for patch, color in zip(bp['boxes'], colors):
            patch.set_facecolor(color)

        # 4. 학생별 총점 순위
        ax4 = plt.subplot(2, 3, 4)
        total_scores = []
        for i in range(20):
            total = sum(self.grades[subject][i] for subject in self.subjects)
            total_scores.append(total)

        sorted_indices = sorted(range(20), key=lambda x: total_scores[x], reverse=True)
        top_10_students = [self.students[i] for i in sorted_indices[:10]]
        top_10_scores = [total_scores[i] for i in sorted_indices[:10]]

        ax4.barh(top_10_students, top_10_scores, color='orange', alpha=0.8)
        ax4.set_title('상위 10명 총점', fontsize=14, fontweight='bold')
        ax4.set_xlabel('총점')

        # 5. 과목간 상관관계 (수학 vs 과학)
        ax5 = plt.subplot(2, 3, 5)
        ax5.scatter(self.grades['수학'], self.grades['과학'], alpha=0.6, color='purple')
        ax5.set_title('수학 vs 과학 점수 상관관계', fontsize=14, fontweight='bold')
        ax5.set_xlabel('수학 점수')
        ax5.set_ylabel('과학 점수')
        ax5.grid(True, alpha=0.3)

        # 6. 과목별 등급 분포 파이 차트
        ax6 = plt.subplot(2, 3, 6)
        math_grades = self.grades['수학']
        grade_counts = {'A': 0, 'B': 0, 'C': 0, 'D': 0}

        for score in math_grades:
            if score >= 90:
                grade_counts['A'] += 1
            elif score >= 80:
                grade_counts['B'] += 1
            elif score >= 70:
                grade_counts['C'] += 1
            else:
                grade_counts['D'] += 1

        ax6.pie(grade_counts.values(), labels=grade_counts.keys(),
                autopct='%1.1f%%', startangle=90, colors=['gold', 'lightgreen', 'orange', 'lightcoral'])
        ax6.set_title('수학 등급 분포', fontsize=14, fontweight='bold')

        plt.tight_layout()
        plt.show()

    def print_summary(self):
        """성적 요약 정보 출력"""
        print("=== 성적 분석 요약 ===")
        for subject in self.subjects:
            scores = self.grades[subject]
            print(f"{subject}: 평균 {np.mean(scores):.1f}, 최고 {max(scores)}, 최저 {min(scores)}")

# 실행
if __name__ == "__main__":
    analyzer = GradeAnalyzer()
    analyzer.create_dashboard()
    analyzer.print_summary()
```

---

## 확장 과제: 인터랙티브 대시보드 만들기

더 고급 기능을 원한다면 plotly나 bokeh 같은 인터랙티브 시각화 라이브러리를 사용해보세요:

```python title=interactive_dashboard.py
# plotly 설치 필요: pip install plotly
import plotly.graph_objects as go
from plotly.subplots import make_subplots
import random

def create_interactive_dashboard():
    """인터랙티브 대시보드 생성"""

    # 샘플 데이터
    months = ['1월', '2월', '3월', '4월', '5월', '6월']
    sales_data = {
        '온라인': [100, 120, 140, 160, 180, 200],
        '오프라인': [80, 85, 90, 88, 92, 95],
        '모바일': [50, 65, 80, 100, 120, 140]
    }

    # 서브플롯 생성
    fig = make_subplots(
        rows=2, cols=2,
        subplot_titles=('월별 판매 추이', '채널별 비교', '총 판매량', '성장률'),
        specs=[[{"secondary_y": False}, {"type": "bar"}],
               [{"type": "pie"}, {"type": "scatter"}]]
    )

    # 선 그래프
    for channel, values in sales_data.items():
        fig.add_trace(
            go.Scatter(x=months, y=values, name=channel, mode='lines+markers'),
            row=1, col=1
        )

    # 막대 그래프
    fig.add_trace(
        go.Bar(x=list(sales_data.keys()),
               y=[sum(values) for values in sales_data.values()],
               name='총 판매량'),
        row=1, col=2
    )

    # 파이 차트
    fig.add_trace(
        go.Pie(labels=list(sales_data.keys()),
               values=[sum(values) for values in sales_data.values()]),
        row=2, col=1
    )

    # 산점도
    growth_rates = [random.uniform(5, 25) for _ in range(6)]
    fig.add_trace(
        go.Scatter(x=months, y=growth_rates, mode='markers+lines',
                   name='성장률', marker=dict(size=10)),
        row=2, col=2
    )

    fig.update_layout(height=600, showlegend=True, title_text="판매 분석 대시보드")
    fig.show()

# 실행 (주석 해제하여 사용)
# create_interactive_dashboard()
```

---

## 연습문제

1. **기본 연습**: 자신의 일주일 공부시간을 막대그래프로 나타내보세요.

2. **응용 연습**: 가상의 주식 데이터를 생성하여 30일간의 주가 변동을 선 그래프로 시각화하고, 이동평균선을 추가해보세요.

3. **종합 연습**: 학교 동아리 활동 참여도 데이터를 만들어 다음을 포함한 대시보드를 제작하세요:
   - 동아리별 회원 수 막대그래프
   - 월별 활동 참여율 선 그래프
   - 학년별 참여 비율 파이차트
   - 활동 만족도 분포 히스토그램

## 핵심 포인트

- **적절한 그래프 선택**: 데이터의 특성에 맞는 시각화 방법을 선택하는 것이 중요합니다
- **가독성 우선**: 화려한 것보다는 명확하고 이해하기 쉬운 차트가 좋습니다
- **일관성 유지**: 색상, 폰트, 스타일을 일관되게 유지하여 전문적인 느낌을 줍니다
- **스토리텔링**: 데이터가 전달하고자 하는 메시지를 명확히 표현해야 합니다

데이터 시각화는 단순히 그래프를 그리는 것이 아니라, 데이터 속에 숨겨진 인사이트를 찾아내고 이를 효과적으로 전달하는 기술입니다. matplotlib를 마스터하면 데이터 분석 능력이 한 단계 향상될 거예요!
