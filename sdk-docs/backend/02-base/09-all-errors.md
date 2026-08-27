---
description: "모든 Error Cases"
---

# 모든 Error Cases

## ErrorCode 정리

### 200번대

#### 200번

- [Success](#Success)

### 400번대

#### 400번

- [accessTokenError](#accesstokenerror)
- [BadParameterException](#badparameterexception)
- [ConditionalCheckFailedException](#conditionalcheckfailedexception)
- [InitializeFail](#initializefail)
- [InvalidParameterValue](#invalidparametervalue)
- [NotFoundException](#notfoundexception)
- [PreconditionFailed](#preconditionfailed)
- [TransactionDataError](#transactiondataerror)
- [TransactionSizeError](#transactionsizeerror)
- [UndefinedParameterException](#undefinedparameterexception)
- [ValidationException](#validationexception)

#### 401번

- [BadUnauthorizedException](#badunauthorizedexception)

#### 402번

- [AbnormalReceipt](#abnormalreceipt)

#### 403번

- [Forbidden](#forbidden)
- [ForbiddenError](#forbiddenerror)
- [ForbiddenException](#forbiddenexception)
- [콘솔에서 입력한 차단된 사유](#콘솔에서-입력한-차단된-사유)

#### 404번

- [NotFoundException](#notfoundexception)
- [ResourceNotFoundException](#resourcenotfoundexception)

#### 405번

- [MethodNotAllowedParameterException](#methodnotallowedparameterexception)

#### 408번

- [ECONNABORTED](#econnaborted)|[Timeout](#timeout)

#### 409번

- [DuplicatedParameterException](#duplicatedparameterexception)
- [UsedReceipt](#usedreceipt)

#### 410번

- [GoneResourceException](#goneresourceexception)

#### 412번

- [PreconditionFailed](#preconditionfailed)

#### 413번

- [ServerErrorException](#servererrorexception)

#### 428번

- [Precondition Required](#precondition-required)

#### 429번

- [ProvisionThroughputExceededException](#provisionthroughputexceededexception)
- [ProvisionThroughputUpdatingException](#provisionthroughputupdatingexception)
- [Too Many Request](#too-many-request)

### 500번대

#### 500번

- [ServerErrorException](#servererrorexception-1)
- [InternalServerError](#internalservererror)

#### 502번

- [BadGateway](#badgateway)

#### 503번

- [ETIMEDOUT](#etimedout)
- [Service Temporarily Unavailable](#service-temporarily-unavailable)

### ParseException

- [ParseException](#parseexception-1)

## 200번대 Error Case

### 200 statusCode

---

#### Success

| message | returnValue : {rows:[]}                                                 |
| :------ | :---------------------------------------------------------------------- |
| 상황    | 존재하지 않는 columnName일 경우 해당 조건에 만족하는 데이터가 없을 경우 |
| 함수    | GetRandomGuildInfoV3                                                    |

## 400번대 Error Case

### 400 statusCode

---

#### accessTokenError

| message | accessToken not exist                                                |
| :------ | :------------------------------------------------------------------- |
| 상황    | 기기 로컬에 액세스 토큰이 존재하지 않는데 토큰 로그인 시도를 한 경우 |
| 함수    | LoginWithTheBackendToken / RefreshTheBackendToken                    |

---

#### BadParameterException

| message | bad {column 명} dataType, 잘못된 {column 명} dataType 입니다                                          |
| :------ | :---------------------------------------------------------------------------------------------------- |
| 상황    | (스키마) 스키마를 정의할 때 선언한 컬럼의 데이터 타입과 insert, update 하려는 데이터 타입이 다른 경우 |
| 함수    | Insert / Update / UpdateV2                                                                            |

---

#### BadParameterException

| message | bad {selectedProbabilityCard uuid}{probability id}, 잘못된 {selectedProbabilityCard uuid/probability id} 입니다 |
| :------ | :-------------------------------------------------------------------------------------------------------------- |
| 상황    | 올바르지 못한 uuid 혹은 id를 입력한 경우                                                                        |
| 함수    | GetProbability / GetProbabilitys                                                                                |

---

#### BadParameterException

| message | bad {title/content} length, 잘못된 {title/content} length 입니다 |
| :------ | :--------------------------------------------------------------- |
| 상황    | title, contents 가 최소 길이/최대 길이를 만족하지 않는 경우      |
| 함수    | SendPost                                                         |

---

#### BadParameterException

| message | bad {컬럼의 데이터 타입} dataType, 잘못된 {컬럼의 데이터 타입} dataType 입니다                       |
| :------ | :--------------------------------------------------------------------------------------------------- |
| 상황    | (스키마) 스키마를 선언한 컬럼의 데이터 타입과 param에 삽입된 컬럼의 데이터 타입이 일치하지 않은 경우 |
| 함수    | UpdateUserScore                                                                                      |

---

#### BadParameterException

| message | bad beginning or end of the nickname must not be blank , 잘못된 beginning or end of the nickname must not be blank 입니다 |
| :------ | :------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 닉네임에 앞/뒤 공백이 있는 경우                                                                                           |
| 함수    | CheckNicknameDuplication / CreateNickname / UpdateNickname                                                                |

---

#### BadParameterException

| message | bad chart id, 잘못된 chart id 입니다  |
| :------ | :------------------------------------ |
| 상황    | 올바르지 못한 uuid / id를 입력한 경우 |
| 함수    | GetChartContents / GetOneChartAndSave |

---

#### BadParameterException

| message | bad email is not match, 잘못된 email is not match 입니다 |
| :------ | :------------------------------------------------------- |
| 상황    | 잘못된 이메일을 입력한 경우                              |
| 함수    | ResetPassword                                            |

---

#### BadParameterException

| message | bad functionName, 잘못된 functionName 입니다 |
| :------ | :------------------------------------------- |
| 상황    | 함수명에 한글이 포함된 경우                  |
| 함수    | InvokeFunction                               |

---

#### BadParameterException

| message | bad goodsCount is too big, 잘못된 goodsCount is too big 입니다 |
| :------ | :------------------------------------------------------------- |
| 상황    | goodsCount가 10 이상인 경우                                    |
| 함수    | CreateGuildV3                                                  |

---

#### BadParameterException

| message | bad goodsType, 잘못된 goodsType 입니다    |
| :------ | :---------------------------------------- |
| 상황    | goodsCount 이상의 goodsType에 기부한 경우 |
| 함수    | ContributeGoodsV3                         |

---

#### BadParameterException

| message | bad guildName, 잘못된 guildName 입니다 |
| :------ | :------------------------------------- |
| 상황    | 길드명을 변경 시도한 경우              |
| 함수    | ModifyGuildV3                          |

---

#### BadParameterException

| message | bad inDate must be, 잘못된 inDate must be 입니다 |
| :------ | :----------------------------------------------- |
| 상황    | inDate가 null 혹은 string.Empty인 경우           |
| 함수    | UpdateUserScore                                  |

---

#### BadParameterException

| message | bad list data length, 잘못된 list data length 입니다                                                |
| :------ | :-------------------------------------------------------------------------------------------------- |
| 상황    | (스키마) 스키마에 list 컬럼을 선언할 때 선택한 list의 크기와 param에 입력한 list의 크기가 다른 경우 |
| 함수    | Insert / Update / UpdateV2                                                                          |

---

#### BadParameterException

| message | bad map data length, 잘못된 map data length 입니다                                               |
| :------ | :----------------------------------------------------------------------------------------------- |
| 상황    | (스키마) 스키마에 map 컬럼을 선언할 때 선택한 map의 크기와 param에 입력한 map의 크기가 다른 경우 |
| 함수    | Insert / Update / UpdateV2                                                                       |

---

#### BadParameterException

| message | bad nickname is too long, 잘못된 nickname is too long 입니다 |
| :------ | :----------------------------------------------------------- |
| 상황    | 20자 이상의 닉네임인 경우                                    |
| 함수    | CheckNicknameDuplication / CreateNickname / UpdateNickname   |

---

#### BadParameterException

| message | bad operator have to been, 잘못된 operator have to been 입니다 |
| :------ | :------------------------------------------------------------- |
| 상황    | Param에 AddCalculation 형식으로 데이터를 수정하지 않은 경우    |
| 함수    | UpdateWithCalculation / UpdateWithCalculationV2                |

---

#### BadParameterException

| message | bad password is not match, 잘못된 password is not match 입니다 |
| :------ | :------------------------------------------------------------- |
| 상황    | 잘못된 OldPassword를 입력한 경우                               |
| 함수    | UpdatePassword                                                 |

---

#### BadParameterException

| message | bad password, 잘못된 password 입니다. |
| :------ | :------------------------------------ |
| 상황    | 잘못된 비밀번호를 입력한 경우         |
| 함수    | ConfirmCustomPassword                 |

---

#### BadParameterException

| message | bad probability id, 잘못된 probability id 입니다 |
| :------ | :----------------------------------------------- |
| 상황    | 확률에 소수점 8자리 이상의 데이터가 존재할 경우  |
| 함수    | GetProbability / GetProbabilitys                 |

---

#### BadParameterException

| message | bad rankData column, 잘못된 rankData column 입니다                  |
| :------ | :------------------------------------------------------------------ |
| 상황    | 랭킹 생성 시 랭킹 항목으로 등록한 컬럼이 param에 존재하지 않을 경우 |
| 함수    | UpdateUserScore                                                     |

---

#### BadParameterException

| message | bad Sender and recipient can not be the same., 잘못된 Sender and recipient can not be the same. 입니다 |
| :------ | :----------------------------------------------------------------------------------------------------- |
| 상황    | 보내는 사람과 받는 사람이 같은 경우                                                                    |
| 함수    | SendMessage                                                                                            |

---

#### BadParameterException

| message | bad table, 잘못된 table 입니다                                                                               |
| :------ | :----------------------------------------------------------------------------------------------------------- |
| 상황    | 갱신을 시도한 랭킹이 굿즈, 메타, 유저 랭킹이 아닌 경우- 랭킹 생성 시 랭킹 항목으로 등록한 테이블이 아닌 경우 |
| 함수    | ContributeGuildGoods / UpdateGuildMetaData / UpdateUserScore / UseGuildGoods                                 |

---

#### BadParameterException

| message | bad token, 잘못된 token 입니다       |
| :------ | :----------------------------------- |
| 상황    | 유효하지 않은 영수증 토큰            |
| 함수    | ChargeTBC / IsValidateGooglePurchase |

---

#### BadParameterException

| message | bad type, 잘못된 type 입니다                                |
| :------ | :---------------------------------------------------------- |
| 상황    | 이미 ChangeCustomToFederation 완료되었는데 다시 시도한 경우 |
| 함수    | ChangeCustomToFederation                                    |

---

#### BadParameterException

| message | bad usedTBC, 잘못된 usedTBC 입니다                     |
| :------ | :----------------------------------------------------- |
| 상황    | uuid에 해당하는 캐시 아이템을 사는데 TBC가 부족한 경우 |
| 함수    | UseTBC                                                 |

---

#### BadParameterException

| message | bad 컬럼이 존재하지 않습니다., 잘못된 컬럼이 존재하지 않습니다.                                                                       |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | (스키마) 스키마를 정의하지 않은 컬럼을 insert, update 하려고 시도한 경우 / (스키마) 스키마를 정의하지 않은 컬럼이 param에 포함된 경우 |
| 함수    | Insert / Update                                                                                                                       |

---

#### ConditionalCheckFailedException

| message | The conditional request failed         |
| :------ | :------------------------------------- |
| 상황    | 존재하지 않는 postIndate를 입력한 경우 |
| 함수    | DeleteUserPostItem                     |

---

#### HttpRequestException

| message | An error occurred while sending the request(2)Error getting response stream(ReadDone2): ReceiveFailure |
| :------ | :----------------------------------------------------------------------------------------------------- |
| 상황    | 네트워크의 상태가 일시적으로 불안정하여 호출/응답에 실패할 경우                                        |
| 함수    | 서버 공통 Error Cases                                                                                  |

---

#### InitializeFail

| message | Please Edit the Backend Settings                                 |
| :------ | :--------------------------------------------------------------- |
| 상황    | Client App ID 혹은 Signature Key가 null 혹은 string.Empty인 경우 |
| 함수    | Initialize                                                       |

---

#### InvalidParameterValue

| message | User name is missing: 고유값 no-reply@backnd.com                                                                                                                                                                       |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 커스텀 아이디 찾기 시 프로젝트 명에 특수문자가 포함된 경우, 에러 발생과 함께 안내메일 미발송- 커스텀 아이디 비밀번호 찾기 시 프로젝트 명에 특수문자가 포함된 경우, 비밀번호는 변경되지만 에러 발생과 함께 안내메일 미발송 |
| 함수    | FindCustomID / ResetPassword                                                                                                                                                                                              |

---

#### NotFoundException

| message | Not Available OS                         |
| :------ | :--------------------------------------- |
| 상황    | android, ios 이외의 기기에서 호출된 경우 |
| 함수    | GetLatestVersion                         |

---

#### PreconditionFailed

| message | value only allow more than 0         |
| :------ | :----------------------------------- |
| 상황    | value가 0 이하인 경우                |
| 함수    | ContributeGuildGoods / UseGuildGoods |

---

#### TransactionDataError

| message | Not Support Type in TransactionRead: {에러가 발생한 명령} |
| :------ | :-------------------------------------------------------- |
| 상황    | transactionList에 Get 명령 이외의 작업이 존재하는 경우    |
| 함수    | TransactionRead / TransactionWrite                        |

---

#### TransactionSizeError

| message | Not Support Transaction Size: {transactionList에 존재하는 작업 개수}           |
| :------ | :----------------------------------------------------------------------------- |
| 상황    | transactionList에 10개를 초과하는 작업이 있거나 혹은 작업이 존재하지 않는 경우 |
| 함수    | TransactionRead / TransactionWrite                                             |

---

#### UndefinedParameterException

| message | undefined access_token, access_token을(를) 확인할 수 없습니다 |
| :------ | :------------------------------------------------------------ |
| 상황    | customLogin 하지 않은 상황에서 시도한 경우                    |
| 함수    | ChangeCustomToFederation                                      |

---

#### UndefinedParameterException

| message | undefined goodsCount must be more then 1, goodsCount must be more then 1을(를) 확인할 수 없습니다 |
| :------ | :------------------------------------------------------------------------------------------------ |
| 상황    | goodsCount가 0이하인 경우                                                                         |
| 함수    | CreateGuildV3                                                                                     |

---

#### UndefinedParameterException

| message | undefined nickname, nickname을(를) 확인할 수 없습니다             |
| :------ | :---------------------------------------------------------------- |
| 상황    | 빈 닉네임 혹은 string.empty로 닉네임 생성&amp;수정을 시도 한 경우 |
| 함수    | CreateNickname / UpdateNickname                                   |

---

#### ValidationException

| message | An operand in the update expression has an incorrect data type                     |
| :------ | :--------------------------------------------------------------------------------- |
| 상황    | number형이 아닌 데이터의 수정을 시도한 경우존재하지 않는 컬럼에 수정을 시도한 경우 |
| 함수    | UpdateWithCalculation                                                              |

---

#### ValidationException

| message | guildIndate is null or empty                |
| :------ | :------------------------------------------ |
| 상황    | guildIndate가 null 혹은 string.Empty인 경우 |
| 함수    | GetGuildRank                                |

---

#### ValidationException

| message | Invalid UpdateExpression: Expression size has exceeded the maximum allowed size                                   |
| :------ | :---------------------------------------------------------------------------------------------------------------- |
| 상황    | 삽입, 수정, 연산하려는 컬럼의 갯수가 총 290개를 넘을 경우- 삽입, 수정, 연산하려는 데이터의 크기가 4MB가 넘을 경우 |
| 함수    | Insert / TransactionWrite / Update / UpdateWithCalculation                                                        |

---

#### ValidationException

| message | rankUuid is null or empty                                                                                                                                                                     |
| :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 상황    | uuid가 null 혹은 string.Empty인 경우                                                                                                                                                          |
| 함수    | ContributeGuildGoods / GetGuildRank / GetMyGuildRank / GetMyRank / GetRankList / GetRankListByScore / GetRankRewardList / GetUserRank / UpdateGuildMetaData / UpdateUserScore / UseGuildGoods |

---

#### ValidationException

| message | Transaction request cannot include multiple operations on one item |
| :------ | :----------------------------------------------------------------- |
| 상황    | 동일한 row에 2개 이상의 작업을 수행했을 경우                       |
| 함수    | TransactionRead / TransactionWrite                                 |

### 401 statusCode

---

#### BadUnauthorizedException

| message | bad accessToken, 잘못된 accessToken 입니다                                                                              |
| :------ | :---------------------------------------------------------------------------------------------------------------------- |
| 상황    | Access Token이 유효하지 않는 경우 or 유저의 Access Token이 올바르지 않거나 만료된 경우(로그인 후 하루 이상 경과한 경우) |
| 함수    | 로그인 시                                                                                                               |

---

#### BadUnauthorizedException

| message | bad bad,accessToken,,잘못된,accessToken,입니다, 잘못된 bad,accessToken,,잘못된,accessToken,입니다 입니다                |
| :------ | :---------------------------------------------------------------------------------------------------------------------- |
| 상황    | Access Token이 유효하지 않는 경우 or 유저의 Access Token이 올바르지 않거나 만료된 경우(로그인 후 하루 이상 경과한 경우) |
| 함수    | 로그인 외 기능 호출 시                                                                                                  |

---

#### BadUnauthorizedException

| message | bad BackendFunctionToken, 잘못된 BackendFunctionToken 입니다 |
| :------ | :----------------------------------------------------------- |
| 상황    | 유니티에서 뒤끝펑션 인증키를 잘못 입력한 경우               |
| 함수    | GetFunctionList / InvokeFunction                             |

---

#### BadUnauthorizedException

| message | bad facebook app info, 잘못된 bad facebook app info 입니다 |
| :------ | :--------------------------------------------------------- |
| 상황    | 뒤끝 콘솔에 입력한 페이스북 정보가 잘못되었을 경우         |
| 함수    | AuthorizeFederation                                        |

---

#### BadUnauthorizedException

| message | bad,signature, 잘못된 signature 입니다                                                                                                                                                                                  |
| :------ | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 상황    | 뒤끝 서버로 요청한 함수의 Param 인자 값 내부에 정수, 소수를 합쳐 14자리 이상의 float/double형 데이터가 포함된 경우 뒤끝서버로 요청한 함수의 Param 인자 값 내부에 4depth를 이상의 Dictionary 타입의 데이터가 포함된 경우 |
| 함수    | 서버 공통 Error Cases(SDK 5.1.0 이하 버전에서만 발생)                                                                                                                                                                   |

---

#### BadUnauthorizedException

| message | bad client_date, 잘못된 client_date입니다                                  |
| :------ | :------------------------------------------------------------------------- |
| 상황    | 서버와 클라이언트의 시간이 UTC+9(한국시간) 기준 10분 이상 차이가 나는 경우 |
| 함수    | 서버 공통 Error Cases                                                      |

---

#### BadUnauthorizedException

| message | bad customId, 잘못된 customId 입니다                                                                 |
| :------ | :--------------------------------------------------------------------------------------------------- |
| 상황    | 존재하지 않는 아이디의 경우- 게스트 계정을 페더레이션 계정으로 변경한 후 게스트 로그인을 시도한 경우 |
| 함수    | CustomLogin / GuestLogin                                                                             |

---

#### BadUnauthorizedException

| message | bad customPassword, 잘못된 customPassword 입니다 |
| :------ | :----------------------------------------------- |
| 상황    | 비밀번호가 틀린 경우                             |
| 함수    | CustomLogin                                      |

---

#### BadUnauthorizedException

| message | bad google_hash, 잘못된 google_hash 입니다                                                  |
| :------ | :------------------------------------------------------------------------------------------ |
| 상황    | 안드로이드 OS 환경에서 Client(게임)와 Server(뒤끝 콘솔) 간 구글 해시키가 일치하지 않는 경우 |
| 함수    | 서버 공통 Error Cases                                                                       |

---

#### BadUnauthorizedException

| message | bad refreshToken, 잘못된 refreshToken 입니다       |
| :------ | :------------------------------------------------- |
| 상황    | 다른 기기로 로그인하여 refresh_token이 만료된 경우 |
| 함수    | LoginWithTheBackendToken / RefreshTheBackendToken  |

---

#### BadUnauthorizedException

| message | bad senderNickname, 잘못된 senderNickname 입니다 |
| :------ | :----------------------------------------------- |
| 상황    | 보내는 사람의 닉네임이 없는 경우                 |
| 함수    | SendMessage                                      |

---

#### BadUnauthorizedException

| message | bad serverStatus: maintenance, 잘못된 serverStatus: maintenance 입니다                               |
| :------ | :--------------------------------------------------------------------------------------------------- |
| 상황    | 프로젝트 상태가 '점검'일 경우(접근 허용 유저 제외)                                                   |
| 함수    | CustomLogin / CustomSignUp / AuthorizeFederation / LoginWithTheBackendToken / RefreshTheBackendToken |

---

#### BadUnauthorizedException

| message | bad serverStatus: maintenance, 잘못된 serverStatus: maintenance,입니다 |
| :------ | :--------------------------------------------------------------------- |
| 상황    | 프로젝트 상태가 '점검'일 경우(접근 허용 유저 제외)                     |
| 함수    | 로그인 외 기능 호출 시                                                 |

---

#### BadUnauthorizedException

| message | bad signature, 잘못된 signature 입니다                            |
| :------ | :---------------------------------------------------------------- |
| 상황    | Client(게임)와 Server(뒤끝 콘솔) 간 시그니처가 일치하지 않는 경우 |
| 함수    | 서버 공통 Error Cases                                             |

### 402 statusCode

---

#### AbnormalReceipt

| message | This receipt has changed status. purchaseState: cancelled      |
| :------ | :------------------------------------------------------------- |
| 상황    | 환불/취소된 영수증                                             |
| 함수    | ChargeTBC / IsValidateApplePurchase / IsValidateGooglePurchase |

### 403 statusCode

---

#### Forbidden

| message | 403 Forbidden                                                                                                           |
| :------ | :---------------------------------------------------------------------------------------------------------------------- |
| 상황    | 한 클라이언트(동일 ip)에서 너무 많은 요청을 보낸 경우 해당 에러가 발생한 클라이언트는 5분 동안 요청을 보낼 수 없습니다. |
| 함수    | 서버 공통 Error Cases                                                                                                   |

---

#### ForbiddenException

| message | Forbidden alreadyMaster, 금지된 alreadyMaster       |
| :------ | :-------------------------------------------------- |
| 상황    | 이미 길드 마스터인 유저가 길드 마스터 교체를 신청한 경우 |
| 함수    | ClaimGuildMaster                                    |

---

#### ForbiddenError

| message | Forbidden applyGuild, 금지된 applyGuild                        |
| :------ | :------------------------------------------------------------- |
| 상황    | 콘솔 설정 조건에 맞지 않는 유저가 길드 가입 요청을 시도한 경우 |
| 함수    | ApplyGuildV3                                                   |

---

#### ForbiddenError

| message | Forbidden createGuild, 금지된 createGuild                 |
| :------ | :-------------------------------------------------------- |
| 상황    | 콘솔 설정 조건에 맞지 않는 유저가 길드 생성을 시도한 경우 |
| 함수    | CreateGuildV3                                             |

---

#### ForbiddenException

| message | Forbidden expelMaster, 금지된 expelMaster |
| :------ | :---------------------------------------- |
| 상황    | 권한에 위배되는 회원을 추방 시도한 경우   |
| 함수    | ExpelMemberV3                             |

---

#### ForbiddenException

| message | Forbidden don't send yourself, 금지된 don't send yourself |
| :------ | :-------------------------------------------------------- |
| 상황    | 자기 자신에게 보낸 경우                                   |
| 함수    | SendPost                                                  |

---

#### ForbiddenException

| message | Forbidden friend, 금지된 friend                                 |
| :------ | :-------------------------------------------------------------- |
| 상황    | 뒤끝 콘솔의 소셜관리 메뉴에서 친구 최대보유수 설정값이 0인 경우 |
| 함수    | RequestFriend                                                   |

---

#### ForbiddenException

| message | Forbidden guild Member, 금지된 guild Member |
| :------ | :------------------------------------------ |
| 상황    | 마스터 이외의 길드원이 함수를 호출한 경우   |
| 함수    | SetRegistrationValueV3                      |

---

#### ForbiddenException

| message | Forbidden guildMember, 금지된 guildMember             |
| :------ | :---------------------------------------------------- |
| 상황    | 길드 마스터 교체를 신청한 유저의 길드원 정보가 없는 경우   |
| 함수    | ClaimGuildMaster                                      |

---

#### ForbiddenException

| message | Forbidden Private table can only be modified by the owner, 금지된 Private table can only be modified by the owner |
| :------ | :---------------------------------------------------------------------------------------------------------------- |
| 상황    | rivate 테이블에서 타인의 정보를 수정, 삭제하고자 하는 경우                                                        |
| 함수    | Delete / DeleteV2 / Update / UpdateV2                                                                             |

---

#### ForbiddenException

| message | Forbidden send message 0, 금지된 send message 0 |
| :------ | :---------------------------------------------- |
| 상황    | 쪽지 최대 보유수가 0인 경우                     |
| 함수    | SendMessage                                     |

---

#### ForbiddenException

| message | Forbidden useGoods, 금지된 useGoods                                   |
| :------ | :-------------------------------------------------------------------- |
| 상황    | 길드 마스터 이외의 길드원이 해당 함수를 호출한 경우 사용을 시도한 경우 |
| 함수    | UpdateCountryCodeV3 / UseGoodsV3 / UseGuildGoods                      |

---

#### 콘솔에서 입력한 차단된 사유

| message | Forbidden blocked user, 금지된 blocked user                                                        |
| :------ | :------------------------------------------------------------------------------------------------- |
| 상황    | 차단당한 계정, 유저인 경우                                                                         |
| 함수    | AuthorizeFederation / CustomLogin / GuestLogin / LoginWithTheBackendToken / RefreshTheBackendToken |

### 404 statusCode

---

#### NotFoundException

| message | column not found, column을(를) 찾을 수 없습니다                                                                                                                        |
| :------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 상황    | 스키마를 선언하지 않은 컬럼이 param에 존재할 경우 / null이거나 존재하지 않는 컬럼에 수정을 시도한 경우 / 스키마를 정의하지 않은 컬럼을 where 조건으로 검색하려 한 경우 |
| 함수    | UpdateUserScore / UpdateWithCalculation                                                                                                                                |

---

#### NotFoundException

| message | country not found, country을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------ |
| 상황    | 국가 코드가 존재하지 않는 경우                    |
| 함수    | GetCountryCodeByIndate / GetMyCountryCode         |

---

#### NotFoundException

| message | data not found, data을(를) 찾을 수 없습니다                          |
| :------ | :------------------------------------------------------------------- |
| 상황    | 존재하지 않는 inDate를 기입한 경우 / 타인의 row Indate를 기입한 경우 |
| 함수    | UpdateUserScore                                                      |

---

#### NotFoundException

| message | enrolled email not found, enrolled email을(를) 찾을 수 없습니다 |
| :------ | :-------------------------------------------------------------- |
| 상황    | 등록한 이메일이 없는 경우                                       |
| 함수    | ResetPassword                                                   |

---

#### NotFoundException

| message | federationId not found, federationId을(를) 찾을 수 없습니다 |
| :------ | :---------------------------------------------------------- |
| 상황    | 유저 정보가 뒤끝 데이터베이스에 없는 경우                   |
| 함수    | UpdateFederationEmail                                       |

---

#### NotFoundException

| message | friendGamer not found, friendGamer을(를) 찾을 수 없습니다       |
| :------ | :-------------------------------------------------------------- |
| 상황    | gamerIndate가 올바르지 않는 경우 / 해당 유저가 친구가 아닌 경우 |
| 함수    | BreakFriend                                                     |

---

#### NotFoundException

| message | game not found, game을(를) 찾을 수 없습니다    |
| :------ | :--------------------------------------------- |
| 상황    | Client App ID 혹은 Signature Key가 잘못된 경우 |
| 함수    | Initialize                                     |

---

#### NotFoundException

| message | gameInfo not found, gameInfo을(를) 찾을 수 없습니다                                                                                                                                                                   |
| :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 상황    | where 조건으로 update, delete할 테이블을 검색했으나 테이블이 존재하지 않은 경우 / 존재하지 않는 row의 삭제를 시도한 경우- 존재하지 않는 inDate인 경우 / 스키마를 정의하지 않은 컬럼을 where 조건으로 검색하려 한 경우 |
| 함수    | Update / DeleteV2 / UpdateWithCalculation                                                                                                                                                                             |

---

#### NotFoundException

| message | gamer not found, gamer을(를) 찾을 수 없습니다                                                                           |
| :------ | :---------------------------------------------------------------------------------------------------------------------- |
| 상황    | 해당 이메일의 게이머가 없는 경우 / 존재하지 않는 유저의 userIndate로 조회를 시도할 경우 / 잘못된 CustomId를 입력한 경우 |
| 함수    | FindCustomID / GetUserRank / ResetPassword                                                                              |

---

#### NotFoundException

| message | gamer가 존재하지 않습니다.                           |
| :------ | :--------------------------------------------------- |
| 상황    | 해당 닉네임, inDate를 가진 유저가 존재하지 않는 경우 |
| 함수    | GetUserInfoByInDate / GetUserInfoByNickName          |

---

#### NotFoundException

| message | guild not found, guild을(를) 찾을 수 없습니다                                                                                         |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 존재하지 않는 길드명, 길드 InDate인 경우 / 길드에 가입하지 않은 유저가 함수를 호출한 경우                                             |
| 함수    | GetGuildGoodsByIndateV3 / GetGuildIndateByGuildNameV3 / GetGuildInfoV4 / GetGuildMemberListV3 / GetGuildRank / SetRegistrationValueV3 |

---

#### NotFoundException

| message | guild rank not found, guild rank을(를) 찾을 수 없습니다                                                                                                    |
| :------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 상황    | 길드 랭킹을 생성할 때 선택하지 않은 굿즈로 갱신을 시도한 경우 / 길드 랭킹을 생성할 때 등록한 메타 데이터가 아닌 다른 메타 데이터로 랭킹 갱신을 시도한 경우 |
| 함수    | ContributeGuildGoods / UpdateGuildMetaData / UseGuildGoods                                                                                                 |

---

#### NotFoundException

| message | guildMember not found, guildMember을(를) 찾을 수 없습니다     |
| :------ | :------------------------------------------------------------ |
| 상황    | 길드에 없는 유저일 경우 / 길드 마스터 교체 시 기존 길드 마스터의 길드원 정보가 없는 경우 |
| 함수    | ClaimGuildMaster / NominateMasterV3 / NominateViceMasterV3 / ReleaseViceMasterV3    |

---

#### NotFoundException

| message | guildRank not found, guildRank을(를) 찾을 수 없습니다                           |
| :------ | :------------------------------------------------------------------------------ |
| 상황    | 해당 길드가 랭킹에 존재하지 않는 경우 / 자신의 길드가 랭킹에 존재하지 않는 경우 |
| 함수    | GetGuildRank / GetMyGuildRank                                                   |

---

#### NotFoundException

| message | it doesn't exist not found, it doesn't exist을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------------------------ |
| 상황    | ndate(게임 정보)가 잘못된 경우                                      |
| 함수    | SendPost                                                            |

---

#### NotFoundException

| message | message not found, message을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------ |
| 상황    | 해당 messageIndate의 쪽지가 없는 경우             |
| 함수    | DeleteReceivedMessage / DeleteSentMessage         |

---

#### NotFoundException

| message | no matching post not found, no matching post을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------------------------ |
| 상황    | 존재하지 않는 postIndate를 입력한 경우                              |
| 함수    | ReceiveUserPostItem                                                 |

---

#### NotFoundException

| message | notice not found, notice을(를) 찾을 수 없습니다 |
| :------ | :---------------------------------------------- |
| 상황    | 해당 공지사항, 이벤트가 존재하지 않는 경우      |
| 함수    | EventOne / NoticeOne                            |

---

#### NotFoundException

| message | post not found, post을(를) 찾을 수 없습니다                                              |
| :------ | :--------------------------------------------------------------------------------------- |
| 상황    | 존재하지 않는 postIndate를 입력한 경우 / 이미 받은 우편에 대하여 다시 받기를 요청한 경우 |
| 함수    | ReceivePostItem / ReceivePostItemAll                                                     |

---

#### NotFoundException

| message | product not found, product을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------ |
| 상황    | 뒤끝 콘솔에 등록한 제품이 없는 경우               |
| 함수    | GetProductList                                    |

---

#### NotFoundException

| message | productId not found, productId을(를) 찾을 수 없습니다 |
| :------ | :---------------------------------------------------- |
| 상황    | 존재하지 않는 아이템을 구매 시도한 경우               |
| 함수    | ChargeTBCUseTBC                                       |

---

#### NotFoundException

| message | rank not found, rank을(를) 찾을 수 없습니다                                                                                                             |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 존재하지 않는 uuid일 경우- 랭킹이 없는 경우                                                                                                             |
| 함수    | GetGuildRank / GetMyGuildRank / GetMyRank / GetRankList / GetRankListByScore / GetRankRewardList / GetRankTableList / GetUserRank / UpdateGuildMetaData |

---

#### NotFoundException

| message | rank reward not found, rank reward을(를) 찾을 수 없습니다 |
| :------ | :-------------------------------------------------------- |
| 상황    | 보상이 존재하지 않는 uuid로 조회를 시도한 경우            |
| 함수    | GetRankRewardList                                         |

---

#### NotFoundException

| message | requestedGamer not found, requestedGamer을(를) 찾을 수 없습니다                   |
| :------ | :-------------------------------------------------------------------------------- |
| 상황    | 해당 gamerIndate가 존재하지 않는 경우- 해당 유저에게 친구 신청을 하지 않았을 경우 |
| 함수    | ExpelMemberV3 / RejectApplicantV3 / RevokeSentRequest                             |

---

#### NotFoundException

| message | requestFriend not found, requestFriend을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------------------ |
| 상황    | 해당 gamerIndate가 친구가 아닐 경우                           |
| 함수    | RejectFriend                                                  |

---

#### NotFoundException

| message | Resource not found, Resource을(를) 찾을 수 없습니다   |
| :------ | :---------------------------------------------------- |
| 상황    | InDate 또는 길드명을 입력하지 않은 경우               |
| 함수    | GetGuildGoodsByIndateV3 / GetGuildIndateByGuildNameV3 |

---

#### NotFoundException

| message | table not found, table을(를) 찾을 수 없습니다                                                                                                     |
| :------ | :------------------------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 존재하지 않는 tableName인 경우                                                                                                                    |
| 함수    | Delete / DeleteV2 / Get / GetV2 / GetMyData / GetRandomGuildInfoV3 / Insert / Update / UpdateWithCalculation / UpdateWithCalculationV2 / UpdateV2 |

---

#### NotFoundException

| message | userRank not found, userRank을(를) 찾을 수 없습니다                      |
| :------ | :----------------------------------------------------------------------- |
| 상황    | 자신의 랭킹이 존재하지 않는 경우 / 해당 유저가 랭킹에 존재하지 않는 경우 |
| 함수    | GetMyRank / GetUserRank                                                  |

---

#### NotFoundException

| message | version not found, version을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------ |
| 상황    | 해당 플랫폼 버전이 등록되어 있지 않은 경우        |
| 함수    | GetLatestVersion                                  |

---

#### NotFoundException

| message | 이미 사용되었거나 틀린 번호입니다. not found, 이미 사용되었거나 틀린 번호입니다.을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------------------------------------------------------------ |
| 상황    | 기간 만료된 쿠폰의 경우 / 중복사용이 허용되지 않는 시리얼 쿠폰을 중복 사용한 경우                       |
| 함수    | UseCoupon                                                                                               |

---

#### NotFoundException

| message | 이미 사용하신 쿠폰입니다. not found, 이미 사용하신 쿠폰입니다.을(를) 찾을 수 없습니다 |
| :------ | :------------------------------------------------------------------------------------ |
| 상황    | 한 명의 게이머가 단일 쿠폰을 두 번 이상 사용 시도한 경우                              |
| 함수    | UseCoupon                                                                             |

---

#### NotFoundException

| message | 전부 사용된 쿠폰입니다. not found, 전부 사용된 쿠폰입니다.을(를) 찾을 수 없습니다 |
| :------ | :-------------------------------------------------------------------------------- |
| 상황    | 단일 쿠폰의 회수율이 100%인 경우                                                  |
| 함수    | UseCoupon                                                                         |

---

#### ResourceNotFoundException

| message | Function not found: 호출한 함수 명 |
| :------ | :--------------------------------- |
| 상황    | 존재하지 않는 함수명을 호출한 경우 |
| 함수    | InvokeFunction                     |

### 405 statusCode

---

#### MethodNotAllowedParameterException

| message | MethodNotAllowed {param value}, 이용할 수 없는 {param value} 입니다                                                      |
| :------ | :----------------------------------------------------------------------------------------------------------------------- |
| 상황    | partition, gamer_id, inDate, updatedAt, sender, receiver, reservationDate, owner_inDate 8가지 필드가 param에 포함된 경우 |
| 함수    | UpdateWithCalculation / Update / UpdateWithCalculationV2 / UpdateV2                                                      |

---

#### MethodNotAllowedParameterException

| message | MethodNotAllowed The recipients message is full, 이용할 수 없는 The recipients message is full 입니다 |
| :------ | :---------------------------------------------------------------------------------------------------- |
| 상황    | 받는 사람의 메시지함이 꽉 찬 경우                                                                     |
| 함수    | SendMessage                                                                                           |

### 408 statusCode

---

#### ECONNABORTED

| message | timeout error                                   |
| :------ | :---------------------------------------------- |
| 상황    | 서버에서 타임아웃 오류가 발생한 경우(최대 20초) |
| 함수    | 서버 공통 Error Cases                           |

---

#### Timeout

| message | timeout error                                                                        |
| :------ | :----------------------------------------------------------------------------------- |
| 상황    | SDK에서 타임아웃 오류가 발생한 경우(SDK에서 설정한 시간이 지난 이후. default: 100초) |
| 함수    | 서버 공통 Error Cases                                                                |

### 409 statusCode

---

#### DuplicatedParameterException

| message | Duplicated alreadyClaimed, 중복된 alreadyClaimed 입니다      |
| :------ | :----------------------------------------------------------- |
| 상황    | 동시 요청으로 다른 길드원이 먼저 길드 마스터 교체를 완료한 경우   |
| 함수    | ClaimGuildMaster                                             |

---

#### DuplicatedParameterException

| message | Duplicated alreadyRequestGamer, 중복된 alreadyRequestGamer 입니다 |
| :------ | :---------------------------------------------------------------- |
| 상황    | 이미 가입 요청한 길드에 다시 가입 요청 한 경우                    |
| 함수    | ApplyGuildV3                                                      |

---

#### DuplicatedParameterException

| message | Duplicated customId, 중복된 customId 입니다 |
| :------ | :------------------------------------------ |
| 상황    | 중복된 customId 가 존재하는 경우            |
| 함수    | CustomSignUp                                |

---

#### DuplicatedParameterException

| message | Duplicated existReqeustFriendGamer, 중복된 existReqeustFriendGamer 입니다 |
| :------ | :------------------------------------------------------------------------ |
| 상황    | 이미 친구 요청한 사람에게 다시 요청한 경우                                |
| 함수    | RequestFriend                                                             |

---

#### DuplicatedParameterException

| message | Duplicated federationId, 중복된 federationId 입니다                     |
| :------ | :---------------------------------------------------------------------- |
| 상황    | 이미 Federation 계정으로 가입된 계정에 커스텀 아이디 변경을 시도한 경우 |
| 함수    | ChangeCustomToFederation                                                |

---

#### DuplicatedParameterException

| message | Duplicated guildName, 중복된 guildName 입니다 |
| :------ | :-------------------------------------------- |
| 상황    | 중복된 길드명으로 생성 시도한 경우            |
| 함수    | CreateGuildV3                                 |

---

#### DuplicatedParameterException

| message | Duplicated nickname, 중복된 nickname 입니다                |
| :------ | :--------------------------------------------------------- |
| 상황    | 이미 중복된 닉네임이 있는 경우                             |
| 함수    | CheckNicknameDuplication / CreateNickname / UpdateNickname |

---

#### DuplicatedParameterException

| message | Duplicated receipt, 중복된 receipt 입니다               |
| :------ | :------------------------------------------------------ |
| 상황    | 이미 사용하거나 취소된 구독 상품의 영수증을 검증한 경우 |
| 함수    | IsValidateGooglePurchase                                |

---

#### UsedReceipt

| message | This receipt has already been used. usedDate: 해당 영수증의 사용된 시간 |
| :------ | :---------------------------------------------------------------------- |
| 상황    | 이미 사용한 영수증 토큰                                                 |
| 함수    | ChargeTBCIsValidateApplePurchaseIsValidateGooglePurchase                |

### 410 statusCode

---

#### GoneResourceException

| message | Gone expired refreshToken, 사라진 expired refreshToken 입니다 |
| :------ | :------------------------------------------------------------ |
| 상황    | 1년 뒤 refresh_token이 만료된 경우                            |
| 함수    | LoginWithTheBackendToken / RefreshTheBackendToken             |

### 412 statusCode

---

#### PreconditionFailed

| message | content length 사전 조건을 만족하지 않습니다.          |
| :------ | :----------------------------------------------------- |
| 상황    | contents 글자 수가 콘솔에서 설정한 글자 수보다 큰 경우 |
| 함수    | SendMessage                                            |

---

#### PreconditionFailed

| message | guildName 사전 조건을 만족하지 않습니다. |
| :------ | :--------------------------------------- |
| 상황    | 길드명 조건이 맞지 않는 경우             |
| 함수    | CreateGuildV3                            |

---

#### PreconditionFailed

| message | guild's version is different 사전 조건을 만족하지 않습니다. |
| :------ | :---------------------------------------------------------- |
| 상황    | Old guild를 조회한 경우- Old guild의 유저가 조회한 경우     |
| 함수    | GetGuildInfoV3 / GetMyGuildInfoV3                           |

---

#### PreconditionFailed

| message | inactiveTable 사전 조건을 만족하지 않습니다.                                                                               |
| :------ | :------------------------------------------------------------------------------------------------------------------------- |
| 상황    | 비활성화된 table의 수정/삭제를 시도한 경우 / 비활성화된 tableName에 삽입/불러오기를 시도한 경우                            |
| 함수    | Delete / DeleteV2 / Get / GetV2 / GetMyData / Insert / Update / UpdateWithCalculation / UpdateV2 / UpdateWithCalculationV2 |

---

#### PreconditionFailed

| message | inadequateAmount 사전 조건을 만족하지 않습니다.          |
| :------ | :------------------------------------------------------- |
| 상황    | 보유중인 굿즈의 양보다 많은 양의 굿즈 사용을 시도한 경우 |
| 함수    | UseGoodsV3 / UseGuildGoods                               |

---

#### PreconditionFailed

| message | JoinedGamer 사전 조건을 만족하지 않습니다.        |
| :------ | :------------------------------------------------ |
| 상황    | 이미 속해있는 길드가 존재하는 경우                |
| 함수    | ApplyGuildV3 / ApproveApplicantV3 / CreateGuildV3 |

---

#### PreconditionFailed

| message | master is active 사전 조건을 만족하지 않습니다.                        |
| :------ | :--------------------------------------------------------------------- |
| 상황    | 현재 길드 마스터의 미접속 기간이 콘솔에 설정한 기준 일수 이하인 경우   |
| 함수    | ClaimGuildMaster                                                       |

---

#### PreconditionFailed

| message | masterInactivePeriod not configured 사전 조건을 만족하지 않습니다.     |
| :------ | :--------------------------------------------------------------------- |
| 상황    | 프로젝트에 길드 마스터 자동 교체 기준 일수가 설정되지 않은 경우             |
| 함수    | ClaimGuildMaster                                                       |

---

#### PreconditionFailed

| message | maxGamerFriend 사전 조건을 만족하지 않습니다. |
| :------ | :-------------------------------------------- |
| 상황    | 요청받은 사람의 friend list 가 꽉 찬 경우     |
| 함수    | AcceptFriend                                  |

---

#### PreconditionFailed

| message | maxRequestedGamerFriend 사전 조건을 만족하지 않습니다. |
| :------ | :----------------------------------------------------- |
| 상황    | 요청한 사람의 friend list 가 꽉 찬 경우                |
| 함수    | AcceptFriend                                           |

---

#### PreconditionFailed

| message | maxRequestFriend 사전 조건을 만족하지 않습니다. |
| :------ | :---------------------------------------------- |
| 상황    | 받는 사람의 request 가 꽉 찬 경우               |
| 함수    | RequestFriend                                   |

---

#### PreconditionFailed

| message | maxSendFriendRequest 사전 조건을 만족하지 않습니다. |
| :------ | :-------------------------------------------------- |
| 상황    | 보내는 사람의 request 가 꽉 찬 경우                 |
| 함수    | RequestFriend                                       |

---

#### PreconditionFailed

| message | memberExist 사전 조건을 만족하지 않습니다.                        |
| :------ | :---------------------------------------------------------------- |
| 상황    | 길드 탈퇴를 시도하는 유저가 길드 마스터인데 길드원이 남아있는 경우 |
| 함수    | WithdrawGuildV3                                                   |

---

#### PreconditionFailed

| message | notGuildMember 사전 조건을 만족하지 않습니다.                                                                  |
| :------ | :------------------------------------------------------------------------------------------------------------- |
| 상황    | 길드에 가입하지 않은 유저가 함수를 호출한 경우                                                                 |
| 함수    | ContributeGuildGoods / GetMyGuildGoodsV3 / GetMyGuildInfoV3 / UpdateGuildMetaData / UseGoodsV3 / UseGuildGoods |

---

#### PreconditionFailed

| message | subscribed guild 사전 조건을 만족하지 않습니다.               |
| :------ | :------------------------------------------------------------ |
| 상황    | 길드에 가입하지 않은 유저가 내 길드 조회, 메타 정보 변경, 탈퇴, 길드 마스터 교체를 시도한 경우 |
| 함수    | ClaimGuildMaster / GetMyGuildInfoV4 / ModifyGuildV3 / WithdrawGuildV3                     |

---

#### PreconditionFailed

| message | type 사전 조건을 만족하지 않습니다. |
| :------ | :---------------------------------- |
| 상황    | amount 가 음수인 경우               |
| 함수    | ContributeGoodsV3                   |

---

#### PreconditionFailed

| message | type 사전 조건을 만족하지 않습니다. |
| :------ | :---------------------------------- |
| 상황    | amount 가 양수인 경우               |
| 함수    | UseGoodsV3                          |

### 413 statusCode

---

#### ServerErrorException

| message | request entity too large                                                                            |
| :------ | :-------------------------------------------------------------------------------------------------- |
| 상황    | 삽입/업데이트할 데이터의 크기가 400KB를 넘는 경우 / 하나의 row(column들의 집합)이 400KB를 넘는 경우 |
| 함수    | Insert / Update / UpdateWithCalculation / UpdateV2 / UpdateWithCalculationV2                        |

### 428 statusCode

---

#### Precondition Required

| message | Precondition Required ranking is being counted                                                                                  |
| :------ | :------------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 기간이 끝난 일회성 랭킹의 갱신을 시도한 경우 / UTC+9 04:00 ~ 05:00(한국 시간으로 새벽 4시 ~ 5시) 사이에 랭킹 갱신을 시도한 경우 |
| 함수    | ContributeGuildGoods / UpdateGuildMetaData / UpdateUserScore / UseGuildGoods                                                    |

### 429 statusCode

---

#### ProvisionThroughputExceededException

| message | ProvisionThroughputExceededException |
| :------ | :----------------------------------- |
| 상황    | 데이터베이스 할당량을 초과한 경우    |
| 함수    | 서버 공통 Error Cases                |

---

#### ProvisionThroughputUpdatingException

| message | ProvisionThroughputUpdatingException     |
| :------ | :--------------------------------------- |
| 상황    | 데이터베이스 할당량을 업데이트 중인 경우 |
| 함수    | 서버 공통 Error Cases                    |

---

#### Too Many Request

| message | guild member count 요청 횟수를 초과하였습니다. |
| :------ | :--------------------------------------------- |
| 상황    | 길드원이 이미 100명 이상인 경우                |
| 함수    | ApplyGuildV3 / ApproveApplicantV3              |

---

#### Too Many Request

| message | 정보보호를 위해 아이디 찾기/비밀번호 초기화는 하루 5회로 제한됩니다. 내일 다시 시도해 주세요. 요청 횟수를 초과하였습니다. |
| :------ | :------------------------------------------------------------------------------------------------------------------------ |
| 상황    | 24시간 이내에 5회 이상 같은 이메일 정보로 아이디/비밀번호 찾기를 시도한 경우                                              |
| 함수    | FindCustomID / ResetPassword                                                                                              |

---

#### Too Many Request

| message | 하루에 50개만 가능합니다. 요청 횟수를 초과하였습니다. |
| :------ | :---------------------------------------------------- |
| 상황    | 하루에 50개 이상의 우편을 보낸 경우                   |
| 함수    | SendPost                                              |

## 500번대 Error Case

### 500 statusCode

---

#### ServerErrorException

| message | Cannot read property 'terms' of undefined |
| :------ | :---------------------------------------- |
| 상황    | 운영 정책이 등록되어 있지 않는 경우       |
| 함수    | GetPolicy                                 |

---

#### InternalServerError

| message | {"message":"Request failed with status code 502"} |
| :------ | :------------------------------------------------ |
| 상황    | 서버가 일시적으로 과부화 상태일 경우              |
| 함수    | 서버 공통 Error Cases                             |

---

#### InternalServerError

| message | {"message":"Request failed with status code 504"} |
| :------ | :------------------------------------------------ |
| 상황    | 서버가 일시적으로 과부화 상태일 경우              |
| 함수    | 서버 공통 Error Cases                             |

### 502 statusCode

---

#### BadGateway

| message | 502 Bad Gateway(정확한 에러 메시지는 서버 공통 Error Cases 페이지 참조) |
| :------ | :---------------------------------------------------------------------- |
| 상황    | 서버가 일시적으로 과부화 상태일 경우                                    |
| 함수    | 서버 공통 Error Cases                                                   |

### 503 statusCode

---

#### ETIMEDOUT

| message | Response timeout                 |
| :------ | :------------------------------- |
| 상황    | 요청에 대한 시간이 오래걸릴 경우 |
| 함수    | 서버 공통 Error Cases            |

---

#### Service Temporarily Unavailable

| message | 503 Service Temporarily Unavailable  |
| :------ | :----------------------------------- |
| 상황    | 서버가 정상적으로 작동하지 않는 경우 |
| 함수    | 서버 공통 Error Cases                |

## ParseException

| message | In order to convert JObject as Param, it must have child value.                                        |
| :------ | :----------------------------------------------------------------------------------------------------- |
| 상황    | 빈 데이터를 Param으로 변환 시도한 경우 / 자식이 존재하지 않는 json 데이터를 Param으로 변환 시도한 경우 |
| 함수    | Parse                                                                                                  |
