---
sidebar_label: 유저
description: "유저"
---

# 유저

유저 정보를 관리 하실 수 있는 기능을 제공 합니다. 아바타, 닉네임, 언어 정보, 메타 정보에 대해서 수정이 가능하며, 유저간의 차단 기능을 제공하고 있습니다.

## 설명

유저 정보에 대한 콜백 및 호출 함수, 정보 클래스입니다.

### 유저 정보

#### 제약 사항

##### 유저 정보 길이 제한

- **닉네임**: 최대 45바이트까지 가능합니다.
- **아바타**: 최대 45바이트까지 가능합니다.
- **메타데이터**: 총 500바이트까지 저장할 수 있습니다.

##### 언어 코드 형식

- **언어 코드**는 ISO 639-1 언어 코드 형식을 따라야 합니다.
- 선택적으로 국가 코드를 포함할 수 있습니다.

자세한 언어 코드 목록은 [이 링크](https://www.localeplanet.com/dotnet/index.html)를 참고하세요.

> **참고**: 한글, 이모지 등 다국어 문자의 경우 1자가 여러 바이트를 차지할 수 있으므로, 바이트 기준으로 길이를 계산해야 합니다.

#### 닉네임 변경 순서
- 커스텀 인증 서버가 응답하는 `nickname`을 먼저 변경합니다.
- 이후 채팅의 `BackndChat::BDChatMain::UpdateNickname` 함수를 호출하여 닉네임을 변경합니다.

채팅 서버는 커스텀 인증 서버가 응답하는 닉네임과 변경할 닉네임이 같은지 확인합니다. 서로 다르면 `OnError` 콜백으로 `FAILED_UPDATE_GAMER_NAME` 에러가 전달됩니다.

#### 콜백 함수

```cpp
// 채널에 접속 되어 있는 유저들 중 특정 유저의 정보가 변경 될 경우 오는 콜백 함수 입니다. 변경 된 유저 정보를 넘겨 줍니다.
void OnUpdatePlayerInfo(std::string channelGroup, std::string channelName, uint64_t channelNumber, BackndChat::PlayerInfo player) { }

// 채팅 서버에 접속 되어 있는 유저들 중 특정 유저의 닉네임이 변경 시 오는 콜백 함수 입니다. 변경 전, 후의 유저 닉네임을 넘겨 줍니다.
void OnChangeGamerName(std::string oldGamerName, std::string newGamerName) { }
```

#### 호출 함수

```cpp
// 자신의 아바타 정보를 변경 하는 함수 입니다. 변경 할 아바타 정보를 넣어서 보내 줍니다.
BackndChat::BDChatMain::UpdateAvatar(std::string avatar);

// 자신의 닉네임을 변경 하는 함수 입니다. 변경 할 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::UpdateNickname(std::string nickname);

// 자신의 언어 정보를 변경 하는 함수 입니다. 변경 할 언어 정보를 넣어서 보내 줍니다. (ISO 639-1 형식 ex: en, ko, ja, zh-CN)
BackndChat::BDChatMain::UpdateLanguage(std::string language);

// 자신의 메타 정보를 변경 하는 함수 입니다. 모든 메타 정보를 넣어서 보내 줍니다. (정보를 덮어쓰기를 합니다.)
BackndChat::BDChatMain::UpdateMetadata(std::map<std::string, std::string> metadata);
```

## 설명

유저간의 차단 기능에 대한 호출 함수 입니다.

### 유저 간 차단

#### 호출 함수

```cpp
// 현재 차단 한 유저 목록(닉네임, 차단 시간)을 얻어 옵니다.
std::vector<BackndChat::BlockPlayerInfo> blockedPlayers = BackndChat::BDChatMain::GetBlockGamers();
```

```cpp
// 유저 차단 함수 입니다. 차단 할 유저의 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendAddBlockGamer(std::string gamerName);

// 유저 차단 해제 함수 입니다. 해제 할 유저의 닉네임을 넣어서 보내 줍니다.
BackndChat::BDChatMain::SendRemoveBlockGamer(std::string gamerName);
```

:::caution 1.4.4 변경 사항
1.4.4부터 `GetBlockGamers()`의 반환 형식이 `std::vector<std::string>`에서 `std::vector<BackndChat::BlockPlayerInfo>`로 변경되었습니다. 닉네임은 `GamerName`으로 확인하세요. 자세한 내용은 [마이그레이션 가이드](/sdk-docs/unreal-chat/migration-guide)를 참고하세요.
:::

차단·해제가 완료되면 `OnSuccess` 콜백으로 `ADD_BLOCK_PLAYER`, `REMOVE_BLOCK_PLAYER`가 전달됩니다. 처리할 수 없으면 `OnError` 콜백으로 아래 에러가 전달됩니다.

| 상황 | 에러 |
| :--- | :--- |
| 유저 이름을 비워서 호출한 경우 | `INVALID_PARAMETER` |
| 이미 차단한 유저를 차단하는 경우 | `ALREADY_BLOCK_PLAYER` |
| 차단하지 않은 유저를 해제하는 경우 | `FAILED_REMOVE_BLOCK_PLAYER` |
| 커스텀 인증 사용 시 유저 간 차단 API를 설정하지 않은 경우 | `NOT_FOUND_BLOCK_URL` |

### 정보 클래스

```cpp
struct BlockPlayerInfo
{
  // 차단 된 플레이어 이름
  std::string GamerName;

  // 차단 시간 정보 (UTC 기준) yyyy-MM-dd HH:mm:ss  ex) 2020-01-01 00:00:00 (차단 시간 정보가 없으면 빈 문자열)
  std::string BlockTime;
};
```

```cpp
struct PlayerInfo
{
  // 플레이어 이름
  std::string GamerName;

  // 플레이어 아바타 이름
  std::string Avatar;

  // 플레이어 언어 정보
  std::string Language;

  // 플레이어 메타 정보
  std::map<std::string, std::string> Metadata;
};
```

### 커스텀 인증 사용 시 유저 간 차단

커스텀 인증을 사용하고 계시다면 연결하고자 하는 유저 간 차단 / 해제 엔드 포인트를 뒤끝 콘솔에서 설정하여야 합니다.

#### 커스텀 인증 사용 시 유저 간 차단 API 설정

콘솔의 채팅 > 설정 메뉴에서 유저 간 차단 / 해제 API 엔드 포인트를 설정할 수 있습니다.  

![custom auth](/img/docs/guide/chat/console/settings-customauth.png)

채팅 서버는 아래의 방식으로 HTTP 요청을 합니다.

#### 차단 API

##### HTTP request

```js
POST https://auth.mygame.com/block
```

##### Headers

```js
Content-Type: application/json; charset=utf8
token: USER_TOKEN
```

##### Request body

| Property name  | Type           | Value           | Description           |
| :------------ | :--------------- |:--------------- |:--------------- |
| source     | string                | thebackend-chat-api               | 요청한 서버 이름 (뒤끝의 채팅 서버)               |
| nickname     | string                | nickname              | 차단 할 유저 닉네임            |

```json
{
  "source": "thebackend-chat-api",
  "nickname" : "nickname"
}
```

##### Responses

형식에 맞는 응답을 반환하도록 구성하세요. 유저 정보는 string 형식의 `nickname`과 `uid`를 필수로 포함해야 합니다.
- `uid`를 통해 유저를 구분하므로, `uid`는 각 유저에게 부여된 고유 값이어야 합니다.
- `blockdate`는 차단 시간 정보(UTC)로, `yyyy-MM-ddTHH:mm:ss.fffZ` 또는 `yyyy-MM-dd HH:mm:ss` 형식으로 전달합니다. 생략하면 `BlockTime`은 빈 문자열입니다.

다음은 유저 차단 API에서 채팅 서버로 보내는 유저 정보 예시입니다.
```json title="Status: 200 OK"
{
  "nickname": "john doe",
  "uid": "1234",
  "blockdate": "2025-12-01T12:00:00.000Z"
}
```

#### 해제 API

##### HTTP request

```js
DELETE https://auth.mygame.com/block
```

##### Headers

```js
Content-Type: application/json; charset=utf8
token: USER_TOKEN
```

##### Request body

| Property name  | Type           | Value           | Description           |
| :------------ | :--------------- |:--------------- |:--------------- |
| source     | string                | thebackend-chat-api               | 요청한 서버 이름 (뒤끝의 채팅 서버)               |
| uid     | string                | uid              | 해제 할 유저 고유 값            |
| nickname     | string                | nickname              | 해제 할 유저 닉네임            |

```json
{
  "source": "thebackend-chat-api",
  "nickname": "john doe",
  "uid": "1234"
}
```

##### Responses

형식에 맞는 응답을 반환하도록 구성하세요. 유저 정보는 string 형식의 `nickname`과 `uid`를 필수로 포함해야 합니다.
- `uid`를 통해 유저를 구분하므로, `uid`는 각 유저에게 부여된 고유 값이어야 합니다.

다음은 유저 해제 API에서 채팅 서버로 보내는 유저 정보 예시입니다.
```json title="Status: 200 OK"
{
  "nickname": "john doe",
  "uid": "1234"
}
```
