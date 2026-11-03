---
sidebar_label: 설치하기
description: "설치하기"
---

# Mac에 설치하기

뒤끝펑션 개발 도구 `backend`를 Mac에 설치하는 방법입니다.  
관리자 권한 없이 현재 사용자 계정에만 설치됩니다.  

## 준비 사항

- Apple Silicon(M1 이상) Mac
  - Intel 프로세서를 사용하는 Mac은 지원하지 않습니다.
- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0) Arm64 버전 (권장)
  - 설치되어 있지 않으면 `backend`가 사용자 폴더에 전용 SDK를 내려받습니다. 편집기의 코드 자동 완성과 디버깅을 사용하려면 직접 설치해 주세요.

:::info Mac에서는 Visual Studio Code를 사용합니다
Mac에서는 Visual Studio를 지원하지 않습니다.  
프로젝트 생성과 디버깅은 [Visual Studio Code](/sdk-docs/function/work-in-ios/visual-studio-code/create) 문서를 참고해 주세요.  
:::

:::caution 1.0.2 이하 버전을 사용했다면
1.0.2 이하 버전의 설치 프로그램(pkg)은 `~/.zshrc`에 기존 버전의 PATH 줄을 추가합니다. 이 줄이 남아 있으면 새 터미널에서 기존 버전이 먼저 실행되어, 설치 명령이 마지막 확인 단계에서 실패합니다.  
해결 방법은 아래 [1.0.2 이하 버전이 설치되어 있는 경우](#102-이하-버전이-설치되어-있는-경우)를 참고해 주세요.  
:::

## 1. 설치하기

**터미널**을 열고 아래 명령을 실행합니다. `sudo`나 관리자 계정은 사용하지 않습니다. 관리자(root)로 실행하면 설치를 거부합니다.  

```bash
curl -fsSL https://developer.thebackend.io/sdk/BackendFunction/live/install.sh | bash
```

설치 스크립트는 아래 순서로 진행합니다.  

1. 최신 버전을 확인하고 설치 파일을 내려받아, **Apple Developer ID 코드 서명**을 확인한 뒤에만 실행합니다. 자세한 내용은 [설치 파일의 코드 서명](#설치-파일의-코드-서명)을 참고해 주세요.  
2. 실행된 설치 파일이 `backend`를 설치합니다. `backend`는 릴리스의 서명을 한 번 더 확인합니다.  
3. `~/.local/bin/backend`를 만들고, `~/.local/bin`이 PATH에 없으면 PATH에 추가하는 줄을 `~/.zprofile`에 한 번 추가합니다.  
4. 깨끗한 새 로그인 셸(zsh)에서 `backend`가 방금 설치한 `backend`를 가리키는지 확인합니다.  

설치가 끝나면 아래와 같은 메시지가 표시됩니다. 버전과 사용자 폴더 경로는 환경에 따라 다릅니다.  

```text
Latest release in this channel: 1.1.1
backend is installed in /Users/<사용자 이름>/Library/Application Support/AFI/Function and available as /Users/<사용자 이름>/.local/bin/backend
```

## 2. 설치 확인하기

**새 터미널 창**을 열고 아래 명령을 실행합니다.  
설치에 사용한 터미널 창에는 PATH 변경이 적용되지 않습니다.  

```bash
backend --version
```

아래와 같이 버전이 표시되면 설치가 완료된 것입니다.  

```text
backend 1.1.1
```

방금 설치한 `backend`가 실행되는지도 확인합니다. 아래 명령의 결과가 `/Users/<사용자 이름>/.local/bin/backend` 경로여야 합니다.  

```bash
command -v backend
```

개발 환경 전체를 점검하려면 아래 명령을 실행합니다.  
결과의 `"ok": true`와 `"dotnet"` 항목의 `"ready": true`를 확인합니다.  

```bash
backend doctor
```

## 3. 인증 정보 등록하기

설치를 확인했다면 `backend config`로 뒤끝펑션 인증키를 등록합니다. 처음 한 번만 하면 되고, 이후에 만드는 모든 프로젝트에서 사용합니다.  

```bash
backend config
```

`Function auth key` 질문에 뒤끝 콘솔에서 복사한 인증키를 붙여 넣고(화면에는 표시되지 않습니다), 나머지 질문은 Enter를 누릅니다.  
함수에서 뒤끝 기능을 호출하고 이를 내 PC에서 테스트 계정으로 실행하려면 클라이언트 앱 ID, 시그니처 키, 패키지 이름도 입력하고 테스트 계정으로 로그인합니다. 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting) 문서를 참고해 주세요.  

등록을 마쳤다면 [Visual Studio Code](/sdk-docs/function/work-in-ios/visual-studio-code/create)에서 프로젝트를 만듭니다.  

## 1.0.2 이하 버전이 설치되어 있는 경우

1.0.2 이하 버전이 있어도 설치 스크립트는 설치 전에 멈추지 않습니다. 경고를 표시하고 기존 버전과 셸 설정 파일은 그대로 둡니다.  
다만 기존 설치 프로그램이 `~/.zshrc`에 추가한 PATH 줄이 남아 있으면 새 터미널에서 기존 버전이 먼저 실행되므로, 설치는 마지막 확인 단계에서 실패로 끝납니다(아래 두 번째 항목).  

```text
install.sh: warning: an older BACKND Function CLI exists at /Library/Application Support/AFI/BFunc/Cli/backend; it was not changed
install.sh: warning: /Users/<사용자 이름>/.zshrc mentions AFI/BFunc from the old installer and was not changed; remove that line if 'backend' still starts the old CLI
```

- 셸 설정 파일(`~/.zshrc`, `~/.bash_profile`, `~/.profile`, `~/.zprofile`)에 기존 설치 프로그램이 남긴 `AFI/BFunc` 줄이 있다는 경고가 나오면, 새 터미널에서 `command -v backend`의 결과를 확인합니다. 기존 버전이 실행된다면 그 줄을 지웁니다.  
- 스크립트가 깨끗한 새 로그인 셸에서 `backend`가 새 버전을 가리키지 않는다고 판단하면 설치를 **실패로 끝내고**, `~/.zprofile`에 넣을 줄을 안내합니다. 이때 새 버전 파일은 이미 설치되어 있습니다. 아래와 비슷한 메시지입니다. 메시지의 경로가 `/Library/Application Support/AFI/BFunc/Cli/backend`이면 `~/.zshrc`에서 `AFI/BFunc` 줄을 지우고, 그 밖의 경우에는 안내에 따라 줄을 고친 뒤 설치 명령을 다시 실행합니다.  

  ```text
  install.sh: backend was installed to /Users/<사용자 이름>/Library/Application Support/AFI/Function, but a clean zsh login shell resolves 'backend' to '/Library/Application Support/AFI/BFunc/Cli/backend' instead of '/Users/<사용자 이름>/.local/bin/backend'. Please add these two lines to /Users/<사용자 이름>/.zprofile, then open a new terminal and check 'command -v backend':
  # BACKND Function CLI (added by install.sh)
  export PATH="$HOME/.local/bin:$PATH"
  Keep the PATH line below any other PATH changes, or remove the earlier 'backend' command.
  ```

- 기존 버전을 지우려면 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version) 문서를 참고해 주세요.  

