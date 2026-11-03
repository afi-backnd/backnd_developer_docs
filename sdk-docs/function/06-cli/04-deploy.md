---
sidebar_label: 배포
description: "배포"
---

# 배포

`backend deploy`는 프로젝트를 서버용으로 빌드한 뒤 서버에 배포합니다.  
배포하기 전에 `backend config`로 [뒤끝펑션 인증키를 등록](/sdk-docs/function/cli/setting)해 주세요. 배포에는 인증키만 있으면 됩니다.  

## 배포하기

프로젝트 폴더에서 아래 명령을 실행합니다.  

```powershell
backend deploy
```

1. 어느 인증 정보(프로필)로 배포하는지 표시합니다.  

   ```text
   Target: profile 'dev' (authProfile in C:\work\MyFunction\backend-function.json) -> https://function.uploader.thebackend.io/function/dotnet8
   ```

2. 프로젝트를 서버용으로 빌드합니다.  
3. 서버에 올리기 직전에 배포할 함수와 **사용할 프로필**을 다시 안내하고 확인 질문을 표시합니다.  

   ```text
   Deploy function 'MyFunction' with profile 'dev'
     Profile source: authProfile in C:\work\MyFunction\backend-function.json
     ZIP:            C:\work\MyFunction\.backend\publish.zip
     SHA256:         53d077a3d0a4ee9288166b66c8b3216068c021a5ab83fc9ae01a736296f39dec
     Other profiles: live
     Change the project's profile with "backend config use <profile>", or pass --profile <profile> for one command.
   Deploy function 'MyFunction' with profile 'dev'? [y/N]:
   ```

   | 줄 | 의미 |
   |---|---|
   | `with profile '...'` | 이 프로필에 등록한 뒤끝 프로젝트에 배포됩니다. |
   | `Profile source` | 프로필이 정해진 곳입니다. `authProfile in ...`은 프로젝트에 연결한 프로필, `--profile`은 이번 명령에서 지정한 프로필, `no profile is linked ...`는 연결한 프로필이 없어 `default`를 사용한다는 뜻입니다. |
   | `Other profiles` | 등록되어 있는 다른 프로필입니다. 프로필이 하나뿐이면 표시되지 않습니다. |

4. 프로필이 맞으면 `y`를 입력해 서버에 올립니다. 다르면 `n`을 입력해 취소합니다.  

결과에 `"ok": true`가 표시되면 배포가 완료된 것입니다.  
뒤끝 콘솔의 뒤끝펑션 메뉴에서 배포한 함수를 확인할 수 있습니다.  

:::info 배포되는 함수 이름
함수 이름은 `backend-function.json`의 `functionName`입니다. `backend new`로 만들었다면 프로젝트 이름과 같습니다.  
같은 이름의 함수가 이미 서버에 있으면 새로 배포한 내용으로 바뀝니다.  
:::

:::caution 배포할 뒤끝 프로젝트가 맞는지 확인하세요
확인 질문의 `with profile '...'`이 배포하려는 뒤끝 프로젝트의 프로필인지 확인해 주세요. 프로필이 다르면 다른 뒤끝 프로젝트(예: 라이브 프로젝트)에 같은 이름의 함수가 바로 배포됩니다.  
개발 프로젝트와 라이브 프로젝트를 함께 쓴다면 프로젝트에는 개발 프로필을 연결해 두고(`backend config use dev`), 라이브 배포에만 `--profile live`를 붙이는 방법을 권장합니다. 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting#개발-프로젝트와-라이브-프로젝트-나눠-쓰기)을 참고해 주세요.  
`--yes`를 사용하면 안내만 표시하고 확인 질문 없이 바로 배포합니다.  
:::

:::info 배포 직후 호출
배포 직후 몇 초 동안은 서버에서 함수를 준비하므로 `backend invoke`가 `ResourceConflictException`(`The function is currently in the following state: Pending`)으로 실패할 수 있습니다. 잠시 기다린 뒤 다시 호출해 주세요.  
:::

## 함수 이름과 설명 지정하기

다른 이름으로 배포하려면 함수 이름을 함께 입력합니다.  

```powershell
backend deploy MyFunction2
```

함수 설명을 함께 등록하려면 `--description`을 사용합니다.  
지정하지 않으면 `backend-function.json`의 `description` 값을 사용합니다.  

```powershell
backend deploy --description "출석 보상을 지급하는 함수"
```

함수 이름은 영문, 숫자, `_`, `-`로 최대 64자입니다. 한글은 사용할 수 없습니다.  

## 배포 전에 확인만 하기

`--dry-run`을 추가하면 빌드와 업로드 없이 배포할 함수 이름, 파일 위치, 사용할 프로필을 보여 줍니다. 인증 정보가 없어도 실행할 수 있습니다.  

