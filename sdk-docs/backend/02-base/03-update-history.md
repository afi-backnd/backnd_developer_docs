---
sidebar_label: "업데이트 내역"
description: "업데이트 상세 내역"
---

# 업데이트 상세 내역

:::caution 뒤끝펑션 0.1.2 버전 비호환 문제
SDK 5.9.0 버전에서 0.1.2 이하(2021-12-28 이전에 배포된 펑션) 버전의 뒤끝펑션을 호출할 경우,  
**펑션 내부에서 호출하는 함수에 에러가 발생하여 정상적으로 작동하지 않게 됩니다.**
  
꼭 SDK를 업그레이드하기 전에 **펑션을 0.2.0 버전으로 업그레이드**하여 사용해주세요.  
:::

:::danger SDK 5.6.0 이하 업데이트 후 410 에러 발생 문제
SDK 5.6.0 이하 버전에서 현재 버전으로 업데이트를 할 경우, 모든 뒤끝 함수 호출에서 **410 GoneResourceException** 에러가 발생할 수 있습니다.  
재로그인 시, 해당 에러가 이후 발생하지 않게되므로 아래와 같은 로그인 함수를 다시 호출할 수 있도록 구성해주세요.  


  * Backend.BMember.CustomLogin
  * Backend.BMember.GuestLogin
  * Backend.BMember.AuthorizeFederation
  * Backend.BMember.LoginWithTheBackendToken
  * Backend.BMember.RefreshTheBackendToken
:::

:::info SDK 5.8.0 403 Forbidden 로직 개편 안내
SDK 5.8.0 버전에서는 과도한 요청으로 발생되는 403 Forbidden 에러 발생 시, 이후부터는 서버로 보내는 송신을 로컬에서 5분 30초동안 금지하며 해당 시간 내 함수 호출 시 다음과 같은 에러를 리턴합니다.  

statusCode : 403  
errorCode : Forbidden  
message : 403 Forbidden by Local  
  
만약 403 에러 처리에 기존 message를 이용할 경우에는 errorCode를 이용하거나 <a href="https://docs.thebackend.io/sdk-docs/backend/base/common-errors/functions/too-many-request/">IsTooManyRequestError</a> 함수를 이용해주세요.  
:::

:::danger SDK 5.11.0 ~ 5.11.3 압축형데이터 이용 불가 안내
SDK 5.11.0에서 5.11.3의 경우 압축형 데이터를 이용할 경우, 데이터 불러오기 시, inDate에 뒷자리에 0이 붙을 경우, inDate값이 변경되는 치명적인 오류가 존재합니다.  
압축형 데이터를 이용하고자 할 경우에는 꼭 5.11.4 이상의 SDK로 진행해주시기 바랍니다.  
:::

---

# 5.18.16
## [Fixes]
### [압축 데이터] 압축 테이블에서 페이징 키를 사용한 다음 페이지 조회가 정상적으로 동작하지 않던 문제가 수정되었습니다.
- 압축 데이터가 설정된 게임 정보 또는 유저 데이터 테이블에서 `firstKey` 또는 `LastEvaluatedKey`를 사용한 다음 페이지 조회가 정상적으로 동작하도록 수정하였습니다.
- `firstKey`는 `FirstKeystring()`, `LastEvaluatedKey`는 `LastEvaluatedKeyString()`으로 가져와 다음 페이지 조회에 사용할 수 있습니다.

---

# 5.18.15
## [Fixes]
### [요청 인증] 숫자로만 구성된 문자열 키가 포함된 Dictionary를 전달할 때 일부 요청에서 401 오류가 발생할 수 있던 문제가 수정되었습니다.
- 5.18.12부터 5.18.14까지 `"2"`, `"1"`과 같이 숫자로만 구성된 문자열 키가 포함된 Dictionary를 일부 요청에 전달할 경우 401 `bad signature` 오류가 발생할 수 있었습니다.
- 해당 문제가 수정되었습니다. 5.18.12부터 5.18.14까지의 버전을 사용 중인 프로젝트는 5.18.16으로 업데이트해 주세요.
### [차트] 빈 목록으로 콘텐츠를 조회할 때 예외가 발생하던 문제가 수정되었습니다.
- 동기 및 비동기 `Backend.CDN.Content.Get`, `Backend.CDN.Probability.Get`에 빈 목록을 전달하면 클라이언트 예외가 발생하던 문제를 수정하였습니다.
- 빈 목록을 전달하면 성공 응답과 빈 콘텐츠 목록을 반환합니다.

