---
sidebar_label: 종료 처리
description: "종료 처리"
---

# 종료 처리

채팅 종료 처리하는 방법을 설명합니다.  

## 설명

채팅 종료 처리는 앱 종료 전에 호출 해 주시는 것을 권장 드리고 있습니다. 내부적으로 네트워크 상태를 감지 하여 자동으로 재연결 처리를 하고 있기 때문에 연결 상태를 따로 확인 하시거나 종료 처리를 앱 실행 중에 하실 필요가 없습니다.   
  
채팅 종료에 관련된 호출 함수와 예제 코드 입니다.

### 호출 함수

```csharp
// 채팅 서버의 접속을 종료하는 함수입니다.  
ChatClient.Dispose();
```

### Example

```csharp
// Unity3D에 있는 종료 함수에 호출 할 수 있도록 넣어 주는 것을 추천 드립니다.  
private void OnApplicationQuit()
{
    ChatClient?.Dispose();
}
```
