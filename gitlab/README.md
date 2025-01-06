# Docker example
gitlab 서버를 도커로 구축하는 방법입니다.
- [gitlab docker docs](https://docs.gitlab.com/ee/install/docker/installation.html)
- [gitlab sucurity docs](https://docs.gitlab.com/ee/security/reset_user_password.html)

## 초기 설정
```bash
# root 계정 초기 패스워드 설정
docker exec -it gitlab gitlab-rake "gitlab:password:reset[root]"
```