## 
- TAS에서 배포하는 Buildpack과 거의 동일한 의미를 가지는 Dockerfile은 Docker나 K8S 환경에서 많이 사용된다.
- TAS에서도 해당 방식으로 배포가 가능한대 이에 대해 알아보는 문서이다.


## 준비
- Docker Registry : 외부망의 경우 docker.hub을 많이 사용하나, 폐쇄망의 경우 local Repository를 구성함
- Docker : CE, CLI, containerd. 
- Docker File

## 환경 준비하기
- Docker Registry : Tanzu의 Harbor를 통해 구성 (VM)
  - VMware harbor Registry tile v2.5.3
  - 
- Docker : Opsmanager VM에 설치함
  - Download: "download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64"
  - [docker-ce](https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/docker-ce_20.10.7~3-0~ubuntu-xenial_amd64.deb)
  - [docker-ce-cli](https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/docker-ce-cli_20.10.7~3-0~ubuntu-xenial_amd64.deb)
  - [containerd.io](https://download.docker.com/linux/ubuntu/dists/xenial/pool/stable/amd64/containerd.io_1.4.6-1_amd64.deb)
 
 - 



1534608108
