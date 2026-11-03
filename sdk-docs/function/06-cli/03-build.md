---
sidebar_label: 빌드
description: "빌드"
---

# 빌드

`backend build`는 서버에 배포할 파일을 만듭니다.  
`backend deploy`가 배포 전에 빌드를 함께 진행하므로, 배포만 할 때는 따로 실행하지 않아도 됩니다.  
배포하기 전에 컴파일 오류가 없는지 확인할 때 사용합니다.  

## 배포용 빌드

프로젝트 폴더에서 아래 명령을 실행합니다.  

```powershell
backend build
```

결과에 `"ok": true`가 표시되면 성공입니다.  

```json
{
  "command": "build",
  "data": {
    "assembly": "C:\\work\\MyFunction\\.backend\\publish\\BackendFunction.dll",
    "configuration": "Release",
    "functionName": "MyFunction",
    "outputDirectory": "C:\\work\\MyFunction\\.backend\\publish",
    "project": "C:\\work\\MyFunction",
    "runtime": "linux-x64",
    "sha256": "53d077a3d0a4ee9288166b66c8b3216068c021a5ab83fc9ae01a736296f39dec",
    "zip": "C:\\work\\MyFunction\\.backend\\publish.zip"
  },
  "error": null,
  "ok": true,
  "schemaVersion": 1
}
```

| 항목 | 설명 |
|---|---|
| `zip` | 서버에 올라갈 파일입니다. 프로젝트 폴더의 `.backend/publish.zip`에 만들어집니다. |
| `sha256` | 배포 파일의 해시 값입니다. 배포할 때 같은 값이 확인 질문에 표시됩니다. |
| `runtime` | 서버 실행 환경입니다. Windows나 Mac에서 빌드해도 항상 서버용(`linux-x64`)으로 빌드합니다. |

빌드할 때마다 이전 빌드 결과를 지우고 새로 만듭니다.  

:::caution Visual Studio와 Visual Studio Code의 빌드 결과는 배포에 사용할 수 없습니다
Visual Studio나 Visual Studio Code에서 빌드한 결과는 내 PC 디버깅용입니다.  
서버에 올릴 파일은 반드시 `backend build` 또는 `backend deploy`로 만들어 주세요.  
:::

## 디버깅용 빌드

내 PC에서 실행할 디버깅용 파일만 만들려면 `--debug`를 추가합니다.  
결과는 `.backend/debug` 폴더에 만들어지며 배포 파일은 만들지 않습니다.  

```powershell
backend build --debug
```

## 빌드에 실패한 경우

컴파일 오류가 있으면 아래와 같이 `BUILD_FAILED` 오류로 끝납니다.  

```json
{
  "command": "build",
  "data": null,
  "error": {
    "code": "BUILD_FAILED",
    "details": {
      "configuration": "Release",
      "exitCode": 1,
      "project": "C:\\work\\MyFunction\\BackendFunction.csproj"
    },
    "message": "dotnet failed; inspect the build diagnostics on stderr"
  },
  "ok": false,
  "schemaVersion": 1
}
```

JSON 위에 표시된 컴파일 메시지에서 `error CS`가 들어 있는 줄을 찾아 파일 이름과 줄 번호를 확인합니다.  

```text
C:\work\MyFunction\Function.cs(1,7): error CS0246: 'BackendAPI' 형식 또는 네임스페이스 이름을 찾을 수 없습니다. using 지시문 또는 어셈블리 참조가 있는지 확인하세요. [C:\work\MyFunction\BackendFunction.csproj]
```

| 오류 코드 | 원인과 해결 방법 |
|---|---|
| `BUILD_FAILED` | 코드에 컴파일 오류가 있습니다. 위와 같이 오류 메시지를 확인해 고칩니다. |
| `PROJECT_NOT_FOUND` | 현재 폴더와 상위 폴더에 `backend-function.json`이 없습니다. 프로젝트 폴더로 이동하거나 `--project`로 지정합니다. 1.0.2 이하 버전에서 만든 프로젝트라면 [기존 버전에서 옮기기](/sdk-docs/function/migrate-from-old-version)를 참고해 `backend migrate`를 먼저 실행합니다. |
| `MIGRATION_REQUIRED` | `BackendFunction.csproj`에 1.0.2 이하 버전의 디버거·ZIP 생성 설정이 남아 있습니다. `backend migrate`로 바뀔 내용을 확인한 뒤 `backend migrate --yes`로 적용합니다. |
| `INVALID_PROJECT` | `BackendFunction.csproj`의 `TargetFramework`가 `net8.0` 하나가 아니거나, `AssemblyName` 또는 `RootNamespace`가 `BackendFunction`이 아닙니다. 오류 메시지를 확인해 고칩니다. |
| `BUSY` | 같은 프로젝트에서 다른 `backend` 명령이 실행 중입니다. 끝난 뒤 다시 실행합니다. |
