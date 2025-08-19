# PF(Packet Filter) 레퍼런스 가이드

## 학습 목표

이 가이드를 통해 PF(Packet Filter)의 기본 개념부터 실제 활용까지 단계별로 학습할 수 있습니다. PF의 핵심 기능인 방화벽 규칙 설정, 네트워크 주소 변환(NAT), 트래픽 제어 등을 실습 예제와 함께 익혀보세요. 초급자도 쉽게 따라할 수 있도록 친근한 설명과 실용적인 예제로 구성했습니다.

---

## 1. PF 소개

PF(Packet Filter)는 OpenBSD에서 개발된 강력하고 유연한 방화벽 시스템입니다. 네트워크 패킷을 검사하고 필터링하여 시스템 보안을 강화하는 도구로, 단순한 방화벽 기능부터 복잡한 네트워크 주소 변환(NAT)까지 다양한 기능을 제공합니다.

PF는 상태 기반 패킷 필터링을 지원하여 연결 상태를 추적하고, 직관적인 문법으로 복잡한 네트워크 규칙도 쉽게 작성할 수 있습니다. FreeBSD, NetBSD 등 다양한 BSD 계열 시스템에서 널리 사용되고 있어 시스템 관리자들에게 필수적인 도구가 되었습니다.

### 1.1 주요 특징

- **상태 기반 필터링**: 연결 상태를 추적하여 보안성 향상
- **NAT/PAT 지원**: 네트워크 주소 변환 및 포트 주소 변환
- **트래픽 제어**: 대역폭 제한 및 QoS 설정
- **로드 밸런싱**: 여러 서버 간 트래픽 분산
- **직관적 문법**: 읽기 쉽고 이해하기 쉬운 규칙 작성

### 1.2 시스템 요구사항

PF는 BSD 계열 운영체제에서 커널 모듈로 동작합니다:

- OpenBSD (네이티브 지원)
- FreeBSD 5.3 이상
- NetBSD 3.0 이상
- macOS (pfctl 명령어로 제한적 사용 가능)

---

## 2. 기본 설정 및 구성

PF를 사용하기 전에 시스템에서 PF 서비스를 활성화하고 기본 설정 파일을 준비해야 합니다. 설정 파일은 주로 `/etc/pf.conf`에 위치하며, 이 파일에 방화벽 규칙과 네트워크 정책을 정의합니다.

PF는 부팅 시 자동으로 시작되도록 설정할 수 있으며, 실시간으로 규칙을 수정하고 적용하는 것도 가능합니다. 설정 변경 후에는 반드시 문법 검사를 통해 오류를 확인하고 적용하는 것이 중요합니다.

### 2.1 PF 활성화

```bash title=pf_enable.sh
# FreeBSD에서 PF 활성화
echo 'pf_enable="YES"' >> /etc/rc.conf
echo 'pf_rules="/etc/pf.conf"' >> /etc/rc.conf
echo 'pflog_enable="YES"' >> /etc/rc.conf

# PF 서비스 시작
service pf start
service pflog start

# PF 상태 확인
pfctl -s info
```

**배경 설명**: 이 예제는 FreeBSD 시스템에서 PF를 시스템 부팅 시 자동으로 시작하도록 설정하는 방법을 보여줍니다. pflog는 PF 로그를 기록하는 서비스입니다.

### 2.2 기본 설정 파일 구조

```bash title=/etc/pf.conf
# 매크로 정의
ext_if = "em0"
int_if = "em1"
webserver = "192.168.1.100"

# 테이블 정의
table <badguys> persist file "/etc/pf.badguys"

# 옵션 설정
set skip on lo0
set block-policy return

# 정규화 규칙
scrub in all

# 필터 규칙
block all
pass out quick on $ext_if
pass in quick on $int_if
pass in on $ext_if proto tcp to $webserver port 80
```

**배경 설명**: PF 설정 파일의 기본 구조를 보여주는 예제입니다. 매크로와 테이블을 활용하여 관리하기 쉬운 규칙을 작성할 수 있습니다.

---

## 3. 기본 문법 및 규칙

