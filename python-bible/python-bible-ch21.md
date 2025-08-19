---
title: '21. 미니 프로젝트 2: 가계부 프로그램'
slug: python-project-expense-tracker
description: '수입과 지출 관리부터 데이터 시각화까지. JSON 파일과 matplotlib을 활용한 실용적인 가계부 애플리케이션'
keywords: [파이썬 가계부, JSON 파일 처리, 데이터 시각화, matplotlib, 개인재정관리, CLI 프로그램]
sidebar_position: 21
---

# 21. 미니 프로젝트 2: 가계부 프로그램

개인 재정 관리는 성인이 되어 반드시 익혀야 할 중요한 생활 기술입니다. 이번 챕터에서는 지금까지 배운 파이썬 지식을 종합해서 실용적인 가계부 프로그램을 만들어보겠습니다. 이 프로젝트를 통해 클래스 설계, 파일 입출력, 데이터 시각화, 예외 처리 등의 기술을 실제 문제에 적용하는 경험을 쌓을 수 있습니다. 단순한 수입과 지출 기록에서 시작해서 카테고리별 분석, 월별 통계, 그래프 시각화까지 단계적으로 기능을 확장해 나가겠습니다.

## 학습 목표

이 챕터를 완료하면 다음과 같은 능력을 갖게 됩니다:

- 실제 문제를 객체지향적으로 분석하고 설계할 수 있습니다
- JSON 파일을 활용한 데이터 영구 저장 기능을 구현할 수 있습니다
- 사용자 친화적인 CLI 인터페이스를 설계할 수 있습니다
- matplotlib을 활용해 데이터를 시각화할 수 있습니다
- 체계적인 단위 테스트를 작성할 수 있습니다

## 21-1 프로젝트 개요

가계부 프로그램은 개인의 수입과 지출을 체계적으로 관리하는 도구입니다. 우리가 만들 프로그램은 다음과 같은 핵심 기능을 제공합니다: 거래 내역 추가/수정/삭제, 카테고리별 분류, 월별/연도별 통계 조회, 예산 설정 및 관리, 그래프를 통한 시각화입니다.

이 프로젝트의 특별한 점은 단순히 기능만 구현하는 것이 아니라, 실제 사용자가 매일 사용할 수 있을 정도로 완성도 높은 프로그램을 만든다는 것입니다. 또한 코드의 재사용성과 확장성을 고려한 설계를 통해 향후 웹 애플리케이션이나 모바일 앱으로 발전시킬 수 있는 기반을 마련합니다.

### 주요 기능 명세

- **거래 관리**: 수입/지출 내역 추가, 수정, 삭제
- **카테고리 시스템**: 식비, 교통비, 문화생활 등 사용자 정의 카테고리
- **통계 기능**: 월별, 연도별, 카테고리별 집계
- **예산 관리**: 카테고리별 예산 설정 및 초과 알림
- **데이터 시각화**: 파이차트, 막대그래프를 통한 지출 분석
- **데이터 백업**: JSON 형태로 안전한 데이터 저장

## 21-2 데이터 구조 설계

효율적인 가계부 프로그램을 만들기 위해서는 먼저 데이터를 어떻게 구조화할지 설계해야 합니다. 우리는 거래(Transaction), 카테고리(Category), 예산(Budget) 등의 개념을 클래스로 모델링하겠습니다.

거래 정보는 날짜, 금액, 카테고리, 설명, 거래 유형(수입/지출) 등의 속성을 가져야 합니다. 카테고리는 이름과 색상 정보를, 예산은 카테고리별 한도액과 기간 정보를 포함해야 합니다. 이러한 구조를 바탕으로 데이터 간의 관계를 명확히 정의하고, 향후 기능 확장 시에도 유연하게 대응할 수 있도록 설계하겠습니다.

