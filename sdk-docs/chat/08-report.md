---
sidebar_label: 신고
description: "신고"
---

# 신고

채팅 메시지 신고가 가능합니다.  
콘솔 화면에서 신고 내역을 확인하여 채팅을 관리할 수 있습니다.

## 설명

채팅 메시지에 관련된 콜백 및 호출 함수, 정보 클래스입니다.  
메시지 정보에서 Index, Tag를 이용하여 정확한 신고를 할 수 있습니다.

### 키워드와 사유

키워드는 콘솔에서의 통계 기능을 적극 활용 가능한 기능 입니다. 콘솔에서 설정 하신 후에 활용 해 보시길 바랍니다.  
사유의 경우는 유저에게 직접 받으실 수 있고 콘솔에서 설정 하신 값 그대로 사용 하실 수 있습니다.

### 콜백 함수

```csharp
// 성공 콜백을 참조 해 주시길 바랍니다.
public void OnSuccess(SUCCESS_MESSAGE success, object param)
{
    switch(success)
    {
        case SUCCESS_MESSAGE.REPORT:
        {
          // 신고 접수가 완료가 되면 오는 콜백 함수 입니다.
        }
        break;
    }
}
```

### 호출 함수

```csharp
// 콘솔에서 지정한 신고 키워드와 사유를 얻어 올 수 있습니다. 현재 유저의 언어 기반으로 정보를 얻어 옵니다.
Dictionary<string, string> reasons = ChatClient.GetReportReasons()
```

```csharp
// 신고 함수 입니다. 메시지 인덱스, 메시지 태그, 신고 키워드, 신고 사유를 넣어서 보내 줍니다.
ChatClient.SendReportChatMessage(UInt64 index, string tag, string keyword, string reason)
```
