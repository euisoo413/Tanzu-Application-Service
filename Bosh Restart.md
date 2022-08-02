### 작업 개요
현 문서는 Bosh를 통한 TAS 플랫폼을 종료하기 위한 작업 순서를 명세한다.

- 작업 필요 사유
  *실제로는 작업이 불필요할 수는 있으나, 클러스터의 안정성을 확보하기 위해 꺼주는 것을 권고함
  - 물리적 서버 단에서 작업이 필요할 경우 
  - 물리적 서버와 연결된 환경의 변화로 인해 정상적인 서비스가 불가능 할 경우

### Container Stop
- Apps Manager를 통할 경우
  - Apps Manager -> Login -> Org -> Space -> app 조회 및 Stop
- cf CLI를 사용할 경우
  - cf target -o [ORG명] -s [SPACE 명]  

### BOSH Stop
- Bosh가 새로운 VM을 생성하는 것이 안되도록 함<br>
```bosh update-resurrection off```
- Bosh를 통해 VM 정지<br>
```bosh -d [Deployment명] stop --skip-drain```
  - Deployment명은 `bosh vms`를 통해 조회함
  - `--skip-drain`에서 `drain`은 안정적으로 순차적으로 내리는 의미를 갖고 있는데 현 시스템에서는 중단을 예고하였기에 해당 과정을 skip함

### 관련 서비스 Stop
- ELK
  - kibana
  - Logstash
  - Elastic Search

### vSphere Stop
- VM 종료
  - bosh, TAS 플랫폼이 올라간 VM을 정지시킴

### 관련 서비스 Start
- ELK
- NFS

### BOSH Start
- Bosh를 통해 VM 시작<br>
```bosh -d [Deployment명] start```
  - Deployment명은 `bosh vms`를 통해 조회함
- Bosh가 새로운 VM을 생성하도록 만듬<br>
```bosh update-resurrection on ```