```python title=models.py
from datetime import datetime
from typing import List, Dict, Optional
from enum import Enum
import json

class TransactionType(Enum):
    INCOME = "수입"
    EXPENSE = "지출"

class Transaction:
    """개별 거래를 나타내는 클래스"""

    def __init__(self, amount: float, category: str, description: str,
                 transaction_type: TransactionType, date: Optional[datetime] = None):
        self.id = self._generate_id()
        self.amount = amount
        self.category = category
        self.description = description
        self.transaction_type = transaction_type
        self.date = date or datetime.now()

    def _generate_id(self) -> str:
        """고유 ID 생성"""
        import uuid
        return str(uuid.uuid4())[:8]

    def to_dict(self) -> Dict:
        """딕셔너리로 변환 (JSON 저장용)"""
        return {
            'id': self.id,
            'amount': self.amount,
            'category': self.category,
            'description': self.description,
            'transaction_type': self.transaction_type.value,
            'date': self.date.isoformat()
        }

    @classmethod
    def from_dict(cls, data: Dict) -> 'Transaction':
        """딕셔너리에서 객체 생성 (JSON 로드용)"""
        transaction = cls(
            amount=data['amount'],
            category=data['category'],
            description=data['description'],
            transaction_type=TransactionType(data['transaction_type'])
        )
        transaction.id = data['id']
        transaction.date = datetime.fromisoformat(data['date'])
        return transaction

class Category:
    """카테고리를 나타내는 클래스"""

    def __init__(self, name: str, color: str = "#3498db"):
        self.name = name
        self.color = color

    def to_dict(self) -> Dict:
        return {'name': self.name, 'color': self.color}

    @classmethod
    def from_dict(cls, data: Dict) -> 'Category':
        return cls(data['name'], data['color'])

class Budget:
    """예산을 나타내는 클래스"""

    def __init__(self, category: str, limit: float, year: int, month: int):
        self.category = category
        self.limit = limit
        self.year = year
        self.month = month

    def to_dict(self) -> Dict:
        return {
            'category': self.category,
            'limit': self.limit,
            'year': self.year,
            'month': self.month
        }

    @classmethod
    def from_dict(cls, data: Dict) -> 'Budget':
        return cls(data['category'], data['limit'], data['year'], data['month'])
```

## 21-3 기능별 구현

이제 가계부의 핵심 기능들을 담당하는 메인 클래스를 구현해보겠습니다. HouseholdBudget 클래스는 모든 거래와 카테고리, 예산 정보를 관리하는 중앙 컨트롤러 역할을 합니다.

이 클래스는 거래 추가/삭제/수정, 카테고리 관리, 각종 통계 조회 기능을 제공합니다. 특히 월별 지출 분석, 카테고리별 집계, 예산 대비 실제 지출 비교 등의 기능을 통해 사용자가 자신의 소비 패턴을 쉽게 파악할 수 있도록 도와줍니다.

```python title=household_budget.py
from typing import List, Dict, Optional
from datetime import datetime, timedelta
from collections import defaultdict
import calendar

from models import Transaction, Category, Budget, TransactionType

class HouseholdBudget:
    """가계부 메인 클래스"""

    def __init__(self):
        self.transactions: List[Transaction] = []
        self.categories: Dict[str, Category] = {}
        self.budgets: List[Budget] = []
        self._setup_default_categories()

    def _setup_default_categories(self):
        """기본 카테고리 설정"""
        default_categories = [
            ('식비', '#e74c3c'),
            ('교통비', '#3498db'),
            ('문화생활', '#9b59b6'),
            ('쇼핑', '#f39c12'),
            ('의료비', '#1abc9c'),
            ('급여', '#27ae60'),
            ('부수입', '#2ecc71')
        ]

        for name, color in default_categories:
            self.categories[name] = Category(name, color)

    def add_transaction(self, amount: float, category: str, description: str,
                       transaction_type: TransactionType, date: Optional[datetime] = None) -> str:
        """거래 추가"""
        if category not in self.categories:
            raise ValueError(f"존재하지 않는 카테고리입니다: {category}")

        transaction = Transaction(amount, category, description, transaction_type, date)
        self.transactions.append(transaction)
        return transaction.id

    def remove_transaction(self, transaction_id: str) -> bool:
        """거래 삭제"""
        for i, transaction in enumerate(self.transactions):
            if transaction.id == transaction_id:
                del self.transactions[i]
                return True
        return False

    def get_monthly_summary(self, year: int, month: int) -> Dict:
        """월별 요약 정보"""
        start_date = datetime(year, month, 1)
        if month == 12:
            end_date = datetime(year + 1, 1, 1) - timedelta(days=1)
        else:
            end_date = datetime(year, month + 1, 1) - timedelta(days=1)

        monthly_transactions = [
            t for t in self.transactions
            if start_date <= t.date <= end_date
        ]

        total_income = sum(t.amount for t in monthly_transactions
                          if t.transaction_type == TransactionType.INCOME)
        total_expense = sum(t.amount for t in monthly_transactions
                           if t.transaction_type == TransactionType.EXPENSE)

        # 카테고리별 지출
        category_expenses = defaultdict(float)
        for t in monthly_transactions:
            if t.transaction_type == TransactionType.EXPENSE:
                category_expenses[t.category] += t.amount

        return {
            'year': year,
            'month': month,
            'total_income': total_income,
            'total_expense': total_expense,
            'net_income': total_income - total_expense,
            'category_expenses': dict(category_expenses),
            'transaction_count': len(monthly_transactions)
        }

    def check_budget_status(self, year: int, month: int) -> Dict:
        """예산 대비 지출 현황 확인"""
        monthly_summary = self.get_monthly_summary(year, month)
        budget_status = {}

        for budget in self.budgets:
            if budget.year == year and budget.month == month:
                spent = monthly_summary['category_expenses'].get(budget.category, 0)
                remaining = budget.limit - spent
                usage_rate = (spent / budget.limit) * 100 if budget.limit > 0 else 0

                budget_status[budget.category] = {
                    'limit': budget.limit,
                    'spent': spent,
                    'remaining': remaining,
                    'usage_rate': usage_rate,
                    'is_over': spent > budget.limit
                }

        return budget_status

    def add_category(self, name: str, color: str = "#3498db"):
        """카테고리 추가"""
        if name in self.categories:
            raise ValueError(f"이미 존재하는 카테고리입니다: {name}")
        self.categories[name] = Category(name, color)

    def set_budget(self, category: str, limit: float, year: int, month: int):
        """예산 설정"""
        if category not in self.categories:
            raise ValueError(f"존재하지 않는 카테고리입니다: {category}")

        # 기존 예산이 있으면 업데이트, 없으면 새로 추가
        for budget in self.budgets:
            if (budget.category == category and
                budget.year == year and budget.month == month):
                budget.limit = limit
                return

        self.budgets.append(Budget(category, limit, year, month))
```