---

# 5.18.14
## [Updates]
### [멀티 캐릭터] 멀티 캐릭터 기능이 추가되었습니다.
- 하나의 계정 아래 최대 20개의 캐릭터를 만들고, 선택한 캐릭터를 독립된 뒤끝 베이스 유저로 사용할 수 있습니다.
- `Backend.MultiCharacter.Character`의 `CreateCharacter`, `GetCharacterList`, `SelectCharacter`, `DeleteCharacter`로 캐릭터를 관리합니다.
- `Backend.BMember.Elevate()`로 기존 계정을 멀티 캐릭터 계정으로 전환할 수 있으며, `Backend.NeedsElevation`·`Backend.IsMultiAccountLogin`·`Backend.IsMultiCharacterLogin`으로 현재 컨텍스트를 구분합니다.
- `Elevate`는 현재 로그인 세션을 그대로 전환하므로 파라미터를 받지 않습니다. 전환에 성공하면 기존 게이머를 첫 번째 캐릭터로 유지하고 계정 컨텍스트로 진입하므로, 로그아웃 없이 캐릭터를 조회·생성·선택·삭제할 수 있습니다.
- 뒤끝 콘솔 > 개발 > 유저 > 환경설정에서 **멀티캐릭터 활성화** 버튼을 클릭하면 멀티 캐릭터를 활성화할 수 있습니다. 멀티 캐릭터로 변경한 프로젝트는 일반 프로젝트로 되돌릴 수 없습니다.
### [길드 기능] 길드 마스터 자동 교체 기능이 추가되었습니다.
- 뒤끝 콘솔 > 뒤끝베이스 > 길드 관리 > 설정에 길드 마스터 자동 교체 기준 일수(1~365일)를 설정하면, 길드 마스터가 해당 일수를 초과해 접속하지 않은 길드에서 길드원이 `ClaimGuildMaster`를 호출해 길드 마스터가 될 수 있습니다.
- 길드 조회 결과에 길드 마스터의 마지막 로그인 시각(`masterLastLogin`)과 비활성 여부(`inactivedMaster`)가 추가되었습니다. 두 값은 기준 일수를 설정한 프로젝트에서만 내려옵니다.
### [길드 기능] 길드 조회 V4 함수가 추가되었습니다.
- `GetGuildListV4`, `GetMyGuildInfoV4`, `GetGuildInfoV4`가 추가되었으며 `masterLastLogin`, `inactivedMaster`는 V4 함수에서만 조회할 수 있습니다.
- V4 응답은 일반 JSON으로 정규화되어 내려오며, 단건 조회 응답의 최상위 키가 `guild`에서 `result`로 변경되었습니다.
- `Backend.NewFunctions.Guild.GetAllGuildListV4`, `GetMyGuildInfoV4`, `GetGuildInfoV4`의 반환값은 `GetReturnValueByGuildList()`, `GetReturnValueByGuildItem()`으로 파싱 없이 사용할 수 있습니다.
### [유저 관리] 토큰 유효성 체크가 계정·캐릭터 컨텍스트에서 모두 동작하도록 통합되었습니다.
- `IsAccessTokenAlive`가 멀티 캐릭터 계정 로그인 상태(캐릭터 미선택)에서도 정상적으로 토큰을 검증합니다.
- 엔드포인트 통합으로 싱글 캐릭터 게이머의 성공 응답 statusCode가 204에서 200으로 변경되었습니다. 멀티 캐릭터의 계정·캐릭터 컨텍스트는 204를 그대로 반환하므로, `GetStatusCode() == 204`로 성공을 판단하는 코드는 `IsSuccess()`를 사용하도록 수정해 주세요.
- `Backend.BMember.VerifyToken`이 `IsAccessTokenAlive`와 동일하게 동작하는 함수로 추가되었습니다.
### [ToolKit] 구글 로그인 SDK(Android) 2.3.1이 배포되었습니다.
- 2.3.0 unitypackage를 Unity 프로젝트에 import할 때 오류가 발생하던 문제를 수정하였습니다.
- 2.3.0 unitypackage를 다운로드한 경우에는 2.3.1 unitypackage를 새로 다운로드하여 사용해 주세요.