PF의 규칙은 상당히 직관적이고 자연어에 가까운 문법을 사용합니다. 기본적으로 "action direction interface protocol source destination" 형태로 구성되며, 각 요소는 선택적으로 사용할 수 있습니다.

규칙은 위에서 아래로 순차적으로 처리되며, 마지막에 매치된 규칙이 최종적으로 적용됩니다. 'quick' 키워드를 사용하면 해당 규칙에 매치될 때 즉시 처리를 종료할 수 있어 성능을 향상시킬 수 있습니다.

### 3.1 기본 규칙 구조

```bash title=basic_rules.conf
# 기본 액션: pass(허용), block(차단)
pass in all
block out all

# 방향 지정: in(들어오는), out(나가는)
pass in on em0
pass out on em0

# 프로토콜 지정
pass in proto tcp
pass in proto udp
pass in proto icmp

# 포트 지정
pass in proto tcp port 22
pass in proto tcp port { 80, 443 }
pass in proto tcp port 8000:8080

# 주소 지정
pass in from 192.168.1.0/24
pass in to 10.0.0.1
pass in from any to any
```

### 3.2 상태 기반 필터링

```bash title=stateful_rules.conf
# 상태 추적 활성화
pass out proto tcp all keep state
pass out proto udp all keep state

# 연결 상태별 제어
pass in proto tcp from any to any port 22 flags S/SA keep state
pass in proto tcp from any to any port 80 flags S/SA synproxy state

# 상태 옵션
pass out proto tcp all keep state (max 1000, source-track rule, max-src-states 10)
```

**배경 설명**: 상태 기반 필터링은 PF의 핵심 기능으로, 기존 연결의 상태를 추적하여 보안을 강화하고 성능을 향상시킵니다.

---

## 4. 네트워크 주소 변환 (NAT)

NAT(Network Address Translation)는 사설 네트워크의 IP 주소를 공인 IP 주소로 변환하여 인터넷 접속을 가능하게 하는 핵심 기능입니다. PF는 다양한 형태의 NAT를 지원하며, 포트 주소 변환(PAT)과 리다이렉션 기능도 함께 제공합니다.

NAT 설정 시 주의할 점은 내부 네트워크의 구조와 외부 인터페이스의 IP 주소를 정확히 파악하는 것입니다. 또한 필요에 따라 특정 포트나 서비스에 대해서만 NAT를 적용하거나 제외할 수도 있습니다.

### 4.1 기본 NAT 설정

```bash title=nat_basic.conf
# 기본 변수 설정
ext_if = "em0"
int_net = "192.168.1.0/24"

# 아웃바운드 NAT (내부 -> 외부)
nat on $ext_if from $int_net to any -> ($ext_if)

# 정적 NAT (1:1 매핑)
nat on $ext_if from 192.168.1.100 to any -> 203.0.113.10

# 포트 범위 지정 NAT
nat on $ext_if from $int_net to any -> ($ext_if:0) port 1024:65535
```

### 4.2 포트 리다이렉션

```bash title=port_redirect.conf
# 웹 서버 리다이렉션
rdr on $ext_if proto tcp from any to any port 80 -> 192.168.1.100 port 80
rdr on $ext_if proto tcp from any to any port 443 -> 192.168.1.100 port 443

# SSH 포트 변경 리다이렉션
rdr on $ext_if proto tcp from any to any port 2222 -> 192.168.1.10 port 22

# 로드 밸런싱 리다이렉션
rdr on $ext_if proto tcp from any to any port 80 -> { 192.168.1.100, 192.168.1.101, 192.168.1.102 } round-robin
```

**배경 설명**: 포트 리다이렉션을 통해 외부에서 들어오는 특정 포트 요청을 내부 서버로 전달할 수 있어, 웹 서버나 메일 서버 운영에 필수적입니다.

---

## 5. 고급 기능

PF의 고급 기능들은 복잡한 네트워크 환경에서 세밀한 제어와 모니터링을 가능하게 합니다. 테이블 기능을 통해 동적으로 IP 주소 목록을 관리하고, 앵커를 사용하여 모듈식 규칙 구성이 가능합니다.