## 21-4 파일 저장 기능 추가

데이터의 영구 보존을 위해 JSON 형태로 파일에 저장하고 불러오는 기능을 구현해보겠습니다. 이 기능을 통해 프로그램을 종료했다가 다시 시작해도 기존 데이터를 그대로 유지할 수 있습니다.

JSON 형태로 저장하는 이유는 사람이 읽기 쉽고, 다른 프로그램과의 호환성이 좋으며, 파이썬에서 쉽게 처리할 수 있기 때문입니다. 저장 시에는 모든 객체를 딕셔너리로 변환하고, 로드 시에는 다시 객체로 복원하는 과정을 거칩니다.

```python title=data_manager.py
import json
import os
from typing import Dict, Any
from datetime import datetime

from household_budget import HouseholdBudget
from models import Transaction, Category, Budget

class DataManager:
    """데이터 저장/로드 관리 클래스"""

    def __init__(self, data_file: str = "household_data.json"):
        self.data_file = data_file

    def save_data(self, budget: HouseholdBudget) -> bool:
        """데이터를 JSON 파일로 저장"""
        try:
            data = {
                'transactions': [t.to_dict() for t in budget.transactions],
                'categories': {name: cat.to_dict() for name, cat in budget.categories.items()},
                'budgets': [b.to_dict() for b in budget.budgets],
                'last_updated': datetime.now().isoformat()
            }

            # 백업 파일 생성
            if os.path.exists(self.data_file):
                backup_file = f"{self.data_file}.backup"
                os.rename(self.data_file, backup_file)

            with open(self.data_file, 'w', encoding='utf-8') as f:
                json.dump(data, f, indent=2, ensure_ascii=False)

            return True

        except Exception as e:
            print(f"데이터 저장 중 오류 발생: {e}")
            return False

    def load_data(self) -> HouseholdBudget:
        """JSON 파일에서 데이터 로드"""
        budget = HouseholdBudget()

        if not os.path.exists(self.data_file):
            return budget

        try:
            with open(self.data_file, 'r', encoding='utf-8') as f:
                data = json.load(f)

            # 카테고리 로드
            budget.categories = {}
            for name, cat_data in data.get('categories', {}).items():
                budget.categories[name] = Category.from_dict(cat_data)

            # 거래 내역 로드
            budget.transactions = []
            for t_data in data.get('transactions', []):
                budget.transactions.append(Transaction.from_dict(t_data))

            # 예산 로드
            budget.budgets = []
            for b_data in data.get('budgets', []):
                budget.budgets.append(Budget.from_dict(b_data))

            print(f"데이터 로드 완료: {len(budget.transactions)}개 거래, "
                  f"{len(budget.categories)}개 카테고리")

        except Exception as e:
            print(f"데이터 로드 중 오류 발생: {e}")
            print("새로운 가계부를 시작합니다.")

        return budget

    def export_csv(self, budget: HouseholdBudget, filename: str = None) -> bool:
        """거래 내역을 CSV로 내보내기"""
        if filename is None:
            filename = f"transactions_{datetime.now().strftime('%Y%m%d')}.csv"

        try:
            import csv

            with open(filename, 'w', newline='', encoding='utf-8') as f:
                writer = csv.writer(f)
                writer.writerow(['날짜', '카테고리', '금액', '유형', '설명'])

                for transaction in sorted(budget.transactions, key=lambda x: x.date):
                    writer.writerow([
                        transaction.date.strftime('%Y-%m-%d'),
                        transaction.category,
                        transaction.amount,
                        transaction.transaction_type.value,
                        transaction.description
                    ])

            print(f"CSV 파일로 내보내기 완료: {filename}")
            return True

        except Exception as e:
            print(f"CSV 내보내기 중 오류 발생: {e}")
            return False
```

