# 🐟 Sunfish
[발표자료 다운로드 (PDF)](./files/sunfish.pdf)

**물리 자원 상태를 고려한 Dynamic Load Balancing**

Docker Swarm 환경에서 
Traefik 리버스 프로시, Prometheus 메트릭 수집,  
커스텀 Metrics API, Grafana 대시보드를  모두 통합 관리하는 가게화된 프로젝트입니다.

---

## 📦 프로젝트 구성

| 컨퍼니트 | 설명 |
|:---|:---|
| **Traefik** | Swarm-aware Reverse Proxy + HTTP Router |
| **Prometheus** | Metrics 수집 및 모닝 |
| **Metrics API** | Python Flask 기반 커스텀 메트릭 서버 |
| **Grafana** | 시각화 대시보드 |
| **Node Exporter** | 서버 CPU, Memory, Disk 메트릭 수집기 |

---

## 📺 전체 아키텍쳐

```
[ Client 요청 ]
     ↓
[ Traefik ]
 ├—— [ Metrics API (Flask) ]  (HTTP 요청 프로키)
 └—— [ Grafana Dashboard (/dashboard)]
 
[ Prometheus ]
 ├—— Metrics API (/metrics) 스크래프
 └—— Node Exporter 스크래프

[ Grafana ]
 ├—— Prometheus 데이터 소스로 연결
 └—— 대시보드 자동 로딩
```

---

## 🚀 빠른 시작

### 1. 클론

```bash
git clone https://github.com/jongseo22/sunfish.git
cd sunfish
```

### 2. Docker Swarm 초기화 (필요시)

```bash
docker swarm init
```

### 3. 배포

```bash
docker stack deploy -c docker-compose.yml sunfish
```

### 4. 접속

| 기능 | 주소 |
|:---|:---|
| Traefik Web Proxy | `http://<서버IP>:8088/` |
| Traefik Dashboard | `http://<서버IP>:8089/dashboard/` |
| Grafana Dashboard | `http://<서버IP>:3000/` (처기 로그인: `admin`/`admin`) |

---

## ⚙️ 주요 설정 설명

| 파일/폴더 | 설명 |
|:---|:---|
| `docker-compose.yml` | 전체 서비스 정의 |
| `prometheus.yml` | Prometheus scrape 설정 |
| `metrics-api/` | Python Flask Metrics API 소스코드 |
| `traefik/` | Traefik 동적 설정 (dynamic_conf.yml) |
| `files/` | Grafana 설정(grafana.ini) 및 Provisioning 데이터 |
| `files/provisioning/datasources` | Prometheus Datasource 자동 등록 |
| `files/provisioning/dashboards` | Grafana Dashboard JSON 저장소 |

---

## 📋 기능 정보

- **Traefik**
  - Swarm 서비스 자동 감지
  - 동적 라우트 설정
  - Dashboard 활성화 (8089 포트)
- **Prometheus**
  - Metrics API, Node Exporter 스크래프
  - 데이터 1분 저장 / 100MB 초과 시 회전
- **Grafana**
  - Prometheus Datasource 자동 등록
  - 대시보드 자동 생성
- **Metrics API**
  - `/metrics` 여러 커스텀 메트릭 제공

---

## 🧰 환경변수

- `TZ=Asia/Seoul` : 모든 컨테이너 시간대를 한국 시간(KST)으로 설정
- `PYTHONUNBUFFERED=1` : Python 로그 시스템 출력 (로그 바라기없이)

---

## 🧹 TODO (개정 예정)

- HTTPS (Let's Encrypt) 자동 인증 연동
- Traefik OAuth2 인증 추가
- Grafana Alerting 기능 통합
- Metrics API 확장 (더 다양한 메트릭 제공)

---

## 📄 라이센스

> 본 프로젝트는 아직 라이센스가 명시되어 있지 않습니다.  
> 최종적으로 오프스 라이센스 (MIT, Apache 2.0 등) 추가 예정입니다.

---

# 👌 기억하세요

Pull Request, 이슈 환영합니다!  
더 다른 건강한 Sunfish 프로젝트를 함께 만드어내요.

---

# ✨

> 가게화된 인프라 모닝을 원하는다면  
> **Sunfish**와 함께라면 최고입니다. 🚀

---

# 📎 추가

- Grafana Default 계정: `admin/admin`
- Traefik Dashboard는 인증 없이 가능하무로, 운영 환경에서는 비돼적 보호가 필요합니다.

---
