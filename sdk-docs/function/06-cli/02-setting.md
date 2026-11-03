---
sidebar_label: 인증 정보 등록
description: "인증 정보 등록"
---

# 인증 정보 등록

`backend config` 명령 하나로 `backend`가 사용하는 인증 정보를 등록하고 수정합니다.  
명령을 실행하면 값을 하나씩 물어보고, 입력한 값은 이 PC의 사용자 계정에 암호화해서 저장합니다.  

## 어떤 값이 필요한가요

| 값 | 어디서 구하나요 | 사용하는 기능 |
|---|---|---|
| 뒤끝펑션 인증키 | 뒤끝 콘솔의 뒤끝펑션 메뉴에서 발급받습니다. [시작하기](/sdk-docs/function/intro#뒤끝펑션-인증키-발급) 문서를 참고해 주세요. | `deploy`, `delete`, `invoke`, `debug --user`, 편집기의 테스트 계정 F5 |
| 클라이언트 앱 ID, 시그니처 키, 패키지 이름 | 유니티 뒤끝 SDK 설정(The Backend > Edit Settings)에 입력한 값과 같습니다. | 테스트 계정 로그인, `debug --user`, 편집기의 테스트 계정 F5, `invoke`, `list`, `status` |
| 테스트 계정 ID, 비밀번호 | 커스텀 회원가입 또는 뒤끝 콘솔에서 만든, 실제로 존재하는 계정입니다. | 테스트 계정 로그인 |

- **배포와 삭제만 사용한다면** 뒤끝펑션 인증키만 등록하면 됩니다. 나머지 질문은 Enter로 넘어가고, 로그인 질문 대신 `Skipping test-user login: the client app ID, signature key and package name are all required.`가 표시됩니다.  
- 내 PC에서 함수만 실행하는 `backend debug`(기본 실행)는 인증 정보가 필요하지 않습니다.  
- 테스트 계정은 게임을 하는 실제 계정이 아니라 **테스트 전용 계정**을 사용해 주세요. 한 계정의 로그인은 한 곳에서만 유지되므로, 다른 곳(게임 클라이언트, 다른 PC 등)에서 같은 계정으로 로그인하면 먼저 로그인한 쪽의 로그인이 끝납니다.  

## 등록하기

프로젝트 폴더에서 PowerShell 또는 터미널을 열고 아래 명령을 실행합니다.  
Windows와 Mac에서 같은 명령을 사용합니다.  

```powershell
backend config
```

### 1. 프로필 확인

첫 줄에 어느 프로필에 저장하는지 표시됩니다. 프로필은 인증 정보를 묶어 두는 이름이며, 따로 지정하지 않으면 `default`입니다.  

```text
Profile: default (default)
Press Enter to keep the value in [brackets]. Secret values are not echoed.
```

두 번째 줄은 "대괄호 안의 값을 그대로 쓰려면 Enter를 누르고, 비밀 값은 입력해도 화면에 표시되지 않는다"는 뜻입니다.  

### 2. 프로젝트 인증 정보 입력

뒤끝 콘솔에서 값을 복사해 붙여 넣고 Enter를 누릅니다.  
인증키와 시그니처 키는 붙여 넣어도 화면에 아무것도 표시되지 않습니다. 정상 동작이므로 그대로 Enter를 누릅니다.  

```text
Function auth key [None]:
Client app ID [None]:
Signature key [None]:
Package name [None]:
```

| 질문 | 입력할 값 |
|---|---|
| `Function auth key` | 뒤끝펑션 인증키입니다. |
| `Client app ID` | 클라이언트 앱 ID입니다. |
| `Signature key` | 시그니처 키입니다. |
| `Package name` | 앱의 패키지 이름입니다. 뒤끝 콘솔에 입력한 패키지 이름과 같아야 합니다. |

- 대괄호 안에는 현재 값이 표시됩니다. 값이 없으면 `None`, 비밀 값은 마지막 4글자만 `****f3a9`처럼 표시됩니다.  
- 값은 영문, 숫자, 기호만 사용할 수 있고 공백은 쓸 수 없습니다. 최대 512바이트입니다.  
- 잘못된 값을 입력하면 같은 질문을 다시 합니다. 세 번 모두 잘못 입력하면 아무것도 저장하지 않고 끝납니다.  
- 값을 입력하지 않고 Enter만 누르면 저장된 값을 그대로 둡니다.  

### 3. 테스트 계정 로그인

클라이언트 앱 ID, 시그니처 키, 패키지 이름이 모두 저장되어 있다면 테스트 계정 로그인 여부를 묻습니다.  
테스트 계정이 필요 없다면 `n`을 입력해 넘어가도 됩니다. 나중에 `backend config`를 다시 실행해 로그인할 수 있습니다.  

```text
Log in a test user for invoke, list, status and "backend debug --user"? [y/N]:
```

`y`를 입력하면 테스트 계정 ID와 비밀번호를 물어보고 로그인합니다. 비밀번호는 입력해도 표시되지 않습니다.  

```text
Use a dedicated test account: signing in can end that account's session in a game client.
Test user ID:
Test user password:
Logging in to BACKND...
Test user logged in (te****01, gamer). The password was not stored.
```

- 로그인하면 `backend`가 로그인 정보(토큰)를 저장합니다. **비밀번호는 저장하지 않습니다.**  
- 저장한 로그인 정보는 `invoke`, `list`, `status`, `debug --user`, 편집기의 테스트 계정 F5가 사용하고, 만료되면 자동으로 갱신합니다.  
- 괄호 안의 `te****01`은 계정 ID의 일부만 보여 주는 표시입니다.  

### 4. 결과 확인

마지막에 저장한 내용을 요약해서 보여 줍니다.  

```text
Saved profile 'default'.
  deploy:       configured
  SDK identity: configured
  test user:    active (te****01), renewed automatically
```

| 항목 | 의미 |
|---|---|
| `deploy` | `configured`면 인증키가 저장되어 `deploy`, `delete`를 사용할 수 있습니다. |
| `SDK identity` | `configured`면 클라이언트 앱 ID, 시그니처 키, 패키지 이름이 모두 저장되어 있습니다. |
| `test user` | `active`면 로그인 정보가 저장되어 `list`, `status`를 사용할 수 있습니다. `invoke`는 인증키(`deploy: configured`)도 있어야 하며, `debug --user`와 테스트 계정 F5에서 함수가 뒤끝 SDK를 호출할 때도 인증키가 필요합니다. 로그인하지 않았다면 `none`입니다. |

요약 아래에는 결과 JSON도 함께 표시됩니다. 인증 정보 값은 `"credentials": "[REDACTED]"`로 가려져 표시되지 않습니다.  

```json
{
  "command": "config",
  "data": {
    "changed": [
      "authKey",
      "clientAppId",
      "signatureKey",
      "packageName"
    ],
    "created": true,
    "credentials": "[REDACTED]",
    "deployConfigured": true,
    "origin": {
      "projectId": null,
      "type": "manual"
    },
    "profile": "default",
    "profileSource": "default",
    "protection": "windows-dpapi-current-user",
    "schemaVersion": 2,
    "sdkIdentityConfigured": true,
    "sdkSessionConfigured": false,
    "session": null,
    "upgradedFromSchema": null
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

위 예시는 테스트 계정에 로그인하지 않은 경우입니다. 로그인했다면 `session`에 로그인 상태가 표시됩니다.  
Mac에서는 `protection`이 `macos-keychain`으로 표시됩니다.  

## 저장 위치

등록한 인증 정보는 프로젝트 폴더가 아니라 현재 PC의 사용자 계정에 암호화해서 저장됩니다. Git에 올라가지 않습니다.  

| 운영체제 | 저장 위치 |
|---|---|
| Windows | `%LOCALAPPDATA%\AFI\Function\profiles`입니다. 현재 Windows 사용자만 복호화할 수 있습니다. |
| Mac | 로그인 키체인(서비스 이름 `io.thebackend.function`)입니다. 프로필마다 프로젝트 인증 정보와 테스트 계정 로그인 정보가 각각 하나씩, 항목 2개로 저장됩니다. |

## 캐릭터가 여러 개인 프로젝트

뒤끝베이스에서 멀티 캐릭터를 사용하는 프로젝트는 테스트 계정으로 로그인하면 계정의 캐릭터 목록이 표시됩니다.  
함수를 실행할 캐릭터의 번호를 입력합니다.  

```text
Test user ID:
Test user password:
Logging in to BACKND...
This project uses multi-character accounts. Fetching the account's characters...
Characters of this account:
  1) Knight  (inDate 2026-08-20T03:16:58.652Z, last login 2026-08-20T03:18:50.961Z)
  2) Mage    (inDate 2026-09-02T11:02:10.001Z, last login never)