## 21-5 데이터 시각화 기능

matplotlib을 활용해서 가계부 데이터를 다양한 그래프로 시각화하는 기능을 구현해보겠습니다. 파이차트로 카테고리별 지출 비율을 보고, 막대그래프로 월별 수입/지출 추이를 확인할 수 있습니다.

시각화는 단순히 데이터를 예쁘게 보여주는 것을 넘어서, 사용자가 자신의 소비 패턴을 직관적으로 이해할 수 있게 도와줍니다. 어느 카테고리에서 돈을 많이 쓰는지, 수입 대비 지출이 어떤 추세인지 한눈에 파악할 수 있게 됩니다.

```python title=visualizer.py
import matplotlib.pyplot as plt
import matplotlib.font_manager as fm
from typing import Dict, List
import calendar
from datetime import datetime, timedelta

from household_budget import HouseholdBudget
from models import TransactionType

# 한글 폰트 설정
plt.rcParams['font.family'] = ['Malgun Gothic', 'AppleGothic', 'DejaVu Sans']
plt.rcParams['axes.unicode_minus'] = False

class BudgetVisualizer:
    """가계부 데이터 시각화 클래스"""

    def __init__(self, budget: HouseholdBudget):
        self.budget = budget

    def plot_monthly_expenses_pie(self, year: int, month: int, save_path: str = None):
        """월별 카테고리별 지출 파이차트"""
        summary = self.budget.get_monthly_summary(year, month)

        if not summary['category_expenses']:
            print("해당 월에 지출 데이터가 없습니다.")
            return

        categories = list(summary['category_expenses'].keys())
        amounts = list(summary['category_expenses'].values())
        colors = [self.budget.categories[cat].color for cat in categories]

        plt.figure(figsize=(10, 8))
        wedges, texts, autotexts = plt.pie(amounts, labels=categories, colors=colors,
                                          autopct='%1.1f%%', startangle=90)

        plt.title(f'{year}년 {month}월 카테고리별 지출 현황\n'
                 f'총 지출: {summary["total_expense"]:,.0f}원', fontsize=14, fontweight='bold')

        # 범례 추가
        plt.legend(wedges, [f'{cat}: {amt:,.0f}원' for cat, amt in zip(categories, amounts)],
                  title="카테고리별 금액", loc="center left", bbox_to_anchor=(1, 0, 0.5, 1))

        plt.tight_layout()

        if save_path:
            plt.savefig(save_path, dpi=300, bbox_inches='tight')

        plt.show()

    def plot_income_expense_trend(self, months: int = 6, save_path: str = None):
        """최근 N개월 수입/지출 추이 막대그래프"""
        end_date = datetime.now()
        start_date = end_date - timedelta(days=months * 31)

        monthly_data = []
        labels = []

        current_date = start_date.replace(day=1)
        while current_date <= end_date:
            summary = self.budget.get_monthly_summary(current_date.year, current_date.month)
            monthly_data.append(summary)
            labels.append(f"{current_date.year}.{current_date.month:02d}")

            # 다음 달로 이동
            if current_date.month == 12:
                current_date = current_date.replace(year=current_date.year + 1, month=1)
            else:
                current_date = current_date.replace(month=current_date.month + 1)

        incomes = [data['total_income'] for data in monthly_data]
        expenses = [data['total_expense'] for data in monthly_data]

        plt.figure(figsize=(12, 6))

        x = range(len(labels))
        width = 0.35

        plt.bar([i - width/2 for i in x], incomes, width, label='수입', color='#27ae60', alpha=0.8)
        plt.bar([i + width/2 for i in x], expenses, width, label='지출', color='#e74c3c', alpha=0.8)

        plt.xlabel('월', fontsize=12)
        plt.ylabel('금액 (원)', fontsize=12)
        plt.title(f'최근 {months}개월 수입/지출 추이', fontsize=14, fontweight='bold')
        plt.xticks(x, labels, rotation=45)
        plt.legend()
        plt.grid(True, alpha=0.3)

        # 금액 형식 지정
        plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda x, p: f'{x:,.0f}'))

        plt.tight_layout()

        if save_path:
            plt.savefig(save_path, dpi=300, bbox_inches='tight')

        plt.show()

    def plot_budget_comparison(self, year: int, month: int, save_path: str = None):
        """예산 대비 실제 지출 비교"""
        budget_status = self.budget.check_budget_status(year, month)

        if not budget_status:
            print("해당 월에 설정된 예산이 없습니다.")
            return

        categories = list(budget_status.keys())
        budgets = [budget_status[cat]['limit'] for cat in categories]
        spent = [budget_status[cat]['spent'] for cat in categories]

        plt.figure(figsize=(12, 6))

        x = range(len(categories))
        width = 0.35

        bars1 = plt.bar([i - width/2 for i in x], budgets, width,
                       label='예산', color='#3498db', alpha=0.8)
        bars2 = plt.bar([i + width/2 for i in x], spent, width,
                       label='실제 지출', color='#e74c3c', alpha=0.8)

        # 예산 초과 표시
        for i, cat in enumerate(categories):
            if budget_status[cat]['is_over']:
                plt.text(i, spent[i] + max(spent) * 0.02, '초과!',
                        ha='center', va='bottom', color='red', fontweight='bold')

        plt.xlabel('카테고리', fontsize=12)
        plt.ylabel('금액 (원)', fontsize=12)
        plt.title(f'{year}년 {month}월 예산 대비 실제 지출', fontsize=14, fontweight='bold')
        plt.xticks(x, categories, rotation=45)
        plt.legend()
        plt.grid(True, alpha=0.3)

        plt.gca().yaxis.set_major_formatter(plt.FuncFormatter(lambda x, p: f'{x:,.0f}'))

        plt.tight_layout()

        if save_path:
            plt.savefig(save_path, dpi=300, bbox_inches='tight')

        plt.show()
```