## 설치 옵션

대부분은 옵션 없이 설치하면 됩니다. 필요한 경우에만 사용해 주세요.  

| 목적 | 환경 변수 | 옵션 |
|---|---|---|
| PATH를 바꾸지 않습니다(`~/.local/bin`과 `~/.zprofile`을 건드리지 않음). 설치 뒤 `backend`를 전체 경로로 실행합니다. | `BACKEND_INSTALL_NO_PATH=1` | `--no-path` |
| 설치 폴더를 바꿉니다. | `BACKEND_HOME=<절대 경로>` | `--install-root <절대 경로>` |

옵션은 `bash -s --` 뒤에 적습니다. 환경 변수를 사용할 때는 `curl ... | BACKEND_INSTALL_NO_PATH=1 bash`처럼 `bash` 앞에 적어야 설치 스크립트에 전달됩니다.  

```bash
curl --proto '=https' --proto-redir '=https' --tlsv1.2 -fsSL https://developer.thebackend.io/sdk/BackendFunction/live/install.sh | bash -s -- --no-path
```

설치 폴더는 절대 경로여야 하고 `:`나 줄 바꿈이 없어야 하며, 현재 사용자 소유이고 그룹·다른 사용자가 쓸 수 없는 위치여야 합니다.  

## 설치 파일의 코드 서명

설치 명령이 내려받는 설치 파일과 `backend`는 뒤끝이 Apple Developer ID 인증서로 서명했고, Apple의 공증(notarization)을 받았습니다.  
설치 명령은 내려받은 설치 파일의 서명을 확인한 뒤에만 실행하고, `backend update`도 새 버전의 서명을 확인한 뒤에만 설치합니다. 서명을 확인하지 못하면 아무것도 설치하지 않고 멈춥니다.  

## 스크립트를 확인한 뒤 설치하기

회사 보안 정책 등으로 실행하기 전에 설치 스크립트를 검토해야 한다면, 파일로 내려받아 내용을 확인한 뒤 실행합니다.  