Run as which character? [1-2]:
Test user logged in (te****01) as character 'Knight'. The password was not stored.
```

- 이후 `invoke`, `debug --user` 등은 선택한 캐릭터로 실행됩니다.  
- 캐릭터가 하나뿐이면 묻지 않고 그 캐릭터를 선택합니다.  
- 같은 계정으로 다시 로그인하면 전에 선택한 캐릭터가 목록에 있는 한 묻지 않고 그 캐릭터를 다시 선택합니다.  
- 다른 캐릭터로 바꾸려면 `backend config`를 다시 실행합니다. 값 질문을 Enter로 넘기고 `Log in again?`에 `n`을 입력하면 이어서 `Run as a different character of the same account? [y/N]:`을 묻고, `y`를 입력하면 비밀번호 없이 캐릭터를 다시 고를 수 있습니다.  
- `backend`는 캐릭터를 만들거나 지우지 않습니다.  

계정에 캐릭터가 하나도 없으면 아래 오류가 표시되고 로그인 정보는 저장되지 않습니다.  
대화형으로 실행했다면 이어서 나머지 값만 저장할지(`Save the other values without a test-user session? [Y/n]`) 묻고, `--stdin`으로 실행했다면 이 오류로 끝납니다.  
게임 클라이언트 등에서 이 계정으로 캐릭터를 만든 뒤 `backend config`를 다시 실행합니다.  

```text
AUTH_NO_CHARACTER: This test account has no character. Create one with this account (for example from the game client) and run backend config again. The session created by this login was discarded locally; no test-user session was saved. The session created by this login was discarded locally; it may have ended that account's other sessions.
```

## 값 바꾸기

이미 등록한 프로필에 `backend config`를 다시 실행하면 저장된 값에 **합쳐서** 저장합니다. 입력하지 않은 값은 그대로 유지됩니다.  
바꾸고 싶은 값만 입력하고 나머지는 Enter를 누릅니다.  

```text
Profile: default (default)
Press Enter to keep the value in [brackets]. Secret values are not echoed.

