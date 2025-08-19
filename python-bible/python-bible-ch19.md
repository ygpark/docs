---
title: '19. 웹 크롤링과 데이터 수집'
slug: python-web-scraping-crawling
description: 'requests와 BeautifulSoup을 활용한 웹 크롤링 기초법. 윤리적이고 첩임감 있는 데이터 수집 방법'
keywords: [파이썬 웹크롤링, requests, BeautifulSoup, 데이터 수집, 웹 스크레이핑, HTML 파싱]
sidebar_position: 19
---

# 19. 웹 크롤링과 데이터 수집

## 학습 목표

이 챕터를 완료하면 다음과 같은 능력을 갖추게 됩니다:

- 웹 크롤링의 기본 개념과 작동 원리를 이해할 수 있습니다
- requests와 BeautifulSoup을 활용해 웹 페이지에서 데이터를 추출할 수 있습니다
- 크롤링 윤리와 법적 고려사항을 숙지하여 책임감 있는 개발자가 될 수 있습니다
- 수집한 데이터를 정제하고 저장하는 방법을 익힐 수 있습니다
- 실제 공개 데이터를 활용한 프로젝트를 완성할 수 있습니다

---

인터넷에는 수많은 정보가 있지만, 이 정보들을 일일이 복사해서 사용하기에는 너무 많고 복잡합니다. 웹 크롤링은 이런 문제를 해결해주는 강력한 도구입니다. 파이썬의 requests와 BeautifulSoup 라이브러리를 활용하면 웹 페이지에서 원하는 정보를 자동으로 수집할 수 있습니다. 하지만 크롤링은 강력한 만큼 올바른 방법과 윤리적 고려사항을 반드시 알아야 합니다.

## 19-1 웹 크롤링이란?

웹 크롤링(Web Crawling)은 웹 페이지를 자동으로 탐색하고 필요한 데이터를 추출하는 기술입니다. 사람이 웹 브라우저로 하는 일을 프로그램이 자동으로 수행한다고 생각하면 됩니다.

### 웹 크롤링의 기본 과정

1. **웹 페이지 요청**: 특정 URL에 HTTP 요청을 보냅니다
2. **HTML 응답 받기**: 서버에서 HTML 문서를 응답으로 받습니다
3. **HTML 파싱**: 받은 HTML을 분석해서 구조를 파악합니다
4. **데이터 추출**: 원하는 정보를 찾아서 추출합니다
5. **데이터 저장**: 추출한 데이터를 파일이나 데이터베이스에 저장합니다

### 웹 크롤링의 활용 분야

- **가격 비교**: 여러 쇼핑몰의 가격 정보 수집
- **뉴스 모니터링**: 다양한 언론사의 기사 수집
- **부동산 정보**: 매물 정보 자동 수집
- **날씨 데이터**: 기상청 데이터 수집
- **학술 연구**: 논문이나 연구 자료 수집

---

## 19-2 크롤링 윤리와 법적 고려사항 (필수 숙지)

웹 크롤링을 시작하기 전에 반드시 알아야 할 윤리적, 법적 고려사항들이 있습니다. 이는 기술적 지식만큼이나 중요합니다.

### robots.txt 확인하기

모든 웹사이트는 `robots.txt` 파일을 통해 크롤링 정책을 공개합니다.

```python title=check_robots.py
import requests

def check_robots_txt(domain):
    """웹사이트의 robots.txt 확인"""
    robots_url = f"{domain}/robots.txt"
    try:
        response = requests.get(robots_url)
        if response.status_code == 200:
            print(f"=== {domain}의 robots.txt ===")
            print(response.text[:500])  # 처음 500자만 출력
        else:
            print(f"{domain}에는 robots.txt가 없습니다.")
    except Exception as e:
        print(f"오류 발생: {e}")

# 예시 실행
check_robots_txt("https://httpbin.org")
```

### 크롤링 윤리 원칙

1. **robots.txt 준수**: 사이트의 크롤링 정책을 반드시 확인하고 따르세요
2. **적절한 간격 유지**: 요청 사이에 충분한 시간 간격을 두세요 (1-2초 권장)
3. **서버 부하 고려**: 동시에 많은 요청을 보내지 마세요
4. **개인정보 보호**: 개인정보가 포함된 데이터는 수집하지 마세요
5. **저작권 존중**: 저작권이 있는 콘텐츠는 무단 사용하지 마세요

