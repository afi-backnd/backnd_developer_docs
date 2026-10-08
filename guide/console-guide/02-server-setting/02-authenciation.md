---
description: "인증정보"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 인증정보

뒤끝 콘솔 **서버 설정-인증정보**에서는 뒤끝 SDK에서 사용할 **Client App Id**와 **Signature Key**를 발급받고 서버와 클라이언트가 통신할 때 필요한 여러 가지 인증값을 설정할 수 있습니다.

<ConsoleLinkButton text="인증 정보 바로가기" menu="settingAuth" feature="인증 정보" title="인증정보" />

![인증 정보 화면](/img/docs/guide/base-authentication/authenciation_guide_1.png)

## 게임 인증 정보

### 인증 정보 연동

뒤끝 기능을 사용하기 위해서는 뒤끝 SDK에 **Client App Id**와 **Signature Key**를 입력해야 합니다.  
사용방법은 [개발자 문서 - 시작하기](/sdk-docs/backend/base/start-up)를 참고해 주세요.

![게임 인증 정보](/img/docs/guide/base-authentication/authenciation_guide_2.png)

<!--
### Client App Id / Signature Key 새로 발급

**Client App Id**와 **Signature Key**를 새로 발급받고 싶으실 경우에는 해당 항목 우측에 새로 발급을 클릭하여 새로운 Key 값을 얻으실 수 있습니다.
새로 발급 시 주의사항은 아래와 같습니다.

- Client App Id 변경 시 뒤끝 SDK에서 인증값 재설정이 필요하여 뒤끝 콘솔에서 설정한 임시 공지, 뒤끝챗 필터링 등 일부 설정이 초기화됩니다.
- Client App Id와 Signature Key를 재발급 받은 경우 SDK에 설정값에도 해당 값을 변경해야 합니다.
- SDK에서 Client App Id를 변경한 경우 뒤끝 SDK 로컬에 저장된 설정값이 초기화됩니다.(게스트 로그인 정보, 유저 액세스 토큰 정보 등)
-->

### Signature Key 교체 <span class="badge badge--info">26.10.20 제공</span> {#signature-key-change}

**Signature Key**는 클라이언트 빌드에 포함되어 배포되는 값입니다.  
따라서 사용 중인 키를 곧바로 교체하면, 기존 빌드를 이용 중인 모든 유저의 인증이 실패하게 됩니다.

이를 방지할 수 있도록 **신규 Signature Key를 추가로 발급**하여 함께 사용할 수 있습니다.  
신규 키를 빌드에 적용해 스토어에 배포가 완료된 후 기존 키를 삭제하면 서비스 중단 없이 키를 교체할 수 있습니다.

#### 키 교체 절차

1. **새 Signature Key 발급**  
   Signature Key 항목 우측의 `+` 버튼을 클릭합니다.  
   안내 모달에서 **발급**을 클릭하면 신규 키가 즉시 발급되며, 기존 키도 삭제 전까지 그대로 동작합니다. (최대 2개)

   ![Signature Key 추가 발급](/img/docs/guide/base-authentication/authenciation_guide_3.png)

2. **신규 키를 빌드에 적용**  
   발급된 키를 복사하여 SDK 설정에 입력한 뒤 빌드합니다. 적용 방법은 [개발자 문서 - 시작하기](/sdk-docs/backend/base/start-up)를 참고해 주세요.

3. **스토어 배포 및 유저 업데이트**  
   신규 키가 적용된 빌드를 배포합니다.  
   이 기간에는 기존 키와 새 키가 모두 동작하므로, 업데이트한 유저와 아직 하지 않은 유저 모두 정상적으로 게임을 이용할 수 있습니다.  
   
   기존 키 삭제는 강제 업데이트 및 신규 Signature Key가 반영된 버전이 스토어를 통해 배포가 적용되고, 정상 접속을 확인하신 후 진행해 주세요.  
   ※ [버전 관리](/guide/console-guide/backnd-base/versions)에서 해당 버전을 등록할 때 **업데이트 방식**을 **강제**로 설정하시면, 유저가 이전 빌드를 계속 사용하는 상황을 줄일 수 있습니다.

4. **기존 키 삭제**  
   기존 키 우측의 **삭제** 버튼을 클릭합니다.  
   확인 모달에 `DELETE`를 입력해야 삭제가 진행되며, 삭제 대상 키의 발급일을 함께 확인할 수 있습니다.

   ![Signature Key 삭제](/img/docs/guide/base-authentication/authenciation_guide_4.png)