## 21-6 단위 테스트 추가

안정적인 프로그램을 만들기 위해 주요 기능들에 대한 단위 테스트를 작성해보겠습니다. pytest를 사용해서 각 메서드가 예상대로 동작하는지 검증합니다.

테스트 코드는 프로그램의 품질을 보장하고, 코드 수정 시 기존 기능이 제대로 작동하는지 확인하는 안전장치 역할을 합니다. 특히 금융 관련 프로그램에서는 계산의 정확성이 매우 중요하므로 철저한 테스트가 필수입니다.

```python title=test_budget.py
import pytest
from datetime import datetime, timedelta
from household_budget import HouseholdBudget
from models import TransactionType, Transaction
from data_manager import DataManager
import tempfile
import os

class TestHouseholdBudget:
    """가계부 기능 테스트"""

    def setup_method(self):
        """각 테스트 전에 실행"""
        self.budget = HouseholdBudget()

    def test_add_transaction(self):
        """거래 추가 테스트"""
        # 정상적인 거래 추가
        transaction_id = self.budget.add_transaction(
            amount=50000,
            category='식비',
            description='점심',
            transaction_type=TransactionType.EXPENSE
        )

        assert len(self.budget.transactions) == 1
        assert self.budget.transactions[0].amount == 50000
        assert transaction_id is not None

    def test_add_transaction_invalid_category(self):
        """존재하지 않는 카테고리로 거래 추가 시 오류 발생 테스트"""
        with pytest.raises(ValueError):
            self.budget.add_transaction(
                amount=10000,
                category='존재하지않는카테고리',
                description='테스트',
                transaction_type=TransactionType.EXPENSE
            )

    def test_remove_transaction(self):
        """거래 삭제 테스트"""
        # 거래 추가
        transaction_id = self.budget.add_transaction(
            amount=30000,
            category='교통비',
            description='버스',
            transaction_type=TransactionType.EXPENSE
        )

        # 거래 삭제
        result = self.budget.remove_transaction(transaction_id)
        assert result is True
        assert len(self.budget.transactions) == 0

        # 존재하지 않는 거래 삭제 시도
        result = self.budget.remove_transaction('nonexistent')
        assert result is False

    def test_monthly_summary(self):
        """월별 요약 테스트"""
        # 이번 달 거래 추가
        current_date = datetime.now()

        self.budget.add_transaction(
            amount=1000000,
            category='급여',
            description='월급',
            transaction_type=TransactionType.INCOME,
            date=current_date
        )

        self.budget.add_transaction(
            amount=300000,
            category='식비',
            description='식비',
            transaction_type=TransactionType.EXPENSE,
            date=current_date
        )

        summary = self.budget.get_monthly_summary(current_date.year, current_date.month)

        assert summary['total_income'] == 1000000
        assert summary['total_expense'] == 300000
        assert summary['net_income'] == 700000
        assert summary['category_expenses']['식비'] == 300000

    def test_budget_management(self):
        """예산 관리 테스트"""
        current_date = datetime.now()

        # 예산 설정
        self.budget.set_budget('식비', 500000, current_date.year, current_date.month)

        # 지출 추가
        self.budget.add_transaction(
            amount=300000,
            category='식비',
            description='식비',
            transaction_type=TransactionType.EXPENSE,
            date=current_date
        )

        # 예산 상태 확인
        status = self.budget.check_budget_status(current_date.year, current_date.month)

        assert '식비' in status
        assert status['식비']['limit'] == 500000
        assert status['식비']['spent'] == 300000
        assert status['식비']['remaining'] == 200000
        assert status['식비']['is_over'] is False

class TestDataManager:
    """데이터 관리 테스트"""

    def test_save_and_load_data(self):
        """데이터 저장/로드 테스트"""
        # 임시 파일 생성
        with tempfile.NamedTemporaryFile(mode='w', suffix='.json', delete=False) as f:
            temp_file = f.name

        try:
            # 테스트 데이터 생성
            budget = HouseholdBudget()
            budget.add_transaction(
                amount=50000,
                category='식비',
                description='테스트',
                transaction_type=TransactionType.EXPENSE
            )

            # 데이터 저장
            data_manager = DataManager(temp_file)
            result = data_manager.save_data(budget)
            assert result is True

            # 데이터 로드
            loaded_budget = data_manager.load_data()
            assert len(loaded_budget.transactions) == 1
            assert loaded_budget.transactions[0].amount == 50000
            assert loaded_budget.transactions[0].category == '식비'

        finally:
            # 임시 파일 삭제
            if os.path.exists(temp_file):
                os.unlink(temp_file)

if __name__ == '__main__':
    pytest.main([__file__, '-v'])
```

