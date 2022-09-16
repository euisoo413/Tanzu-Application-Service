## TAS를 위한 모니터링


- TAS를 모니터링하기 위해 네 가지를 봐야함
  1. canary-app-health
  2. cli-health
  3. MySQL Health
  4. Diego/Capacity
  5. Router

### 장애 시나리오 
- 위의 케이스 별 시나리오를 바탕으로 무엇이 문제일지 확인해본다
  |--|(1) Canary|(2) cli|(3) MySQL|(4) Diego/Capacity| (5) Router|문제점|
  |--|--|--|--|--|--|--|
  |1|Err|OK|OK|OK|OK|blobstore 장애|
  |2|OK|OK|OK|OK|OK|
  
  
### Apps Error Log 유형별
- Exited 0 : 일반적인 종료 혹은 에플리케이션의 로직 상 `Crashed`
- Exit code 4 : 애플리케이션이 Cloud Foundry에 의해 할당된 포트에서 수신하지 않아 TCP 상태 점검에 실패
  - 해결 : 일반적으로 자원 부족으로 나타나며, 많은 경우 Diego Cell을 늘리는 것으로 해결 가능
- Exit code 64 : 응용 프로그램이 Cloud Foundry에서 할당한 포트에 바인딩되어 있지만 일정 시간이 지난 후 상태 점검에 실패
  - 사유 : 약 1분 (정하기 나름) 내에 푸쉬되지 않으면 기다리지 않고 에러가 날 수 있어 `cf push -t 180 [APP-NAME]`처럼 수정
- 
