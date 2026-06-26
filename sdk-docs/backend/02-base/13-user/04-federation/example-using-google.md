---
sidebar_label: "구글 로그인 인증 예제"
sidebar_position: 2.5
---

# Sign In with Google 사용

:::danger GPGS 및 Sign In with Google 이용 주의사항
**GPGS V1은 구글로부터의 지원이 종료되었습니다.  
GPGS V1을 사용해 오셨거나, 새롭게 사용하시고자 하는 경우 Sign In with Google을 사용해 주세요.**  
   
**GPGS V2 로그인 적용 시 Google Play Games 앱의 업데이트 상태나 캐시 데이터 문제로 인해 정상적으로 이용되지 않을 수 있습니다.  
이용자가 로그인 이상을 호소하는 경우, Google Play Games 앱이 최신 버전인지 확인하고 캐시 삭제 후 재시도하도록 안내해 주시기 바랍니다.  
또한 GPGS V2 로그인은 안드로이드 기기에서만 지원되므로, 보다 안정적인 연동을 위해 구글 로그인을 적용하실 것을 권장드립니다.**

GPGS V2 및 Sign In with Google은 다음과 같은 조건으로 이용이 가능합니다.

| 구분 | GPGS V2 | [deprecated] GPGS V1 | Sign In with Google |
|---|---|---|---|
| **로그인 정보** | PGS games_lite 정보 이용 | 구글 계정 정보 이용 | 구글 계정 정보 이용 |
| **계정간 호환** | GPGS V2 단독 <br /> GPGS V1 및 Sign In with Google 호환 불가 | Sign In with Google 호환 <br /> GPGS V2 호환 불가 | GPGS V1 호환 <br /> GPGS V2 호환 불가 |
| **iOS 지원** | 미지원 | 미지원 | 지원 |
:::

## 설명

구글 페데레이션 로그인은 <a href="/sdk-docs/backend/toolkit/google-login/install-sdk" target="_blank">뒤끝 구글 로그인 SDK</a>를 이용하여 진행할 수 있습니다.

혹은 구글 로그인 성공 이후의 액세스 토큰을 얻을 수 있다면 구글 로그인에 관련된 유료 에셋이나, <a href="https://github.com/googlesamples/google-signin-unity">구글 공식 유니티 구글 로그인 SDK(2018년 마지막 업데이트)</a>, 안드로이드 구글 플러그인을 이용해 직접 구현한 SDK도 사용할 수 있습니다.

GPGS와 뒤끝 구글 로그인 SDK은 혼합 사용이 불가능하니, GPGS V1와 관련된 플러그인을 삭제한 후에 진행해주시기 바랍니다.