또한 대역폭 제어와 우선순위 설정을 통해 네트워크 자원을 효율적으로 관리할 수 있으며, 실시간 모니터링과 로깅 기능으로 네트워크 상태를 지속적으로 파악할 수 있습니다.

### 5.1 테이블과 매크로 활용

```bash title=tables_macros.conf
# 매크로 정의
web_servers = "{ 192.168.1.100, 192.168.1.101 }"
db_servers = "{ 192.168.1.200, 192.168.1.201 }"
admin_net = "10.0.0.0/8"

# 테이블 정의
table <spammers> persist file "/etc/pf.spammers"
table <allowed_ssh> { 203.0.113.0/24, 198.51.100.0/24 }

# 테이블 사용 규칙
block quick from <spammers>
pass in proto tcp from <allowed_ssh> to any port 22
pass in proto tcp from $admin_net to $db_servers port 3306
```

### 5.2 대역폭 제어 (ALTQ)

```bash title=bandwidth_control.conf
# ALTQ 큐 설정
altq on $ext_if cbq bandwidth 100Mb queue { std, http, ssh, bulk }
queue std bandwidth 20% cbq(default)
queue http bandwidth 60% priority 3 cbq(red)
queue ssh bandwidth 15% priority 7 cbq(red)
queue bulk bandwidth 5% priority 1 cbq(red)

# 트래픽 분류 규칙
pass out on $ext_if proto tcp port 80 queue http
pass out on $ext_if proto tcp port 22 queue ssh
pass out on $ext_if proto tcp port { 20, 21 } queue bulk
```

**배경 설명**: ALTQ를 사용한 대역폭 제어로 중요한 서비스(SSH, HTTP)에 우선순위를 부여하고, 대용량 파일 전송 등은 낮은 우선순위로 설정할 수 있습니다.

---

## 6. 실무 예제

실제 운영 환경에서 자주 사용되는 PF 설정 시나리오들을 살펴보겠습니다. 이 예제들은 소규모 사무실부터 중간 규모의 서버 환경까지 다양한 상황에서 활용할 수 있는 실용적인 구성을 보여줍니다.

각 예제는 특정 목적과 요구사항을 만족하도록 설계되었으며, 실제 환경에서 바로 적용하거나 필요에 따라 수정하여 사용할 수 있습니다. 보안과 기능성의 균형을 맞춘 실무적인 접근을 중점으로 구성했습니다.

### 6.1 소규모 사무실 방화벽

```bash title=office_firewall.conf
# 네트워크 인터페이스 정의
ext_if = "em0"
int_if = "em1"
dmz_if = "em2"

# 네트워크 세그먼트
internal_net = "192.168.1.0/24"
dmz_net = "192.168.100.0/24"
web_server = "192.168.100.10"
mail_server = "192.168.100.20"

# 허용된 관리자 IP
table <admins> { 203.0.113.50, 198.51.100.100 }

# 기본 정책
set skip on lo0
scrub in all

# 기본 차단, 로깅
block log all

# 내부 네트워크 -> 인터넷 허용
pass out on $ext_if from $internal_net nat-to ($ext_if)

# 관리자 SSH 접근
pass in on $ext_if proto tcp from <admins> to ($ext_if) port 22

# 웹/메일 서버 서비스
pass in on $ext_if proto tcp to $web_server port { 80, 443 } rdr-to $web_server
pass in on $ext_if proto tcp to $mail_server port { 25, 110, 143, 993, 995 } rdr-to $mail_server

# 내부 네트워크 -> DMZ 접근 제한
pass in on $int_if proto tcp from $internal_net to $dmz_net port { 80, 443, 22 }
```

**배경 설명**: 이 설정은 내부 네트워크, DMZ, 인터넷을 분리하여 보안을 강화한 소규모 사무실용 방화벽 구성입니다. 웹서버와 메일서버를 DMZ에 배치하여 보안성을 높였습니다.

### 6.2 웹 서버 보호 설정

