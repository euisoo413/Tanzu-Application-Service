## vSphere - BOSH/Opsman - TAS 의 관계
### 1. vSphere - Opsmanager의 관계
- 최초 설치 시, vSphere에서 Opsman OVA 파일로 설치
- Opsmanager에 필요한 ip, storage 정보들을 포함하여 VM 생성함

### 2. Opsman - BOSH의 관계
- Opsman은 최초로 Bosh Tile을 생성함. (opsmanager에도 bosh가 들어있음)
- 배포를 위해 Opsmanager setting에서 아래 3번 항목의 내용을 포함해야 함

### 3. Bosh - vSphere의 관계
1. Bosh 타일 설치 시, vSphere에 대한 정보 필요
  - vCenter 관련 정보 (이름, 주소, 계정ID/PW)
  - vCenter에서 사용할 정보들 (Disk)
  - vCenter의 AZ 설정
    - Cluster와 Hostgrp 정보를 통해 AZ 설정을 함 
  - vCenter IP 대역 설정 (네트워크 이름, vCenter의 NW이름, IP대역 및 DNS 등)

2. CPI 통신
  - `bosh task --cpi` 명령어 사용 시 실제 사용되는 cpi 조회 가능


### 4. Opsman - TAS의 관계
- Opsman에서 TAS Tile을 Import한 후 Setting - 설정을 한 후 배포를 누르는 행위는 아래 `Deployment Manifest`를 수정하는 것과 동일한 효과를 갖는다.
- 

### 5. TAS - Bosh의 관계
- Service Broker
  - 사용자 cf cs -> Service Broker "Manifest 생성" -> 

### 관계 설정을 하며 주의할 점
- 사용자가 직접 Bosh 명령어를 통해 배포를 할 수도 있고, Opsmanager을 통해 배포할 수도 있는데 이에 대해 업데이트를 하거나 관리할 떄 오류가 발생 할 수 있다.
  - 가령, 



## 고가용성 구성
- TAS Referance Archi, 의왕 DC 기준

1. vCenter Level 
  - [vSphere 설정하기](https://docs.pivotal.io/ops-manager/2-10/vsphere/config.html#create-az) : Bosh Director Tile - Setting - AZ 
    - 
    - [Host Groups in vSphere](https://docs.pivotal.io/ops-manager/2-10/vsphere/host-groups.html)
    - [Host Affinity Rule](https://docs.pivotal.io/ops-manager/2-10/vsphere/host-groups.html#host-affinity-rule)
3. VM Resource 수량에 따른 찍히는 것
4. 




## Bosh Deployment 정리
### Bosh 배포를 위해 필요한 사항
1. Bosh Releaase 
2. Bosh Stemcell
3. Bosh Manifest

#### Bosh Release
- stemcell 위에서 소프트웨어를 구축 배포하는데 사용되는 소스코드 (예. redis-server)
- release는 하위 jobs라는 프로세스를 가지고 있음

#### Bosh Manifest
- Manifest는 Release와 Stemcell 그리고 하드웨어를 이용해 실제 배포를 요청하는 계획서
- Manifest 파일의 중복 사용을 위해 config 파일을 만들어 놓을 수도 있음
- configs 종류
1. Cloud-Config
2. Runtime-Config

```
bosh configs
bosh config [번호]
```

```
---
name: zookeeper // deployment Name

releases:
- name: zookeeper // Release Name
  version: 0.0.5 // Release Name
  url: https://bosh.io/d/github.com/cppforlife/zookeeper-release?v=0.0.5 // rla
  sha1: 65a07b7526f108b0863d76aada7fc29e2c9e2095
  
stemcells:
- alias: default
  os: ubuntu-xenial
  version: latest

instance_groups: // This is what we call VMs
- name: zookeeper
  azs: [z1, z2, z3]
  instances: 5
  jobs:
  - name: zookeeper
    release: zookeeper
    properties: {}
  vm_type: default
  stemcell: default
  persistent_disk: 10240
  networks:
  - name: default
  
```
* runtime-config

```
releases:
- name: strongswan
  version: 6.0.0

addons:
- name: security
  jobs:
  - name: strongswan
    release: strongswan
    properties:
      strongswan:
        ca_cert: ...
  - name: syslog_drain
    release: syslog
    properties:
      syslog_drain_ips: [10.10.0.20]
  include:
    deployments: [dep1, dep2]

```





## 추가적인 논의 자료
1. Docker




2. om-cli

## 작업 
- Swapon
  - VM Real Name
4. 
