---
sidebar_label: 바이너리 데이터 수신 이벤트
---

# OnMatchRelay

public MatchRelayEventHandler **OnMatchRelay**;

## 전달인자

| Value | Type                | Description                                   |
| :---- | :------------------ | :-------------------------------------------- |
| args  | MatchRelayEventArgs | 서버에서 브로드캐스팅 한 바이너리 데이터 정보 |

## MatchRelayEventArgs

| Value          | Type        | Description                                       |
| :------------- | :---------- | :------------------------------------------------ |
| From           | SessionInfo | 데이터를 보낸 세션 정보(데이터를 보낸 유저 정보) |
| BinaryUserData | byte[]      | 보낸 데이터                                       |

## 설명

클라이언트가 SendDataToInGameRoom 함수로 서버로 보낸 메시지를 게임방에 접속한 모든 클라이언트에게 브로드캐스팅 했을 때 호출되는 이벤트입니다.  
자기 자신이 메시지를 보냈을 때도 호출됩니다.  

수신 받은 바이너리 데이터는 각 게임에 맞는 타입으로 변환하여 이용하면 됩니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

## Example

```js
Backend.Match.OnMatchRelay = (MatchRelayEventArgs args) => {
    // TODO
};
```

## ArgumentCase

** 바이너리 데이터를 수신 받았을 때**  
From : 보낸 유저 세션 정보  
BinaryUserData : 보낸 데이터
