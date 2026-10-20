---
sidebar_label: 마이그레이션 가이드
description: "마이그레이션 가이드"
---

# 마이그레이션 가이드

이 문서는 채팅 SDK를 업그레이드할 때 기존 코드에서 수정해야 하는 내용을 버전별로 안내합니다.

## 1.4.1 이하에서 1.4.4 이상으로 업그레이드

### 번역 콜백 매개변수 변경

`IChatClientListener.OnTranslateMessage`의 매개변수가 `List<MessageInfo>`에서 `Dictionary<string, MessageInfo>`로 변경되었습니다.  
기존 형식으로 구현한 스크립트는 인터페이스를 구현하지 않은 것으로 처리되어 컴파일 에러(CS0535)가 발생합니다.

```diff
- public void OnTranslateMessage(List<MessageInfo> messages)
+ public void OnTranslateMessage(Dictionary<string, MessageInfo> messages)
```

`List`를 순회하던 코드는 `Dictionary`를 순회하도록 변경하세요. 키는 요청한 번역 도착 언어 코드입니다. (언어별 번역 결과를 제공하지 않는 채팅 서버에서는 `#0`, `#1`... 형식) 자세한 내용은 [번역](/sdk-docs/chat/translate) 문서를 참고하세요.

```diff
- foreach (var messageInfo in messages)
+ foreach (var pair in messages)
  {
-     Debug.Log(messageInfo.Message);
+     // pair.Key: 번역 도착 언어 코드, pair.Value: 번역된 메세지
+     Debug.Log($"[{pair.Key}] {pair.Value.Message}");
  }
```

## 1.3.x 이하에서 1.4.0 이상으로 업그레이드

### 차단 목록 반환 형식 변경

`ChatClient.GetBlockGamers()`의 반환 형식이 `List<string>`에서 `List<BlockPlayerInfo>`로 변경되었습니다. 차단한 유저의 닉네임은 `GamerName`, 차단 시간은 `BlockTime`으로 확인합니다.

```diff
- List<string> blockedPlayers = ChatClient.GetBlockGamers();
+ List<BlockPlayerInfo> blockedPlayers = ChatClient.GetBlockGamers();

- foreach (var gamerName in blockedPlayers)
+ foreach (var blockedPlayer in blockedPlayers)
  {
-     Debug.Log(gamerName);
+     Debug.Log($"{blockedPlayer.GamerName} ({blockedPlayer.BlockTime})");
  }
```

자세한 내용은 [유저](/sdk-docs/chat/user) 문서를 참고하세요.

## 1.1.0 이하에서 1.2.1 이상으로 업그레이드

**1.1.0 이하**에서 **1.2.1 이상**으로 업그레이드하는 경우, 반드시 아래 마이그레이션 절차를 따라야 합니다.

### 폴더 정리 및 재설정

1. 기존 SDK가 **1.1.0 이하** 버전이었다면, `Assets > TheBackend > BackendChat` 폴더를 삭제하세요.
2. SDK 재설치 후, Unity 인스펙터에서 `Chat UUID`를 다시 등록해 주세요.
  (Chat UUID는 콘솔 > 채팅 > 설정 페이지에서 확인 가능)
    

### 네임스페이스 변경

**1.2.1 이상** 버전부터는 네임스페이스가 다음과 같이 변경되었습니다.
이 변경으로 인해 기존 스크립트 일부에서 컴파일 에러가 발생할 수 있으니, 전역 치환을 권장드립니다.
```diff
- using BackendChat;
+ using BackndChat;
```

### 요약 체크리스트

- [x]  폴더 삭제 (`BackendChat` 폴더)
- [x]  Chat UUID 재설정
- [x]  네임스페이스 변경
