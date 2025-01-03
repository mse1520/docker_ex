# Docker example
Docker 의 기본적인 설정과 사용법 및 예제를 담은 프로젝트입니다.
- [docker docs](https://docs.docker.com/)
- [command](./docs/command.md)
- [elasticsearch](./elastic/README.md)

## 설치 및 초기 설정
### 설치
설치 `OS`는 `Ubuntu`를 기준으로 기술 하였습니다.
```bash
# uninstall docker
for pkg in docker.io docker-doc docker-compose podman-docker containerd runc; do sudo apt-get remove $pkg; done
# install packages
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
# Add Docker’s official GPG key
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
# set up the repository
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
# install docker engine
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

### 설치후 작업
`Docker` 는 기본적으로 `root` 권한으로 명령어가 실행된다.  
그렇기에 `docker` 명령어 사용시 `sudo` 를 붙이게 되는데 여간 번거로운 일이아니다.  
해결하는 방법으로는 `docker` 그룹을 생성하고 그룹에 사용할 유저를 추가하는 방법이있다.  
기술한 방법은 사용할 유저를 그룹에 추가하는 방법이다.  
*이 방법은 보안적으로 문제가 되기 때문에 `rootless mode` 를 이용하기를 권장한다.*
```bash
# Create the docker group
sudo groupadd docker
# Add your user to the docker group
sudo usermod -aG docker $USER
# 변경된 그룹을 활성화합니다
newgrp docker
```

### logging 설정
`docker container` 의 `로그`가 `무한히 증가`하는 것을 막기위한 설정입니다.
1. `/etc/docker/daemon.json` 파일 내부에 다음의 내용을 작성합니다.
2. `daemon.json` 파일이 `없을시` 생성해야 하며 `root계정 권한`으로 생성 및 수정이 가능합니다.
    ```json
    {
      "log-driver": "json-file",
      "log-opts": {
        "max-size": "10m",
        "max-file": "3" 
      }
    }
    ```
3. 파일생성 후 docker를 재실행 합니다.
    ```bash
    sudo systemctl restart docker
    ```