### 법적 주의사항

- **공개 데이터 우선**: 가능하면 공개 API나 오픈 데이터를 활용하세요
- **상업적 이용 금지**: 개인 학습 목적으로만 사용하세요
- **데이터 재배포 금지**: 수집한 데이터를 함부로 공유하지 마세요

---

## 19-3 requests 라이브러리

requests는 파이썬에서 HTTP 요청을 쉽게 만들 수 있게 해주는 라이브러리입니다.

### requests 설치

```bash
uv add requests
```

### 기본 사용법

```python title=requests_basic.py
import requests
import time

def safe_request(url, delay=1):
    """안전한 웹 요청 함수"""
    try:
        # 요청 간격 유지 (서버 부하 방지)
        time.sleep(delay)

        # User-Agent 설정 (봇이 아닌 일반 브라우저로 인식시키기)
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }

        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()  # 오류 발생시 예외 발생

        return response
    except requests.RequestException as e:
        print(f"요청 실패: {e}")
        return None

# 예시: 공개 API 테스트
url = "https://httpbin.org/html"
response = safe_request(url)

if response:
    print("응답 상태 코드:", response.status_code)
    print("응답 헤더:", dict(response.headers))
    print("HTML 내용 (처음 200자):")
    print(response.text[:200])
```

---

## 19-4 BeautifulSoup 활용

BeautifulSoup은 HTML과 XML 문서를 파싱하고 원하는 데이터를 추출하는 라이브러리입니다.

### BeautifulSoup 설치

```bash
uv add beautifulsoup4 lxml
```

### 기본 사용법

```python title=beautifulsoup_basic.py
import requests
from bs4 import BeautifulSoup
import time

def parse_html_safely(url):
    """HTML을 안전하게 파싱하는 함수"""
    try:
        # 안전한 요청
        time.sleep(1)  # 1초 대기
        headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }

        response = requests.get(url, headers=headers, timeout=10)
        response.raise_for_status()

        # BeautifulSoup으로 파싱
        soup = BeautifulSoup(response.text, 'lxml')
        return soup

    except Exception as e:
        print(f"파싱 실패: {e}")
        return None

# 예시: httpbin.org HTML 파싱
url = "https://httpbin.org/html"
soup = parse_html_safely(url)

if soup:
    # 제목 추출
    title = soup.find('title')
    if title:
        print("페이지 제목:", title.text)

    # 모든 링크 찾기
    links = soup.find_all('a')
    print(f"\n페이지의 링크 {len(links)}개:")
    for link in links[:3]:  # 처음 3개만 출력
        href = link.get('href')
        text = link.text.strip()
        print(f"- {text}: {href}")

    # h1 태그 찾기
    h1_tags = soup.find_all('h1')
    print(f"\nH1 태그 {len(h1_tags)}개:")
    for h1 in h1_tags:
        print(f"- {h1.text.strip()}")
```

### CSS 선택자 활용

```python title=css_selector.py
from bs4 import BeautifulSoup

# 예시 HTML
html_content = '''
<html>
<head><title>뉴스 사이트</title></head>
<body>
    <div class="news-list">
        <article class="news-item">
            <h2 class="title">파이썬 학습법</h2>
            <p class="summary">파이썬을 효과적으로 학습하는 방법</p>
            <span class="date">2025-01-15</span>
        </article>
        <article class="news-item">
            <h2 class="title">웹 크롤링 기초</h2>
            <p class="summary">웹 크롤링을 시작하는 방법</p>
            <span class="date">2025-01-14</span>
        </article>
    </div>
</body>
</html>
'''

soup = BeautifulSoup(html_content, 'lxml')

# CSS 선택자로 데이터 추출
print("=== CSS 선택자 활용 ===")

# 모든 뉴스 제목
titles = soup.select('.news-item .title')
print("뉴스 제목들:")
for title in titles:
    print(f"- {title.text}")

# 첫 번째 뉴스 아이템
first_news = soup.select_one('.news-item')
if first_news:
    title = first_news.select_one('.title').text
    summary = first_news.select_one('.summary').text
    date = first_news.select_one('.date').text
    print(f"\n첫 번째 뉴스:")
    print(f"제목: {title}")
    print(f"요약: {summary}")
    print(f"날짜: {date}")
```

