---
sidebar_label: "게임 시작 이벤트"
description: "OnMatchInGameStart"
---

# OnMatchInGameStart

public MatchInGameStartEventHandler **OnMatchInGameStart**;

## 설명

게임방에 모든 유저가 접속하여도 바로 게임을 진행할 수 없습니다.  
모든 유저가 게임방에 접속한 이후 **[콘솔에서 설정한 매치 시작 대기시간](/guide/console-guide/backnd-match/basic)**이 지난 이후에 모든 유저에게 게임 시작 이벤트가 호출됩니다.  
게임 시작 이벤트가 호출된 후 게임을 진행할 수 있습니다.  

> 이벤트가 호출되기 위해서는 반드시 [메시지 송수신 함수](/sdk-docs/backend/match/pingpong)가 호출되어야 합니다.  

### 게임 시작

뒤끝 매치에서 발생시키는 게임 시작 이벤트는 모든 유저의 데이터를 브로드캐스팅 할 준비가 되었다는 이벤트입니다.  
게임 시작 이벤트가 호출된 후 게임에서 필요한 데이터 동기화, 유저들 간 로딩 상황 공유 등 다양한 데이터 설정을 진행할 수 있습니다.  
위 작업이 끝난 뒤 실제 게임 시작 메시지는 클라이언트 단에서 별도로 제작하는 것을 추천드립니다.  

## Example

```js
Backend.Match.OnMatchInGameStart = () => {
  // TODO
};
```
