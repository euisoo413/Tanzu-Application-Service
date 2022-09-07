## 백업 

### BBR
- BBR CLI 다운로드
  - 일반적으로 Opsmanager VM에서 작업하는 것이 가장 좋으나, Jumpbox VM에서 작업해도됨
- BBR 
링크생성해서 /usr 밑에 갖다두기
```
#ln -s /var/vcap/store/bbr-1.9.7-linux-asdi     /usr/local/bin/bbr
```

### BOSH Director BBR Backup
#### 사전 점검
- bosh Director의 백업 범위
1. Blobstore : Bosh Release 
2. DB(Postgre) : Bosh Deployment Meta Data (Manifest)
3. Credhub : vSphere에 저장되어있긴 함 :Bosh Stemcell

* 백업 최적화
- 필요없는 파일들 지워놓기
- #bosh clean-up --all   // 필요없는 파일들, 미사용 파일들 삭제함 

- 






실제 시스템의 백업복구를 해보고,


주의 사항 service broker
- Pending 체인지 주의
- cf cli를 막아야 함 (API를 막기위해 LB를 막는 것을 추천)
  * 안막을 경우, Garbage가 생길 가능성이 있음 

- 소스와 데스티네이션이 다를 경우
  - 가비지 콜렉션에 대한 고민
  - 예방을 하는 수 밖에 없음

### TAS BBR Backup


#### 타일별 백업 고민

Isolation Segment 복구
- Bosh CCK / BBR이 아님을 유의 (BBR의 대상은 NFS, Mysql, Bosh VM) 

- cf set-org로 shared 로 바꿔서 복구할 수도 있으나 백엔드에 연관된 것들과의 호환성을 고려할 필요가 있음


- MySQL 백업은 마스터를 살리는 것을 먼저 실행해보고 BBR을 수행해봄 

- NFS를 외부에 설치하면 BBR을 할수 없다.







서비스 브로커의 인스턴스 생성 과정
1) CF create 서비스
2) Cloud Controller가 받아서 정보를 p-CC 등 서비스 브로커 타일에 전달
3) 서비스 브로커는 Bosh로 서비스 생성 요청
4) 보쉬는 VM (서비스 인스턴스) 생성







TAS 모니터링
- 헬스워치 - 네모 네개 - TAS - 
- TAS MySQL (Persistent) Health가 정상이어야 백업할 수 있는 상태
- App Instances - 
- Blobstore 정상여부 : 
  * CLI Health에서 5분마다 Push가 되면 됨
  * Canary App Health 

- 백업을 할 때 
(BBR Instance)를 설정하느냐 안하느냐의 차이는
없으면 NFS만 백업할 수 있고, 나머지 VM에 대해 백업을 하지 않음 
BBR VM 내부에 개별 VM의 백업 스크립트를 가지고 있음 

#### Idea
* BBR Metadata에서 특정 정보만 남겨서 그것만 백업 가능할까?
- 예시) 백업 후 결과 디렉토리 및 메타데이터에서 "-name: nfs " 등을 삭제하면 특정 VM만 백업
- 결과) 불가능 - BBR VM에서 해당 작업이 가능하지 않음, 이미 스크립트가 돌고 있어서 해당 파일을 찾고 있기 때문에 추가적인 작업 불가



bosh cck 는 VM과 Disk(Persistent만) 체크함
cck의 6가지 결과물에 대해 디스크에 문제가 없을 경우는 다른 선택지가 나온다




- nfs 서버를 정지시키고 재 시작 시 앱 자도 ㅇ재시작


* nfs 디스크에 문제를 일으키고 테스트 해보기
- 
- 블롭스토어 엔진엑스 리스타트를 안하면,  




* 단순하게 scp로 옮겨서 압축해제하기 테스트
- tar 압축해제 -> store 디렉토리 아래로
- /var/vcap/store는 루트 권한인데 scp는 루트 권한이 없어서 chmod해서 권한을 바꿔놓고 한다. ~/tmp를 777로만 해도 가능함



* NFS의 persistent 디스크를 삭제할 경우 (vCenter에서) BOSH는 인지하지 못하고 TAS에서는 CF PUSH, Restart 등을 못함 

1. referance삭제
* bosh cck 를 했을때는 reference 삭제하라고 하는데 이는 Disk CID를 삭제한다는 의미임. 

2. bosh recreate
- 단지 --recreate는 Vm만 생각하는 것으로 Disk를 생각했을 때는 의미없는 활동임
- Disk --recreate-persistent-disk해야하는데, 이는 opsmanager 에서 해줘도 되는데 이건 시간이 오래걸리니 bosh 명령어로 실행해본다.
- 과정 
  - manifest 추출하기
  - deploy하기 : --recreate-persistent-disk --fix
- VM과 다르게 Disk의 경우 재배포 방법만 있음, VM은 cck할 수 있음
-  --recreate-persistent-disk <- 이 명령어는 가상 디스크를 새로 만들고 정보를 복사 해넣는다는 것이고 모든 인스턴스에 적용이 됨 

- 작업 중에 VM OS 이 마운트를 물고 있다고 생각해서 , skip-drain 을 해줘서 넘어갔음 : 
