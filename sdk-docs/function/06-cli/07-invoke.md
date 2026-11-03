---
sidebar_label: 원격 호출과 조회
description: "원격 호출과 조회"
---

# 원격 호출과 조회

서버에 배포된 함수를 유니티 없이 `backend`로 호출하거나, 배포된 함수 목록을 확인할 수 있습니다.  
이 명령들은 `backend config`에서 **테스트 계정으로 로그인해 둔 정보**가 필요하고, `invoke`는 뒤끝펑션 인증키도 등록되어 있어야 합니다. [인증 정보 등록](/sdk-docs/function/cli/setting)을 먼저 진행해 주세요.  
로그인 정보가 만료되면 `invoke`, `list`, `status`가 실행하는 도중에 자동으로 갱신합니다. 멀티 캐릭터 프로젝트는 로그인할 때 고른 캐릭터로 실행됩니다.  

## 함수 호출하기

함수에 전달할 값은 `backend debug`와 같은 **요청 파일**(JSON)로 만듭니다. 예를 들어 프로젝트의 `requests` 폴더에 `hello.json`을 만들고 아래와 같이 작성합니다.  
요청 파일은 유니티에서 `InvokeFunction`을 호출할 때 `Param`으로 보내는 값과 같은 형태의 JSON 객체입니다.  

```json
{
  "value": "HelloWorld!"
}
```

아래 명령으로 함수를 호출합니다. 먼저 사용하는 프로필이 `Target:` 줄로 표시되고, 이어서 확인 질문이 표시되면 `y`를 입력합니다.  

```powershell
backend invoke MyFunction --data hello
```

```text
Target: profile 'default' (default) -> https://api.thebackend.io/backend-function/base/v1
Invoke remote function 'MyFunction' using profile 'default' at https://api.thebackend.io/backend-function/base/v1?
Request: requests/hello.json (29 bytes, sha256 7a13a3c3af9c…). Function execution may mutate game data [y/N]:
```

`--data`에는 아래 값을 사용할 수 있습니다.  

