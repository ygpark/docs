---
title: '16. 비동기 프로그래밍 기초'
slug: python-async-programming-basics
description: 'async/await, asyncio 라이브러리를 활용하여 비동기 프로그래밍의 기초 개념과 실습 학습'
keywords: [파이썬 비동기, async await, asyncio, 비동기 프로그래밍, 쉵루틴, coroutine]
sidebar_position: 16
---

# 16. 비동기 프로그래밍 기초

## 학습 목표

이 챕터를 마치면 여러분은 동기와 비동기 프로그래밍의 차이점을 명확히 이해하고, Python의 async/await 키워드를 사용해서 간단한 비동기 함수를 작성할 수 있게 됩니다. 또한 asyncio 라이브러리의 기본 기능을 활용하여 동시에 여러 작업을 효율적으로 처리하는 프로그램을 만들 수 있습니다.

## 16-1. 동기와 비동기의 차이

프로그래밍에서 '동기(Synchronous)'와 '비동기(Asynchronous)'라는 개념은 작업을 처리하는 방식의 차이를 나타냅니다. 동기 프로그래밍은 한 번에 하나의 작업만 순서대로 처리하는 방식이고, 비동기 프로그래밍은 여러 작업을 동시에 처리할 수 있는 방식입니다. 이 차이를 이해하면 더 효율적인 프로그램을 작성할 수 있습니다.

### 동기 프로그래밍의 특징

동기 프로그래밍에서는 각 작업이 완료될 때까지 다음 작업이 기다립니다:

```python title=sync_example.py
import time

def make_coffee():
    print("커피를 끓이기 시작합니다...")
    time.sleep(3)  # 3초 대기 (커피 끓이는 시간)
    print("커피가 완성되었습니다!")
    return "커피"

def make_toast():
    print("토스트를 굽기 시작합니다...")
    time.sleep(2)  # 2초 대기 (토스트 굽는 시간)
    print("토스트가 완성되었습니다!")
    return "토스트"

def prepare_breakfast():
    start_time = time.time()

    coffee = make_coffee()
    toast = make_toast()

    end_time = time.time()
    print(f"아침식사 준비 완료! 총 {end_time - start_time:.1f}초 걸렸습니다.")
    return [coffee, toast]

# 실행
prepare_breakfast()
```

### 비동기 프로그래밍의 특징

비동기 프로그래밍에서는 여러 작업을 동시에 시작하고 기다릴 수 있습니다:

```python title=async_example.py
import asyncio

async def make_coffee():
    print("커피를 끓이기 시작합니다...")
    await asyncio.sleep(3)  # 3초 대기 (비동기적으로)
    print("커피가 완성되었습니다!")
    return "커피"

async def make_toast():
    print("토스트를 굽기 시작합니다...")
    await asyncio.sleep(2)  # 2초 대기 (비동기적으로)
    print("토스트가 완성되었습니다!")
    return "토스트"

async def prepare_breakfast():
    start_time = asyncio.get_event_loop().time()

    # 두 작업을 동시에 시작
    coffee_task = make_coffee()
    toast_task = make_toast()

    # 두 작업이 모두 완료되기를 기다림
    coffee, toast = await asyncio.gather(coffee_task, toast_task)

    end_time = asyncio.get_event_loop().time()
    print(f"아침식사 준비 완료! 총 {end_time - start_time:.1f}초 걸렸습니다.")
    return [coffee, toast]

# 실행
asyncio.run(prepare_breakfast())
```

## 16-2. async/await 기본 개념

Python에서 비동기 프로그래밍을 구현하기 위해 `async`와 `await` 키워드를 사용합니다. 이 키워드들은 Python 3.5에서 도입되어 비동기 프로그래밍을 더 쉽고 직관적으로 만들어 줍니다.

### async 키워드

`async` 키워드는 함수를 비동기 함수(코루틴)로 만들어 줍니다:

```python title=async_function.py
async def hello_async():
    print("비동기 함수입니다!")
    return "완료"

# 비동기 함수는 직접 호출할 수 없습니다
# hello_async()  # 이렇게 하면 오류!

# asyncio.run()을 사용해야 합니다
import asyncio
result = asyncio.run(hello_async())
print(result)
```

### await 키워드

`await` 키워드는 비동기 작업이 완료될 때까지 기다립니다:

```python title=await_example.py
import asyncio

async def slow_operation():
    print("시간이 오래 걸리는 작업을 시작합니다...")
    await asyncio.sleep(2)  # 2초 대기
    print("작업이 완료되었습니다!")
    return "결과"

async def main():
    print("메인 함수 시작")
    result = await slow_operation()  # 완료될 때까지 기다림
    print(f"받은 결과: {result}")
    print("메인 함수 종료")

asyncio.run(main())
```

## 16-3. 간단한 비동기 함수 만들기

