---
sidebar_label: "슈퍼 게이머 변경"
description: "OnChangeSuperGamer"
---

# OnChangeSuperGamer

public MatchInGameChangeSuperGamerEventHandler **OnChangeSuperGamer**;

## 전달인자

| Value | Type                                 | Description                                       |
| :---- | :----------------------------------- | :------------------------------------------------ |
| args  | MatchInGameChangeSuperGamerEventArgs | 변경 전 슈퍼 게이머 정보 & 변경 후 슈퍼 게이머 정보 |

## MatchInGameChangeSuperGamerEventArgs

| Value              | Type                                            | Description                                      |
| :----------------- | :---------------------------------------------- | :----------------------------------------------- |
| ErrInfo            | ErrorCode                                       | 에러 정보(성공/실패 정보)                       |
| Reasion            | string                                          | 에러 상세 정보                                   |
| OldSuperUserRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 변경 전 슈퍼 게이머(접속이 끊긴 슈퍼 게이머 정보) |
| NewSuperUserRecord | [MatchUserGameRecord](/sdk-docs/backend/match/argument) | 변경 된 슈퍼 게이머(현재 슈퍼 게이머 정보)        |

## 설명

슈퍼 게이머 유저가 게임 서버와 접속이 끊어지고, 서버에서 게임방에 있는 유저 중 1명을 새로운 슈퍼 게이머로 지정하였을 때 호출되는 이벤트 핸들러 입니다.  
현재 게임방에 참여하고 있는 모든 유저에게 호출됩니다.  

슈퍼 게이머를 클라이언트 단에서 수동으로 변경하는 기능은 제공하고 있지 않습니다.  

## Example

```js
Backend.Match.OnChangeSuperGamer = (MatchInGameChangeSuperGamerEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 슈퍼 게이머가 변경된 경우 **  
ErrInfo : ErrorInfo.Success  
OldSuperUserRecord : 이전 슈퍼 게이머 정보  
NewSuperUserRecord : 서버에서 새로 지정한 슈퍼 게이머 정보
