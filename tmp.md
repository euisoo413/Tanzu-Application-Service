

erb " 

보쉬의 라이프 사이클


os-conf 
- 


deployment.yml 은 runtime 670973


TAS SSH LB는 브레인이 2개 이상일 경우, 사용됨.
대규모 사이트의 경우 두는 경우도 있음
->SSH Container 접근 자체에 대해 보안 이슈 

UAA, 
외부로 노출되는 것중 443이 아닌 것이 있다면 TCP로드벨런서를 써서 해야함. (많지는 않지만) 

IS 타일 적용 시 별도의 라우터가 적용되면, 타스 ㅌ 

## 

## SSL Termination


비공인 사설 IP 는 TAS 타일 내부에 
타일을 올리면 /var/tempest/workspace/defalut 하단에 



om 명령어는 -e, -p등을 넣어야 인증 및 사용가능 

sha256sum 

om cli 사용하기
# om -e [.yml] -p [PATH:파일명] #업로드하기 

아키텍처 설계
- NW
installation PAS 참고
Domain Port SSL Termination 위치 결정 (LB/GoRouter) 

- DB
TAS DB(플랫폼DB_Mysql)를 외부로 뺼것인지? (백업 복구 및 
보통은 운영이더라도 빼지 않음
TAS Fileservice (s3) Blobstore를 외부로 뺄 것인가
ㅇ Blobstore는 VM 1대만 할 수 있음
. 컨테이너 이미지는 
. 
ㅇ 

- Isolation Segment 사용 여부
. VM분리 ? (gorouter, diego 
. 노이즈 네이버후드 : TAS에서는 동 VM에 여러 컨테이너가 올라갈 경우 문제가 됨 옆 VM에 영향을 받는 것을 원치 않을 경우
. 



- 이중화 구성
. doc.pivotal.io scaling TAS for VMs 

Qurorm
application 
Blobstore는 스케일이 안됨 

mysql : UAA 등 TAS 컴포넌트에서 DB가 필요할 경우


고가용성/Failover(순단을 전제로한 무중단 서비스)
- scale out


복구
- DR:
*GSLB 및 DNS를 썼을 때의 문제점은 전파속도인데, 클라이언트나 그 중간 중간에 네트워크나 DNS 서버들이 기존 DNS 정보를 가지고 있을 경우 "버려야 할 집단"이 있을 수도 있고, 운영자는 이를 알고 있으며 미리 그 범위를 정하는 것이 중요함 

- autohealing vm: vm 프로비저닝 시간에 영향을 받음
- autohealing 컨테이너:1분 미만이나 컨테이너 기동시간에 영향을 받음
- BBR 복구 :3시간 이상 중단 전제 

*고가용성과 복구가 동시에 일어날 수도 있으나, 용어를 명확하게 할 필요는 있음 

Failback
- 장애 상태에서 원래 상태로 돌아오는 것
- 단순한 복구와도 다른 의미


DNS설계를 하는 목적은
Health watch나 smoke 테스트에 쓰인다. 

DNS, NTP 이중화 여부? 

dns 테스트의 위치는 cf push나 cf login을 하려고 하는 위치에서 해야함. (클라이언트 PC nslookup cf cli가 

또는 웹 브라우저에서 (엔드유저) 접근하려고 하는 장비(환경)에서 룩업이 되는 것


시스템 도메인과 앱스 도메인을 바꾸면 인증서도 교체하고 컨테이너용 manifest.yml의 변경이 필요함 api.sys도 변경됨. 

이론상으로는 고정되는 LB에 등록될 IP


HAProxy/Go behavior for Client Cert Vali
- 고객이 서버의 기관 인증을 하는 것처럼 반대로 서버가 고객을 검증
- Mutual TLS 

Zipkin 호출관계를 추출해 내는 기능 ( 에이가 비를 비가 씨를) 
요청 시 헤더에 그 것을 기록해둠
기록하는 걸 고라우터가 해줄 수 있는데 그걸 끄고 킬 수 있음 

고라우터가 거절하다 쉐어드 고 라우터를 못쓰게 할 수 있음
프록 

고라우터가 HA프록시에 매달려 있는데, 고라우터를 셧다운하려고 했을 때 HA프록시에게 트래픽 그만 보내라고 얘기하는 것 (Graceful 셧다우 ㄴ시간울 준다) 

지속된 20초 동안은 트래픽을 받고 셧다운 : 고라우터가 메인터넌스나 셧다운 될 때에도 비슷한 기능이 있음 


시큐어 서비스 암호화
:개발자들이 비밀번호 등을 알지 못하고 필요시 플랫폼이 비밀번호 가져와 쓴다. 

블롭스토어는 외부로 빼는게 권고사항이긴함 (s3등) 

çf push 할 때마다 백업이되는데 몇개나 할거냐 

app autoscaler는 스케일링 해주는 역할을 하며, 이것 자체는 컨테이너로 올라감 

*.sys ->cloud controller로 가고 그 클라우드 컨트를


advanced Features : 용량이 없는데 있는 것처럼 뻥튀기함.
graceful shutdown


kill -3 : app이 제대로 되어있다면 drain -> shutdown 스타트 

BBS : Container의 DB
Metric Registrar : Configure metric registarar
개발자들이 자기 애플리케이션에서 나오는 메트릭을 앱메트릭으로 등록할 수 있는데 매우 어렵다. 커스터마이징 

Errands :후속작업 

일반적으로 최초 설치시는 on 을 해줘야함. 안하면, 보쉬에서 CLI로 켜줘야함. 
최초 설치 후 apply change 시에는 굳이 안해줘도 이미 프로세스가 돌고 있다. 따라서, 설정에서 off해주면 앞으로 다시 켤일은 없다.
















blobstore 내부 확인해보기 

- ssh blobstore
- cd /var/vcap/store : 영구
- cd /var/vcap/data : 휘발성데이터 

cf 이미지 등
- cd /var/vcap/store/shared 

buildpack
- OS
- library 등


droplet : 

cf buildpacks 명령어 실행 시
- stack :OS를 뜻함 -> 실질적으로 çflinuxfs3는 TAS를 뜻함
- locked 가 True로 바꾸면 TAS는 업그레이드를 하지 않는다
- 커스터마이징을 하려고 한다면 이름을 바꿔야함


cf apps의 IS변경은 가능하다. (관리자만)
- IS -> TAS 등 

cf Push 는 개발자도 할 수 있음
- 푸쉬 하면 알아서 찍을 IS를 찾아감


cf create-buildpack -> cf update-buildpack을 해야 locked 등 더 다양한 설정이 가능 

TAS - staged droplet을 통해 기존 버전 관리를 할 수 있고 이걸 다시 재 배포할 수 있음쳑ㅊ 

* TAS를 쓰는 모든 Org-Space는 공통적인 버전 갯수를 갖게됨
[TAS Tile - setting - File storage - revision 갯수 제한] 

* IS를 다르게 가져가더라도 블롭스토어 세팅은 하나만 있기때문에 


## 컨테이너 배포과정에 대해 확인 

deps -> 빌드팩이 만든 정보들이 들어가는 폴더 / 빌드팩이 여러개면 여러개의 폴더가 생김 

rootfs -> 컨테이너의 일부
buildpack + 사용자 추가 코드 -> droplet -> 컨테이너의 일부 

container에서 root 계정 못쓰게 하는 이유는 보안상의 이유 : 컨테이너에서 VM을 접근할 수 있기 때문 

cf create: cc -> uaa
set org : cc
cf push : brain , bbs, schedule diego 

upload -> 압축 > nfs_server 블롭스토어 -> 

스태이징 용 앱/VM에 빌드팩을 다운받고 


----> 빌드 팩


droplet 아웃풋 2기가 넘을 수 없음
디스크가 따로 붙는데 그것까지 해서 4기가가 맥스이므로,,
빌드 한 것 2기가 + 그걸 압축한 것이 2기가해서 이걸 한 컨테이너에 있어야하는데, 삭제할 때까지 그걸 유지해야 하므로 4기가 맥스라고 한다. 

TAS Tile -> App Containers -> 