## [Deprecated]
### [멀티 캐릭터] 구 멀티 캐릭터 계정 함수가 삭제되었습니다.
- `Backend.MultiCharacter.Account`가 삭제되었습니다. 아래 함수를 사용하던 코드는 컴파일되지 않으므로 `Backend.BMember`의 대응 함수로 교체해야 합니다.
  - `CreateAccount`, `LoginAccount`, `AutoLoginAccount`, `LogoutAccount`, `WithdrawAccount`, `AuthorizeFederation`, `ChangeCustomToFederation`, `ChangeToFederation`, `DeleteLocalData`
  - `UpdateCustomEmail`, `ResetPassword`, `UpdatePassword`, `ConfirmCustomPassword`, `FindAccountId`
- 멀티 캐릭터 계정의 회원가입·로그인·탈퇴는 싱글 캐릭터 프로젝트와 동일하게 `Backend.BMember`를 사용하며, 기존 계정을 멀티 캐릭터 계정으로 전환할 때만 `Backend.BMember.Elevate`를 호출합니다.
- 캐릭터 조회·생성·선택·삭제는 `Backend.MultiCharacter.Character`를 계속 사용합니다.
### [길드 기능] 길드 조회 V3 함수가 deprecated 되었습니다.
- `GetGuildListV3`, `GetMyGuildInfoV3`, `GetGuildInfoV3` 호출 시 컴파일 경고가 발생합니다. 동작은 유지되므로 기존 코드는 계속 사용할 수 있으나, 신규 개발은 V4 함수를 사용해 주세요.

---

# 5.18.13
## [Updates]
### [길드 기능] 길드 굿즈 기부/사용 함수에 double 타입 amount가 추가되었습니다.
- `ContributeGoodsV5`, `UseGoodsV5` 함수에 double 타입 `amount` 오버로드가 추가되어 int 범위를 초과하는 수치를 전달할 수 있습니다.
### [페더레이션 인증] 스팀 로그인에 Partner(Web API) 방식 인증이 추가되었습니다.
- 뒤끝 콘솔 > 프로젝트 설정 > 인증 정보 > 스팀 계정 인증 정보에 Partner Identity를 입력한 후, `SteamUser.GetAuthTicketForWebApi(identity)`로 발급한 티켓으로 기존과 동일하게 페더레이션 로그인을 진행하면 서버가 Partner API로 검증합니다.
- Partner Identity를 입력하지 않으면 기존 방식이 그대로 동작하며, Partner Identity를 입력한 상태에서 기존 방식으로 로그인하면 오류가 발생합니다.
### [데이터 처리] 2^53을 초과하는 정수 파라미터에 대한 경고 로그가 추가되었습니다.
- `Param`에 2^53을 초과하는 정수를 추가할 경우 서버 저장 시 정밀도가 손실될 수 있어 경고 로그가 출력됩니다. 해당 값은 문자열로 전송해 주세요.
### [ToolKit] 구글 로그인 SDK(Android) 2.3.0이 배포되었습니다.
- 로그인 도중 화면 회전·프로세스 재시작 시 앱이 강제 종료되거나 콜백이 유실되던 문제, 콜백이 중복 호출될 수 있던 문제가 수정되었습니다.

## [Fixes]
### [길드 기능] 큰 재화 수치 전송 시 값이 유실되는 문제가 수정되었습니다.
- 길드 굿즈 기부/사용 및 길드 재화 리더보드 갱신 시 매우 큰 수치가 지수 표기(예: 3E+21)로 전송되어 값이 유실될 수 있는 문제를 수정하였습니다.
### [영수증 검증] details의 한글이 유니코드 이스케이프 형태로 저장되는 문제가 수정되었습니다.
- 영수증 검증/환불 요청 시 `details` 등 요청 데이터에 포함된 한글이 `\uXXXX` 형태로 저장되는 문제를 수정하였습니다.
### [위치 정보] 자동 갱신 결과가 수동 설정 값을 덮어쓰는 문제가 수정되었습니다.
- 위치 정보 자동 조회(autoLoadLocationProperties)와 수동 설정(`CustomizeLocationProperties`)을 함께 사용할 때, 나중에 완료된 자동 조회 결과가 수동 설정 값을 덮어쓰지 않도록 수정하였습니다.