```bash title=webserver_protection.conf
# 웹 서버 보호를 위한 고급 설정
web_server = "192.168.1.100"
max_conn_rate = 15
max_conn_states = 500

# DDoS 방어를 위한 테이블
table <abusers> persist

# 연결 제한 규칙
pass in on $ext_if proto tcp from any to $web_server port 80 \
    flags S/SA keep state \
    (max-src-conn-rate $max_conn_rate/60, \
     max-src-states $max_conn_states, \
     overload <abusers> flush global)

# 차단된 IP에서의 접근 거부
block quick from <abusers>

# HTTPS 트래픽
pass in on $ext_if proto tcp from any to $web_server port 443 \
    flags S/SA keep state \
    (max-src-conn-rate $max_conn_rate/60, \
     max-src-states $max_conn_states)

# 웹 관리 인터페이스 (제한된 접근)
pass in on $ext_if proto tcp from <admins> to $web_server port 8080 \
    flags S/SA keep state
```

**배경 설명**: 웹 서버를 DDoS 공격과 과도한 연결로부터 보호하는 설정입니다. 연결 수 제한과 자동 차단 기능으로 서버 안정성을 확보합니다.

---

## 7. 관리 및 모니터링

PF의 효과적인 운영을 위해서는 지속적인 모니터링과 관리가 필수입니다. pfctl 명령어를 통해 실시간으로 규칙을 확인하고 수정할 수 있으며, 다양한 통계 정보와 로그를 통해 네트워크 상태를 파악할 수 있습니다.

정기적인 로그 분석과 규칙 최적화를 통해 보안성을 높이고 성능을 개선할 수 있습니다. 또한 백업과 복구 절차를 마련해두어 장애 상황에 신속하게 대응할 수 있도록 준비하는 것이 중요합니다.

### 7.1 pfctl 명령어 활용

```bash title=pfctl_commands.sh
# PF 상태 및 정보 확인
pfctl -s info              # PF 전반적인 상태
pfctl -s rules             # 현재 로드된 규칙들
pfctl -s states            # 활성 연결 상태들
pfctl -s Tables            # 테이블 목록

# 규칙 관리
pfctl -f /etc/pf.conf      # 설정 파일 로드
pfctl -n -f /etc/pf.conf   # 문법 검사 (실제 적용 안함)
pfctl -e                   # PF 활성화
pfctl -d                   # PF 비활성화

# 테이블 관리
pfctl -t spammers -T show  # 테이블 내용 확인
pfctl -t spammers -T add 1.2.3.4  # IP 추가
pfctl -t spammers -T delete 1.2.3.4  # IP 제거
pfctl -t spammers -T flush # 테이블 비우기

# 통계 및 로그
pfctl -s info | grep -E "(Evaluations|Packets)"
pfctl -s states | wc -l    # 현재 연결 수
```

### 7.2 로그 분석 스크립트

```bash title=log_analysis.sh
#!/bin/sh
# PF 로그 분석 스크립트

LOG_FILE="/var/log/pflog"
TODAY=$(date +%Y-%m-%d)

echo "=== PF 로그 분석 보고서 ($TODAY) ==="

# 상위 차단된 IP 주소
echo "\n상위 10개 차단된 IP:"
tcpdump -n -r $LOG_FILE 2>/dev/null | \
    grep "block" | \
    awk '{print $3}' | \
    cut -d'.' -f1-4 | \
    sort | uniq -c | \
    sort -nr | head -10

# 차단된 포트별 통계
echo "\n차단된 포트별 통계:"
tcpdump -n -r $LOG_FILE 2>/dev/null | \
    grep "block" | \
    grep -o "port [0-9]*" | \
    sort | uniq -c | \
    sort -nr | head -10

# 시간대별 차단 횟수
echo "\n시간대별 차단 통계:"
tcpdump -n -r $LOG_FILE 2>/dev/null | \
    grep "block" | \
    awk '{print $1}' | \
    cut -d':' -f1 | \
    sort | uniq -c
```

**배경 설명**: 이 스크립트는 PF 로그를 분석하여 보안 위협을 파악하고, 공격 패턴을 분석하는 데 도움을 줍니다. 정기적으로 실행하여 네트워크 보안 상태를 점검할 수 있습니다.