---

## 19-5 데이터 정제와 저장

수집한 데이터는 정제 과정을 거쳐 깔끔하게 정리한 후 저장해야 합니다.

```python title=data_processing.py
import requests
from bs4 import BeautifulSoup
import json
import csv
import re
from datetime import datetime

class NewsCollector:
    """뉴스 수집기 클래스"""

    def __init__(self):
        self.headers = {
            'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36'
        }
        self.collected_data = []

    def clean_text(self, text):
        """텍스트 정제 함수"""
        if not text:
            return ""

        # 불필요한 공백 제거
        text = re.sub(r'\s+', ' ', text.strip())

        # 특수문자 제거 (필요시)
        text = re.sub(r'[^\w\s가-힣.,!?-]', '', text)

        return text

    def collect_sample_data(self):
        """샘플 데이터 수집 (실제로는 공개 API나 허용된 사이트 사용)"""
        # 실제 크롤링 대신 샘플 데이터 생성
        sample_news = [
            {
                "title": "파이썬 프로그래밍 기초",
                "summary": "파이썬 프로그래밍의 기본 개념을 알아봅시다.",
                "date": "2025-01-15",
                "url": "https://example.com/news1"
            },
            {
                "title": "웹 크롤링 윤리",
                "summary": "웹 크롤링을 할 때 지켜야 할 윤리적 원칙들",
                "date": "2025-01-14",
                "url": "https://example.com/news2"
            }
        ]

        # 데이터 정제
        for news in sample_news:
            cleaned_news = {
                "title": self.clean_text(news["title"]),
                "summary": self.clean_text(news["summary"]),
                "date": news["date"],
                "url": news["url"],
                "collected_at": datetime.now().isoformat()
            }
            self.collected_data.append(cleaned_news)

        print(f"{len(self.collected_data)}개의 뉴스를 수집했습니다.")

    def save_to_json(self, filename="news_data.json"):
        """JSON 파일로 저장"""
        with open(filename, 'w', encoding='utf-8') as f:
            json.dump(self.collected_data, f, ensure_ascii=False, indent=2)
        print(f"데이터를 {filename}에 저장했습니다.")

    def save_to_csv(self, filename="news_data.csv"):
        """CSV 파일로 저장"""
        if not self.collected_data:
            print("저장할 데이터가 없습니다.")
            return

        fieldnames = self.collected_data[0].keys()

        with open(filename, 'w', newline='', encoding='utf-8') as f:
            writer = csv.DictWriter(f, fieldnames=fieldnames)
            writer.writeheader()
            writer.writerows(self.collected_data)

        print(f"데이터를 {filename}에 저장했습니다.")

    def print_summary(self):
        """수집 결과 요약 출력"""
        print(f"\n=== 수집 결과 요약 ===")
        print(f"총 {len(self.collected_data)}개의 뉴스")

        if self.collected_data:
            print("\n수집된 뉴스 제목:")
            for i, news in enumerate(self.collected_data, 1):
                print(f"{i}. {news['title']}")

# 사용 예시
if __name__ == "__main__":
    collector = NewsCollector()
    collector.collect_sample_data()
    collector.save_to_json()
    collector.save_to_csv()
    collector.print_summary()
```

---

## 19-6 크롤링 모범 사례와 주의사항

### 모범 사례

