## Maestro compatibility table
- https://docs.pivotal.io/ops-manager/2-10/security/pcf-infrastructure/maestro-tile-compatibility.html
## IF ALL service tiles are compatable with Maestro=> Maestro
- https://docs.pivotal.io/ops-manager/2-10/security/pcf-infrastructure/advanced-certificate-rotation.html#services-rotation 
## Maestro incompatibile(TKGi)=>  Ops Manager API 
- https://docs.pivotal.io/ops-manager/2-10/security/pcf-infrastructure/managing-certificates.html
- service tls ca: https://docs.pivotal.io/ops-manager/2-10/security/pcf-infrastructure/services_tls_ca_rotate.html
## Opsman 2.7-2.8, with Mysql, redis, rabbitmq, gemfire 
- https://community.pivotal.io/s/article/Rotating-services-tls-ca-certificate-using-credhub-transitional-certificate-rotation-feature?language=en_US

## Certs Expired already: 
- How to rotate an already expired /services/tls_ca certificate: https://community.pivotal.io/s/article/How-to-rotate-and-already-expired-services-tls-ca-certificate?language=en_US


## Root CA, Leaf Cert
### 개념
- 중요 데이터를 다루는 대부분의 웹 사이트들은 HTTPS를 통해 서비스를 하는데, SSL 인증서는 HTTPS 통신을 통해 웹 사이트의 콘텐츠가 브라우저에 전송되는 동안 사이트에서 주고받는 모든 데이터를 암호화 한다. 엄밀히 말하면, 보안이 적용된 통신 채널을 제공하는 것이다.

SSL 인증서를 통해 암호화/복호화를 거치게 되며, 클라이언트와 서버 간의 통신을 보안화하여 스니핑이나 피싱을 방지할 수 있다.

SSL인증서는 CA라고하는 인증서 발급회사에서 시작하는 Root 인증서에서 시작된다. 일반적으로 인증서는 Root, Intermediate, Leaf(서버) 3단계로 구분하며, 이를 인증서 체인이라고 한다. 일반적으로 사용자들이 구매하는 인증서는 Leaf 인증서를 의미한다. 


인증을 받는 절차
- 사용자는 브라우저를 통해 특정 웹 사이트에 접근하면, 브라우저는 해당 웹 사이트에서 인증서를 내려받는다. 브라우저는 내려받은 인증서를 분석하여 Root에 다시 연결하기 시작한다. 이렇게 계속 추적하며 루트 인증서에 도달할 때까지 역추적한다.

### 일반적인 CA 인증서 생성 과정 
*openSSL을 사용한 경우*
[Root CA 인증서를 생성함]
1. RSA Key Pair 생성
2. CSR 생성을 위한 rootca_openssl.conf 생성
3. 1번과 2번을 통해 "Self-Signed" 인증서를 생성할 수 있음

[SSL 인증서 생성]
1. SSL 호스트에서 사용할 키 페어 생성함
2. CSR 생성을 위한 host_openssl.conf 생성
3. 1,2번 과정을 통해 "Self-Signed" 인증서로 서명한 인증서를 발급할 수 있음

### TAS 플랫폼에서 인증서 생성을 하는 경우
#### 개요
TAS 플랫폼은 기본적으로 SSL 통신을 함. Platform 서비스를 위한 VM 들 간에도 서로 ssl 통신을 하기 때문에 정상적인 서비스를 위해 정상적인 인증서를 등록해두어야 함

#### 회전을 하는 이유
TAS에서는 내부적으로 위와 같은 오픈소스를 사용하여 pivotal 명의의 Root CA를 생성하고, TAS VM들은 내부적으로 SSL 통신을 하는데 이 때 사용하기 위한 SSL인증서를 pivotal의 Root CA가 서명한 인증서를 통해 통신을 한다. 현재 당사에서 사용하고 있는 인증서는 4년을 유효기간으로 두었기 때문에, 주기적으로 작업을 해줘야 함

#### GoRouter가 사용하는 SSL 인증서
- Go Router에 사용될 인증서는 RootCA에서 인증한 인증서로 생성된다. (물론, 개별적으로 CA가 서명한 인증서를 사용할 수도 있음)
- 따라서, 이 인증서를 조회해보면 RootCA 정보(유효기한, 발급기관 등)를 볼 수 있고, 일반 사용자들이 App Container에 접근했을 때 사용하게 될 인증서이기도 함