```bash
curl --proto '=https' --proto-redir '=https' --tlsv1.2 -fsSL https://developer.thebackend.io/sdk/BackendFunction/live/install.sh -o /tmp/backend-install.sh
less /tmp/backend-install.sh
bash /tmp/backend-install.sh
```

## 회사 네트워크에서 설치가 실패하는 경우

회사 보안 장비가 HTTPS 통신을 중간에서 검사하는 환경에서는 설치가 `NETWORK_ERROR`로 끝날 수 있습니다.  
`backend`는 macOS 키체인의 인증서가 아니라 `backend`에 포함된 인증서 목록으로 HTTPS 연결을 확인합니다.  
네트워크 담당자에게 `developer.thebackend.io`를 HTTPS 검사 예외로 등록해 달라고 요청해 주세요.  

## 설치 위치

| 항목 | 위치 |
|---|---|
| 설치 폴더 | `~/Library/Application Support/AFI/Function` |
| 명령 실행 경로 | `~/.local/bin/backend` (설치 폴더의 `bin/backend`로 연결) |
| 설치된 버전 | `~/Library/Application Support/AFI/Function/versions` |
| 내려받은 .NET SDK와 디버거 | `~/Library/Application Support/AFI/Function/tools` |
| 등록한 인증 정보와 테스트 계정 로그인 정보 | 로그인 키체인(서비스 이름 `io.thebackend.function`). 프로필마다 항목 2개로 저장됩니다. |

## 업데이트하기

새 버전이 배포되면 아래 명령으로 업데이트합니다.  
새 버전을 확인한 뒤 확인 질문이 나오면 `y`를 입력합니다.  

```bash
backend update
```

업데이트는 새 버전을 별도 폴더에 설치한 뒤 전환하므로 등록한 인증 정보와 프로젝트는 그대로 유지됩니다.  
프로젝트에 포함된 SDK DLL은 자동으로 바뀌지 않습니다. 화면 예시와 실패했을 때의 해결 방법은 [업데이트](/sdk-docs/function/upgrade) 문서를 참고해 주세요.  

## 제거하기

설치 폴더를 지워도 **키체인에 저장된 인증 정보와 테스트 계정 로그인 정보는 남습니다.** 키체인 항목은 `backend`가 아닌 macOS가 보관하기 때문입니다.  
키체인 항목까지 지우려면 설치 폴더를 지우기 **전에** 프로필마다 항목 이름을 확인해 둡니다.  

1. 프로필 이름을 확인합니다. 결과의 `profile` 값이 프로필 이름입니다. 따로 만들지 않았다면 `default` 하나입니다.  

   ```bash
   backend config list
   ```

2. 프로필마다 아래 명령으로 항목 이름을 확인하고, 결과의 `credentialStore`와 `sessionStore`에 있는 `account` 값을 각각 복사해 둡니다.  

   ```bash
   backend config show --profile default
   ```

3. 터미널에서 아래 명령을 실행해 설치 폴더와 실행 경로를 삭제합니다.  

   ```bash
   rm -rf "$HOME/Library/Application Support/AFI/Function"
   rm -f "$HOME/.local/bin/backend"
   ```

4. 2번에서 확인한 `account` 값으로 키체인 항목을 삭제합니다. 프로필마다 프로젝트 인증 정보(`credentialStore`)와 테스트 계정 로그인 정보(`sessionStore`) 두 항목을 지웁니다. 테스트 계정에 로그인한 적이 없다면 `sessionStore` 항목은 없으며, 이 경우 삭제 명령이 항목을 찾을 수 없다고 알려 줍니다.  

   ```bash
   security delete-generic-password -s io.thebackend.function -a "<credentialStore의 account 값>"
   security delete-generic-password -s io.thebackend.function -a "<sessionStore의 account 값>"
   ```

5. `~/.zprofile`을 열어 아래 두 줄을 삭제합니다. 다른 프로그램도 `~/.local/bin`을 사용한다면 `export` 줄은 남겨 둡니다.  

   ```bash
   # BACKND Function CLI (added by install.sh)
   export PATH="$HOME/.local/bin:$PATH"
   ```

- 설치 폴더를 바꿔 설치했다면 3번의 첫 줄을 그 폴더로 바꿔 실행해 주세요.  
- 삭제되는 것은 이 Mac에 저장된 값뿐입니다. `backend`는 제거할 때 서버에 로그아웃을 요청하지 않습니다.  
- 함수 프로젝트 폴더는 삭제되지 않습니다. 프로젝트 안의 `.backend` 폴더(빌드·디버깅 파일)는 필요하면 직접 삭제해 주세요. 다시 실행하면 자동으로 만들어집니다.  
