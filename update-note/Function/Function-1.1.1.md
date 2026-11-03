---
title: Function-1.1.1
date: 2026-11-03T10:00
slug: Function-1-1-1
---

:::info 업데이트 요약
뒤끝펑션 개발 도구가 명령줄 도구 `backend` 하나로 바뀌었습니다. 설치, 프로젝트 생성, 로컬 실행, 배포, 업데이트를 모두 `backend`로 진행합니다.  
뒤끝펑션에서 호출하는 함수들이 뒤끝베이스 5.18.17 버전을 기반으로 동작하도록 업데이트하였습니다.  
:::

:::caution 1.0.2 이하 버전을 사용하고 있다면
설치 프로그램(Setup.exe, pkg), Visual Studio 프로젝트 템플릿(VSIX), Visual Studio Code BackendFunction 확장은 더 이상 사용하지 않습니다.  
Windows에서는 기존 개발 도구를 먼저 제거해 주세요. 기존 프로젝트는 `backend migrate`로 옮겨 그대로 사용할 수 있습니다. 자세한 방법은 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version) 문서를 참고해 주세요.  
:::

<!--truncate-->
## 설치

**Windows** (PowerShell)

```powershell
irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1 | iex
```

**Mac** (터미널)

```bash
curl -fsSL https://developer.thebackend.io/sdk/BackendFunction/live/install.sh | bash
```

이미 설치했다면 `backend update`로 업데이트합니다. 자세한 설치 방법은 [Windows](/sdk-docs/function/work-in-windows/install) / [Mac](/sdk-docs/function/work-in-ios/install) 문서를 참고해 주세요.  

## Versions
뒤끝펑션 SDK는 뒤끝베이스 5.18.17을 베이스로 하고 있습니다.  

## 1.1.1 Update
**[개발 도구]**
- 개발 도구를 명령줄 도구 `backend` 하나로 바꿨습니다. Windows(x64)와 Apple Silicon Mac(arm64)을 지원하며, Intel 프로세서를 사용하는 Mac은 지원하지 않습니다.
- 설치 명령 한 줄로 사용자 폴더에 설치합니다. 관리자 권한이 필요하지 않습니다.
- `backend update`로 개발 도구를 업데이트합니다. 뒤끝이 서명한 최신 버전 정보를 확인하고, 내려받은 파일의 서명과 게시자를 확인한 뒤 설치합니다. [업데이트](/sdk-docs/function/upgrade)
- `backend update --project <프로젝트 폴더> --yes`로 프로젝트의 뒤끝펑션 SDK DLL을 업데이트합니다. 바꾸기 전 파일은 프로젝트 안에 백업됩니다.
- `backend new`로 Visual Studio(`--vs`, Windows) 또는 Visual Studio Code(`--vscode`)용 프로젝트를 만듭니다. [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/create) / [Visual Studio Code (Windows)](/sdk-docs/function/work-in-windows/visual-studio-code/create) / [Visual Studio Code (Mac)](/sdk-docs/function/work-in-ios/visual-studio-code/create)
- `backend new`가 만드는 기본 템플릿을 뒤끝펑션 SDK를 초기화(`Backend.Initialize`)하는 형태로 바꿨습니다. 빈 함수로 시작하려면 `--template empty`를 사용합니다. [뒤끝펑션 템플릿](/sdk-docs/function/template)
- `backend migrate`로 1.0.2 이하 버전에서 만든 프로젝트를 새 형식으로 옮깁니다. 함수 코드는 바꾸지 않습니다.

**[인증 정보]**
- `backend config`로 뒤끝펑션 인증키, 클라이언트 앱 ID, 시그니처 키, 패키지 이름, 테스트 계정을 질문에 답하며 등록합니다. [인증 정보 등록](/sdk-docs/function/cli/setting)
- 인증 정보는 Windows에서는 현재 사용자만 복호화할 수 있는 형태로, Mac에서는 키체인에 저장합니다.
- 프로필을 여러 개 등록하고 `backend config use <프로필>`로 프로젝트마다 사용할 프로필을 정할 수 있습니다. `backend deploy`와 `backend delete`는 확인 질문 바로 위에 사용할 프로필을 다시 안내합니다.

**[로컬 실행과 디버깅]**
- `backend debug`는 인증 정보 없이 내 PC에서 함수를 실행합니다. 함수에 전달할 값은 프로젝트의 `requests` 폴더에 있는 요청 파일로 정합니다. [로컬 실행](/sdk-docs/function/cli/debug)
- `backend debug --user`로 등록한 테스트 계정으로 로그인해 뒤끝 기능을 호출하는 함수를 실행합니다.
- Visual Studio와 Visual Studio Code에서 F5로 중단점 디버깅을 할 수 있습니다. 오프라인과 테스트 계정 두 가지 실행 구성이 만들어집니다.

**[배포와 원격 명령]**
- `backend deploy`로 배포하고, `backend delete`, `backend list`, `backend status`, `backend invoke`로 서버의 함수를 삭제·조회·호출합니다. [Backend CLI](/sdk-docs/function/cli/backend-cli)

**[뒤끝펑션 SDK]**
- 펑션 함수들이 뒤끝베이스 5.18.17 버전을 기반으로 동작하도록 업데이트하였습니다. [뒤끝 기능 호출](/sdk-docs/function/backnd-features/call-features)
- 영수증 검증 `ValidateReceipt`(Unity IAP 5.x, 원스토어)를 사용할 수 있습니다. 뒤끝펑션에서는 스토어 타입을 직접 지정합니다. [영수증 검증](/sdk-docs/function/backnd-features/receipt-check/validate-receipt)
- 뒤끝 데이터베이스를 함수에서 읽고 쓸 수 있습니다. [뒤끝 데이터베이스 사용](/sdk-docs/function/backnd-features/database)

**[개발자 문서]**
- 요청 값 읽기·결과 반환하기·호출한 사용자 확인하기를 [요청과 응답](/sdk-docs/function/request-response/read-input) 문서로 정리했습니다.
- 튜토리얼에 [아이템 뽑기](/sdk-docs/function/tutorial/gacha)와 [거래소 만들기](/sdk-docs/function/tutorial/exchange)를 추가하고, [영수증 검증 및 아이템 지급](/sdk-docs/function/tutorial/receipt-check)을 Unity IAP 5.x와 원스토어 기준으로 바꿨습니다.
