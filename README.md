# ElasticsearchDocker

Docker Compose를 이용해 **Elasticsearch** + **Kibana** 환경을 손쉽게 구성할 수 있습니다.

## 구성 요소

| 서비스 | 이미지 | 기본 포트 |
|---|---|---|
| Elasticsearch | `docker.elastic.co/elasticsearch/elasticsearch` | `9200` |
| Kibana | `docker.elastic.co/kibana/kibana` | `5601` |

## 사전 요구사항

- [Docker](https://docs.docker.com/get-docker/) 20.10+
- [Docker Compose](https://docs.docker.com/compose/install/) v2.0+

## 빠른 시작

### 1. 환경 변수 설정

`.env` 파일을 열어 필요한 값을 수정합니다.

```env
# Elastic Stack 버전
STACK_VERSION=8.12.2

# Elasticsearch 클러스터 이름
CLUSTER_NAME=es-cluster

# elastic 유저 비밀번호 (반드시 변경하세요)
ELASTIC_PASSWORD=P@ssw0rd

# kibana_system 유저 비밀번호 (반드시 변경하세요)
KIBANA_PASSWORD=P@ssw0rd

# Elasticsearch 포트
ES_PORT=9200

# Kibana 포트
KIBANA_PORT=5601
```

### 2. 컨테이너 실행

```bash
docker compose up -d
```

### 3. 서비스 확인

```bash
docker compose ps
```

### 4. 접속

- **Elasticsearch**: http://localhost:9200
  - 사용자: `elastic`
  - 비밀번호: `.env`의 `ELASTIC_PASSWORD` 값
- **Kibana**: http://localhost:5601
  - 사용자: `elastic`
  - 비밀번호: `.env`의 `ELASTIC_PASSWORD` 값

Kibana가 완전히 기동될 때까지 1~2분 정도 소요될 수 있습니다.

### 5. Elasticsearch 동작 확인

```bash
curl -u elastic:changeme http://localhost:9200
```

정상 응답 예시:

```json
{
  "name" : "elasticsearch",
  "cluster_name" : "es-cluster",
  "version" : { ... },
  "tagline" : "You Know, for Search"
}
```

## 종료 및 데이터 삭제

컨테이너만 중지 (데이터 보존):

```bash
docker compose down
```

컨테이너 + 볼륨(데이터) 모두 삭제:

```bash
docker compose down -v
```

## 디렉토리 구조

```
.
├── docker-compose.yml              # Elasticsearch + Kibana 서비스 정의
├── .env                            # 환경 변수 (버전, 포트, 비밀번호 등)
├── elasticsearch/
│   └── config/
│       └── elasticsearch.yml       # Elasticsearch 설정 파일
└── kibana/
    └── config/
        └── kibana.yml              # Kibana 설정 파일
```

## 주의사항

- `.env` 파일의 비밀번호는 운영 환경에서 반드시 강력한 값으로 교체하세요.
- Elasticsearch는 `single-node` 모드로 실행됩니다. 클러스터 구성이 필요한 경우 `docker-compose.yml`을 수정하세요.