Function auth key [****0001]:
Client app ID [a1b2c3d4e5f6]:
Signature key [****0002]:
Package name [com.example.game]:

A test-user session exists (te****01, logged in 2026-10-26T01:02:03Z). Log in again? [y/N]:
No changes. Profile 'default' was left untouched.
```

- 바뀐 값이 없으면 위처럼 `No changes. Profile 'default' was left untouched.`가 표시되고 아무것도 저장하지 않습니다.  
- 로그인 정보가 이미 있으면 `Log in again?`을 묻습니다. 다른 테스트 계정으로 바꾸거나 로그인이 끝난 경우에만 `y`를 입력합니다.  
- **클라이언트 앱 ID나 패키지 이름을 바꾸면** 저장된 로그인 정보가 다른 프로젝트의 것이 되므로 `Changing the client app ID or package name discards the saved test-user session of profile 'default'. Continue? [y/N]`을 묻고, `y`를 입력하면 로그인 정보를 지웁니다. 이후 다시 로그인해 주세요.  
- 인증키나 시그니처 키만 바꾸면 로그인 정보는 유지됩니다.  

### 처음부터 다시 입력하기

저장된 값을 모두 지우고 처음부터 입력하려면 `--reset`을 사용합니다.  
`Discard all stored values of profile 'default'? [y/N]`에 `y`를 입력해야 진행합니다.  

```powershell
backend config --reset
```

## 등록 정보 확인하기

인증 정보 값은 표시되지 않고, 등록 여부와 로그인 상태만 확인할 수 있습니다.  
`config show`는 지금 사용하는 프로필 하나를, `config list`는 저장된 모든 프로필을 보여 줍니다.  

```powershell
backend config show
backend config list
```

| 항목 | 설명 |
|---|---|
| `deployConfigured` | `true`면 인증키가 등록되어 있어 `deploy`, `delete`를 사용할 수 있습니다. |
| `sdkIdentityConfigured` | `true`면 클라이언트 앱 ID, 시그니처 키, 패키지 이름이 모두 등록되어 있습니다. |
| `sdkSessionConfigured` | `true`면 테스트 계정 로그인 정보가 저장되어 있습니다. |
| `session.state` | `active`는 사용할 수 있는 상태, `expired`는 만료된 상태입니다. |
| `session.userHint` | 로그인한 테스트 계정 ID를 일부만 가려서 보여 줍니다. |
| `session.character` | 멀티 캐릭터 프로젝트에서 선택한 캐릭터입니다. |
| `session.renewable` | `true`면 만료되었을 때 자동으로 갱신합니다. |

프로필이 없으면 `config show`는 아래 오류로 끝납니다.  

```json
"error": {
  "code": "AUTH_REQUIRED",
  "message": "Profile 'default' does not exist; run backend config or use BACKEND_* environment variables"
}
```

Mac에서는 `config show` 결과에 `credentialStore`와 `sessionStore`가 함께 표시됩니다(`config list`에는 표시되지 않습니다). 각 `account` 값이 키체인에 저장된 항목의 이름입니다.  

## 로그인 정보 갱신하기

저장된 로그인 정보는 `invoke`, `list`, `status`, `debug --user`, 편집기의 테스트 계정 F5를 실행할 때 필요하면 자동으로 갱신됩니다.  
바로 갱신하려면 아래 명령을 실행합니다. 질문 없이 끝납니다.  

```powershell
backend config --renew
```

아래와 같은 경우에는 로그인 정보를 갱신할 수 없어 `AUTH_SESSION_EXPIRED` 오류가 납니다.  

- 같은 테스트 계정으로 다른 곳(게임 클라이언트 등)에서 로그인해 이 로그인이 끝났습니다.  
- 로그인 정보의 갱신 기간이 지났습니다.

이때는 `backend config`를 다시 실행해 로그인해 주세요. 멀티 캐릭터 프로젝트에서는 비밀번호 없이 캐릭터 세션을 복구할지 묻는 질문(`Restore the character session from the stored account session? [Y/n]`)이 먼저 나올 수 있습니다.  

## 여러 프로필 사용하기

뒤끝 프로젝트를 여러 개 사용한다면 프로필 이름을 나누어 인증 정보를 따로 등록합니다.  
프로필 이름은 영문, 숫자, `_`, `-`로 최대 64자입니다.  

### 개발 프로젝트와 라이브 프로젝트 나눠 쓰기

개발용 뒤끝 프로젝트에서 배포·테스트하고, 준비가 끝난 함수만 라이브 프로젝트에 배포하는 경우의 예입니다.  

1. 프로젝트마다 프로필을 등록합니다. 각 명령에서 해당 뒤끝 프로젝트의 값을 입력합니다.  

   ```powershell
   backend config --profile dev
   backend config --profile live
   ```

2. 함수 프로젝트 폴더에서 개발 프로필을 연결합니다. 연결한 프로필은 `backend-function.json`의 `authProfile`에 저장됩니다.  

   ```powershell
   backend config use dev
   ```

   ```text
   Project 'MyFunction' now uses profile 'dev' (before: the default profile; no profile was linked).
   ```

3. 평소에는 `backend deploy`, `backend invoke`를 그대로 실행합니다. 연결한 `dev` 프로필로 개발 프로젝트에 배포하고 호출합니다.  
4. 라이브에 배포할 때만 `--profile live`를 붙입니다. 프로젝트에 연결된 프로필은 `dev` 그대로 남으므로, 다음 배포가 실수로 라이브에 올라가지 않습니다.  

   ```powershell
   backend deploy --profile live
   ```

배포와 삭제는 서버에 올리기 직전에 사용할 프로필을 안내하고 확인 질문을 표시합니다. 질문에 답하기 전에 `with profile '...'`이 배포하려는 프로젝트의 프로필인지 확인해 주세요. 라이브에 배포할 때는 확인 질문을 건너뛰는 `--yes`를 사용하지 않는 것을 권장합니다.  

```text
Deploy function 'MyFunction' with profile 'live'
  Profile source: --profile
  ZIP:            C:\work\MyFunction\.backend\publish.zip
  SHA256:         53d077a3d0a4ee9288166b66c8b3216068c021a5ab83fc9ae01a736296f39dec
  Other profiles: dev
  Change the project's profile with "backend config use <profile>", or pass --profile <profile> for one command.