## GitHub 프로젝트 관리

마지막으로 완성된 가계부 프로그램을 GitHub에 올려서 버전 관리를 해보겠습니다. 프로젝트를 체계적으로 관리하고 다른 개발자들과 협업할 수 있는 환경을 구축하는 것은 현대 개발에서 필수적인 스킬입니다.

```bash
# 프로젝트 초기화
git init
git add .
git commit -m "가계부 프로그램 초기 구현"

# GitHub 저장소와 연결
git remote add origin https://github.com/your-username/household-budget.git
git push -u origin main

# 기능별 브랜치 생성
git checkout -b feature/visualization
git checkout -b feature/data-export
```

## 정리 및 다음 단계

이번 챕터에서는 객체지향 프로그래밍의 핵심 개념들을 활용해서 실용적인 가계부 프로그램을 완성했습니다. 단순한 데이터 입력에서 시작해서 시각화, 예산 관리, 데이터 저장까지 포괄적인 기능을 구현했습니다.

이 프로젝트를 통해 배운 것들을 바탕으로 다음과 같은 확장을 고려해볼 수 있습니다: 웹 인터페이스 추가, 은행 API 연동을 통한 자동 거래 입력, 머신러닝을 활용한 지출 패턴 분석과 예측, 모바일 앱 개발, 다중 사용자 지원 기능입니다.

가장 중요한 것은 실제로 이 프로그램을 사용해보면서 불편한 점을 찾아 개선해나가는 것입니다. 사용자의 피드백을 바탕으로 한 지속적인 개선이야말로 좋은 소프트웨어를 만드는 핵심입니다.