#### 참고: 기존 키 삭제 시 유저 영향

기존 키를 삭제하면 **즉시 반영**되며, 해당 키가 포함된 빌드에서는 인증이 실패합니다.  

| 유저 상태 | 기존 키 삭제 후 동작 |
|---|---|
| 신규 키 적용 빌드 이용 중 | 영향 없음. 토큰이 만료되기 전에 업데이트한 경우 기기에 저장된 토큰을 그대로 사용하므로 재로그인도 필요하지 않습니다. |
| 기존 키 빌드로 로그인한 상태 | 기기에 저장된 액세스 토큰과 리프레시 토큰이 유지되어, 토큰이 만료되기 전까지는 게임을 계속 이용할 수 있습니다. 액세스 토큰의 유효 기간은 1일이므로, 삭제 시점에 접속 중이던 유저도 최대 1일 이내에 영향을 받습니다. |
| 기존 키 빌드에서 토큰이 만료된 경우 | 토큰이 필요한 동작을 수행할 때 게임이 종료됩니다. 새 키가 적용된 빌드로 업데이트한 뒤, 재로그인을 진행해야 합니다. |
| 기존 키 빌드에서 새로 로그인을 시도하는 경우 | 토큰 로그인, 토큰 갱신, 신규 로그인 모두 실패합니다. |

위 동작은 로그인 방식과 관계없이 동일하게 적용됩니다.  
- 커스텀 로그인, 페더레이션 로그인(구글·애플·페이스북·스팀 등), 게스트 로그인 모두 동일
- 다만 세션 만료 후 재로그인 시 게스트 로그인은 기기에 저장된 정보로 자동 처리되어 유저가 별도로 입력할 내용은 없음

:::caution 유의사항
- **기존 키를 삭제할 경우 해당 키의 빌드를 실행하면 아래와 같은 에러가 발생하며 재로그인도 불가능**합니다. 새 키가 적용된 빌드로 업데이트해야 다시 이용할 수 있습니다.

  ```
  statusCode : 404
  errorCode : NotFoundException
  message : game not found, game을(를) 찾을 수 없습니다
  ```
- **삭제한 키는 복구할 수 없습니다.** 동일한 값으로 다시 발급할 수 없으므로, 삭제 후에는 새 키를 발급하여 빌드에 다시 적용해야 합니다.
- **지원 SDK 버전**: Signature Key 2개 동시 사용은 뒤끝 SDK **5.4.0 이상**에서 지원됩니다. 5.4.0 미만 버전에서는 새로 발급한 키로 인증할 수 없으므로, SDK를 5.4.0 이상으로 업데이트한 빌드에 새 키를 적용해 주세요.
:::
<br />

## 스토어 인증 정보

:::danger **APPLE 개발자 계정 간 게임 이관 시 주의사항**
최초 운영하던 계정에서 타 계정으로 게임을 이관하는 경우 Team ID가 변경되면서 유저들이 로그인할 때의 유니크한 값(유저별 sku)이 변경되어 기존 정보로 로그인이 불가합니다.

이는 Apple의 정책으로 인한 부분으로 원활한 게임 서비스를 위해서는 뒤끝의 유료 기술 지원이 필요합니다.

**이관을 진행하시고자 하는 경우 커뮤니티 혹은 help@backnd.com로 사전에 문의하시어 게임 운영에 불편 없으시기 바랍니다.**  
:::

**Android package Name**은 애플리케이션을 구별하는 값입니다.  
구글 플레이 스토어 / 원스토어용으로 각각 입력할 수 있습니다.  
Unity 창 상단에 `File → BuildSettings → PlayerSettings → OtherSettings → package Name`에서 확인하실 수 있습니다.

**구글 플레이 스토어와 원스토어에는 같은 패키지 이름을 입력할 수 없습니다.** <br/>
**[Play Store 와 ONE Store에 같은 패키지 이름을 적용합니다.]** 옵션을 체크하시면 같은 패키지 이름을 적용할 수 있습니다. <br/>
같은 패키지 이름을 적용할 경우, 푸시 스토어 등록 등에서 구글 플레이 스토어 패키지 이름과 통합되어 관리됩니다.

:::danger **동일 패키지 이름 사용시 주의사항**
Goolge Play Store와  ONE Store에 동일 패키지 이름을 적용/적용 해제할 경우, 예약된 푸시/ 반복 푸시가 취소/반복 종료 처리가 되며,
푸시 스토어 정보 등 패키지 이름을 사용하는 일부 기능에 등록된 인증 정보가 삭제될 수 있습니다.
:::