Deploy function 'MyFunction' with profile 'live'? [y/N]:
```

### 프로젝트에 연결한 프로필 바꾸기

`backend config use <프로필 이름>`은 현재 프로젝트에 연결한 프로필을 바꿉니다. 바꾼 뒤에는 `deploy`, `delete`, `invoke`, `list`, `status`, `debug --user`가 모두 새 프로필을 사용합니다.  

```powershell
backend config use live
```

```text
Project 'MyFunction' now uses profile 'live' (before: 'dev').
```

- 등록하지 않은 프로필 이름을 입력하면 `AUTH_REQUIRED` 오류로 끝나고 아무것도 바뀌지 않습니다. 먼저 `backend config --profile <이름>`으로 등록해 주세요.  
- 프로젝트 폴더 밖에서 실행하면 `PROJECT_NOT_FOUND` 오류로 끝납니다. 프로젝트 폴더에서 실행하거나 `--project`를 지정합니다.  
- 프로필 이름은 `--profile` 없이 `config use` 뒤에 적습니다. 함께 입력하면 `INVALID_ARGUMENT` 오류로 끝납니다.  
- `backend-function.json`은 소스 관리에 함께 올라가는 파일입니다. 팀이 같은 저장소를 사용한다면 프로필 이름(`dev`, `live` 등)을 팀 안에서 맞춰 주세요.  
- 프로젝트를 만들 때 `backend --profile dev new MyFunction`처럼 지정해도 같은 값이 `authProfile`에 저장됩니다.  

### 프로필이 정해지는 순서

모든 명령이 아래 순서로 프로필을 정합니다.  

1. `--profile`로 지정한 이름  
2. 프로젝트의 `backend-function.json`에 적힌 `authProfile`(`backend config use`로 연결한 프로필)  
3. `default`  

- `backend config` 첫 줄에 `Profile: live (authProfile in C:\work\MyFunction\backend-function.json)`처럼 어디서 정해졌는지 표시됩니다.  
- 서버로 요청하는 명령(`deploy`, `delete`, `invoke`, `list`, `status`)은 실행할 때마다 `Target: profile 'live' (authProfile in ...)`처럼 사용하는 프로필을 표시합니다. 다른 뒤끝 프로젝트에 요청하지 않도록 이 줄을 확인해 주세요.  
- 연결한 프로필이 없는 프로젝트에서 배포하면 안내에 `Profile source: no profile is linked to this project, so the default profile is used`가 표시됩니다. `default`가 의도한 프로젝트가 아니라면 `n`을 입력하고 `backend config use`로 프로필을 연결해 주세요.  
- 프로젝트 폴더가 아닌 곳에서 `backend delete MyFunction`처럼 함수 이름만 입력하면 프로젝트를 찾을 수 없으므로 `default`를 사용합니다. 다른 프로필을 쓰려면 `--profile`을 함께 입력해 주세요.  
- 프로젝트 폴더에서 ZIP 파일을 직접 지정해 배포(`backend deploy 함수이름 파일.zip`)할 때 프로젝트에 `authProfile`이 있고 `--profile`, `--project`가 없으면 어느 프로필을 쓸지 알 수 없어 `PROFILE_AMBIGUOUS` 오류로 끝납니다. [배포](/sdk-docs/function/cli/deploy#이미-만든-배포-파일-올리기)를 참고해 주세요.  

## 기존 debugConfig.json에서 가져오기

1.0.2 이하 버전에서 만든 프로젝트의 `debugConfig.json`에 인증 정보를 적어 두었다면 `backend config`가 가져올 수 있습니다.  
[`backend migrate`](/sdk-docs/function/migrate-from-old-version)로 1.1.0 형식으로 옮겨 `backend-function.json`이 생긴 프로젝트 폴더에서 실행하면 파일을 발견했다는 질문이 나옵니다. 옮기기 전의 프로젝트에서는 질문이 나오지 않습니다.  

```text
Found credentials in C:\work\MyOldFunction\debugConfig.json. Offer them as defaults for empty fields? [y/N]:
```

`y`를 입력하면 값이 **비어 있는 항목**의 기본값으로 파일의 값이 표시됩니다. Enter를 누르면 그 값을 저장합니다.  

```text
Function auth key [****1111 from debugConfig.json]:
Client app ID [legacy-client-app-id from debugConfig.json]:
Signature key [****2222 from debugConfig.json]:
Package name [com.example.legacy from debugConfig.json]:
```

- 이미 저장된 값과 파일의 값이 다르면 `debugConfig.json has different values for: ...`로 바꿀지 따로 묻습니다. `n`이면 저장된 값을 그대로 둡니다.  
- 파일에 테스트 계정 ID와 비밀번호가 있으면 프로필을 새로 만들 때(`--reset`으로 처음부터 다시 입력할 때 포함) 로그인 질문의 기본값으로 사용합니다. 이미 저장된 프로필에 `backend config`를 다시 실행할 때는 사용하지 않으므로 직접 입력합니다.  
- 파일 자체는 바뀌지 않습니다. 가져온 뒤에는 `debugConfig.json`에서 인증 정보를 지워 주세요. 인증 정보가 남아 있는 동안은 `backend config`를 실행할 때마다 이 질문이 나옵니다.  

## 자동화 환경에서 사용하기

대화형 입력을 할 수 없는 빌드 서버나 스크립트에서는 `--stdin`으로 JSON 파일의 내용을 읽어 등록합니다.  
`backend config`를 대화형 입력 없이 실행하면 `INVALID_ARGUMENT: backend config is interactive. Use --stdin for automation, or run it in a terminal.` 오류가 나므로 자동화에는 반드시 `--stdin`을 사용합니다.  

### JSON 파일 만들기

아래 내용으로 `backend-config.json` 파일을 만들고 값을 채웁니다. **프로젝트 폴더 밖**(예: 사용자 폴더)에 만들어 Git에 올라가지 않게 해 주세요.  

```json
{
  "authKey": "뒤끝 콘솔에서 복사한 뒤끝펑션 인증키",
  "clientAppId": "클라이언트 앱 ID",
  "signatureKey": "시그니처 키",
  "packageName": "앱의 패키지 이름",
  "userId": "테스트 계정 ID",
  "password": "테스트 계정 비밀번호"
}
```

| 키 | 설명 |
|---|---|
| `authKey`, `clientAppId`, `signatureKey`, `packageName` | 프로젝트 인증 정보입니다. 적은 키만 저장되고 적지 않은 키는 저장된 값을 유지합니다. 영문, 숫자, 기호만 사용할 수 있고 공백은 쓸 수 없습니다. |
| `userId`, `password` | 테스트 계정으로 로그인합니다. 두 키를 함께 적어야 하며, 클라이언트 앱 ID, 시그니처 키, 패키지 이름이 프로필이나 같은 파일에 있어야 합니다. 로그인하지 않으려면 두 키를 모두 지웁니다. |
| `characterNickname` 또는 `characterUuid` | 멀티 캐릭터 프로젝트에서 실행할 캐릭터를 닉네임 또는 UUID로 지정합니다. 둘 중 하나만 적을 수 있고, 캐릭터가 하나뿐이면 생략해도 됩니다. |

위 형식에 없는 키를 적으면 `INVALID_ARGUMENT` 오류가 납니다. 값은 오류 메시지에 표시되지 않습니다.  
위 예시의 한글 안내 문구를 값으로 바꾸지 않고 실행하면 `authKey must be a non-empty visible ASCII string of at most 512 bytes`처럼 거부되어 아무것도 저장되지 않습니다.  

### 파일로 실행하기

**Windows (PowerShell)**

```powershell
Get-Content -Raw $HOME\backend-config.json | backend config --stdin
```

**Mac (터미널)**

```bash
backend config --stdin < ~/backend-config.json
```

결과에 `Saved profile 'default'.`와 요약이 표시되면 저장된 것입니다. 결과 JSON의 `changed`에 저장된 항목이 표시됩니다.  

- 명령 인자에 값을 직접 적지 않고 파일에서 읽으므로 명령 기록에 인증 정보가 남지 않습니다. `backend`는 인증 정보를 명령 인자로 받지 않습니다.  
- 이미 저장된 값과 **다른 값**으로 바꾸거나 로그인 정보를 지워야 하는 경우에는 확인이 필요합니다. 입력을 파일에서 읽는 동안에는 질문에 답할 수 없으므로 `--yes`를 함께 입력합니다. 없으면 `CONFIRMATION_REQUIRED` 오류로 끝납니다.  
- 멀티 캐릭터 프로젝트에서 캐릭터가 여러 개인데 `characterNickname`, `characterUuid`를 적지 않으면 `CHARACTER_SELECTION_REQUIRED` 오류로 끝나고, 선택할 수 있는 캐릭터 목록이 `error.details`에 표시됩니다. 같은 계정으로 전에 선택해 저장된 캐릭터가 목록에 있으면 그 캐릭터를 다시 선택하므로 이 오류가 나지 않습니다.  
- 이미 로그인한 멀티 캐릭터 프로필의 캐릭터만 바꾸려면 `userId`, `password` 없이 `characterNickname`만 적은 파일을 사용합니다. 비밀번호 없이 바뀝니다.  

:::danger 비밀번호가 든 파일은 사용 후 지워 주세요
`backend-config.json`에는 인증키와 테스트 계정 비밀번호가 그대로 적혀 있습니다.  
등록이 끝나면 파일을 삭제하고, 다른 사람과 공유하거나 Git에 올리지 마세요.  
빌드 서버에서는 Jenkins credentials 같은 비밀 값 저장 기능으로 파일을 만들고, 로그에 값이 남지 않도록 주의해 주세요.  
:::

### 등록하지 않고 환경 변수로 배포하기

프로필을 등록하기 어려운 빌드 서버에서는 환경 변수 `BACKEND_AUTH_KEY`에 인증키를 넣으면 프로필을 등록하지 않고도 배포와 삭제를 할 수 있습니다.  
확인 질문을 건너뛰려면 `--yes --non-interactive`를 함께 사용합니다.  

```powershell
backend deploy --yes --non-interactive
```

- `BACKEND_AUTH_KEY`, `BACKEND_CLIENT_APP_ID`, `BACKEND_SIGNATURE_KEY`, `BACKEND_PACKAGE_NAME`, `BACKEND_ACCESS_TOKEN`, `BACKEND_SESSION_SECRET_KEY` 같은 인증 환경 변수가 **하나라도** 있으면 `deploy`, `delete`는 등록한 프로필을 사용하지 않고 환경 변수만 사용합니다. 실행할 때 `Target: credentials from the BACKEND_* environment`가 표시되면 환경 변수를 사용하고 있는 것입니다.  
- 환경 변수가 남아 있는 PowerShell 또는 터미널 창에서는 등록한 프로필 대신 환경 변수가 사용되므로, 쓰지 않을 때는 창을 닫아 주세요.  
- 빌드 서버의 비밀 값 저장 기능(예: Jenkins credentials)으로 환경 변수를 전달하고, 로그에 인증키가 남지 않도록 주의해 주세요.  

## 오류가 발생한 경우

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `INVALID_ARGUMENT` | 터미널이 아닌 곳에서 `--stdin` 없이 실행했거나, 입력한 값이 규칙에 맞지 않습니다. 메시지를 확인해 주세요. |
| `CONFIRMATION_REQUIRED` | 확인 질문에 답할 수 없는 환경입니다. 내용을 확인한 뒤 `--yes`를 함께 입력합니다. |
| `CANCELLED` | 확인 질문에 `y`(또는 `yes`) 외의 값을 입력했거나 Ctrl+C로 취소했습니다. 아무것도 저장하지 않았습니다. |
| `AUTH_REQUIRED` | 저장할 값이 없거나, 프로필 또는 로그인 정보가 없습니다. 프로필은 인증키가 있거나 클라이언트 앱 ID, 시그니처 키, 패키지 이름이 세 가지 모두 있어야 저장할 수 있습니다. `backend config`를 실행합니다. |
| `AUTH_LOGIN_FAILED` | 테스트 계정 로그인에 실패했습니다. 클라이언트 앱 ID, 시그니처 키, 패키지 이름, 테스트 계정 ID와 비밀번호가 맞는지 확인합니다. 대화형으로 실행했다면 나머지 값만 저장할지(`Save the other values without a test-user session? [Y/n]`) 묻습니다. |
| `AUTH_NO_CHARACTER` | 계정에 캐릭터가 없습니다. 위 [캐릭터가 여러 개인 프로젝트](#캐릭터가-여러-개인-프로젝트)를 참고해 캐릭터를 만든 뒤 다시 실행합니다. |
| `AUTH_SESSION_EXPIRED` | 로그인 정보가 만료되었거나 다른 곳의 로그인으로 끝났습니다. `backend config`를 다시 실행해 로그인합니다. |
| `AUTH_DEVICE_BLOCKED` | 서버가 이 PC의 로그인 갱신을 거부했습니다. 뒤끝 콘솔에서 차단 여부를 확인합니다. |
| `CHARACTER_SELECTION_REQUIRED`, `CHARACTER_NOT_FOUND` | 자동화 입력에서 캐릭터를 지정하지 않았거나, 지정한 캐릭터가 계정에 없습니다. `error.details`의 목록을 확인합니다. |
| `PROFILE_CHANGED` | 입력하는 동안 다른 `backend` 명령이 같은 프로필을 바꿨습니다. `backend config`를 다시 실행합니다. |
| `BUSY` | 다른 `backend` 명령이 인증 정보를 사용하고 있습니다. 잠시 뒤 다시 실행합니다. |
| `AUTH_INVALID` | 저장된 프로필을 현재 사용자 계정으로 복호화할 수 없습니다. 메시지대로 `backend config --reset`으로 다시 등록합니다. Mac에서 로그인 키체인이 잠겨 있거나 키체인이 접근을 거부한 경우에도 이 오류가 나며, 이때는 키체인 잠금을 풀고(`security unlock-keychain`) 다시 실행합니다. |
| `CONFIG_ERROR` | 저장된 프로필 파일의 형식이 올바르지 않습니다. `backend config --reset`으로 다시 등록합니다. |