---

# 5.18.12
## [Fixes]
### [데이터 처리] JSON 직렬화 및 파싱 안정화 작업이 진행되었습니다.
- JSON 직렬화 및 파싱 과정의 안정성을 개선하였습니다.

---

# 5.18.11
## [Updates]
### [버전 관리] 버전 관리에 스팀이 추가 되었습니다.
- `GetLatestVersion` 함수의 `VersionInfo`를 통해 스팀을 선택할 수 있습니다.

## [Fixes]
### [데이터 처리] Param.Parse 함수 오류가 수정 되었습니다.
- 수십 KB 크기의 JSON 문자열을 `Param.Parse(string)`로 파싱할 때 간헐적으로 IndexOutOfRangeException이 발생하는 문제를 수정 하였습니다.

---

# 5.18.10
## [Fixes]
### [페더레이션] GPGS V2 로그인 오류가 수정 되었습니다.
- GPGS V2 로그인이 정상적으로 되지 않는 문제를 수정 하였습니다.
### [공지사항] 비동기 NoticeListV2 offset 함수 오류가 수정 되었습니다.
- 비동기 NoticeListV2 함수 중에 offset을 입력 하는 함수 호출 시 제대로 다음 공지사항 리스트를 가져 오지 못하는 오류를 수정 하였습니다.

---

# 5.18.9
## [Updates]
### [길드 기능] 길드 생성 시 오픈 길드 여부를 지정할 수 있는 옵션이 추가되었습니다.
- `CreateGuildV4`에 `isOpenGuild` 파라미터가 추가되어 오픈 길드 여부를 함께 설정할 수 있습니다.
### [그룹 기능] 그룹 변경 시 리더보드 연동 게임정보 처리 옵션이 추가되었습니다.
- 그룹 변경 시 이전 그룹의 리더보드에 등록된 데이터는 삭제되며, `Update` 함수의 `LeaderboardValuePolicy` 파라미터로 리더보드에 사용되는 집계 필드 데이터를 유지/초기화/리더보드 재등록할지 선택할 수 있습니다.
### [랜덤 조회] 그룹별 구분 동작이 추가되었습니다.
- 콘솔 설정에 따라 랜덤 조회를 그룹 기준으로 운영할 수 있으며, 그룹 변경 시 RandomPool 데이터도 변경 된 그룹의 RandomPool로 이동합니다.  

---

# 5.18.8
## [Updates]
### [영수증 검증] IAP 5.x 버전 지원 영수증 검증 기능이 추가되었습니다.
- 최신 버전의 IAP 5.x 버전을 지원하는 영수증 검증 기능이 추가되었습니다. IAP 5.x 버전으로 업데이트한 후, 영수증 검증 기능을 이용해 주세요.
### [환불 관리] 환불 관리 기능이 추가되었습니다.
- 환불 관리 기능을 통해 환불 요청을 조회하고, 환불 상태를 업데이트할 수 있습니다.

## [Fixes]
### [차트] CDN 차트 다운로드 방식이 개선되었습니다.
- CDN 차트 다운로드 방식이 개선되어, 차트 다운로드 시 빠른 속도로 다운로드할 수 있도록 개선되었습니다.

---

