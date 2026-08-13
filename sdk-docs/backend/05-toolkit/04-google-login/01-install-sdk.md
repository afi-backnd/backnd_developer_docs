---
description: "뒤끝 구글 로그인 SDK"
---

# 뒤끝 구글 로그인 SDK

뒤끝 구글 로그인 SDK는 Andorid, iOS에서 페더레이션 로그인을 위해 사용하는 구글 로그인 기능을 더 쉽게 사용하도록
뒤끝에서 가공하여 제공하는 SDK입니다.

구글 클라우드 플랫폼에 업로드 키, sha-1 인증키를 등록하는 것만으로 콘솔 설정이 완료되며, 
SDK에서 간단한 핸들러를 등록하고 로그인 함수를 호출하는 것으로 쉽게 로그인이 가능해집니다.

뒤끝 SDK 버전에 상관없이 사용 가능합니다.

:::caution GPGS 안내  
해당 로그인 방식은 구글이 유니티에서 지원중인 GPGS 와 연동된 로그인 방식이 아닌 앱에서 주로 사용되는 구글 로그인 방식입니다.  
**GPGS 의 기능의 리더보드 혹은 업적등을 이용하실 경우에는 해당 플러그인을 사용하지 말고 GPGS 플러그인을 이용하여 로그인을 구현해 주시기 바랍니다.**  
:::  

:::danger Android용 구글 로그인 SDK 버전 안내
안드로이드 구글 로그인은 기존 Google Sign-In 방식 기반의 **안정화 버전(2.3.0)만 제공**합니다.  