```python title=best_practices.py
import requests
from bs4 import BeautifulSoup
import time
import random
from urllib.robotparser import RobotFileParser

class EthicalCrawler:
    """윤리적 크롤링을 위한 기본 클래스"""

    def __init__(self, base_url, delay_range=(1, 3)):
        self.base_url = base_url
        self.delay_range = delay_range
        self.session = requests.Session()

        # User-Agent 설정
        self.session.headers.update({
            'User-Agent': 'Mozilla/5.0 (Educational Purpose Only)'
        })

        # robots.txt 확인
        self.check_robots_txt()

    def check_robots_txt(self):
        """robots.txt 확인"""
        try:
            robots_url = f"{self.base_url}/robots.txt"
            rp = RobotFileParser()
            rp.set_url(robots_url)
            rp.read()

            # 테스트 URL로 크롤링 가능 여부 확인
            test_url = f"{self.base_url}/"
            can_crawl = rp.can_fetch('*', test_url)

            print(f"robots.txt 확인 결과: {'크롤링 가능' if can_crawl else '크롤링 제한'}")

        except Exception as e:
            print(f"robots.txt 확인 실패: {e}")

    def polite_request(self, url):
        """예의 바른 요청"""
        # 랜덤 딜레이 (서버 부하 방지)
        delay = random.uniform(*self.delay_range)
        time.sleep(delay)

        try:
            response = self.session.get(url, timeout=10)
            response.raise_for_status()
            return response

        except requests.RequestException as e:
            print(f"요청 실패 ({url}): {e}")
            return None

    def crawl_with_respect(self, urls):
        """존중하는 마음으로 크롤링"""
        results = []

        for i, url in enumerate(urls, 1):
            print(f"진행률: {i}/{len(urls)} - {url}")

            response = self.polite_request(url)
            if response:
                # 여기서 실제 데이터 추출 작업 수행
                results.append({
                    'url': url,
                    'status': response.status_code,
                    'content_length': len(response.text)
                })

        return results

# 사용 예시 (실제로는 허용된 사이트만 사용)
if __name__ == "__main__":
    # 테스트용 공개 API 사이트
    crawler = EthicalCrawler("https://httpbin.org")

    test_urls = [
        "https://httpbin.org/html",
        "https://httpbin.org/json"
    ]

    results = crawler.crawl_with_respect(test_urls)
    print("\n크롤링 결과:")
    for result in results:
        print(f"- {result['url']}: {result['status']} ({result['content_length']}바이트)")
```

---

## 실습: 뉴스 수집기 만들기 (윤리적 크롤링)

공개 API를 활용한 안전하고 윤리적인 데이터 수집기를 만들어보겠습니다. 실제 뉴스 사이트를 크롤링하는 대신, JSONPlaceholder라는 공개 테스트 API를 사용합니다.

JSONPlaceholder는 개발자들이 테스트 목적으로 사용할 수 있는 무료 공개 API입니다. 이를 통해 크롤링의 기본 원리를 안전하게 학습할 수 있습니다.

