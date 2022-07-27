
##



### TAS Authentication

#### UAA

* uaa의 대상이 되는 타겟 URI를 지정함
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
* uaac 계정 매핑
```
uaac member add healthwatch.admin [생성한 id] 
```