구글은 2024년 9월 인증 간소화 정책([Credential Manager replaces legacy APIs](https://android-developers.googleblog.com/2024/09/streamlining-android-authentication-credential-manager-replaces-legacy-apis.html))에서 레거시 인증 API의 단계적 제거 일정을 공지했습니다.  
당시 공지에는 이 SDK가 의존하는 Google Sign-In for Android가 H2 2025에 제거 예정으로 안내되었으나, 현재 구글 공식 마이그레이션 문서([About the migration from legacy Google Sign-In](https://developer.android.com/identity/sign-in/legacy-gsi-migration))에서는 Android용 Google 로그인 API가 deprecated 상태이며 향후 Google Play services Auth SDK 릴리스에서 제거될 예정이라고만 안내하고 있습니다.  

즉, 구글의 레거시 API 지원 종료 방향은 유지되고 있으나 구체적인 강제 종료 시점은 현재 공식 문서에서 확정되어 있지 않습니다.  

뒤끝은 구글의 이전 공지에 맞춰 인증관리자(Credential Manager) 기반 신규 버전을 준비했으나, 신규 버전은 구 버전 Google Play Services 및 일부 구글 계정 환경에서 로그인 오류가 다수 발생했습니다. 해당 오류는 뒤끝 SDK가 임의로 변경한 동작이 아니라 구글 인증 라이브러리, Google Play Services, 사용자 계정 환경 사이의 호환성 문제에 기인하므로 뒤끝에서 직접 수정하거나 우회하기 어렵습니다.  

실제로 신규 인증관리자(Credential Manager) 방식은 최근까지도 사용자 기기 환경에 따른 로그인 오류가 보고되고 있습니다. 이러한 오류의 상당수는 앱이 번들하는 라이브러리 버전이 아니라 최종 사용자 기기의 Google Play Services 버전·계정 상태에 좌우됩니다. 구글 공식 [Credential Manager 오류 트러블슈팅](https://developer.android.com/identity/sign-in/credential-manager-troubleshooting-guide) 문서는 `NoCredentialException`을 기기 계정·Google Play Services 상태에 따른 문제로 설명하고, 다계정 환경의 `TransactionTooLargeException`은 Google Play Services 24.40.XX 이상에서 수정된다고 안내하여 수정이 기기 측 Play Services에 실립니다. 따라서 앱·SDK 측 라이브러리 업데이트만으로는 사용자 기기 환경에서 발생하는 오류를 근절하기 어렵습니다.  
또한 Unity용 공식 Credential Manager 구현이 제공되지 않아(구글의 google-signin-unity 저장소는 2026년 아카이브되었고, [Unity 공식 문서](https://docs.unity.com/en-us/authentication/platform-signin/google)에도 Credential Manager 기반 공식 경로가 없음), 게임 환경에서 신규 방식을 안정적으로 적용하기 어렵습니다.  

구글의 레거시 API 강제 종료 시점이 명확히 공지되고 위 문제가 해결되는 시점에 신규 버전을 다시 제공할 예정이며, 그 전까지는 안정화 버전인 2.3.0 사용을 권장합니다.  

상기 내용은 안드로이드 구글 로그인에만 해당하며 iOS 구글 로그인은 관련 없습니다.  
:::

:::danger 1.0.0 -> 2.0.0 이상 마이그레이션 안내
만약 1.0.0 버전에서 2.0.0으로 업그레이드를 할 경우, 기존 Assets > The Backend > Toolkit > GoogleLogin 폴더를 제거한 후, 2.0.0 버전을 import 해주시기 바랍니다.  

기존에 유니티 인스펙터에 입력한 GCP의 webClientId, ios URL Schema, ios Client ID가 초기화 됩니다. 꼭 한번 더 정보들을 **재입력** 해주시기 바랍니다.
:::

## 구글 로그인 SDK 적용  
:::danger 적용 후 Google Play에 업로드하여 테스트를 꼭 해주세요!
구글 플레이에서는 앱 업로드 시, 업로드된 앱에 대하여 난독화, 보안, 무결성 검사, 최적화등의 작업이 자동으로 진행될 수 있습니다.  
이로 인해 유니티에서 빌드한 앱에서는 정상작동이 되더라도, 구글 플레이에서 다운받은 앱에서는 정상 작동이 되지 않을 수 있습니다.  

구글 로그인 SDK와 같은 외부 SDK를 import했을 경우에는 구글 플레이에서 내부 테스트 혹은 알파,베타 테스트에 앱을 업로드한 후 다운받는 실제 테스트가 필요합니다.  
:::

**Android용** : [BackendGoogleLogin-Android-2.3.0.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-Android-2.3.0.unitypackage) \[2026-08-13]  
**iOS용** : [BackendGoogleLogin-iOS-2.1.0.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-iOS-2.1.0.unitypackage) \[2024-04-24]  

:::info Android 2.3.0 업데이트 내용
안드로이드 2.3.0은 기존 Google Sign-In 방식을 유지하면서 다음 안정성 문제를 수정한 버전입니다.
- 로그인 도중 화면 회전 등 구성 변경이나 프로세스 재시작이 발생하면 앱이 강제 종료되거나 로그인 콜백이 유실되던 문제를 수정하였습니다.
- 로그인 결과 콜백이 중복 호출될 수 있던 문제를 수정하였습니다.
- 콜백 미등록 등 예외 상황에서 예외가 게임 코드로 전파되지 않도록 예외 처리를 강화하였습니다.

2.2.0에서 업데이트할 경우, 기존 Assets > TheBackend > Toolkit > GoogleLogin > Android 폴더를 제거한 후 2.3.0을 import 해주시기 바랍니다.
:::

구글 로그인 SDK는 **Android, iOS만 지원**하고 있으며, 유니티 에디터에서는 작동하지 않습니다.  

## External Dependency Manager for Unity 설치
구글 로그인 SDK 2.0.0 버전 이후부터는 External Dependency Manager for Unity(이하 EDM4U)에 의해 안드로이드용 플러그인, iOS용 cocoapod 파일이 생성됩니다.  

EDM4U는 파이어베이스나 애드몹 등 서드파티 설치시에 함께 설치되는 경우가 많습니다.

유니티에서 Assets > ExternalDependencyManager 폴더가 있는지 확인해주세요.  

![](/img/docs/guide/toolkit/google-login/asset-edm4u.png)

만약 해당 폴더가 존재하지 않는 다면 아래 링크에서 다운 받을 수 있습니다.
- [다운로드(.unitypackage)](https://developers.google.com/unity/archive#external_dependency_manager_for_unity)  

구글 로그인 SDK가 설치된 이후, 유니티 상단의 Assets > External Dependency Manager > Android Resolver > Force Resolve를 클릭하여 종속성된 플러그인들을 다운받습니다.

![](/img/docs/guide/toolkit/google-login/use-force-resolve.png)

EDM4U가 정상적으로 실행되었다면 성공했다는 모달이 표시됩니다.  

![](/img/docs/guide/toolkit/google-login/edm4u-success.png)

성공 이후에는 Assets > Plugins > Android 폴더에서 구글 로그인용 플러그인이 생성된 것을 확인하실 수 있습니다.

![](/img/docs/guide/toolkit/google-login/check-android-plugins.png)

### EDM4U Google.IOSResolver.dll 에러
EDM4U 설치 시, 다음과 같은 에러가 발생할 수 있습니다.  
해당 에러는 사용하는 데에는 문제가 없지만, 만약 해결을 원하실 경우 해당 유니티 버전의 iOS 플랫폼을 추가해야합니다.

![](/img/docs/guide/toolkit/google-login/edm4u-ios-error.png)

### EDM4U 사용이 불가할 경우
만약 EDM4U 사용이 불가능하거나, 에러가 지속적으로 발생할 경우, android 플러그인들을 직접 설치해야합니다.  

그러나 직접 설치 시에는 이름은 같으나 버전이 다른 플러그인들의 중복 발생으로 인해 빌드가 실패할 수 있으니, 이름이 같고 버전이 다른 플러그인들은 제거를 해주셔야합니다.  

**Android 추가 플러그인(EDM4U 사용이 불가능한 경우)** : [BackendGoogleLogin-extra-plugins.unitypackage](https://developer.thebackend.io/sdk/google-login/BackendGoogleLogin-extra-plugins.unitypackage) \[2024-02-22]  
:::caution progard user안내
만약 프로가드를 사용중일 경우, 아래와 같은 예외 처리가 필요합니다.  
```
-keep class io.thebackend.googlelogin.** {*;}
```
:::

## 초기 구글 콘솔 설정  
구글로 로그인을 사용하기 위해서는 구글 클라우드 플랫폼(GCP)에서 Client ID를 생성해야 합니다.  
Android와 iOS 두 플랫폼 모두 지원할 경우, 공동의 GCP 프로젝트에서 각각 Adnorid, iOS Client ID를 생성하면 됩니다.  

**GPGS로 로그인 과정 중 GPGS 플러그인 사용 및 구글 콘솔에서 GPGS 설정하는 부분을 제외하고는 동일한 방법으로 진행됩니다.**  


안드로이드 : [안드로이드 추가 콘솔 설정](/sdk-docs/backend/toolkit/google-login/android/android-settings)  
iOS : [iOS 추가 콘솔 설정](/sdk-docs/backend/toolkit/google-login/ios/ios-settings)  
