---
title: '15. 정규표현식'
slug: python-regular-expressions
description: '정규표현식 기초부터 고급 패턴, re 모듈 활용법까지 텍스트 처리의 강력한 도구 완전 정복'
keywords: [정규표현식 regex, re 모듈, 패턴 매칭, 텍스트 처리, 문자열 검색, 데이터 추출]
sidebar_position: 15
---

# 15. 정규표현식

## 학습 목표

이 챕터를 마치면 여러분은 정규표현식의 기본 개념부터 실무에서 자주 사용되는 패턴까지 완전히 이해하게 됩니다. 텍스트 데이터에서 원하는 정보를 정확하게 찾아내고, 데이터 검증 및 추출 작업을 자동화할 수 있는 능력을 갖추게 될 것입니다.

---

## 15-1 정규표현식이란?

정규표현식(Regular Expression, 줄여서 Regex)은 문자열에서 특정한 패턴을 찾거나 검증하기 위해 사용하는 강력한 도구입니다. 복잡한 문자열 처리 작업을 간단한 패턴으로 표현할 수 있어서, 데이터 검증, 텍스트 분석, 로그 처리 등 다양한 분야에서 활용됩니다. 처음에는 암호 같아 보이지만, 패턴을 익히면 텍스트 처리의 강력한 무기가 됩니다.

정규표현식은 특별한 문자들(메타문자)을 사용해서 패턴을 정의합니다. 예를 들어, 이메일 주소나 전화번호 같은 특정 형식의 데이터를 찾거나 검증할 때 매우 유용합니다.

```python title=basic_regex.py
import re

# 간단한 패턴 매칭 예제
text = "안녕하세요, 제 이메일은 user@example.com 입니다."
pattern = r'\w+@\w+\.\w+'

# 이메일 패턴 찾기
match = re.search(pattern, text)
if match:
    print(f"이메일을 찾았습니다: {match.group()}")
else:
    print("이메일을 찾지 못했습니다.")
```

## 15-2 기본 패턴과 메타문자

정규표현식의 핵심은 메타문자를 이해하는 것입니다. 메타문자는 특별한 의미를 가진 문자들로, 패턴을 정의하는 데 사용됩니다. 가장 기본적인 메타문자들부터 차근차근 알아보겠습니다.

### 기본 메타문자

- `.`: 줄바꿈을 제외한 모든 문자 하나
- `*`: 앞의 문자가 0번 이상 반복
- `+`: 앞의 문자가 1번 이상 반복
- `?`: 앞의 문자가 0번 또는 1번
- `^`: 문자열의 시작
- `$`: 문자열의 끝
- `[]`: 문자 클래스 (대괄호 안의 문자 중 하나)
- `|`: OR 연산자

```python title=metacharacters.py
import re

# 다양한 메타문자 예제
texts = [
    "hello",
    "helllo",
    "helo",
    "hi there",
    "Hello World"
]

patterns = {
    r'hel+o': "hel + l이 1번 이상 + o",
    r'hel*o': "hel + l이 0번 이상 + o",
    r'hel?o': "hel + l이 0번 또는 1번 + o",
    r'^h': "h로 시작",
    r'o$': "o로 끝남",
    r'[Hh]ello': "H 또는 h + ello"
}

for pattern, description in patterns.items():
    print(f"\n패턴: {pattern} ({description})")
    for text in texts:
        if re.search(pattern, text):
            print(f"  ✓ '{text}' 매칭")
        else:
            print(f"  ✗ '{text}' 매칭 안됨")
```

### 문자 클래스와 범위

문자 클래스는 특정 범위의 문자들을 매칭할 때 사용합니다.

```python title=character_classes.py
import re

# 문자 클래스 예제
text = "제 전화번호는 010-1234-5678이고, 우편번호는 06234입니다."

patterns = {
    r'\d': "숫자 하나",
    r'\d+': "연속된 숫자",
    r'\d{3}': "정확히 3자리 숫자",
    r'\d{2,4}': "2~4자리 숫자",
    r'[0-9]{3}-[0-9]{4}-[0-9]{4}': "전화번호 패턴",
    r'[0-9]{5}': "5자리 우편번호"
}

for pattern, description in patterns.items():
    matches = re.findall(pattern, text)
    print(f"{description}: {matches}")
```

