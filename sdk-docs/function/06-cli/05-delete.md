---
sidebar_label: 삭제
description: "삭제"
---

# 삭제

`backend delete`는 서버에 배포된 함수를 삭제합니다.  
삭제한 함수는 되돌릴 수 없습니다. 클라이언트에서 해당 함수를 더 이상 호출하지 않는지 먼저 확인해 주세요.  
삭제하려면 `backend config`로 [뒤끝펑션 인증키를 등록](/sdk-docs/function/cli/setting)해야 합니다.  

## 삭제하기

삭제할 함수 이름을 입력합니다.  

```powershell
backend delete MyFunction
```

먼저 어느 인증 정보(프로필)를 사용하는지 표시되고, 이어서 삭제할 함수와 프로필을 다시 안내한 뒤 확인 질문이 표시됩니다. 프로필을 확인한 뒤 `y`를 입력합니다.  

```text
Target: profile 'dev' (authProfile in C:\work\MyFunction\backend-function.json) -> https://function.uploader.thebackend.io/function

Delete function 'MyFunction' with profile 'dev'
  Profile source: authProfile in C:\work\MyFunction\backend-function.json
  Other profiles: live
  Change the project's profile with "backend config use <profile>", or pass --profile <profile> for one command.
Delete remote function 'MyFunction' with profile 'dev'? [y/N]:
```

결과에 `"ok": true`가 표시되면 삭제가 완료된 것입니다.  
프로젝트 폴더에서 실행할 때는 함수 이름을 생략할 수 있으며, 이때는 `backend-function.json`의 `functionName`을 삭제합니다.  

:::caution 사용하는 프로필을 확인하세요
프로필은 `--profile`, 프로젝트의 `authProfile`(`backend config use`로 연결한 프로필), `default` 순서로 정해집니다.  
프로젝트 폴더 안에서 실행하면 함수 이름을 입력해도(`backend delete MyFunction`) 그 프로젝트의 `authProfile`을 사용합니다. 프로젝트 폴더가 아닌 곳에서 실행하면 `default`를 사용하므로, 다른 프로필의 함수를 삭제하려면 `--profile`을 함께 입력해 주세요.  
확인 질문의 `with profile '...'`이 삭제하려는 뒤끝 프로젝트의 프로필인지 확인해 주세요. 자세한 내용은 [인증 정보 등록](/sdk-docs/function/cli/setting#여러-프로필-사용하기)을 참고해 주세요.  
:::

## 삭제 전에 확인만 하기

`--dry-run`을 추가하면 서버에 요청하지 않고 삭제할 함수 이름과 사용할 프로필만 보여 줍니다. 인증 정보가 없어도 실행할 수 있습니다.  

```powershell
backend delete MyFunction --dry-run
```

```json
{
  "command": "delete",
  "data": {
    "dryRun": true,
    "endpoint": "https://function.uploader.thebackend.io/function",
    "function": "MyFunction",
    "manifest": "C:\\work\\MyFunction\\backend-function.json",
    "method": "DELETE",
    "profile": "default",
    "profileSource": "default",
    "project": "C:\\work\\MyFunction"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

`profile`이 사용할 프로필이고, `profileSource`가 그 프로필이 정해진 곳(`flag`는 `--profile`, `project`는 프로젝트의 `authProfile`, `default`는 기본값)입니다. `--project`로 프로젝트를 직접 지정하면 `authProfile`이 없어도 `project`로 표시됩니다.  

## 삭제에 실패한 경우

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `AUTH_REQUIRED` | 인증키가 등록되지 않았습니다. `Target:` 줄의 프로필을 확인하고 [인증 정보 등록](/sdk-docs/function/cli/setting)을 진행합니다. |
| `PROJECT_NOT_FOUND` | 프로젝트 폴더 밖에서 함수 이름 없이 실행했습니다. 함수 이름을 입력하거나, 프로젝트 폴더로 이동하거나, `--project`로 프로젝트를 지정합니다. |
| `CONFIRMATION_REQUIRED` | 확인 질문에 답할 수 없는 환경입니다. 자동화 스크립트에서는 `--yes`를 함께 입력합니다. |
| `CANCELLED` | 확인 질문에 `y`(또는 `yes`) 외의 값을 입력해 취소되었습니다. |
| `REMOTE_HTTP` | 서버가 요청을 거부했습니다. `error.details`의 응답 내용에서 함수 이름과 인증키를 확인합니다. |
| `REMOTE_TIMEOUT` | 응답 시간이 초과되었습니다. 서버에서는 삭제가 완료되었을 수 있으므로, 다시 삭제하기 전에 뒤끝 콘솔에서 함수를 확인합니다. |
| `REMOTE_TRANSPORT` | 네트워크 연결에 실패했습니다. 인터넷 연결과 방화벽 설정을 확인합니다. 삭제가 완료되었는지 알 수 없으므로 뒤끝 콘솔에서 함수를 확인합니다. |
