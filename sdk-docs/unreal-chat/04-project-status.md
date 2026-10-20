---
sidebar_label: 프로젝트 상태
description: "프로젝트 상태"
---

# 프로젝트 상태

뒤끝 콘솔에서 설정한 프로젝트 상태(온라인, 오프라인, 점검)를 Unreal 프로젝트에서 조회하는 방법을 설명합니다.

:::info
Unreal SDK에는 프로젝트 상태를 조회하는 함수가 없습니다. Unreal Engine의 HTTP 모듈로 뒤끝 API를 직접 호출하여 조회합니다.  
뒤끝 SDK(Unity)의 [프로젝트 상태 조회](/sdk-docs/backend/base/sdk-utils/get-project-status)와 같은 API를 사용합니다.
:::

## 설명

프로젝트 상태는 뒤끝 콘솔의 [프로젝트](/guide/console-guide/server-setting/project) 메뉴에서 설정합니다.  
게임을 시작할 때 프로젝트 상태를 조회하여 점검 안내를 표시하거나 접속을 제한하는 데 사용할 수 있습니다.

:::caution 채팅 서버는 프로젝트 상태를 확인하지 않습니다
프로젝트 상태를 점검으로 설정해도 채팅 서버 접속은 차단되지 않습니다.  
점검 중에 채팅을 막으려면 조회한 상태에 따라 게임에서 `BackndChat::BDChatMain::Initialize`를 호출하지 않도록 처리해 주세요.
:::

### Client App Id 확인

상태 조회에는 Chat UUID가 아닌 **Client App Id**가 필요합니다.  
뒤끝 콘솔의 [인증정보](/guide/console-guide/server-setting/authenciation)에서 Client App Id를 복사하세요. Signature Key는 필요하지 않습니다.

Client App Id는 `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxxxx` 형식입니다.

### 모듈 추가

HTTP 요청과 JSON 파싱에 필요한 `HTTP`, `Json` 모듈을 게임 모듈의 `Build.cs`에 추가합니다.

```csharp
PrivateDependencyModuleNames.AddRange(new string[] { "HTTP", "Json" });
```

## API

#### HTTP request

```js
GET https://api.thebackend.io/data/setting/project
```

#### Headers

| Header | Description |
| :----- | :---------- |
| client_app_id | 뒤끝 콘솔 인증정보의 Client App Id |
| os_version | 기기의 OS 버전 |
| device | 기기 모델명 |

세 헤더는 모두 필수이며, 헤더가 없거나 빈 값이면 `404`를 반환합니다.  
`os_version`과 `device`는 플랫폼에 따라 빈 값이 나올 수 있으므로 아래 예제처럼 기본값을 지정해 주세요.

#### Responses

```json title="Status: 200 OK"
{
    "serverStatus": 0
}
```

| serverStatus | Description |
| :----------: | :---------: |
|      0       |   온라인    |
|      1       |  오프라인   |
|      2       |    점검     |

요청 헤더가 없거나 값이 잘못되면 `404`와 함께 원인을 반환합니다.

| 원인 | message |
| :--- | :------ |
| Client App Id가 잘못된 경우 | game not found |
| `client_app_id` 헤더가 없거나 빈 값인 경우 | clientAppId not found |
| `os_version` 헤더가 없거나 빈 값인 경우 | osVersion not found |
| `device` 헤더가 없거나 빈 값인 경우 | device not found |

```json title="Status: 404 Not Found"
{
    "errorCode": "NotFoundException",
    "message": "game not found, game을(를) 찾을 수 없습니다",
    "statusCode": 404
}
```

## Example

Unreal Engine 5.3과 4.27에서 같은 코드를 사용할 수 있습니다.  
요청 완료 콜백은 게임 스레드에서 호출되므로 콜백 안에서 UI를 바로 변경할 수 있습니다.

### 조회 함수