## 15-3 re 모듈 활용

파이썬에서 정규표현식을 사용하려면 `re` 모듈을 import해야 합니다. 이 모듈은 패턴 매칭을 위한 다양한 함수들을 제공합니다. 각 함수의 특성을 이해하고 상황에 맞게 활용하는 것이 중요합니다.

### 주요 함수들

```python title=re_module_functions.py
import re

sample_text = """
이메일: john@example.com, jane@test.org
전화번호: 010-1234-5678, 02-987-6543
웹사이트: https://www.python.org, http://example.com
"""

# 1. re.search() - 첫 번째 매칭 찾기
email_pattern = r'\w+@\w+\.\w+'
first_email = re.search(email_pattern, sample_text)
if first_email:
    print(f"첫 번째 이메일: {first_email.group()}")

# 2. re.findall() - 모든 매칭 찾기
all_emails = re.findall(email_pattern, sample_text)
print(f"모든 이메일: {all_emails}")

# 3. re.finditer() - 매칭 객체들의 이터레이터
phone_pattern = r'\d{2,3}-\d{3,4}-\d{4}'
for match in re.finditer(phone_pattern, sample_text):
    print(f"전화번호: {match.group()}, 위치: {match.start()}-{match.end()}")

# 4. re.sub() - 치환하기
# 이메일 주소를 마스킹
masked_text = re.sub(r'(\w+)@(\w+)\.(\w+)', r'***@\2.\3', sample_text)
print("마스킹된 텍스트:")
print(masked_text)

# 5. re.split() - 분할하기
data = "사과,바나나;오렌지:포도"
fruits = re.split(r'[,;:]', data)
print(f"과일 목록: {fruits}")
```

### 컴파일된 패턴 사용

반복적으로 같은 패턴을 사용할 때는 미리 컴파일해두면 성능상 이점이 있습니다.

```python title=compiled_pattern.py
import re

# 패턴 컴파일
email_pattern = re.compile(r'\b[A-Za-z0-9._%+-]+@[A-Za-z0-9.-]+\.[A-Z|a-z]{2,}\b')
phone_pattern = re.compile(r'\d{2,3}-\d{3,4}-\d{4}')

# 테스트 데이터
contacts = [
    "김철수: kim@example.com, 010-1234-5678",
    "이영희: lee@test.org, 02-987-6543",
    "박민수: park@company.co.kr, 031-555-7890"
]

print("연락처 정보 추출:")
for contact in contacts:
    email_match = email_pattern.search(contact)
    phone_match = phone_pattern.search(contact)

    name = contact.split(':')[0]
    email = email_match.group() if email_match else "없음"
    phone = phone_match.group() if phone_match else "없음"

    print(f"이름: {name}, 이메일: {email}, 전화: {phone}")
```

## 15-4 그룹화와 캡처

그룹화는 정규표현식의 강력한 기능 중 하나입니다. 괄호를 사용해서 패턴의 일부를 그룹으로 만들면, 매칭된 결과에서 특정 부분만 추출하거나 재사용할 수 있습니다. 복잡한 데이터에서 필요한 정보만 정확히 뽑아낼 때 매우 유용합니다.

```python title=grouping_capture.py
import re

# 다양한 형식의 날짜 처리
date_text = """
오늘은 2025-01-15이고,
어제는 01/14/2025였습니다.
내일은 15.01.2025가 될 것입니다.
"""

# 그룹을 사용한 날짜 패턴
date_patterns = [
    (r'(\d{4})-(\d{2})-(\d{2})', "YYYY-MM-DD"),
    (r'(\d{2})/(\d{2})/(\d{4})', "MM/DD/YYYY"),
    (r'(\d{2})\.(\d{2})\.(\d{4})', "DD.MM.YYYY")
]

print("날짜 추출 및 변환:")
for pattern, format_name in date_patterns:
    matches = re.finditer(pattern, date_text)
    for match in matches:
        print(f"{format_name} 형식: {match.group()}")
        print(f"  그룹1: {match.group(1)}")
        print(f"  그룹2: {match.group(2)}")
        print(f"  그룹3: {match.group(3)}")
        print()

# 이름있는 그룹 사용
log_pattern = r'(?P<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) \[(?P<level>\w+)\] (?P<message>.*)'
log_line = "2025-01-15 14:30:25 [ERROR] 데이터베이스 연결 실패"

match = re.match(log_pattern, log_line)
if match:
    print("로그 분석:")
    print(f"시간: {match.group('timestamp')}")
    print(f"레벨: {match.group('level')}")
    print(f"메시지: {match.group('message')}")
```

