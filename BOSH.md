## BOSH의 개념
BOSH is a project that unifies release engineering, deployment, and lifecycle management of small and large-scale cloud software. BOSH can provision and deploy software over hundreds of VMs. It also performs monitoring, failure recovery, and software updates with zero-to-minimal downtime.

## BOSH와 타 개념간의 관계
- Ops Manager : Tile을 시공하기 위한 설계도를 만드는 역할을 함 (Tile UI를 통해 설정 변경하고 Apply Change)
- BOSH : Human "시공"의 역할을 함 (설계도를 바탕으로), 문제가 생길 경우 BOSH는 VM을 살리는 노력을 함 
- vCenter : HW. VM의 실제 생성하는 역할(노가다)

## VM을 살리려는 노력들
- BOSH
  - Resurrection
  - BOSH CCK
- vCenter : 
  - DRS
  - HA

* 

## BOSH와 Opsmanager의 동작 방식
- Opsmanager는 vCenter를 통해 OVA파일로 VM을 생성함
- API로 vCenter 통신을 하여 Bosh VM을 생성함. bosh.state.jason 생성
- Opsmanager는 이 이후 BOSH Director VM을 통해 각종 Tile (VM)을 생성함
- BOSH는 위의 Opsmanager의 요청을 통해 vCenter에 생성하는 역할을 함

## Stemcell
- Opsmanager가 vCenter에 생성된 후, vCenter Datastore에 sc-~@!#NJ 형태의 파일로 vm TEMPLATE를 저장함
- 위의 동작 방식 과정에서, opsmanager -> BOSH -> vCenter에서 VM 생성을 할 때 이 sc template를 사용함
- `/var/vcap/store`에 저장되어 있음. tar file 은 persistence Disk (VM Disk)

## BOSH가 VM을 생성하기 위해 필요한 파일
- `bosh upload-stemcell` : bosh가 VM을 생성하는 템플릿 (ex. bosh-stemcell-621.109[version]-vsphere[platform]-esxi-ubuntu[OS]-xenial[OS version]-go_agent.tgz
  - 획득 방법 : 주로, OS 제공자에서 전달하는 내용을 다운받아야 함
- `bosh upload-release` : stemcell 위에 올라갈 애플리케이션 같은 것 (= WAR/JAR) (ex.minio-boshrelease~)
  - 획득 방법 : 주로, 깃헙이나 개발사에서 제공하는 파일을 다운받아야함
- `manifest.yml` : 설계도. 어떤 vSphere(혹은, 플랫폼)에 어떤 vm 사이즈, 네트워크, az 등으로 구성할지를 알려줌
  - 획득 방법 : tgz 파일이나 패키지 내 별도의 yml 참조
  - 아래 config, os-conf 처럼 bosh vm의 형태를 결정하기 위해 필요하다.
  - Opsmanager의 경우, yml파일을 UI를 통해 작성하고 이를 apply change해서 반영한다는 것과 같음

## Config와 Runtime-config
- cpi-config : vCenter info 
- cloud-config : vm_type, disk_type, network name, az name
- 
- 



|---|---|---|
|타일|역할|
|TAS|
|BOSH|
|vSphere|VM생성/


## BOSH 명령어들

* BOSH가 찍은 여러 VM에 명령어를 보내고 싶은 경우
```
bosh -d "cf-asdasd' ssh [bosh가 알고 있는 vm 이름] -c "커맨드 내용' 
```
* BOSH가 찍은 TAS VM의 로그를 보고 싶은 경우
```
bosh -d "cf-asdasd' logs [bosh가 알고 있는 vm 이름]/[instance] -f
```



