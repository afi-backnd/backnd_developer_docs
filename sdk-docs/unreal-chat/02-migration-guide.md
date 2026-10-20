---
sidebar_label: 마이그레이션 가이드
description: "마이그레이션 가이드"
---

# 마이그레이션 가이드

이 문서는 채팅 SDK를 업그레이드할 때 기존 코드에서 수정해야 하는 내용을 버전별로 안내합니다.

## 1.3.0 이하에서 1.4.4 이상으로 업그레이드

Unreal SDK는 1.3.0 다음 버전으로 1.4.4가 배포되었습니다. 아래 내용을 모두 확인하세요.

### 번역 콜백 매개변수 변경

`IBDChatClientListener::OnTranslateMessage`의 매개변수가 `std::vector<BackndChat::MessageInfo>`에서 `std::map<std::string, BackndChat::MessageInfo>`로 변경되었습니다.  
기존 형식으로 구현한 클래스는 인터페이스 함수를 재정의하지 않은 것으로 처리되어 컴파일 에러(C3668 또는 C2259)가 발생합니다.

```diff
- virtual void OnTranslateMessage(std::vector<BackndChat::MessageInfo> messages) override;
+ virtual void OnTranslateMessage(std::map<std::string, BackndChat::MessageInfo> messages) override;
```

`std::vector`를 순회하던 코드는 `std::map`을 순회하도록 변경하세요. 키는 요청한 번역 도착 언어 코드입니다. (언어별 번역 결과를 제공하지 않는 채팅 서버에서는 `#0`, `#1`... 형식) 자세한 내용은 [번역](/sdk-docs/unreal-chat/translate) 문서를 참고하세요.

```diff
- for (const BackndChat::MessageInfo& messageInfo : messages)
+ for (const auto& pair : messages)
  {
-     UE_LOG(LogTemp, Log, TEXT("%s"), UTF8_TO_TCHAR(messageInfo.Message.c_str()));
+     // pair.first: 번역 도착 언어 코드, pair.second: 번역된 메세지
+     UE_LOG(LogTemp, Log, TEXT("[%s] %s"), UTF8_TO_TCHAR(pair.first.c_str()), UTF8_TO_TCHAR(pair.second.Message.c_str()));
  }
```

### 차단 목록 반환 형식 변경

`BackndChat::BDChatMain::GetBlockGamers()`의 반환 형식이 `std::vector<std::string>`에서 `std::vector<BackndChat::BlockPlayerInfo>`로 변경되었습니다. 차단한 유저의 닉네임은 `GamerName`, 차단 시간은 `BlockTime`으로 확인합니다.

```diff
- std::vector<std::string> blockedPlayers = BackndChat::BDChatMain::GetBlockGamers();
+ std::vector<BackndChat::BlockPlayerInfo> blockedPlayers = BackndChat::BDChatMain::GetBlockGamers();

- for (const std::string& gamerName : blockedPlayers)
+ for (const BackndChat::BlockPlayerInfo& blockedPlayer : blockedPlayers)
  {
-     UE_LOG(LogTemp, Log, TEXT("%s"), UTF8_TO_TCHAR(gamerName.c_str()));
+     UE_LOG(LogTemp, Log, TEXT("%s (%s)"), UTF8_TO_TCHAR(blockedPlayer.GamerName.c_str()), UTF8_TO_TCHAR(blockedPlayer.BlockTime.c_str()));
  }
```

자세한 내용은 [유저](/sdk-docs/unreal-chat/user) 문서를 참고하세요.

### 전역 using namespace std 제거

SDK 헤더(`BDChatDefine.h`, `IBDChatClientListener.h`)가 `using namespace std;`를 전역이 아닌 `BackndChat` 네임스페이스 안에서만 선언합니다.  
SDK 헤더를 포함하는 코드에서 `string`, `vector`, `map`을 `std::` 없이 사용했다면 컴파일 에러(C2061 등)가 발생하므로 `std::`를 붙여 주세요.

```diff
- virtual void OnChangeGamerName(string oldGamerName, string newGamerName) override;
+ virtual void OnChangeGamerName(std::string oldGamerName, std::string newGamerName) override;
```

`OnJoinChannelPlayer`, `OnLeaveChannelPlayer`, `OnUpdatePlayerInfo`의 `channelGroup`, `channelName` 매개변수도 같은 방법으로 수정합니다.
