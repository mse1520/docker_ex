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
`Elasticsearch`는 버전마다 기능과 사용법이 상이하며,  
`Spring`과 같이 사용하기 위해 `정확한 버전`을 선택해야합니다.
1. `Spring boot`에서의 `Elasticsearch`는 `spring-boot-starter-data-elasticsearch` 패키지를 사용합니다.
2. 공식 문서([Spring Data Elasticsearch](https://docs.spring.io/spring-data/elasticsearch/docs/current/reference/html/))를 통해 내가 선택한 boot 버전과 `호환`되는 `elasticsearch` 버전을 선택합니다.

## v1
v1은 차세대 방식을 이용한 직관적이고 간단한 방법입니다.  
다만 Docker 환경에서 관리측면이 어려운점이 있기때문에 v2 방법을 사옹합니다.
1. es01 폴더의 내용을 토대로 Docker를 실행합니다.
2. 다음의 명령어를 통해 `kibana` 및 `node`를 추가하는 `키를 발급`받습니다.
    ```bash
    # node
    docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node
    
    # kibana
    docker exec -it es01 /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
    docker exec -it kib /usr/share/kibana/bin/kibana-verification-code
    ```
3. es02 폴더의 내용을 참고하여 발급받은 키를 등록하고 실행합니다.

## v2
v2는 보안 인증 파일을 직접 생성하고 실행하는 방법입니다.  
기본적인 초기 셋팅을 쉘스크립트를 이용해서 자동화 하였습니다.  
자세한 내용을 알고싶으시면 쉘스크립트내용을 분석해보셔도 좋을것 같습니다.
1. `compose.yml`, `instances.yml` 파일의 내용을 참고해서 노드 정보를 작성합니다.
    - instances.yml 파일은 보안파일을 생성하는데 쓰입니다.
2. 초기 `실행파일 설정`을 위해 다음의 명령어를 실행합니다.
    ```bash
    chmod -R a+x bin
    ```
3. `초기 서버설정`을 위해 다음의 명령어를 실행합니다.
    ```bash
    ./bin/init
    ```
4. 기초 `폴더 생성` 명령어를 실행합니다.
    ```bash
    ./bin/dir
    ```
5. server01 `Dockerfile`에서 `보안 인증 파일`을 `생성`하므로 `한번은` 실행합니다.
    ```bash
    docker compose up -d --build
    ```
6. `인증 파일`을 `복사`합니다.
    ```bash
    ./bin/cert
    ```
7. `Docker container`를 `다시 생성`합니다.
    ```bash
    docker compose down
    docker compose up -d
    ```
8. es01 노드가 실행되면 `kibana`와 `logstash` `비밀번호`를 설정해줍니다.  
**노드가 완전히 실행되는 것을 확인후 실행하여야 합니다.**
    ```bash
    ./bin/user
    ```
9. server02 폴더의 내옹을 토대로 `추가 노드` Docker container를 실행합니다.

## 추가 지식

### cron 작성방법
```bash
# 분(0-59) 시간(0-23) 일(1-31) 월(1-12) 요일(0-7)
* * * * * # 매분마다 실행
*/15 * * * * # 15분마다 실행
15,30 * * * * # 매시 15분, 30분에 실행
15-30 * * * * # 매시 15분에서 30분까지 매분마다 실행
```