## 15-5 실무 예제: 데이터 검증과 추출

실제 업무에서 가장 많이 사용되는 정규표현식 패턴들을 살펴보겠습니다. 이메일, 전화번호, URL 등의 검증은 웹 개발이나 데이터 처리에서 필수적인 작업입니다. 각 패턴의 의미를 정확히 이해하고 필요에 따라 수정해서 사용할 수 있어야 합니다.

```python title=data_validation.py
import re

class DataValidator:
    """데이터 검증을 위한 정규표현식 클래스"""

    def __init__(self):
        # 다양한 검증 패턴들
        self.patterns = {
            'email': re.compile(r'^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$'),
            'phone_kr': re.compile(r'^01[0-9]-\d{3,4}-\d{4}$'),
            'phone_general': re.compile(r'^0\d{1,2}-\d{3,4}-\d{4}$'),
            'url': re.compile(r'^https?://[^\s/$.?#].[^\s]*$'),
            'password': re.compile(r'^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$'),
            'korean_name': re.compile(r'^[가-힣]{2,4}$'),
            'credit_card': re.compile(r'^\d{4}-\d{4}-\d{4}-\d{4}$')
        }

    def validate_email(self, email):
        return bool(self.patterns['email'].match(email))

    def validate_phone(self, phone):
        return (bool(self.patterns['phone_kr'].match(phone)) or
                bool(self.patterns['phone_general'].match(phone)))

    def validate_url(self, url):
        return bool(self.patterns['url'].match(url))

    def validate_password(self, password):
        """최소 8자, 대소문자, 숫자, 특수문자 포함"""
        return bool(self.patterns['password'].match(password))

    def validate_korean_name(self, name):
        return bool(self.patterns['korean_name'].match(name))

# 테스트 데이터
validator = DataValidator()

test_data = {
    'emails': ['test@example.com', 'invalid-email', 'user@domain.co.kr'],
    'phones': ['010-1234-5678', '02-123-4567', '010-12345-678'],
    'urls': ['https://www.python.org', 'http://example.com', 'not-a-url'],
    'passwords': ['Password123!', 'weak', 'Strong@Pass123'],
    'names': ['김철수', '이영희', 'John', '박']
}

print("데이터 검증 결과:")
for category, items in test_data.items():
    print(f"\n{category.upper()} 검증:")
    for item in items:
        if category == 'emails':
            result = validator.validate_email(item)
        elif category == 'phones':
            result = validator.validate_phone(item)
        elif category == 'urls':
            result = validator.validate_url(item)
        elif category == 'passwords':
            result = validator.validate_password(item)
        elif category == 'names':
            result = validator.validate_korean_name(item)

        status = "✓" if result else "✗"
        print(f"  {status} {item}")
```

## 실습: 이메일/전화번호 검증기

사용자 입력을 받아서 이메일과 전화번호의 유효성을 검사하는 프로그램을 만들어보겠습니다. 이 예제는 실제 웹 서비스의 회원가입 폼에서 사용할 수 있는 수준의 검증 로직을 포함합니다.