---

## 8. 문제 해결 및 팁

PF 운영 중 발생할 수 있는 일반적인 문제들과 해결 방법, 그리고 효율적인 운영을 위한 실용적인 팁들을 정리했습니다. 이 섹션의 내용들은 실제 운영 경험을 바탕으로 작성되었으며, 문제 상황에서 신속하게 원인을 파악하고 해결할 수 있도록 도움을 줍니다.

특히 성능 최적화와 보안 강화를 위한 모범 사례들을 포함하여, 안정적이고 효율적인 PF 환경을 구축할 수 있는 지침을 제공합니다.

### 8.1 일반적인 문제와 해결책

```bash title=troubleshooting.conf
# 문제: 설정 파일 문법 오류
# 해결: 상세한 문법 검사
pfctl -nv -f /etc/pf.conf  # 자세한 문법 검사

# 문제: 연결이 예상대로 동작하지 않음
# 해결: 실시간 패킷 추적
tcpdump -i pflog0 -n      # PF 로그 실시간 모니터링
pfctl -s states | grep "your_ip"  # 특정 IP 연결 상태 확인

# 문제: 성능 저하
# 해결: 규칙 최적화
# 자주 매치되는 규칙을 위쪽에 배치
pass in quick proto tcp port 80  # quick 키워드 사용
pass in quick proto tcp port 443
# 덜 사용되는 규칙들...

# 문제: 메모리 부족
# 해결: 상태 테이블 크기 조정
set limit states 50000
set limit frags 10000
set timeout { tcp.established 3600 }
```

### 8.2 성능 최적화 팁

```bash title=optimization_tips.conf
# 1. 불필요한 로깅 제거
block quick from <spammers>  # log 키워드 제거

# 2. 상태 추적 최적화
pass out proto tcp all keep state (source-track rule, max-src-states 100)

# 3. 인터페이스별 규칙 그룹화
# 외부 인터페이스
block in on $ext_if all
pass in on $ext_if proto tcp port { 80, 443 }

# 내부 인터페이스
pass quick on $int_if all

# 4. 테이블 vs 개별 IP
# 좋음: table <servers> { 1.2.3.4, 5.6.7.8, 9.10.11.12 }
# 나쁨: from 1.2.3.4 to any or from 5.6.7.8 to any...

# 5. 정기적인 상태 정리
set timeout { tcp.closed 10, tcp.finwait 10 }
```

**배경 설명**: 이러한 최적화 기법들을 적용하면 PF의 성능을 크게 향상시킬 수 있으며, 대용량 트래픽 환경에서도 안정적인 서비스를 제공할 수 있습니다.

---

## 9. 결론

PF(Packet Filter)는 강력하고 유연한 방화벽 솔루션으로, 단순한 패킷 필터링부터 복잡한 네트워크 주소 변환, 로드 밸런싱까지 다양한 기능을 제공합니다. 이 가이드에서 다룬 내용들을 차근차근 실습해보면서 PF의 핵심 개념과 활용 방법을 익혀보세요.

처음에는 기본적인 방화벽 규칙부터 시작하여 점진적으로 고급 기능들을 추가해나가는 것을 권장합니다. 실제 운영 환경에 적용하기 전에는 반드시 테스트 환경에서 충분히 검증하고, 정기적인 모니터링과 로그 분석을 통해 지속적으로 보안성을 점검하시기 바랍니다.

### 다음 단계

1. **실습 환경 구축**: 가상머신이나 테스트 서버에서 PF 설정 연습
2. **로그 분석 자동화**: 정기적인 보안 점검을 위한 스크립트 작성
3. **고급 기능 학습**: CARP, pfsync 등의 고가용성 기능 학습
4. **커뮤니티 참여**: BSD 커뮤니티에서 최신 정보와 모범 사례 공유

PF 마스터가 되는 길은 꾸준한 학습과 실습에 있습니다. 이 가이드가 여러분의 네트워크 보안 여정에 도움이 되기를 바랍니다!
