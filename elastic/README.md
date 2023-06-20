# Elasticsearch + Docker
- `Docker` 를 이용해 `Elasticsearch` 설정하는 방법에 대해 기술합니다.
- 이 글의 설명은 `8.7.0` 버전으로 기술합니다.
- 이 글은 `Multi Node`를 기본으로 설명합니다.

## 초기 설정
Elasticsearch를 사용하기 위해서 기본적으로 설정해줘야 할 서버 설정이 있습니다.
```bash
sudo echo "vm.max_map_count=262144" >> /etc/sysctl.conf
sudo sysctl -p /etc/sysctl.conf 
```

## 버전 선택 요령
Elasticsearch 는 버전마다 기능과 사용법이 상이하며,  
Spring과 같이 사용하기 위해 정확한 버전을 선택해야합니다.
1. `Spring boot`에서의 `Elasticsearch`는 `spring-boot-starter-data-elasticsearch` 패키지를 사용합니다.
2. 공식 문서([Spring Data Elasticsearch](https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/))를 통해 내가 선택한 boot 버전과 `호환`되는 `elasticsearch` 버전을 선택합니다.

## v1
v1은 차세대 방식을 이용한 직관적이고 간단한 방법입니다.
1. es01 폴더의 내용을 토대로 Docker를 실행합니다.
2. 다음의 명령어를 통해 `kibana` 및 `node`를 추가하는 `키를 발급`받습니다.
    ```bash
    # node
    docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node
    # kibana
    docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
    ```
3. es02 폴더의 내용을 참고하여 발급받은 키를 등록하고 실행합니다.

## v2
v2는 보안 인증 파일을 직접 생성하고 실행하는 방법입니다.
1. es01 폴더의 내용을 토대로 Docker를 실행합니다.
2. Dockerfile의 내용을 토대로 생성된 `es-cert 폴더`를 `복사`합니다.
    ```bash
    docker cp es01:/usr/share/elasticsearch/config/es-certs .
    ```
3. Dockerfile에서 보안 인증 파일을 생성하므로 `Docker`를 `재실행` 합니다.
    ```bash
    docker compose down
    docker compose up -d
    ```
3. es01 노드가 실행되면 `kibana 비밀번호`를 설정해줍니다.
    ```bash
    curl -s -X POST --cacert es-certs/ca/ca.crt -u "elastic:12345a" -H "Content-Type: application/json" https://localhost:9200/_security/user/kibana_system/_password -d '{ "password": "12345a" }'
    ```
4. es02 폴더의 내옹을 토대로 Docker를 실행합니다.