# 5.18.7
## [Updates]
### [게임 운영 관리] 공지사항, 이벤트, 약관 및 정책 기능이 개선되었습니다.  
- 공지사항, 이벤트, 약관 및 정책 기능이 뒤끝 콘솔에서 언어 별로 관리할 수 있도록 개선되었습니다.
### [쿠폰 관리] Web Coupon 을 직접 사용할 수 있는 기능이 추가되었습니다.  
- Web Coupon을 직접 사용할 수 있는 기능이 추가되었습니다. 웹뷰, SDK 함수 등 다양한 방식으로 Web Coupon을 사용할 수 있습니다.  
### [Deprecated] 신규 함수들이 제공됨에 따라 Deprecated 되었던 함수들이 삭제되었습니다.
- [랭킹]
    - Backend.URank.User.GetRankTableList
    - Backend.URank.User.GetRankList
    - Backend.URank.User.GetMyRank
    - Backend.URank.User.GetUserRank
    - Backend.URank.User.GetRankListByScore
    - Backend.URank.User.GetRankRewardList
    - Backend.URank.User.GetPastRankList
    - Backend.URank.User.UpdateUserScore
    - Backend.URank.Guild.GetRankTableList
    - Backend.URank.Guild.GetRankList
    - Backend.URank.Guild.GetMyGuildRank
    - Backend.URank.Guild.GetGuildRank
    - Backend.URank.Guild.GetRankListByScore
    - Backend.URank.Guild.UpdateGuildMetaData
    - Backend.URank.Guild.ContributeGuildGoods
    - Backend.URank.Guild.UseGuildGoods
    - Backend.URank.Guild.GetRankRewardList
    - Backend.URank.Guild.GetPastRankList
- [차트]
    - Backend.Chart.GetChartList
    - Backend.Chart.GetChartListV2
    - Backend.Chart.GetChartListByFolder
    - Backend.Chart.GetChartListByFolderV2
    - Backend.Chart.GetChartContents
    - Backend.Chart.GetOneChartAndSave
    - Backend.Chart.GetOneChartAndSaveV2
    - Backend.Chart.GetAllChartAndSave
    - Backend.Chart.GetAllChartAndSaveV2
    - Backend.Chart.GetChartByFolderAndSave
    - Backend.Chart.GetChartByFolderAndSaveV2
    - Backend.Chart.GetLocalChartData
    - Backend.Chart.GetLocalChartDataV2
    - Backend.Chart.DeleteLocalChartData
    - Backend.Chart.DeleteLocalChartDataV2
- [확률]
    - Backend.Probability.GetProbabilityContents
    - Backend.Probability.GetProbabilityCardList
    - Backend.Probability.GetProbabilityCardListV2
- [약관 및 정책]
   - Backend.Policy.GetPolicy
- [길드]
    - Backend.Guild.UseGoodsV3
    - Backend.Guild.UseGoodsV4
    - Backend.Guild.ModifyGuildV3
    - Backend.Guild.ModifyGuildV4
    - Backend.Guild.ContributeGoodsV3
    - Backend.Guild.ContributeGoodsV4

---

# 5.18.6
## [Updates]
### [길드 기능] 길드명을 변경하는 기능이 추가되었습니다.  
- 길드 마스터가 길드명을 변경할 수 있는 기능이 추가되었습니다.
### [우편 기능] 웹숍 우편 수령 기능이 추가되었습니다.
- 웹숍에서 구매한 아이템을 우편에서 수령할 수 있는 기능이 추가되었습니다.

---

# 5.18.5
## [Updates]
### [리더보드] 데이터베이스 서비스 연동 리더보드 기능 추가
   - 데이터베이스 서비스와 연동하여 리더보드를 사용할 수 있도록 기능이 추가되었습니다.
   - 리더보드에 데이터베이스의 테이블을 연동하여, 다양한 조건으로 리더보드를 구성할 수 있습니다.
### [게임 캐시 관리] 원스토어 영수증 검증을 통한 TBC 충전 기능 추가
   - 원스토어 영수증 검증을 통한 TBC 충전 기능이 추가되었습니다.
   - 원스토어 결제 후, 영수증을 검증하여 TBC를 충전할 수 있습니다.
   
---

# 5.18.4
## [Updates]
### [실시간 알림] 길드 추방 알림 추가
- 길드에서 추방되었을 때, 실시간 알림을 받을 수 있게 되었습니다.

