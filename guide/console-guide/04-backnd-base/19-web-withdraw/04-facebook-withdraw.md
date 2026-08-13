---
description: "페이스북 로그인 유저 회원 탈퇴 웹 링크"
---

import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 페이스북 로그인 유저 회원 탈퇴 웹 링크

## 설명

```
Backend.BMember.AuthorizeFederation("", FederationType.FaceBook);
```

뒤끝에서 다음과 위와 함수로 애플 로그인을 진행한 유저가 회원 탈퇴 웹 링크를 통해 회원 탈퇴를 진행하려 할 경우, 다음과 같은 설정이 필요합니다.

1. 뒤끝 콘솔 > 인증 정보 > Facebook 인증 정보 - Facebook App ID 입력
2. Facebook Developer > JavaScript SDK 허용된 도메인 > storage.thebackend.io 추가

:::info 페이스북 로그인 구현 방법
페이스북 로그인 구현에 관련해서는 [SDK 문서 > 게임 유저 관리 > 페데레이션 인증 > 페이스북 인증 예제](/sdk-docs/backend/base/user/federation/example-using-facebook) 문서를 참고해주세요.
:::

## 1. Facebook App ID 입력
뒤끝 콘솔 > 인증 정보 > Facebook 인증 정보에서 로그인을 위해 Facebook App ID가 입력되어 있어야합니다.

<ConsoleLinkButton text="인증 정보 바로가기" menu="settingAuth" feature="인증 정보" title="페이스북 로그인 유저 회원 탈퇴 웹 링크" />

![](/img/docs/guide/console-guide/backnd-base/web-withdraw/facebook-withdraw/facebook-auth.png)


## 2. JavaScript SDK에 허용된 도메인 설정
1) [페이스북 개발자 페이지](https://developers.facebook.com/)로 이동하여 로그인 연동된 프로젝트 진입  
2) 왼쪽 사이드 메뉴에서 제품 > Facebook 로그인 > 설정 탭 진입  
3) 클라이언트 OAuth 설정 > JavaScript SDK에 허용된 도메인에 https://storage.thebackend.io 값 입력  

![](/img/docs/guide/console-guide/backnd-base/web-withdraw/facebook-withdraw/facebook-url.png)

:::danger 도메인 미설정 시
만약 입력을 하지 않을 경우, 웹 탈퇴 링크에서 다음과 같은 에러가 발생할 수 있습니다.  

![](/img/docs/guide/console-guide/backnd-base/web-withdraw/facebook-withdraw/facebook-bad-url.png)
:::
