# Docker CLI
Docker에서 자주 쓰는 명령어입니다.

```bash
# 실행되고 있는 컨테이너 확인하기
# 옵션: -a 전체보기
docker ps

# [SERVICE]에서 /bin/bash 명령을 실행합니다
# /bin/bash 명령을 실행시 컨테이너 내부에 접속하게 됩니다
docker exec -it [SERVICE] /bin/bash

# 설치된 이미지 리스트를 보여줍니다
docker images

# 사용되지 않는 이미지를 삭제합니다
docker image prune -a

# 사용되지 않는 캐시를 삭제합니다(이미지 포함)
docker system prune -a

# 도커 컴포즈 옵션의 내용을 읽어서 실행합니다
# 옵션: --build 컨테이너의 이미지를 빌드하는 옵션
docker compose up -d

# 실행한 도커 컴포즈 컨테이너를 정지합니다
docker compose down

# 실행한 도커 컴포즈 컨테이너의 로그를 확인합니다
# 옵션: [SERVICE]를 입력하면 컴포즈의 특정 서비스의 로그만 확인할 수 있습니다
docker compose logs [SERVICE]
```