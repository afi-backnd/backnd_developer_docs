# 서버 공통 에러 케이스

서버로 요청하는 모든 뒤끝의 기능에서 공통적으로 발생할 수 있는 에러 케이스와 이때 반환되는 BackendReturnObject 정보입니다.  

:::info 에러 예외 처리 방법 안내
statusCode와 errorCode는 동일한 코드를 가진 다른 에러 케이스가 존재할 수 있습니다.  
 status와 errorCode로 예외 처리가 한번 진행된 이후 message를 이용해서 예외 처리 해주시는 것을 추천드립니다.  
  
message 예외 처리 시에는 Equal 보다는 Contains(포함)기능을 사용하여 예외 처리를 진행해주시기 바랍니다.  
:::

> message를 이용한 에러 예외 처리를 할 경우, Equal 보다는 Contains(포함) 기능을 사용하여 예외 처리를 진행해주시기 바랍니다.  

- **네트워크의 상태가 일시적으로 불안정하여 호출/응답에 실패할 경우**  
  statusCode : 400  
  errorCode : HttpRequestException  
  message : An error occurred while sending the request(2)  
  Error getting response stream(ReadDone2): ReceiveFailure

  > 예외 처리 제공 함수 : [IsClientRequestFailError](/sdk-docs/backend/base/common-errors/functions/fail-to-request)


- **디바이스 정보를 찾지 못했을 경우**  
  statusCode : 400  
  errorCode : UndefinedParameterException  
  message : undefined device_unique_id, device_unique_id을(를) 확인할 수 없습니다

- **안드로이드 OS 환경에서 Client(게임)와 Server(뒤끝 콘솔) 간 구글 해시키가 일치하지 않는 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad google_hash, 잘못된 google_hash 입니다

- **유저의 Access Token이 올바르지 않거나 만료된 경우(로그인 시)**[<링크참조>](/sdk-docs/backend/base/common-errors/functions/fail-to-request)
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad accessToken, 잘못된 accessToken입니다

  > BMember.CustomLogin, BMember.CustomSignUp, BMember.AuthorizeFederation, BMember.LoginWithTheBackendToken, BMember.RefreshTheBackend.Token 함수만 해당됩니다.  
  > 예외 처리 제공 함수 : [IsBadAccessTokenError](/sdk-docs/backend/base/common-errors/functions/bad-accesstoken)

- **유저의 Access Token이 올바르지 않거나 만료된 경우(로그인 외 기능 호출 시)**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,accessToken,,잘못된,accessToken,입니다, 잘못된 bad,accessToken,,잘못된,accessToken,입니다 입니다

  > 예외 처리 제공 함수 : [IsBadAccessTokenError](/sdk-docs/backend/base/common-errors/functions/bad-accesstoken)

- **Client(게임)와 Server(뒤끝 콘솔) 간 시그니처가 일치하지 않는 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad signature, 잘못된 signature 입니다

- **서버와 클라이언트의 시간이 UTC+9(한국시간) 기준 10분 이상 차이가 나는 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad client_date, 잘못된 client_date입니다

- **뒤끝콘솔에서 프로젝트 상태가 점검인 경우(로그인 시)**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad serverStatus: maintenance, 잘못된 serverStatus: maintenance입니다

  > BMember.CustomLogin, BMember.CustomSignUp, BMember.AuthorizeFederation, BMember.LoginWithTheBackendToken, BMember.RefreshTheBackendToken 함수만 해당됩니다.  

- **뒤끝콘솔에서 프로젝트 상태가 점검인 경우(로그인 외 기능 호출 시)**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다, 잘못된 bad,serverStatus:,maintenance,,잘못된,serverStatus:,maintenance,입니다 입니다

  > 예외 처리 제공 함수 : [IsMaintenanceError](/sdk-docs/backend/base/common-errors/functions/maintenance-server)

- **과도한 요청이 감지된 경우**  
  동일한 ip에서 초당 1회 이상의 호출이 감지되었을 경우 발생합니다.  
  해당 에러가 발생한 클라이언트는 5분 동안 요청을 보낼 수 없습니다.  
  statusCode : 403  
  errorCode : Forbidden  
  message : 403 Forbidden(아래와 같은 에러코드)
  > 예외 처리 제공 함수 : [IsTooManyRequestError](/sdk-docs/backend/base/common-errors/functions/too-many-request)

```js
<html>
  <head>
    <title>403 Forbidden</title>
  </head>
  <body>
    <center>
      <h1>403 Forbidden</h1>
    </center>
  </body>
</html>
```

  


- **과도한 요청 감지 이후 금지 시간동안 요청을 보낼 경우**  
  statusCode : 403  
  errorCode : Forbidden  
  message : 403 Forbidden by Local

  > 예외 처리 제공 함수 : [IsTooManyRequestError](/sdk-docs/backend/base/common-errors/functions/too-many-request)

