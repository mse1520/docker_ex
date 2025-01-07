# Docker example
gitlab 서버를 도커로 구축하는 방법입니다.
- [gitlab docker docs](https://docs.gitlab.com/ee/install/docker/installation.html)
- [gitlab sucurity docs](https://docs.gitlab.com/ee/security/reset_user_password.html)

## 초기 설정
```bash
# 계정 초기 비밀번호
docker exec -it gitlab cat /etc/gitlab/initial_root_password
```