비동기 함수를 만들 때는 `async def`로 함수를 정의하고, 내부에서 `await`을 사용하여 다른 비동기 작업을 기다립니다. 실제로 유용한 비동기 함수의 예제를 통해 연습해봅시다.

```python title=simple_async.py
import asyncio
import random

async def download_file(filename, size_mb):
    """파일 다운로드를 시뮬레이션하는 비동기 함수"""
    print(f"{filename} 다운로드 시작... ({size_mb}MB)")

    # 파일 크기에 따라 다운로드 시간 계산 (MB당 0.5초)
    download_time = size_mb * 0.5
    await asyncio.sleep(download_time)

    print(f"{filename} 다운로드 완료!")
    return f"{filename} (완료)"

async def check_internet_connection():
    """인터넷 연결 상태를 확인하는 비동기 함수"""
    print("인터넷 연결 상태 확인 중...")
    await asyncio.sleep(1)

    # 90% 확률로 연결 성공
    if random.random() > 0.1:
        print("인터넷 연결 정상!")
        return True
    else:
        print("인터넷 연결 실패!")
        return False

async def send_notification(message):
    """알림을 보내는 비동기 함수"""
    print(f"알림 전송 중: {message}")
    await asyncio.sleep(0.5)
    print("알림 전송 완료!")

async def main():
    # 비동기 함수들을 순차적으로 실행
    await check_internet_connection()
    await download_file("문서.pdf", 3)
    await send_notification("다운로드가 완료되었습니다!")

# 실행
asyncio.run(main())
```

## 16-4. asyncio 기초

`asyncio`는 Python의 비동기 프로그래밍을 위한 내장 라이브러리입니다. 이벤트 루프를 통해 비동기 작업들을 관리하고 실행하며, 여러 유용한 함수들을 제공합니다.

### 동시 실행 - asyncio.gather()

여러 비동기 작업을 동시에 실행하고 모든 결과를 기다릴 때 사용합니다:

```python title=asyncio_gather.py
import asyncio
import time

async def task_with_delay(name, delay):
    print(f"{name} 작업 시작")
    await asyncio.sleep(delay)
    print(f"{name} 작업 완료")
    return f"{name} 결과"

async def run_tasks_sequentially():
    """순차 실행"""
    start_time = time.time()

    result1 = await task_with_delay("작업1", 2)
    result2 = await task_with_delay("작업2", 3)
    result3 = await task_with_delay("작업3", 1)

    end_time = time.time()
    print(f"순차 실행 완료: {end_time - start_time:.1f}초")
    return [result1, result2, result3]

async def run_tasks_concurrently():
    """동시 실행"""
    start_time = time.time()

    results = await asyncio.gather(
        task_with_delay("작업1", 2),
        task_with_delay("작업2", 3),
        task_with_delay("작업3", 1)
    )

    end_time = time.time()
    print(f"동시 실행 완료: {end_time - start_time:.1f}초")
    return results

async def main():
    print("=== 순차 실행 ===")
    await run_tasks_sequentially()

    print("\n=== 동시 실행 ===")
    await run_tasks_concurrently()

asyncio.run(main())
```

### 작업 생성과 관리 - asyncio.create_task()

`create_task()`를 사용하면 작업을 미리 시작하고 나중에 결과를 받을 수 있습니다:

```python title=asyncio_tasks.py
import asyncio

async def background_task(name, duration):
    print(f"{name} 백그라운드 작업 시작")
    await asyncio.sleep(duration)
    print(f"{name} 백그라운드 작업 완료")
    return f"{name} 완료"

async def main():
    print("메인 작업 시작")

    # 작업들을 미리 시작 (백그라운드에서 실행됨)
    task1 = asyncio.create_task(background_task("데이터베이스 정리", 3))
    task2 = asyncio.create_task(background_task("로그 파일 압축", 2))

    # 다른 작업을 하면서...
    print("다른 중요한 작업을 처리합니다...")
    await asyncio.sleep(1)
    print("중요한 작업 완료!")

    # 백그라운드 작업들의 완료를 기다림
    result1 = await task1
    result2 = await task2

    print(f"모든 작업 완료: {result1}, {result2}")

asyncio.run(main())
```

## 16-5. 비동기 프로그래밍 사용 사례

비동기 프로그래밍은 특히 입출력(I/O) 작업이 많은 상황에서 유용합니다. 네트워크 요청, 파일 처리, 데이터베이스 접근 등이 대표적인 예입니다.

### 언제 비동기를 사용해야 할까?

- **네트워크 요청**: 웹 API 호출, 웹 크롤링
- **파일 입출력**: 대용량 파일 처리
- **데이터베이스 접근**: 여러 쿼리 동시 실행
- **사용자 인터페이스**: 응답성이 중요한 애플리케이션

### 언제 사용하지 말아야 할까?

