## 네트워크 추가하기
### 목적 
- TAS 환경에서 추가로 네트워크를 할당해야하는 경우가 있다. 여러 케이스가 있겠으나, 가장 대표적인 경우는 `새로운 서비스`를 별도의 영역으로 나눌 때 일 것이다.
- 즉, 이 문서는 isolation segments를 별도로 만들고자 할 때 사용함

|작업순서|작업대상|작업내용|
|--|--|--|
|1|네트워크 대역할당|vCenter에 네트워크 입력을 요청함|
|2|LB할당|NSX-T 혹은 일반 LB에 L4 혹은 L7을 생성하고 VIP를 할당함|
|3|방화벽 정책|사용자 대역에서 VIP로 방화벽이 열려야 함|
|4|고라우터 생성|고라우터를 생성하여 위 LB에 매핑해주기 위해 생성|
|5|DNS매핑|기존 생성된 앱들이 마이그레이션이 될 수 있도록 |
|6|vpn 정책|
