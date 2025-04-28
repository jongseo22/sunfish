# 🐟 Sunfish
[발표자료 다운로드 (PDF)](./files/sunfish.pdf) / Notion: https://www.notion.so/5-1cacdde58aac808d95dacfc0c93a8ba0?pvs=4

**물리 자원 상태를 고려한 Dynamic Load Balancing**

Docker Swarm 환경에서 각 노드의 CPU, RAM 등 물리자원 상태를 모니터링하고
이에 따라 트래픽을 동적 분산시키는 Dynamic Load Balancing 프로젝트입니다.
Node Exporter와 Prometheus로 메트릭을 수집하고 Grafana로 모니터링,
그리고 자체 개발한 python script (Metrics API) 에 따라
Traefik으로 Load Balancing 하는 구조입니다.

---

## 📦 프로젝트 및 아키텍쳐 구성

| Components | Description |
|:---|:---|
| **Node Exporter** | 서버 CPU, Memory, Disk 등 메트릭 수집기 |
| **Prometheus** | Metrics 수집 및 모니터링 (데이터 1분 저장, 100MB 초과 시 reset) |
| **Grafana** | 시각화 대시보드 자동생성 |
| **Metrics API** | Python Flask 기반 커스텀 메트릭 서버 |
| **Traefik** | Swarm 기반 Reverse Proxy + 동적 Router |

```
[ Client 요청 ]
     ↓ 
[ Prometheus / Grafana ]
 ├—— Node Exporter 메트릭 수집
 ├—— Prometheus query에 따른 물리자원 정보 수집
 └—— Grafana 대시보드 자동 로딩

[ Metrics API / Traefik ]
 ├—— Flask를 활용하여 best-node 정보 전달
 └—— Traefik_conf 실시간 변동
```

---

## 🚀 빠른 시작

### 1. 클론

```bash
git clone https://github.com/jongseo22/sunfish.git
cd sunfish
```

### 2. 배포

```bash
docker stack deploy -c docker-compose.yml sunfish
```

### 3. 접속

| 기능 | 주소 |
|:---|:---|
| Grafana Dashboard | `http://<마스터서버IP>:3000/` (초기 로그인: `admin`/`admin`) |
| Traefik Web Proxy | `http://<마스터서버IP>:8088/` |
| Traefik Dashboard | `http://<마스터서버IP>:8089/dashboard/` |

---

## ⚙️ 주요 설정 설명

| 파일/폴더 | 설명 |
|:---|:---|
| `docker-compose.yml` | 전체 서비스 정의 |
| `prometheus.yml` | Prometheus scrape 설정 |
| `files/grafana.ini` | Grafana 설정 데이터 |
| `files/provisioning/datasources` | Prometheus Datasource 자동 등록 |
| `files/provisioning/dashboards` | Grafana Dashboard JSON 저장소 |
| `metrics-api/` | Python Flask Metrics API 소스코드 |
| `traefik/` | Traefik 동적 설정 (dynamic_conf.yml) |

---

## 🧹 TO-DO (개선 예정)

- 물리자원 사용량에 따른 컨테이너 자동 migration 기능 추가
- 다양한 어플리케이션에 적용시킬 수 있는 맞춤형 PromQL 구성

---

이슈 제의 언제든 환영입니다!
더 나은 Sunfish 프로젝트를 만들어보아요~
