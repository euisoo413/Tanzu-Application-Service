
### UAA
Cloud Foundry User Account and Authentication(CF UAA)은 ID 관리 및 인증 서비스입니다. OAuth 2.0 프로바이더로 클라이언트 애플리케이션에 대한 인증과 토큰 발행 업무를 수행합니다. 

### OAuth 2.0
인증을 위한 개방형 표준 프로토콜입니다. 
이 프로토콜에서는 Third-Party 프로그램에게 리소스 소유자를 대신하여 리소스 서버에서 제공하는 자원에 대한 접근 권한을 위임하는 방식을 제공합니다.
구글, 페이스북, 카카오, 네이버 등에서 제공하는 간편 로그인 기능도 OAuth2 프로토콜 기반의 사용자 인증 기능을 제공하고 있습니다.




### UAAC 
UAAC는 Cloud Foundry에서 사용하는 UAA를 커맨드라인 버전으로 사용할 수 있도록 한다. 
UAA


### UAAC Authorized_Grant_types
#### Authorization Code Grant│권한 부여 승인 코드 방식

권한 부여 승인을 위해 자체 생성한 Authorization Code를 전달하는 방식으로 많이 쓰이고 기본이 되는 방식입니다. 간편 로그인 기능에서 사용되는 방식으로 클라이언트가 사용자를 대신하여 특정 자원에 접근을 요청할 때 사용되는 방식입니다. 보통 타사의 클라이언트에게 보호된 자원을 제공하기 위한 인증에 사용됩니다. Refresh Token의 사용이 가능한 방식입니다.

![Oauth_process](img/Oauth Process.png)




#### Client Credential Grant | 클라이언트 자격증명 승인 방식
OAuth2의 권한 부여 방식 중 가장 간단한 방식으로 클라이언트 자신이 관리하는 리소스 혹은 권한 서버에 해당 클라이언트를 위한 제한된 리소스 접근 권한이 설정되어 있는 경우 사용됩니다.

이 방식은 자격증명을 안전하게 보관할 수 있는 클라이언트에서만 사용되어야 하며, Refresh Token은 사용할 수 없습니다.

#### Refresh Token