- **CPU 집약적 작업**: 복잡한 계산, 이미지 처리
- **간단한 순차 작업**: 복잡성이 이익을 상회하는 경우

## 실습: 비동기 웹 요청 프로그램

여러 웹사이트의 응답 시간을 동시에 측정하는 프로그램을 만들어보겠습니다. 이 예제는 실제 네트워크 요청 대신 시뮬레이션을 사용하여 안전하게 연습할 수 있도록 구성했습니다.

```python title=async_web_checker.py
import asyncio
import random
import time

async def check_website(site_name, base_delay=1):
    """웹사이트 응답 시간을 체크하는 시뮬레이션"""
    print(f"{site_name} 응답 시간 측정 시작...")

    # 실제 네트워크 지연을 시뮬레이션
    # 각 사이트마다 다른 응답 시간을 가짐
    response_time = base_delay + random.uniform(0.5, 2.0)
    await asyncio.sleep(response_time)

    # 간혹 실패하는 경우도 시뮬레이션
    if random.random() < 0.1:  # 10% 확률로 실패
        print(f"{site_name} - 응답 실패!")
        return {"site": site_name, "status": "실패", "time": None}
    else:
        print(f"{site_name} - 응답 시간: {response_time:.2f}초")
        return {"site": site_name, "status": "성공", "time": response_time}

async def monitor_websites():
    """여러 웹사이트를 동시에 모니터링"""
    websites = [
        ("Google", 0.5),
        ("GitHub", 1.0),
        ("Python.org", 0.8),
        ("Stack Overflow", 1.2),
        ("MDN Docs", 0.9)
    ]

    print("웹사이트 모니터링 시작...\n")
    start_time = time.time()

    # 모든 웹사이트를 동시에 체크
    tasks = [check_website(name, delay) for name, delay in websites]
    results = await asyncio.gather(*tasks, return_exceptions=True)

    end_time = time.time()
    total_time = end_time - start_time

    # 결과 분석
    print(f"\n=== 모니터링 결과 ===")
    print(f"총 소요 시간: {total_time:.2f}초")

    successful = [r for r in results if isinstance(r, dict) and r["status"] == "성공"]
    failed = [r for r in results if isinstance(r, dict) and r["status"] == "실패"]

    print(f"성공: {len(successful)}개, 실패: {len(failed)}개")

    if successful:
        avg_time = sum(r["time"] for r in successful) / len(successful)
        print(f"평균 응답 시간: {avg_time:.2f}초")

        fastest = min(successful, key=lambda x: x["time"])
        slowest = max(successful, key=lambda x: x["time"])
        print(f"가장 빠른 사이트: {fastest['site']} ({fastest['time']:.2f}초)")
        print(f"가장 느린 사이트: {slowest['site']} ({slowest['time']:.2f}초)")

async def main():
    await monitor_websites()

    print("\n5초 후 다시 모니터링합니다...")
    await asyncio.sleep(5)

    await monitor_websites()

# 실행
if __name__ == "__main__":
    asyncio.run(main())
```

이 실습 프로그램은 동시에 여러 웹사이트의 응답 시간을 측정하는 시뮬레이션을 보여줍니다. 실제 웹 요청 라이브러리인 `aiohttp`를 사용하면 실제 웹사이트에 요청을 보낼 수 있습니다.

## 정리

비동기 프로그래밍은 처음에는 복잡해 보일 수 있지만, 입출력이 많은 작업에서는 매우 효율적입니다. `async`와 `await` 키워드를 사용하여 비동기 함수를 만들고, `asyncio.gather()`나 `asyncio.create_task()`를 활용하여 여러 작업을 동시에 처리할 수 있습니다.

비동기 프로그래밍의 핵심은 대기 시간을 효율적으로 활용하는 것입니다. 한 작업이 완료되기를 기다리는 동안 다른 작업을 처리함으로써 전체적인 프로그램 성능을 크게 향상시킬 수 있습니다.

## 연습문제

1. **기본 비동기 함수**: 1초에서 5초 사이의 랜덤한 시간을 기다리는 비동기 함수를 만들고, 이 함수를 5번 동시에 실행하는 프로그램을 작성하세요.

2. **파일 처리 시뮬레이션**: 서로 다른 크기의 파일 3개를 동시에 처리하는 비동기 함수를 만드세요. 각 파일의 처리 시간은 파일 크기에 비례해야 합니다.

3. **카운터 프로그램**: 10초 동안 매초마다 카운트를 출력하는 함수와, 동시에 사용자 입력을 받는 함수를 만들어서 동시에 실행하는 프로그램을 작성하세요. (힌트: `asyncio.sleep(1)`과 `input()` 대신 적절한 비동기 대안을 사용하세요)

4. **응답 시간 비교**: 같은 작업을 동기적으로 실행했을 때와 비동기적으로 실행했을 때의 시간 차이를 측정하는 프로그램을 만들어보세요.
