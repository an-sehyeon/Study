# Elasticsearch & Kibana 설치
## 1. WSL 설치
## WSL이란?
```plantuml
WSL(Windows Subsystem for Linux)은 Windows 환경에서 Linux를 실행할 수 있도록 지원하는 기능이다.

Docker Desktop은 내부적으로 Linux 환경을 사용하기 때문에 WSL2 설치가 필요하다.
```
### 설치
```plantuml
PowerShell을 관리자 권한으로 실행
wsl --install

설치가 완료되면 PC 재부팅
```
### 설치 확인
``` 
wsl --status

실행 결과
기본 배포: Ubuntu
기본 버전: 2

출력되면 정상 설치된 상태이다.
```

## 2. Docker Desktop 설치
## docker란?
* Docker는 애플리케이션을 컨테이너 단위로 실행할 수 있도록 지원하는 플랫폼.
* Elasticsearch와 Kibana를 직접 설치하는 대신 Docker 컨테이너를 통해 간편하게 실행할 수 있다.

### 설치
```plantuml
Docker Desktop 공식 홈페이지에서 Windows용 Docker Desktop을 다운로드.(Windows-AMD64)

설치 시 아래 옵션을 활성화한다.
Use WSL 2 based engine

설치 완료 후 Docker Desktop 실행.
```

## 3. Docker 설치 확인
``` 
PowerShell에서 명령어 실행

Docker 버전 확인
docker --version

Docker Compose 버전 확인
docker compose version

실행중인 컨테이너 확인
docker ps
```

## 4. Elasticsearch 실행 환경 구성
### 프로젝트 폴더 구성
```plantuml
C:\study\elasticsearch

docker-compose.yml 생성
```

## 5. docker-compose.yml 작성
```
version: '3.8'
services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:8.6.0
    container_name: elasticsearch
    environment:
      - discovery.type=single-node
      - xpack.security.enabled=false
    ports:
      - "9200:9200"
      - "9300:9300"
    volumes:
      - esdata:/usr/share/elasticsearch/data
    networks:
      - es_network

  kibana:
    image: docker.elastic.co/kibana/kibana:8.6.0
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    ports:
      - "5601:5601"
    depends_on:
      - elasticsearch
    networks:
      - es_network

networks:
  es_network:
    driver: bridge    

volumes:
  esdata:
    driver: local
```
Kibana란? <br>
- 키바나는 엘라스틱서치의 데이터를 시각화하고 대시보드로 보여주는 도구로 엘라스틱 서치의 데이터를 쉽게 조회, 분석, 모니터링 할 수 있도록 도와주는 도구

### 🚫 주의할 점
- xpack.security.enabled=false 설정을 true로 해주면 접속시 username, password가 필요함.
- 이 때 엘라스틱서치의 기본 username은 elastic으로 설정되어 있는데 kibana에서 해당 USERNAME을 통해 접근하려면 아래와 같은 문제가 발생한다.
<br> **"value of "elastic" is forbidden. This is a superuser account that cannot write to system indices that Kibana needs to function."** <br>
- 이는 Elasticsearch 8.x 버전에서 elastic 사용자가 Kibana에 필요한 시스템 인덱스를 작성할 수 없기 때문이다.
- 이를 해결하려면 서비스 계정 토큰을 사용하거나 kibana_system 사용자를 사용해야 한다.

## 6. Elasticsearch & Kibana 실행

### PowerShell에서 프로젝트 폴더로 이동

```bash
cd C:\study\elasticsearch
```

### Elasticsearch, Kibana 실행

```bash
docker compose up -d
```

#### 명령어 설명

```plantuml
docker compose
→ docker-compose.yml 파일을 기반으로 컨테이너 실행

up
→ 컨테이너 생성 및 실행

-d
→ 백그라운드 실행
```

### 컨테이너 실행 확인

```bash
docker ps
```

실행 결과

```plantuml
elasticsearch
kibana
```

두 컨테이너가 모두 실행 중이라면 정상적으로 실행된 상태이다.

---

## 7. Elasticsearch 접속 확인

브라우저 접속

```text
http://localhost:9200
```

실행 결과

```json
{
  "name": "elasticsearch",
  "cluster_name": "docker-cluster",
  "version": {
    "number": "8.6.0"
  }
}
```

JSON 형태의 응답이 출력되면 정상적으로 실행된 상태이다.

---

## 8. Kibana 접속 확인

브라우저 접속

```text
http://localhost:5601
```

Kibana 메인 화면이 출력되면 정상적으로 실행된 상태이다.

---

## 9. Nori 형태소 분석기 설치

### Nori란?

* Elasticsearch에서 제공하는 한국어 형태소 분석기
* 한국어 문장을 의미 단위로 분리하여 검색 정확도를 높여주는 기능

예를 들어 상품명이 아래와 같다고 가정한다.

```text
빨간색 물결 악세서리
```

기본 분석기의 경우

```text
빨간색
물결
악세서리
```

정도로만 분리된다.

하지만 Nori를 사용하면

```text
빨간색
물결
악세서리
```

뿐만 아니라 형태소 단위 분석을 통해 보다 정확한 검색 결과를 제공할 수 있다.

예를 들어 사용자가

```text
물결
```

또는

```text
악세서리
```

를 검색했을 때 해당 상품을 빠르게 검색할 수 있다.

---

### Nori 설치

PowerShell 실행

```bash
docker exec -it elasticsearch bin/elasticsearch-plugin install analysis-nori
```

설치 중 아래 메시지가 출력되면

```text
Continue with installation? [y/N]
```

```text
y
```

를 입력한다.

---

## 10. Elasticsearch 재시작

Nori 플러그인 설치 후 Elasticsearch를 재시작한다.

```bash
docker restart elasticsearch
```

---

## 11. Nori 설치 확인

설치된 플러그인 목록 확인

```bash
docker exec -it elasticsearch bin/elasticsearch-plugin list
```

실행 결과

```text
analysis-nori
```

가 출력되면 정상적으로 설치된 상태이다.