| 입력 | 의미 |
|---|---|
| `hello` (영문, 숫자, `-`, `_`로 된 이름. 최대 64자) | 프로젝트의 `requests/hello.json`을 사용합니다. 프로젝트 폴더에서 실행하거나 `--project`를 지정해야 합니다. |
| `..\inputs\case1.json`처럼 `.`, `\`, `/`가 들어간 값 | 파일 경로입니다. 프로젝트 폴더가 아니라 명령을 실행한 현재 폴더 기준입니다. |
| `-` | 파일 대신 입력(stdin)에서 JSON을 읽습니다. 이때는 확인 질문에 답할 수 없으므로 `--yes`를 함께 입력합니다. |

- 프로젝트 폴더에서 실행할 때는 함수 이름을 생략할 수 있습니다. 이때는 `backend-function.json`의 `functionName`을 호출합니다.  
- **`--data`를 생략하면 빈 객체 `{}`를 전달합니다.** `backend debug`와 달리 `requests/default.json`을 자동으로 읽지 않으므로, 그 파일로 호출하려면 `--data default`를 입력해 주세요.  
- 전달할 값은 JSON 객체(`{ }`)여야 하고, 크기는 8MB까지입니다.  
- `--data hello.json`처럼 `.json`을 붙이면 이름이 아니라 경로로 처리됩니다. `requests` 폴더의 파일은 이름만 적어 주세요.  

결과의 `data.functionResult`에서 함수가 반환한 값을 확인합니다.  

:::caution 실제 서버에서 실행됩니다
`invoke`는 배포된 함수를 로그인한 테스트 계정으로 실제 서버에서 실행합니다.  
함수가 게임 정보 저장, 아이템 지급 같은 기능을 호출하면 실제 데이터가 바뀝니다.  
:::

:::caution 요청 파일에는 인증 정보를 적지 마세요
`requests` 폴더의 파일은 Git에 올라갑니다. 요청 파일에 등록된 인증 정보(인증키, 시그니처 키, 액세스 토큰 등)와 같은 값이 들어 있으면 호출하지 않고 `REQUEST_CONTAINS_CREDENTIAL` 오류로 끝납니다.  
:::

:::info 결과의 ok와 함수의 성공 여부는 다릅니다
`"ok": true`는 서버와의 통신이 성공했다는 뜻입니다.  
함수가 의도한 동작을 했는지는 `functionResult`의 내용으로 판단해 주세요.  
:::

### 호출하기 전에 확인만 하기

`--dry-run`을 추가하면 서버에 요청하지 않고 호출할 함수, 사용할 프로필, 전달할 요청 파일만 보여 줍니다. 로그인 정보가 없어도 실행할 수 있으므로 `--data`가 올바른지 확인할 때 유용합니다.  

```powershell
backend invoke MyFunction --data hello --dry-run
```

```json
{
  "command": "invoke",
  "data": {
    "dryRun": true,
    "endpoint": "https://api.thebackend.io/backend-function/base/v1",
    "function": "MyFunction",
    "manifest": "C:\\work\\MyFunction\\backend-function.json",
    "method": "POST",
    "profile": "default",
    "profileSource": "default",
    "project": "C:\\work\\MyFunction",
    "request": {
      "bytes": 29,
      "path": "requests/hello.json",
      "sha256": "7a13a3c3af9c8e9427bd669961ac2f75fa5fc92b9ed0a8f0b2763896f050d660"
    },
    "requestBytes": 82
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

## 배포된 함수 목록 보기

```powershell
backend list
```

결과의 `data.functions`에 배포된 함수 이름이 표시됩니다.  

## 함수가 배포되어 있는지 확인하기

```powershell
backend status MyFunction
```

결과의 `data.exists`가 `true`면 해당 이름의 함수가 배포되어 있습니다.  
`status`는 이름이 있는지만 확인합니다. 배포한 시각이나 버전은 뒤끝 콘솔에서 확인해 주세요.  

## 오류가 발생한 경우

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `AUTH_REQUIRED` | 테스트 계정 로그인 정보가 없습니다. `Target:` 줄의 프로필을 확인하고 `backend config`로 로그인합니다. |
| `AUTH_SESSION_EXPIRED` | 로그인 정보가 만료되었거나 다른 곳의 로그인으로 끝났습니다. `backend config`로 다시 로그인합니다. |
| `REQUEST_MISSING` | `--data`로 지정한 요청 파일이 없습니다. 이름이면 `requests` 폴더를, 경로면 현재 폴더 기준 경로를 확인합니다. |
| `REQUEST_CONTAINS_CREDENTIAL` | 요청 파일에 등록된 인증 정보와 같은 값이 들어 있습니다. 파일에서 지워 주세요. |
| `INVALID_ARGUMENT` | `--data`로 지정한 파일이 JSON 객체가 아니거나, 같은 키가 반복되거나, 크기를 넘었습니다. |
| `PROJECT_NOT_FOUND` | 프로젝트 폴더 밖에서 함수 이름을 생략했거나, 이름만 적은 `--data`를 사용했습니다. 프로젝트 폴더에서 실행하거나 `--project`를 지정합니다. |
| `CONFIRMATION_REQUIRED` | 확인 질문에 답할 수 없는 환경입니다(예: `--data -`로 입력을 읽는 경우). 자동화 스크립트에서는 `--yes`를 함께 입력합니다. |
| `CANCELLED` | 확인 질문에 `y`(또는 `yes`) 외의 값을 입력해 취소되었습니다. |
| `REMOTE_HTTP` | 서버가 요청을 거부했습니다. 함수 이름이 맞는지, 함수가 배포되어 있는지 확인하고 `error.details`의 응답 내용도 확인합니다. 응답이 `ResourceConflictException`(`... state: Pending`)이면 배포 직후 함수를 준비하는 중이므로 잠시 뒤 다시 호출합니다. |
| `REMOTE_TIMEOUT` | 응답 시간이 초과되었습니다. 함수는 서버에서 실행되었을 수 있으므로, 다시 호출하기 전에 데이터가 바뀌었는지 확인합니다. |
| `REMOTE_TRANSPORT` | 네트워크 연결에 실패했습니다. 인터넷 연결과 방화벽 설정을 확인합니다. 함수가 서버에서 실행되었는지 알 수 없으므로, 다시 호출하기 전에 데이터가 바뀌었는지 확인합니다. |
