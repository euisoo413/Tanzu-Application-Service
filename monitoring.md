## TAS를 위한 모니터링


- TAS를 모니터링하기 위해 네 가지를 봐야함
  1. canary-app-health
  2. cli-health
  3. MySQL Health
  4. Diego/Capacity
  5. Router

### 장애 시나리오 
- 위의 케이스 별 시나리오를 바탕으로 무엇이 문제일지 확인해본다
  |--|(1) Canary|(2) cli|(3) MySQL|(4) Router|문제점|
  |--|--|--|--|--|--|
  |--|Error|OK|OK|OK|
  