### 인증서의 확인
#### 브라우저를 통한 확인 방법
- 브라우저 주소 입력창에서 https://를 호출한 뒤, "자물쇠" 모양을 클릭하여 자세한 사항을 확인 할 수 있음
![image](https://user-images.githubusercontent.com/63759241/203208207-1509146d-97f4-4c90-90de-ac71710b08dd.png)

#### openssl 명령어로 확인하기
```
$ openssl x509 -in [SSL 인증서 경로]/[cert 파일명] -noout -dates
```
[Ex]
```
$ openssl x509 -in /data/ca_ssl.crt -noout -dates
```

#### 인증서가 비정상적인 경우 사례
- 인증서 교체 작업 중 (신규 인증서 적용 / 기존 인증서 미삭제), Apps Manager에 접근은 되었지만 Apps Manager에 아무것도 보이지 않는 문제가 있었음.
- 신규 인증서를 다른 인증서로 잘못 넣은 문제였던 것인데, Apps Manager UI는 기존 인증서로 인증을 하였으나, 기타 다른 VM들은 기존 인증서로 작업


### with Ops Manager API 

```
# ssh into ops manager VM.

$ uaac target https://<opsman.domain.url>/uaa --skip-ssl-validation
Target: https://<opsman.domain.url>/uaa
Context: admin, from client opsman

$ uaac token owner get
Client ID:  opsman
Client secret:
User name:  admin
Password:  <opsman ui admin password>

Successfully fetched token via owner password grant.
Context: admin, from client opsman

$ uaac context
[1]*[https://<opsman.domain.url>/uaa]
  skip_ssl_validation: true

  [0]*[admin]
      user_id: 
      client_id: opsman
      access_token: xxxxx.....
      token_type: bearer
      expires_in: 43199
      scope: opsman.admin scim.me uaa.admin clients.admin
      jti: 

$ export TOKEN="<uaac context의 결과에서 access_token을 붙여넣음>"


# ops manager root CA조회
$ curl -k https://<opsman.domain.url>/api/v0/certificate_authorities -H "Authorization: Bearer $TOKEN"
HTTP/1.1 200 OK
{
  "certificate_authorities": [
    {
      "guid": "f7bc18f34f2a7a9403c3",
      "issuer": "Pivotal",
      "created_on": "2017-01-09",
      "expires_on": "2021-01-09",
      "active": true,
      "cert_pem": "-----BEGIN CERTIFICATE-----\nMIIC+zCCAeOgAwIBAgI....etc"
    }
  ]
}

# 만료 예정 인증서 조회
$ curl "https://OPS-MAN-FQDN/api/v0/deployed/certificates?expires_within=6m" \
      -H "Authorization: Bearer YOUR-UAA-ACCESS-TOKEN"
     
     
# ops manager에 새로운  root CA생성
$ curl "https://OPS-MAN-FQDN/api/v0/certificate_authorities/generate" \
  -X POST \
  -H "Authorization: Bearer YOUR-UAA-ACCESS-TOKEN" \
  -H "Content-Type: application/json" \
  -d '{}'


# ops manager root CA조회
$ curl -k https://<opsman.domain.url>/api/v0/certificate_authorities -H "Authorization: Bearer $TOKEN"

HTTP/1.1 200 OK
{
  "certificate_authorities": [
    {
      "guid": "f7bc18f34f2a7a9403c3",
      "issuer": "Pivotal",
      "created_on": "2017-01-09",
      "expires_on": "2021-01-09",
      "active": true,
      "cert_pem": "-----BEGIN CERTIFICATE-----\nMIIC+zCCAeOgAwIBAgI....etc"
    }
    {
      "guid": "a8ee01e33e3e3e3303e3",
      "issuer": "Pivotal",
      "created_on": "2017-04-09",
      "expires_on": "2021-04-09",
      "active": false,
      "cert_pem": "-----BEGIN CERTIFICATE-----\zBBBC+eAAAe1gAwAAAeZ....etc"
    }
  ]
}

# Ops Manager UI에 로그인 to https://OPS-MAN-FQDN 

# BOSH Director tile> Config>  Recreate All VMs 체크

# Apply Changes



# 적용 후 ops manager root CA조회
$ curl -k https://<opsman.domain.url>/api/v0/certificate_authorities -H "Authorization: Bearer $TOKEN"

# 만료 예정 인증서 조회
$ curl "https://OPS-MAN-FQDN/api/v0/deployed/certificates?expires_within=6m" \
      -H "Authorization: Bearer YOUR-UAA-ACCESS-TOKEN"
     


  
```