## [Fixes]
### [실시간 알림] 실시간 알림 연결 끊김 상태에서 발생 하는 예외 수정
- 실시간 알림 연결 끊김 상태에서 발생 하는 예외를 수정하였습니다.
### [1대1 문의 플러그인] 1대1 문의 플러그인 Android 버전 첨부파일 추가 문제 수정
- 첨부파일 추가 시, Android 10 이상에서 발생하는 문제를 수정하였습니다.
- 1:1 문의 플러그인 Android 버전 1.1.0로 업데이트 해 주시기 바랍니다.

---

# 5.18.3
## [Updates]
### [페더레이션 인증] Line 계정 로그인이 추가 되었습니다.
- 페더레이션 회원가입/로그인에서 라인 계정이 사용 가능하게 되었습니다.
- 라인 계정으로 페더레이션 계정 전환이 가능해졌습니다.

---

# 5.18.2
## [Fixes]
### [리더보드 기능] 리더보드 불러오기에서 `LeaderboardTableItem` 에 누락된 변수를 수정 하였습니다.   
  - `LeaderboardTableItem`으로 데이터를 가져올때 리더보드 초기화 시간(initializationTime)도 가져올수있도록 수정하였습니다.  
### [페더레이션 인증] `GetGPGS2AccessToken` 함수 에러 처리 로직을 수정하였습니다.  
  - 특정 에러 발생시 콜백이 중복으로 처리되는 구조를 수정 하였습니다.  
  - 특정 에러로 발생하는 에러 메시지를 수정하였습니다.  
### [트랜젝션] 유저데이터/게임정보 `TransactionWrite` 사용시 특정 상황에서 발생하는 에러를 수정하였습니다.
  - `TransactionWrite` 함수 사용시 테이블 데이터가 압축데이터 일 때 Param이 없는 함수를 사용하면 발생하는 에러를 수정하였습니다.

## [Updates]
### [길드 기능] `GetGuildInDateByGuildName` 로 검색할 수 있는 범위가 개선 되었습니다.  
  - `GetGuildInDateByGuildName` 함수의 파라미터를 추가하여 플레이어가 속해있는 그룹에 상관없이 전체 그룹에서 길드를 검색 할 수 있습니다.  
### [길드 기능] 길드굿즈 기부/사용, 길드 메타데이터 수정 함수의 기능이 개선 된 함수가 추가 되었습니다.  
  - `ContributeGoodsV5`, `UseGoodsV5`, `ModifyGuildV5` 함수가 추가 되었습니다.   
  - 추가된 함수들은 리더보드 초기화 시간을 제외한 시간에 정상 동작합니다.
  - `ContributeGoodsV5` 함수가 추가됨에 따라 기존 `ContributeGoodsV3`, `ContributeGoodsV4`는 Deprecated 되었습니다.  
  - `UseGoodsV5` 함수가 추가됨에 따라 기존 `UseGoodsV3`, `UseGoodsV4`는 Deprecated 되었습니다.      
  - `ModifyGuildV5` 함수가 추가됨에 따라 기존 `ModifyGuildV3`, `ModifyGuildV3`는 Deprecated 되었습니다.  

---

# 5.18.1
## [Fixes]
### [게임 정보 관리] /  [유저 데이터 관리] 트랜젝션 쓰기에서 특정 상황에서 데이터가 정상적으로 처리 되지 않는 문제 수정.  
- 게임 정보 관리 트랜젝션 쓰기에서 `TransactionWriteV2` 함수를 동기로 사용했을때 데이터가 정상적으로 처리 되지 않는 현상을 수정하였습니다.  
- 유저 데이터 관리 트랜젝션 쓰기에서 `TransactionWrite` 함수를 동기로 사용했을때 데이터가 정상적으로 처리 되지 않는 현상을 수정하였습니다.  

---

# 5.18.0
## [Fixes]
### [Android] Unity6에서 GameActivity로 진입점 설정시, 오류 발생 문제 수정.  
Unity6에서 GameActivity로 진입점 설정시, 예외가 발생하던 문제를 수정했습니다.  

### [실시간 알림] 간헐적으로 재접속이 중복 발생하던 문제 수정.  
실시간 알림에서 연결 종료로 자동 재접속이 발생할 때, 간헐적으로 재접속이 중복 발생하던 문제를 수정했습니다.  

