---
sidebar_label: 번역
description: "번역"
---

# 번역

번역 기능을 통해 유저들이 서로 다른 언어를 사용하더라도 원활하게 소통할 수 있도록 해줍니다. 번역 기능은 자동 번역과 온디맨드 번역 두 가지 방법으로 구현할 수 있습니다. 자동 번역은 서버에서 자동으로 번역한 결과를 채널에 전송하며, 온디맨드 번역은 이미 받은 채팅 메세지에 대해서 유저가 원할 때 번역을 요청하는 방법입니다.

## 자동 번역

채팅 메시지를 자동으로 번역합니다. 유저가 메시지를 전송했을 때 채팅 서버가 (같은 채널에 속한 인원 기준) 각 언어별로 번역한 메시지를 채널에 뿌려줍니다. 이를 통해 실시간으로 번역문을 받아볼 수 있습니다. 자동 번역에서 허용 및 제외할 언어를 콘솔에서 설정하여 요금을 조정할 수 있습니다.

활성화 여부 및 언어 목록을 콘솔에서 설정하세요.

## 온디맨드 번역

이미 받은 채팅 메세지와 번역 도착 언어를 서버로 요청하면 번역문을 받을 수 있습니다. 번역된 채팅은 요청자에게만 전송됩니다.  
`languages` 매개변수에 도착어 목록을 넣어서 여러 언어로 번역 요청이 가능합니다.

ISO 639-1 language code 형식을 따라야 하며, 국가 코드를 포함할 수 있습니다. ([language code list](https://www.localeplanet.com/dotnet/index.html))

아래는 온디맨드 번역을 사용 할 수 있는 콜백 및 호출 함수 입니다.

### 콜백 함수

```csharp
// 번역된 채팅 메세지를 확인할 수 있는 콜백 함수입니다.
// key: 요청한 번역 도착 언어 코드, value: 해당 언어로 번역된 메세지
public void OnTranslateMessage(Dictionary<string, MessageInfo> messages) { }
```

:::caution 1.4.4 변경 사항
1.4.4부터 `OnTranslateMessage`의 매개변수가 `List<MessageInfo>`에서 `Dictionary<string, MessageInfo>`로 변경되었습니다. 이전 형식으로 구현한 코드는 컴파일 에러가 발생하므로 [마이그레이션 가이드](/sdk-docs/chat/install/migration-guide)를 참고하여 수정하세요.
:::

번역 결과는 다음 기준으로 전달됩니다.

- 키는 `SendTranslateChatMessage`의 `languages`에 넣은 언어 코드 그대로입니다.
- 번역에 실패한 언어는 결과에 포함되지 않습니다. 응답에 번역 결과가 하나도 없으면 빈 `Dictionary`가 전달됩니다.
- 번역 서비스를 사용할 수 없거나 요청 값이 잘못된 경우에는 `OnTranslateMessage` 대신 `OnError` 콜백으로 `DISABLED_SERVICE`, `INVALID_PARAMETER` 등의 에러가 전달됩니다.
- 번역 결과의 `Language`는 번역된 언어가 아니라 원문을 작성한 유저의 언어입니다. 번역된 언어는 키로 확인하세요.
- 번역 결과의 `Index`, `Tag`는 원본 메세지와 같으므로 원본 메세지를 찾을 때 사용할 수 있습니다.
- `Dictionary`의 순회 순서는 요청한 언어 순서와 다를 수 있습니다.

```csharp
public void OnTranslateMessage(Dictionary<string, MessageInfo> messages)
{
    if (messages.Count == 0)
    {
        Debug.Log("번역 결과가 없습니다.");
        return;
    }

    foreach (var pair in messages)
    {
        // pair.Key: 번역 도착 언어 코드 (예: en), pair.Value.Message: 번역된 메세지
        Debug.Log($"[{pair.Key}] {pair.Value.GamerName}: {pair.Value.Message}");
    }
}
```

:::info 키가 #0, #1 형식으로 전달되는 경우
채팅 서버가 언어별 번역 결과를 제공하지 않으면 키가 언어 코드 대신 `#0`, `#1`... 형식(`ChatClient.TranslateUnknownLanguagePrefix` + 순서)으로 전달됩니다.  
실제 언어 코드는 `#`으로 시작하지 않으므로, 키가 `#`으로 시작하면 번역된 언어를 알 수 없는 결과로 처리하세요.
:::

### 호출 함수

```csharp
// 온디맨드 번역을 요청하는 함수입니다. 번역하고 싶은 채팅 메세지와 번역 도착어 목록을 넣어서 보냅니다. (ISO 639-1 형식 ex: en, ko, ja, zh-CN)
ChatClient.SendTranslateChatMessage(MessageInfo message, List<string> languages)
```