```python title=contact_validator.py
import re

class ContactValidator:
    """연락처 정보 검증 클래스"""

    def __init__(self):
        # 더 정교한 이메일 패턴
        self.email_pattern = re.compile(
            r'^[a-zA-Z0-9.!#$%&\'*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$'
        )

        # 한국 전화번호 패턴들
        self.phone_patterns = {
            'mobile': re.compile(r'^01[016789]-\d{3,4}-\d{4}$'),
            'seoul': re.compile(r'^02-\d{3,4}-\d{4}$'),
            'local': re.compile(r'^0(3[1-3]|4[1-4]|5[1-5]|6[1-4])-\d{3,4}-\d{4}$')
        }

    def validate_email(self, email):
        """이메일 주소 검증"""
        if not email:
            return False, "이메일을 입력해주세요."

        if len(email) > 254:
            return False, "이메일이 너무 깁니다."

        if not self.email_pattern.match(email):
            return False, "올바른 이메일 형식이 아닙니다."

        return True, "유효한 이메일입니다."

    def validate_phone(self, phone):
        """전화번호 검증"""
        if not phone:
            return False, "전화번호를 입력해주세요."

        # 공백이나 특수문자 제거 후 하이픈 추가
        clean_phone = re.sub(r'[^\d]', '', phone)

        if len(clean_phone) == 11 and clean_phone.startswith('01'):
            formatted = f"{clean_phone[:3]}-{clean_phone[3:7]}-{clean_phone[7:]}"
        elif len(clean_phone) == 10 and clean_phone.startswith('02'):
            formatted = f"{clean_phone[:2]}-{clean_phone[2:6]}-{clean_phone[6:]}"
        elif len(clean_phone) == 10:
            formatted = f"{clean_phone[:3]}-{clean_phone[3:6]}-{clean_phone[6:]}"
        else:
            return False, "올바른 전화번호 형식이 아닙니다."

        # 패턴 검증
        for phone_type, pattern in self.phone_patterns.items():
            if pattern.match(formatted):
                return True, f"유효한 {phone_type} 번호입니다: {formatted}"

        return False, "지원하지 않는 전화번호 형식입니다."

def main():
    """메인 실행 함수"""
    validator = ContactValidator()

    print("=== 연락처 정보 검증기 ===")
    print("종료하려면 'quit'을 입력하세요.\n")

    while True:
        choice = input("검증할 항목을 선택하세요 (1: 이메일, 2: 전화번호, quit: 종료): ")

        if choice.lower() == 'quit':
            print("프로그램을 종료합니다.")
            break

        if choice == '1':
            email = input("이메일 주소를 입력하세요: ")
            is_valid, message = validator.validate_email(email)
            print(f"결과: {message}\n")

        elif choice == '2':
            phone = input("전화번호를 입력하세요: ")
            is_valid, message = validator.validate_phone(phone)
            print(f"결과: {message}\n")

        else:
            print("올바른 선택이 아닙니다.\n")

if __name__ == "__main__":
    main()
```

## 확장 과제: 로그 파일 분석기

웹 서버 로그나 애플리케이션 로그를 분석하는 프로그램을 만들어보겠습니다. 실제 업무에서 로그 분석은 매우 중요한 작업이며, 정규표현식을 활용하면 효율적으로 처리할 수 있습니다.