```powershell
backend deploy --dry-run
```

```json
{
  "command": "deploy",
  "data": {
    "buildRequired": true,
    "dryRun": true,
    "endpoint": "https://function.uploader.thebackend.io/function/dotnet8/",
    "function": "MyFunction",
    "manifest": "C:\\work\\MyFunction\\backend-function.json",
    "method": "POST",
    "profile": "default",
    "profileSource": "default",
    "project": "C:\\work\\MyFunction",
    "sha256": null,
    "zip": "C:\\work\\MyFunction\\.backend/publish.zip"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

`profile`은 사용할 프로필 이름이고, `profileSource`는 그 프로필이 어디서 정해졌는지를 알려 줍니다. `flag`는 `--profile`, `project`는 프로젝트의 `authProfile`(`--project`로 프로젝트를 직접 지정하면 `authProfile`이 없어도 `project`로 표시됩니다), `default`는 기본값입니다.  

## 이미 만든 배포 파일 올리기

`backend build`로 만든 ZIP 파일을 그대로 올리려면 함수 이름과 파일 경로를 함께 입력합니다.  

```powershell
backend deploy MyFunction C:\work\MyFunction\.backend\publish.zip
```

ZIP 파일을 직접 올릴 때는 프로젝트 정보를 사용하지 않으므로 사용할 프로필을 명확히 해야 합니다.  
현재 폴더가 `authProfile`이 지정된 프로젝트 안인데 `--profile`이나 `--project`가 없으면, 프로젝트의 프로필과 `default` 중 어느 쪽인지 알 수 없어 아래 오류로 끝납니다.  

```json
"error": {
  "code": "PROFILE_AMBIGUOUS",
  "message": "This directory belongs to the project in C:\\work\\MyFunction\\backend-function.json, which uses profile 'live', but an explicit ZIP deploy would use profile 'default'. Pass --profile live (or --profile default) or --project <dir> to choose."
}
```

메시지대로 `--profile`로 프로필을 지정해 다시 실행합니다.  

```powershell
backend deploy MyFunction C:\work\MyFunction\.backend\publish.zip --profile live
```

## 배포 후 확인하기

- 배포 직후 처음 호출할 때는 함수가 활성화되는 시간 때문에 응답이 느릴 수 있습니다. [응답 시간](/sdk-docs/function/basic/respone-time) 문서를 참고해 주세요.  
- 내 PC에서 실행할 때와 서버에서 실행할 때는 실행 환경과 권한이 다릅니다. 배포한 뒤 유니티에서 실제로 호출해 결과를 확인해 주세요.  

## 배포에 실패한 경우

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `AUTH_REQUIRED` | 인증키가 등록되지 않았습니다. `Target:` 줄의 프로필을 확인하고 [인증 정보 등록](/sdk-docs/function/cli/setting)을 진행합니다. |
| `PROFILE_AMBIGUOUS` | ZIP 파일을 직접 올릴 때 사용할 프로필을 정할 수 없습니다. `--profile` 또는 `--project`를 지정합니다. |
| `INVALID_ARGUMENT` | 함수 이름이 규칙(영문, 숫자, `_`, `-`로 최대 64자)에 맞지 않거나, 직접 올린 파일이 ZIP이 아니거나, ZIP의 최상위에 `BackendFunction.dll`이 없습니다. 메시지를 확인합니다. |
| `PROJECT_NOT_FOUND` | 프로젝트 폴더 밖에서 ZIP 파일 없이 배포했습니다. 프로젝트 폴더로 이동하거나 `--project`로 지정합니다. |
| `CONFIRMATION_REQUIRED` | 확인 질문에 답할 수 없는 환경입니다. 자동화 스크립트에서는 `--yes`를 함께 입력합니다. |
| `CANCELLED` | 확인 질문에 `y`(또는 `yes`) 외의 값을 입력해 취소되었습니다. |
| `REMOTE_HTTP` | 서버가 요청을 거부했습니다. `error.details`의 응답 내용을 확인합니다. 인증키가 올바른지, 뒤끝펑션이 활성화되어 있는지 확인합니다. |
| `REMOTE_TIMEOUT` | 응답 시간이 초과되었습니다. 서버에서는 배포가 완료되었을 수 있으므로, 다시 배포하기 전에 뒤끝 콘솔에서 함수를 확인합니다. |
| `REMOTE_TRANSPORT` | 네트워크 연결에 실패했습니다. 인터넷 연결과 방화벽 설정을 확인합니다. |
| `BUILD_FAILED` | 빌드에 실패했습니다. [빌드](/sdk-docs/function/cli/build#빌드에-실패한-경우) 문서를 참고해 주세요. |
