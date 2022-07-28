### TAS UAA User Config


#### 목적
TAS Platform에서 Healthwatch를 사용하여 Grafana를 사용할 경우, 
일반적으로 로그인 시, ID : Admin, PW : 난수 형태로 로그인을 한다.
이럴 경우 매번 난수값을 기억해야하는 어려움이 있어, 이를 해결하기 위해 일반 계정을 생성한다.

물론 회사에서 사용중인 LDAP연동을 할 수도 있으나, 사용 목적에 맞지 않기 때문에 배제하였음



#### 활용방안
현 문서는 `Healthwatch - Grafana` 연동을 위해서만 작성하였으나, 
기타 Tanzu Application Service에서 필요할 경우 계정을 새롭게 생성하는 것도 가능하다.

#### UAA 설정 방법

* uaa 타겟 지정\
cf 타겟과 마찬가지로, 
```
uaac target [uaa.sys.pas.*.com] 
```

* uaac 인증 절차 수행
```
uaac token client get admin [Admin Client Credentials password]
```
> password 확인 방법
>>`Ops Manager TAS Tile - Credentials TAB - Admin Client Credentials` 클릭 후 난수 비밀번호 확인

* uaac 계정 생성
```
uaac user add [생성하려는 id]
```
* uaac 계정 매핑\
생성한 User가 특정 타일에서 동작할 수 있도록 권한을 넣어 줄 수 있음
```
uaac member add healthwatch.admin [생성한 id] 
```

* uaac 계정 생성 확인
```
uaac users | grep "username"
```