:::info 서드파티 마켓 등록 안내  
현재 뒤끝은 ‘Google Play, Apple App Store, ONE Store’에 대한 스토어 정보 등록 기능을 제공하고 있습니다.  
이 외의 서드파티 마켓(예: Amazon Appstore, Huawei AppGallery 등)에 대해서는 아직 스토어 정보 등록 기능을 지원하지 않습니다.  
다만, 해당 마켓에서 앱을 배포하려는 경우 **패키지 네임 등록은 지원하고 있으니** help@backnd.com 메일로 문의해 주세요.
:::

![base-authentication](/img/docs/guide/base-authentication/base-authentication-new_android.png)


**Google Hash Key**는 안드로이드 환경에서 뒤끝 서버와 구글 서버가 통신할 때 사용되는 키입니다.  
해당 키를 이용하여 유효성 검증 및 구글 관련 기능 사용이 가능해집니다.

구글 해시키는 아래 2개의 개발자 문서를 참고하여 조회할 수 있습니다.  
문서를 참고하여 조회한 구글 해시키를 뒤끝 콘솔에 기입하면 안드로이드 환경에서 정상적으로 게임 이용이 가능합니다.

- [게임 내에서 함수를 호출하여 구글 해시키 확인](/sdk-docs/backend/base/sdk-utils/get-hash/by-function)
- [유니티 인스펙터 창에서 구글 해시키 확인](/sdk-docs/backend/base/sdk-utils/get-hash/by-unity-inspector)
- 해시키를 입력할 수 있는 칸은 총 4개입니다.
- 릴리즈/디버그 칸은 편의를 위해 구분한 것으로 성능상의 차이는 존재하지 않습니다.
- **SDK 5.0.0 미만에서는 위 2칸만 사용할 수 있습니다.**

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정인증정보---스토어-인증-정보_2.png" />

**iOS Bundle Identifier**는 애플리케이션을 구별하는 값입니다.  
또한 웹 로그인을 위한 **iOS Bundle Identifier**를 입력할 수 있습니다.  
Unity 창 상단에 `File → BuildSettings → PlayerSettings → OtherSettings → package Name`에서 확인하실 수 있습니다.

![base-authentication](/img/docs/guide/base-authentication/base-authentication-new_ios.png)

**iOS Bundle Identifier for Web Login**은 뒤끝 애플 로그인 SDK Android 버전을 이용하여, 안드로이드에서 애플 로그인을 구현하려고 할 경우 입력해야하는 SignInWithApple의 Services ID 값입니다.

**Apple 개발자 계정의 Team ID**은 애플에서 회원 탈퇴 시 필요한 애플 토큰 만료 REST API 호출에 필요한 값이며, 해당 앱을 소유중인 애플 개발자 계정의 team ID 입니다.

**Apple 프로젝트의 Key 이름**은 애플에서 회원 탈퇴 시 필요한 애플 토큰 만료 REST API 호출에 필요한 값이며, 해당 앱에 연결된 Sign in with Apple 서비스에 접근하기 위해 발급받은 Key의 이름입니다.

**Apple 프로젝트 Key(P8 파일)**은 애플에서 회원 탈퇴 시 필요한 애플 토큰 만료 REST API 호출에 필요한 값이며, 해당 앱에 연결된 Sign in with Apple 서비스에 접근하기 위해 발급받은 Key 입니다.

위 3개의 애플 회원 탈퇴 시 필요한 값들의 입력 방법은 [개발자문서-애플 토큰 만료](/sdk-docs/backend/base/user/revoke-apple)를 참고해주세요.

## Facebook 인증 정보

Facebook App ID와 Facebook App Secret은 페이스북 페더레이션 기능 이용 시 반드시 필요한 값입니다.  
**앱 ID**와 **앱 시크릿 코드**는 `Facebook 개발자 페이지 → 설정 → 기본 설정`에서 확인하실 수 있습니다.  
자세한 내용은 [페더레이션 인증 예제-Facebook](/sdk-docs/backend/base/user/federation/example-using-facebook)을 참고해 주세요.

![base-authentication](/img/docs/guide/base-authentication/base-authentication-facebook.png)

<img src="https://developer.thebackend.io/static/img/newconsole/serversetting/서버설정대지-1.png" />

## 스팀 계정 인증 정보
Steam App ID, Web API Key는 스팀 인증 기능 이용 시 반드시 필요한 값입니다.  
![base-authentication-steam](/img/docs/guide/base-authentication/base-authentication-steam.png)

