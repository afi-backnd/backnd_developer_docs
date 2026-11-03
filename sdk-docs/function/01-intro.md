---
sidebar_label: 시작하기
description: "시작하기"
---

# 시작하기

뒤끝펑션은 개발사가 직접 작성한 C# 함수를 뒤끝 서버에 배포하고, 게임 클라이언트에서 뒤끝 SDK의 뒤끝베이스 기능으로 호출하는 기능입니다.  

:::warning 뒤끝베이스와 함께 사용해야 합니다
클라이언트에서 배포된 함수를 호출하는 방법은 [뒤끝펑션 호출](/sdk-docs/backend/base/function/execute-function) 문서를 참고해 주세요.  
:::

:::info 1.1.0부터 개발 도구가 바뀌었습니다
1.1.0부터는 명령줄 도구 `backend` 하나로 설치, 프로젝트 생성, 로컬 실행, 배포를 모두 진행합니다.  
설치 프로그램(Setup.exe, pkg), Visual Studio 프로젝트 템플릿(VSIX), Visual Studio Code용 BackendFunction 확장은 더 이상 필요하지 않습니다.  
1.0.2 이하 버전을 사용하고 있다면 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version) 문서를 먼저 확인해 주세요.  
:::

## 뒤끝펑션 인증키 발급

뒤끝펑션을 사용하려면 뒤끝 콘솔에서 먼저 뒤끝펑션을 활성화해야 합니다. 뒤끝 콘솔에서 뒤끝펑션 메뉴를 선택한 뒤 활성화하면 인증키를 발급받을 수 있습니다.  

![](/img/docs/guide/function/intro/function-key.png)

발급받은 인증키는 두 곳에서 사용합니다.  

- **유니티:** 뒤끝 SDK를 임포트한 뒤 상단 메뉴 The Backend > Edit Settings의 Function Auth Key에 입력합니다.  
- **배포 도구:** 함수를 서버에 배포할 때 사용합니다. 등록 방법은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  

![](/img/docs/guide/function/intro/function-unity.png)

## 개발 환경

| 항목 | 요구 사항 |
|---|---|
| 운영체제 | Windows 10 이상(64비트) 또는 Apple Silicon(M1 이상) Mac |
| 편집기 | Visual Studio Code(Windows·Mac) 또는 Visual Studio 2022 17.8 이상(Windows) |
| .NET | [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) |

- Intel 프로세서를 사용하는 Mac은 지원하지 않습니다.  
- Mac에서는 Visual Studio를 지원하지 않습니다. Visual Studio Code를 사용해 주세요.  
- `backend`는 .NET 8 SDK를 찾지 못하면 사용자 폴더에 전용 SDK를 내려받아 빌드에 사용합니다. 편집기의 코드 자동 완성과 디버깅을 원활하게 사용하려면 .NET 8 SDK를 직접 설치해 주세요.  

## 설치하기

운영체제에 맞는 명령을 복사해 실행합니다. 관리자 권한 없이 현재 사용자 계정에만 설치됩니다.  

### Windows

시작 메뉴에서 **PowerShell**을 열고 아래 명령을 실행합니다.  

```powershell
irm https://developer.thebackend.io/sdk/BackendFunction/live/install.ps1 | iex
```

### Mac

**터미널**을 열고 아래 명령을 실행합니다. `sudo`는 붙이지 않습니다.  

```bash
curl -fsSL https://developer.thebackend.io/sdk/BackendFunction/live/install.sh | bash
```

### 설치 확인하기

설치가 끝나면 **새 PowerShell 창**(Mac은 **새 터미널 창**)을 열고 아래 명령을 실행합니다. `backend 1.1.1`처럼 버전이 표시되면 설치된 것입니다.

```shell
backend --version
```

1.0.2 이하 버전이 설치된 PC에서 설치가 멈추거나 실패할 때의 해결 방법과 설치 옵션, 업데이트, 제거 방법은 [Windows에 설치하기](/sdk-docs/function/work-in-windows/install) / [Mac에 설치하기](/sdk-docs/function/work-in-ios/install) 문서를 참고해 주세요.  

## 처음 함수 배포하기

아래 순서대로 진행하면 함수를 만들어 서버에 배포하고 유니티에서 호출할 수 있습니다.  
  
각 단계의 자세한 설명은 연결된 문서를 참고해 주세요.  

1. `backend`를 설치합니다. [설치하기](#설치하기)
2. `backend config`로 인증 정보를 등록합니다. 처음 한 번만 하면 됩니다. [인증 정보 등록](/sdk-docs/function/cli/setting)
3. 프로젝트를 만듭니다. Windows: [Visual Studio Code](/sdk-docs/function/work-in-windows/visual-studio-code/create) / [Visual Studio](/sdk-docs/function/work-in-windows/visual-studio/create), Mac: [Visual Studio Code](/sdk-docs/function/work-in-ios/visual-studio-code/create)
4. 내 PC에서 함수를 실행해 결과를 확인합니다. [로컬 실행](/sdk-docs/function/cli/debug)
5. 함수를 서버에 배포합니다. [배포](/sdk-docs/function/cli/deploy)
6. 유니티에서 배포한 함수를 호출합니다. [뒤끝펑션 호출](/sdk-docs/backend/base/function/execute-function)

처음부터 끝까지 한 번에 따라 해 보려면 [에코 함수 튜토리얼](/sdk-docs/function/tutorial/echo-function)을 진행해 주세요.  

### 명령으로 진행하기

설치를 마친 뒤 새로 연 PowerShell 창(Mac은 터미널 창)에서 아래 명령을 단계마다 하나씩 실행합니다. Windows와 Mac의 명령은 같습니다.  
`MyFunction`은 프로젝트 폴더 이름이자 서버에 배포될 함수 이름입니다.  

**2. 인증 정보 등록하기** (처음 한 번만)

```shell
backend config
```

`Function auth key` 질문에만 뒤끝 콘솔에서 복사한 인증키를 붙여 넣고(화면에는 표시되지 않습니다), 나머지 질문은 Enter를 누릅니다. 클라이언트 앱 ID 등을 입력하지 않았으므로 `Skipping test-user login: ...`이 표시되고, 이어서 `Saved profile 'default'.`와 저장 요약이 표시되면 등록이 끝난 것입니다. 등록한 인증 정보는 이후에 만드는 프로젝트에서도 그대로 사용합니다.  
  
함수에서 뒤끝 기능(게임 정보 저장, 영수증 검증 등)을 호출하고 이를 내 PC에서 테스트 계정으로 실행(`backend debug --user`)하거나 `backend invoke`로 호출하려면, 클라이언트 앱 ID, 시그니처 키, 패키지 이름도 입력하고 테스트 계정으로 로그인합니다. 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  

**3. 프로젝트 만들기**

```shell
backend new MyFunction
cd MyFunction
```

**4. 내 PC에서 실행하기**

```shell
backend debug
```

템플릿 함수는 뒤끝 SDK를 초기화한 뒤 받은 값을 그대로 돌려주기만 하므로, 등록한 인증 정보 없이 내 PC에서 실행됩니다.  
함수에는 프로젝트의 `requests/default.json` 내용이 전달되며, 결과에 아래와 같이 `"ok": true`와 함수가 반환한 값이 표시되면 정상입니다.  

```json
"execution": {
  "error": null,
  "ok": true,
  "result": {
    "input": {
      "message": "hello"
    },
    "message": "Hello from Backend Function"
  }
}
```

**5. 서버에 배포하기**

```shell
backend deploy
```

배포할 함수와 프로필을 안내한 뒤 확인 질문이 나오면 `y`를 입력합니다.  