```python title=log_analyzer.py
import re
from datetime import datetime
from collections import defaultdict, Counter

class LogAnalyzer:
    """로그 파일 분석기"""

    def __init__(self):
        # 다양한 로그 형식 패턴
        self.log_patterns = {
            'apache': re.compile(
                r'(?P<ip>\d+\.\d+\.\d+\.\d+) - - \[(?P<timestamp>[^\]]+)\] "(?P<method>\w+) (?P<url>[^"]*)" (?P<status>\d+) (?P<size>\d+|-)'
            ),
            'nginx': re.compile(
                r'(?P<ip>\d+\.\d+\.\d+\.\d+) - - \[(?P<timestamp>[^\]]+)\] "(?P<method>\w+) (?P<url>[^"]*)" (?P<status>\d+) (?P<size>\d+) "(?P<referer>[^"]*)" "(?P<user_agent>[^"]*)"'
            ),
            'application': re.compile(
                r'(?P<timestamp>\d{4}-\d{2}-\d{2} \d{2}:\d{2}:\d{2}) \[(?P<level>\w+)\] (?P<logger>\w+): (?P<message>.*)'
            )
        }

    def analyze_web_logs(self, log_lines, log_type='apache'):
        """웹 서버 로그 분석"""
        pattern = self.log_patterns[log_type]

        # 통계 수집
        stats = {
            'total_requests': 0,
            'status_codes': Counter(),
            'methods': Counter(),
            'ips': Counter(),
            'urls': Counter(),
            'errors': []
        }

        for line in log_lines:
            match = pattern.match(line.strip())
            if match:
                data = match.groupdict()
                stats['total_requests'] += 1
                stats['status_codes'][data['status']] += 1
                stats['methods'][data['method']] += 1
                stats['ips'][data['ip']] += 1
                stats['urls'][data['url']] += 1

                # 에러 로그 수집 (4xx, 5xx)
                if data['status'].startswith(('4', '5')):
                    stats['errors'].append({
                        'timestamp': data['timestamp'],
                        'ip': data['ip'],
                        'status': data['status'],
                        'url': data['url']
                    })

        return stats

    def generate_report(self, stats):
        """분석 결과 리포트 생성"""
        print("=== 로그 분석 리포트 ===\n")

        print(f"총 요청 수: {stats['total_requests']:,}")

        print("\n[ 상태 코드 분포 ]")
        for status, count in stats['status_codes'].most_common():
            percentage = (count / stats['total_requests']) * 100
            print(f"{status}: {count:,}회 ({percentage:.1f}%)")

        print("\n[ HTTP 메서드 분포 ]")
        for method, count in stats['methods'].most_common():
            percentage = (count / stats['total_requests']) * 100
            print(f"{method}: {count:,}회 ({percentage:.1f}%)")

        print("\n[ 상위 10개 IP 주소 ]")
        for ip, count in stats['ips'].most_common(10):
            print(f"{ip}: {count:,}회")

        print("\n[ 상위 10개 요청 URL ]")
        for url, count in stats['urls'].most_common(10):
            print(f"{url}: {count:,}회")

        if stats['errors']:
            print(f"\n[ 에러 로그 ({len(stats['errors'])}건) ]")
            for error in stats['errors'][:5]:  # 최근 5건만 표시
                print(f"{error['timestamp']} - {error['ip']} - {error['status']} - {error['url']}")

# 샘플 로그 데이터 생성
sample_logs = [
    '192.168.1.1 - - [15/Jan/2025:10:30:15 +0900] "GET /index.html" 200 1234',
    '192.168.1.2 - - [15/Jan/2025:10:30:16 +0900] "POST /api/login" 200 567',
    '192.168.1.1 - - [15/Jan/2025:10:30:17 +0900] "GET /favicon.ico" 404 -',
    '192.168.1.3 - - [15/Jan/2025:10:30:18 +0900] "GET /admin" 403 0',
    '192.168.1.2 - - [15/Jan/2025:10:30:19 +0900] "GET /dashboard" 200 2345',
    '192.168.1.4 - - [15/Jan/2025:10:30:20 +0900] "GET /api/data" 500 0'
]

# 로그 분석 실행
if __name__ == "__main__":
    analyzer = LogAnalyzer()
    stats = analyzer.analyze_web_logs(sample_logs, 'apache')
    analyzer.generate_report(stats)
```

## 연습문제

1. **기본 패턴 연습**: 주민등록번호 형식(XXXXXX-XXXXXXX)을 검증하는 정규표현식을 작성하세요.

2. **데이터 추출**: 텍스트에서 모든 URL을 찾아내는 정규표현식을 작성하고, http와 https를 구분해서 출력하세요.

3. **문자열 치환**: HTML 태그를 모두 제거하는 정규표현식을 작성하세요.

4. **그룹 활용**: "2025년 1월 15일" 형식의 날짜를 "2025-01-15" 형식으로 변환하는 프로그램을 작성하세요.

5. **실무 응용**: CSV 파일에서 유효하지 않은 이메일 주소를 찾아서 보고서를 생성하는 프로그램을 작성하세요.

---

**다음 챕터 예고**: Chapter 16에서는 비동기 프로그래밍의 기초를 배워보겠습니다. async/await 키워드를 사용해서 효율적인 비동기 코드를 작성하는 방법을 알아보겠습니다.