해당 값은 Steamworks에서 확인 가능합니다.  
자세한 내용은 [Steam 로그인 인증 예제](/sdk-docs/backend/base/user/federation/example-using-steam#2-appid-발급)를 참고해 주세요.

## 구글 로그인 인증 정보
[구글 로그인으로 뒤끝에 가입한 유저에 대한 회원 탈퇴 웹](/guide/console-guide/backnd-base/web-withdraw/web-withdraw)을 이용하거나, [GPGS v2 페데레이션 로그인 기능](/sdk-docs/backend/base/user/federation/example-using-gpgs2)을 이용하시려면 구글 로그인 인증 정보를 입력해야 합니다.  

뒤끝 구글 로그인 혹은 GPGS V2 SDK에 사용하던 web client id를 가진 [Google Cloud Platform](https://console.cloud.google.com/apis/credentials) 프로젝트를 선택합니다.  

해당 프로젝트에서 사용된 web client id를 클릭합니다.

![](/img/docs/guide/console-google-info/select-webclient-id.png)

클라이언트 ID와 클라이언트 보안 비밀번호를 복사합니다.  

![](/img/docs/guide/console-google-info/client-id-in-google.png)

구글 로그인 인증 정보에 복사된 값들을 붙여넣기 합니다.  

![](/img/docs/guide/console-google-info/backend-console-google-info.png)

## 라인 로그인 인증 정보
LINE Channel ID, LINE Channel Secret은 라인 로그인 인증을 사용하는 경우 반드시 필요한 값입니다. 
![](/img/docs/guide/base-authentication/base-authentication-line.png)

해당 값은 [LINE Developers](https://developers.line.biz/console/)에서 확인 가능합니다.  
자세한 내용은 [LINE 로그인 인증 예제](/sdk-docs/backend/base/user/federation/example-using-line)를 참고해 주세요.

## Apple 계정 변경 로깅

Apple 계정을 사용하는 서비스의 경우, 계정 변경 웹훅을 통해 유저 계정 상태 변경 정보를 수신할 수 있습니다.  
**2026년 1월 1일부터 한국 개발사는 Apple 정책에 따라 웹훅 수신 URL 등록이 필수**이므로, 아래 설정을 반드시 확인해 주세요.

![](/img/docs/guide/base-authentication/base-authentication-apple_webhook.png)


### 웹훅 수신 URL

Apple Developer 콘솔에 등록해야 하는 웹훅 수신 전용 URL입니다.  
해당 URL로 Apple이 계정 변경 정보를 전송하며, 뒤끝에서는 이를 수신하여 게임 정보 로그로 저장합니다.  
URL을 Apple Developer에 등록하지 않으면 Apple 계정 변경 정보가 전송되지 않습니다.

- **등록 위치**: Apple Developer → Certificates, Identifiers & Profiles → Identifiers → App ID → Sign in with Apple → Configure


### 자동 탈퇴 처리

Apple에서 계정 삭제(account-delete) 이벤트를 수신한 경우, 해당 유저를 자동으로 탈퇴 처리할지 여부를 설정합니다.

- 미체크: 계정 삭제 이벤트는 로그로만 저장됩니다.
- 체크: account-delete 수신 시, 식별 가능한 유저를 자동으로 탈퇴 처리합니다.
  - 탈퇴 처리는 수신 즉시 스케줄러에 등록되며, 최대 1시간 이내에 처리됩니다.
- 개별 연동 해제(consent-revoked) 이벤트는 자동 탈퇴 처리 대상에 포함되지 않습니다.

### 참고 사항

- 모든 Apple 계정 변경 이벤트는 게임 정보 로그에 저장됩니다.
  - 행동유형: `Apple_email_enabled`, `Apple_email_disabled`, `Apple_consent_revoked`, `Apple_account_delete`
- 유저 정보의 웹훅 수신 및 로그 저장 시 비용이 발생합니다.
- Apple 계정 삭제로 탈퇴 처리되는 유저가 길드 마스터인 경우, 길드 마스터 위임 규칙이 자동으로 적용됩니다.
  - 부길드 마스터가 있을 경우: 최근 14일 이내 접속한 부길드 마스터 중, 가장 먼저 길드에 가입한 유저에게 위임
  - 최근 14일 이내 접속한 부길드 마스터가 없을 경우 → 최근 14일 이내 접속한 길드원 중, 가장 먼저 길드에 가입한 유저에게 위임
