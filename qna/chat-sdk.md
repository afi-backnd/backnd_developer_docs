---
description: "채팅 SDK — Q&A"
---

# 채팅 SDK — Q&A

## Q. 채팅 SDK는 어떤 기능을 제공하나요?

| 기능 | 설명 |
|---|---|
| 채널 | 채팅방 생성/참여/나가기 |
| 메시지 | 실시간 메시지 전송/수신 |
| 번역 | 메시지 자동 번역 |
| 신고 | 메시지/유저 신고 |

## Q. 채팅 SDK 설치 방법은?

최신 `.unitypackage` 파일을 다운로드하여 Unity 프로젝트에 Import합니다.
베이스 SDK와 함께 사용하는 경우 `Backend.dll` 중복 주의가 필요합니다.

자세한 내용은 [채팅 시작하기](../sdk-docs/chat/01-intro.md)를 참고하세요.

## Q. 채널에 입장하는 방법은?

```csharp
BackndChat.Channel.JoinChannel(channelId, callback =>
{
    if (callback.IsSuccess())
    {
        Debug.Log("채널 입장 성공");
    }
});
```

## Q. 메시지를 수신하려면 어떻게 해야 하나요?

이벤트 핸들러를 등록하여 실시간으로 메시지를 수신할 수 있습니다.
자세한 내용은 [메시지 문서](../sdk-docs/chat/07-message.md)를 참고하세요.

## Q. 채팅 SDK와 베이스 SDK를 함께 사용할 수 있나요?

네, 함께 사용 가능합니다. 임포트 시 `Backend.dll` 파일이 충돌하지 않도록 주의해야 합니다.
[베이스 SDK와 함께 사용하기](../sdk-docs/chat/02-install/) 문서를 참고하세요.

## Q. Unreal Engine에서도 채팅 기능을 사용할 수 있나요?

네, Unreal Engine 전용 채팅 SDK가 별도로 제공됩니다. [언리얼 채팅 문서](../sdk-docs/unreal-chat/)를 참고하세요.
