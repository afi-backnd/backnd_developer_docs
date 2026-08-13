---
sidebar_label: "프로젝트 버전 조회"
description: "GetLatestVersion"
---

# GetLatestVersion

public BackendReturnObject **GetLatestVersion**();  
public BackendReturnObject **GetLatestVersion**(VersionInfo **versionInfo**);

## 파라미터

| Value       | Type              | Description                          |
| :---------- | :---------------- | :----------------------------------- |
| versionInfo | VersionInfo(enum) | (Optional) 버전 정보를 조회할 스토어 |

### VersionInfo

| Value           | Description        |
| :-------------- | :----------------- |
| GooglePlayStore | 구글 플레이 스토어    |
| AppStore        | 애플 앱스토어        |
| OneStore        | 원스토어            |
| Steam           | 스팀               |

## 설명

콘솔에서 등록한 버전 정보 중, 가장 최신의 정보를 받아옵니다.  
android, ios 기기에서 작동하며, editor에서는 오류를 리턴합니다.  

VersionInfo를 통해 GooglePlayStore, AppStore, OneStore, Steam을 선택할 수 있습니다.

## Example

### 동기

```cs
Backend.Utils.GetLatestVersion(); // or Backend.Utils.GetLatestVersion(VersionInfo.GooglePlayStore);

//Example(비동기 및 SendQueue에서도 동일한 로직으로 사용할 수 있습니다.)
var bro = Backend.Utils.GetLatestVersion();
if (!bro.IsSuccess())
{
    Debug.Log("버전 조회에 실패하였습니다.");
    return;
}

string version = bro.GetReturnValuetoJSON()["version"].ToString();
//최신 버전일 경우
if(version  == Application.version)
{
    return;
}

//현재 앱의 버전과 버전관리에서 설정한 버전이 맞지 않을 경우
int forceUpdate = bro.GetReturnValuetoJSON()["type"].ToString();
if(forceUpdate == "1")
{
    Debug.Log("업데이트를 하시겠습니까? y/n");
}
else if(forceUpdate == "2")
{
    Debug.Log("업데이트가 필요합니다. 스토어에서 업데이트를 진행해주시기 바랍니다");

//해당 앱의 스토어로 가게 해주는 유니티 함수
#if UNITY_ANDROID
    Application.OpenURL("market://details?id=" + Application.identifier);
#elif UNITY_IOS
    Application.OpenURL("https://itunes.apple.com/kr/app/apple-store/" + "id1461432877");
#endif
}
```

### 비동기

```cs
Backend.Utils.GetLatestVersion((callback) =>
{
    // 이후 처리
});

Backend.Utils.GetLatestVersion(VersionInfo.AppStore, (callback) =>
{
    // 이후 처리
});
```

### SendQueue

```cs
SendQueue.Enqueue(Backend.Utils.GetLatestVersion, (callback) =>
{
    // 이후 처리
});

SendQueue.Enqueue(Backend.Utils.GetLatestVersion, VersionInfo.OneStore,  (callback) =>
{
    // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**android, ios 이외의 기기에서 호출된 경우**  
errorCode : NotFoundException  
message : Not Available OS

**해당 플랫폼 버전이 등록되어 있지 않은 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : version not found, version을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    "version" : "1.0.0.1",
    "type" : 1
}
```

| type |  Description  | 권장 처리                                                                                       |
| :--: | :-----------: | :---------------------------------------------------------------------------------------------- |
|  1   | 선택 업데이트 | 업데이트를 하지 않아도 게임을 계속 이용할 수 있도록 안내 후 진행 여부를 유저가 선택하게 합니다. |
|  2   | 강제 업데이트 | 업데이트를 완료하기 전까지 게임 진행을 차단하고 스토어로 이동하도록 안내합니다.                 |