```python title=news_collector_project.py
import requests
import json
import csv
from datetime import datetime
import time

class PublicAPICollector:
    """공개 API를 활용한 데이터 수집기"""

    def __init__(self):
        self.base_url = "https://jsonplaceholder.typicode.com"
        self.collected_posts = []
        self.collected_comments = []

    def collect_posts(self, limit=10):
        """게시글 수집"""
        print("게시글을 수집중입니다...")

        try:
            response = requests.get(f"{self.base_url}/posts", timeout=10)
            response.raise_for_status()

            posts = response.json()[:limit]  # 제한된 개수만 수집

            for post in posts:
                # 데이터 정제 및 추가 정보 포함
                processed_post = {
                    "id": post["id"],
                    "title": post["title"].strip(),
                    "body": post["body"].strip().replace('\n', ' '),
                    "userId": post["userId"],
                    "collected_at": datetime.now().isoformat()
                }
                self.collected_posts.append(processed_post)

                # 요청 간격 유지
                time.sleep(0.1)

            print(f"{len(self.collected_posts)}개의 게시글을 수집했습니다.")

        except Exception as e:
            print(f"게시글 수집 실패: {e}")

    def collect_comments_for_post(self, post_id):
        """특정 게시글의 댓글 수집"""
        try:
            response = requests.get(
                f"{self.base_url}/posts/{post_id}/comments",
                timeout=10
            )
            response.raise_for_status()

            comments = response.json()

            for comment in comments:
                processed_comment = {
                    "id": comment["id"],
                    "postId": comment["postId"],
                    "name": comment["name"].strip(),
                    "email": comment["email"],
                    "body": comment["body"].strip().replace('\n', ' '),
                    "collected_at": datetime.now().isoformat()
                }
                self.collected_comments.append(processed_comment)

            time.sleep(0.1)  # 요청 간격 유지
            return len(comments)

        except Exception as e:
            print(f"댓글 수집 실패 (post_id: {post_id}): {e}")
            return 0

    def collect_all_data(self):
        """모든 데이터 수집"""
        # 게시글 수집
        self.collect_posts(5)  # 5개만 수집

        # 각 게시글의 댓글 수집
        print("\n각 게시글의 댓글을 수집중입니다...")
        for post in self.collected_posts:
            comment_count = self.collect_comments_for_post(post["id"])
            print(f"게시글 {post['id']}: {comment_count}개 댓글 수집")

    def save_data(self):
        """데이터 저장"""
        # JSON 파일로 저장
        data = {
            "posts": self.collected_posts,
            "comments": self.collected_comments,
            "collection_info": {
                "collected_at": datetime.now().isoformat(),
                "total_posts": len(self.collected_posts),
                "total_comments": len(self.collected_comments)
            }
        }

        with open("collected_data.json", "w", encoding="utf-8") as f:
            json.dump(data, f, ensure_ascii=False, indent=2)

        # CSV 파일로 저장
        if self.collected_posts:
            with open("posts.csv", "w", newline="", encoding="utf-8") as f:
                writer = csv.DictWriter(f, fieldnames=self.collected_posts[0].keys())
                writer.writeheader()
                writer.writerows(self.collected_posts)

        if self.collected_comments:
            with open("comments.csv", "w", newline="", encoding="utf-8") as f:
                writer = csv.DictWriter(f, fieldnames=self.collected_comments[0].keys())
                writer.writeheader()
                writer.writerows(self.collected_comments)

        print("\n데이터 저장 완료:")
        print("- collected_data.json: 전체 데이터")
        print("- posts.csv: 게시글 데이터")
        print("- comments.csv: 댓글 데이터")

    def print_summary(self):
        """수집 결과 요약"""
        print(f"\n=== 데이터 수집 완료 ===")
        print(f"게시글: {len(self.collected_posts)}개")
        print(f"댓글: {len(self.collected_comments)}개")

        if self.collected_posts:
            print(f"\n수집된 게시글 제목 (처음 3개):")
            for i, post in enumerate(self.collected_posts[:3], 1):
                title = post["title"][:50] + "..." if len(post["title"]) > 50 else post["title"]
                print(f"{i}. {title}")

# 실행
if __name__ == "__main__":
    print("공개 API 데이터 수집기 시작")
    print("=" * 40)

    collector = PublicAPICollector()
    collector.collect_all_data()
    collector.save_data()
    collector.print_summary()

    print("\n주의: 이는 교육 목적의 예제입니다.")
    print("실제 웹사이트 크롤링시에는 반드시 robots.txt와 이용약관을 확인하세요.")
```

---

## 확장 과제: 공개 API 활용 데이터 수집기

더 고급 기능을 가진 데이터 수집기를 만들어보세요:

1. **에러 처리 강화**: 네트워크 오류, 타임아웃 등에 대한 robust한 처리
2. **데이터 검증**: 수집한 데이터의 유효성 검사
3. **진행률 표시**: 수집 진행 상황을 시각적으로 표시
4. **설정 파일**: JSON이나 YAML로 수집 설정 관리
5. **로깅**: 수집 과정과 오류를 로그 파일에 기록

---

## 연습문제

### 기본 문제

1. requests를 사용해 "https://httpbin.org/json" 에서 JSON 데이터를 가져와 출력하는 프로그램을 작성하세요.

2. BeautifulSoup을 사용해 간단한 HTML 문자열에서 모든 링크를 추출하는 함수를 만드세요.

3. 수집한 데이터를 CSV와 JSON 두 형식으로 저장하는 함수를 작성하세요.

### 응용 문제

4. 공개 API(예: JSONPlaceholder)를 활용해 사용자 정보를 수집하고, 수집한 데이터를 정제하여 저장하는 프로그램을 작성하세요.

5. 웹 크롤링 시 발생할 수 있는 다양한 예외 상황들을 처리하는 안전한 크롤러 클래스를 설계하세요.

### 도전 문제

6. 여러 개의 공개 API에서 데이터를 수집하고, 수집한 데이터들을 통합하여 의미있는 분석 리포트를 생성하는 프로그램을 작성하세요.

---

이번 챕터에서는 웹 크롤링의 기본 개념부터 윤리적 고려사항, 그리고 실제 구현까지 모든 과정을 다뤘습니다. 특히 크롤링 윤리와 법적 고려사항은 기술적 능력만큼이나 중요하니 반드시 숙지하고 실천하시기 바랍니다. 다음 챕터에서는 수집한 데이터를 활용해 실전 프로젝트를 진행해보겠습니다!