- **뒤끝 콘솔에서 서버 설정이 테스트 모드인데 10명을 초과하는 계정의 회원가입/로그인 시도를 한 경우**  
  statusCode : 403  
  errorCode : ForbiddenException  
  message : Forbidden Active User(로그인에 성공한 상태의 유저) exceed 10., 금지된 Active User(로그인에 성공한 상태의 유저) exceed 10.  

- **차단당한 디바이스에서 접속을 시도한 경우**  
  statusCode : 403  
  errorCode : ForbiddenException  
  message : Forbidden blocked device, 금지된 blocked device

- **서버에서 타임아웃 오류가 발생한 경우(최대 20초)**  
  statusCode : 408  
  errorCode : ECONNABORTED
  message : timeout error

- **SDK에서 타임아웃 오류가 발생한 경우(SDK에서 설정한 시간이 지난 이후. default: 100초)**  
  statusCode : 408  
  errorCode : Timeout  
  message : timeout error(n)

  > n은 1~5까지 출력될 수 있습니다.  

- **FREE 플랜에서 제공량을 모두 소진했을 경우**  
  statusCode : 424  
  errorCode : FailedDependency  
  message : Failed Dependency

- **데이터베이스 할당량을 초과한 경우**  
  statusCode : 429  
  errorCode : ProvisionThroughputExceededException  
  message : ProvisionThroughputExceededException

- **데이터베이스 할당량 업데이트 중인 경우**  
  statusCode : 429  
  errorCode : ProvisionThroughputUpdatingException  
  message : ProvisionThroughputUpdatingException

- **서버가 일시적으로 과부화 상태일 경우**  
  statusCode : 500  
  errorCode : InternalServerError  
  message : {"message":"Request failed with status code 502"} 또는 {"message":"Request failed with status code 504"}

  > 예외 처리 제공 함수 : [IsServerError](/sdk-docs/backend/base/common-errors/functions/temporary-overflow)

- **서버가 일시적으로 과부화 상태일 경우**  
  statusCode : 502  
  errorCode : BadGateway  
  message : 502 Bad Gateway(아래와 같은 에러코드)
  > 예외 처리 제공 함수 : [IsServerError](/sdk-docs/backend/base/common-errors/functions/temporary-overflow)

```js
<html>
  <head>
    <title>502 Bad Gateway</title>
  </head>
  <body>
    <center>
      <h1>502 Bad Gateway</h1>
    </center>
  </body>
</html>
```

  


- **요청에 대한 시간이 오래걸릴 경우**  
  statusCode : 503  
  errorCode : ETIMEDOUT  
  message : Response timeout

  > where절이 복잡한 데이터 불러오기, 많은 데이터가 있는 테이블에 대한 유저 랜덤 불러오기 등 검색에 오래걸릴 경우 발생할 수 있습니다.  
  > 예외 처리 제공 함수 : [IsServerError](/sdk-docs/backend/base/common-errors/functions/temporary-overflow)

- **서버가 정상적으로 작동하지 않는 경우**  
  statusCode : 503  
  errorCode : Service Temporarily Unavailable  
  message : 503 Service Temporarily Unavailable
  > 예외 처리 제공 함수 : [IsServerError](/sdk-docs/backend/base/common-errors/functions/temporary-overflow)

---

## SDK 5.7.0 이상 버전으로 업그레이드 시 발생하는 Error Cases

- **5.7.0 버전 업데이트 후, 정보갱신이 완전히 이루어지지 않았을 경우**  
  statusCode : 410  
  errorCode : GoneGoneResourceExceptionResourceException  
  message : Gone Gone Please proceed with the login process again, 사라진 Please proceed with the login process again 입니다, 사라진 Gone Please proceed with the login process again, 사라진 Please proceed with the login process again 입니다 입니다
  > 위와 같은 에러가 발생했을 경우, 아래와 같은 로그인 함수를 한번 더 호출해주시면 정보 갱신이 이루어지면서 에러가 발생하지 않게 됩니다.  
  
* Backend.BMember.CustomLogin
* Backend.BMember.GuestLogin
* Backend.BMember.AuthorizeFederation
* Backend.BMember.LoginWithTheBackendToken
* Backend.BMember.RefreshTheBackendToken
  

---

## SDK 5.1.0 이하 버전에서만 발생하는 Error Cases

- **뒤끝 서버로 요청한 함수의 Param 인자 값 내부에 정수, 소수를 합쳐 14자리 이상의 float/double형 데이터가 포함된 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,signature,,잘못된,signature,입니다, 잘못된 bad,signature,,잘못된,signature,입니다 입니다

- **뒤끝서버로 요청한 함수의 Param 인자 값 내부에 4depth를 이상의 Dictionary 타입의 데이터가 포함된 경우**  
  statusCode : 401  
  errorCode : BadUnauthorizedException  
  message : bad bad,signature,,잘못된,signature,입니다, 잘못된 bad,signature,,잘못된,signature,입니다 입니다