```cpp
#include "HttpModule.h"
#include "Interfaces/IHttpRequest.h"
#include "Interfaces/IHttpResponse.h"
#include "Dom/JsonObject.h"
#include "Serialization/JsonReader.h"
#include "Serialization/JsonSerializer.h"

// onComplete(serverStatus): 0 온라인, 1 오프라인, 2 점검, -1 조회 실패
static void RequestProjectStatus(const FString& clientAppId, TFunction<void(int32)> onComplete)
{
    FString osVersion = FPlatformMisc::GetOSVersion();
    FString device = FPlatformMisc::GetDeviceMakeAndModel();

    // 플랫폼에 따라 빈 값이 나올 수 있고, 빈 값이면 서버가 404를 반환합니다.
    if (osVersion.IsEmpty())
    {
        osVersion = TEXT("UnknownOS");
    }

    if (device.IsEmpty())
    {
        device = TEXT("UnknownDevice");
    }

    auto request = FHttpModule::Get().CreateRequest();
    request->SetURL(TEXT("https://api.thebackend.io/data/setting/project"));
    request->SetVerb(TEXT("GET"));
    request->SetHeader(TEXT("client_app_id"), clientAppId);
    request->SetHeader(TEXT("os_version"), osVersion);
    request->SetHeader(TEXT("device"), device);

    request->OnProcessRequestComplete().BindLambda([onComplete = MoveTemp(onComplete)](FHttpRequestPtr, FHttpResponsePtr response, bool connected)
    {
        int32 serverStatus = -1;

        if (connected && response.IsValid() && response->GetResponseCode() == 200)
        {
            TSharedPtr<FJsonObject> json;
            const TSharedRef<TJsonReader<>> reader = TJsonReaderFactory<>::Create(response->GetContentAsString());

            if (FJsonSerializer::Deserialize(reader, json) && json.IsValid())
            {
                json->TryGetNumberField(TEXT("serverStatus"), serverStatus);
            }
        }
        else if (response.IsValid())
        {
            UE_LOG(LogTemp, Warning, TEXT("ProjectStatus failed: %d %s"), response->GetResponseCode(), *response->GetContentAsString());
        }

        onComplete(serverStatus);
    });

    request->ProcessRequest();
}
```

### 호출

```cpp
// 뒤끝 콘솔 인증정보의 Client App Id로 변경해 주세요.
static const TCHAR* const PROJECT_CLIENT_APP_ID = TEXT("xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxxxx");

void AMyHUD::BeginPlay()
{
    Super::BeginPlay();

    // 응답이 오기 전에 액터가 제거될 수 있으므로 약한 참조로 캡처합니다.
    TWeakObjectPtr<AMyHUD> weakThis(this);
    RequestProjectStatus(PROJECT_CLIENT_APP_ID, [weakThis](int32 serverStatus)
    {
        if (!weakThis.IsValid())
        {
            return;
        }

        switch (serverStatus)
        {
        case 0:
            // 온라인
            break;

        case 1:
            // 오프라인
            break;

        case 2:
            // 점검: 점검 안내를 표시하고 채팅 접속을 진행하지 않습니다.
            break;

        default:
            // 조회 실패: 재시도하거나 게임 정책에 맞게 처리합니다.
            break;
        }
    });
}
```

## 예제 게임에서 확인하기

[예제 게임](/sdk-docs/unreal-chat/example-game)은 시작할 때 프로젝트 상태를 조회하여 채팅 목록에 `SYSTEM : Project status : Online (Client App Id)` 형식으로 표시합니다. 조회에 실패하면 `Request failed`를 빨간색으로 표시합니다.

예제 게임의 Client App Id는 비어 있어, 그대로 실행하면 상태를 조회하지 않고 `Client App ID is not set`을 표시합니다. 다음 중 한 가지 방법으로 Client App Id를 지정해 주세요.

- `Source/SampleClient/SampleClientHUD.cpp`의 `PROJECT_CLIENT_APP_ID`에 Client App Id를 입력합니다.
- 패키징한 실행 파일을 실행할 때 `-ClientAppId=xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxxxxxx` 인자를 추가합니다.

:::info
- 프로젝트 상태는 예제 게임을 시작할 때 한 번만 조회합니다. 콘솔에서 상태를 변경한 뒤에는 예제 게임을 다시 실행해 주세요.
- 채널에 입장하면 채팅 목록이 초기화되면서 상태 메시지도 사라집니다.
:::