### [리더보드] 리더보드 정보 조회에서 초기화 설정값을 항상 false로 전달하던 문제 수정.  
리더보드 정보 조회시, isReset의 값을 항상 false로 전달하던 문제를 수정했습니다.  

### [로컬 차트] 로컬 차트를 최신 차트로 업데이트 할 때 뒤끝 콘솔에서 차트 파일이 적용되지 않은 로컬 차트 삭제.  
로컬 차트를 최신 차트로 업데이트 할 때 뒤끝 콘솔에서 차트 파일이 적용되지 않은 차트를 로컬에서 삭제하도록 수정 했습니다. 
`GetContentDictionarySortByChartName`함수가 Deprecated 되었습니다. `GetContentDictionarySortByChartId`함수를 사용해 주세요.  

---

# 5.17.1
## [Fixes]
### [클라우드 세이브] 특정 데이터 형식이 클라우드 세이브로 업로드 되지 않던 문제 수정.
json 데이터의 첫번째 depth에서 key문자열이 숫자로 시작하면, 해당 데이터가 클라우드 세이브로 업로드 되지 않던 문제를 수정했습니다.  

---

# 5.17.0
## [Updates]
### [계정 전환] 페더레이션 계정 간 로그인 전환 기능 추가.  
페더레이션 계정에서 다른 페더레이션 계정으로 로그인 방식을 전환 할 수 있는 기능이 추가 되었습니다.

### [유저 정보] 유저 정보 조회 API의 V2버전 추가.  
리턴값에 유저가 속한 Group 이름이 추가된 유저 정보 조회 API의 V2버전을 추가했습니다.  

### [구글 인증] 신규 구글 계정 인증 SDK 추가.   
안드로이드 인증관리자를 사용하는 신규 구글 계정 인증 SDK를 추가했습니다.  

## [Fixes]
### 확률 테이블 파싱 에러 수정.  
- 뽑기 확률 테이블에서 ContentJson을 사용해서 Json 형식으로 변환시 오버플로우가 발생하는 현상을 수정했습니다.  

---

# 5.16.3
## [Fixes]
### [데이터 처리] 언마셜링이 문화권에 영향을 받지 않도록 수정.  
GetFlattenJSON()같은 언마셜링을 하는 함수에서 숫자 데이터 변환시, 문화권에 영향을 받지 않도록 수정했습니다.  

---

# 5.16.2
## [Fixes]
### [클라우드 세이브] 업로드 시, 제한 용량 계산 오류 수정.  
클라우드 세이브에서 데이터 업로드 시, white-space를 제거하고 용량을 계산하도록 수정했습니다.  

## [Updates]
### [실시간 알림] 재연결 시도가 실패했을 때, 알림을 주는 이벤트 핸들러를 추가.  
실시간 알림에서 재연결 시도가 실패했을 때, 알림을 받을 수 있도록 이벤트 핸들러를 추가했습니다.  

### [실시간 알림] 현재 연결된 상태인지를 확인할 수 있는 속성값 추가.
현재 연결된 상태인지를 확인할 수 있는 속성값을 추가했습니다.  

---

# 5.16.1
## [Fixes]
### 게임 정보 조회에서 Where의 Between 사용 시, iOS에서 오류 발생 수정
게임 정보 조회에서 Where 조건 중, Between을 사용하면  
iOS에서 Malformed URL 오류가 발생하던 문제를 수정했습니다.  

### CDN 차트에서 ContentItem의 contentJson에서 데이터 변환 오류 수정
CDN 차트에서 ContentItem의 contentJson으로 가져온 JsonData를  
foreach에서 사용할 때, InvalidOperationException이 발생하던 문제를 수정했습니다.  

---

# 5.16.0
## [Updates]
### 클라우드 세이브 기능 추가
원격 저장소에 JSON 문자열 형태로 데이터를 업로드하고 다운로드 받을 수 있습니다.  
콘솔에서 컬렉션을 생성한 후, 해당 컬렉션에 데이터를 저장할 수 있습니다.  

### CDN 차트에서 로컬 삭제 기능 추가
CDN 차트를 통해 로컬에 저장한 차트를 삭제하는 기능이 추가되었습니다.

---
