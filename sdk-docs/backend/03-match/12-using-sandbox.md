---
sidebar_label: 샌드박스 매칭 (AI매칭)
---

# 샌드박스 매칭(AI 매칭)

뒤끝 콘솔에서 매칭의 샌드박스를 활성화 한 경우 사용이 가능한 기능입니다.  

샌드박스 매칭은 뒤끝 콘솔에서 설정한 **샌드박스 매칭 전환 시간이 지날 때까지 유저들 간 매칭이 되지 않았을 경우
현재 매칭이 된 유저들끼리만 게임을 성사**시키는 모드입니다.  

- 예를 들어 1:1 게임에서 매칭 서버에 1명의 유저만 존재하는 경우 샌드박스가 활성화되어 있으면 이 1명의 유저만 매칭이 성공되어 인게임 서버로 진입할 수 있게 됩니다.  
- 개발사에서는 모자란 유저 수만큼 자유롭게 AI를 생성하여 유저들 간 AI 매칭을 진행할 수 있습니다.  
- 게임이 완료된 후에는 일반 게임과 동일하게 게임의 결과를 서버로 전송하여 게임을 완료시킬 수 있습니다.  

## 샌드박스 게임의 매칭

매칭 서버에서 매칭 진행 중 뒤끝 콘솔에서 설정한 샌드박스 매칭 전환 시간이 지날 경우 매칭 서버에서 샌드박스 매칭을 성사시킵니다.  

- 샌드박스 매칭이 성사된 경우 [OnMatchMakingResponse](/sdk-docs/backend/match/server/request-match/responses) 이벤트 핸들러에서 리턴되는 RoomInfo의 **m_enableSandBox 플래그가 true**로 수신이 됩니다.  

## 샌드박스 게임의 진행

샌드박스 매칭이 된 유저는 일반 매칭이 된 유저와 동일한 로직으로 이벤트가 처리됩니다.  

> 모자란 유저 수만큼의 AI 생성 및 AI의 로직의 처리는 모두 클라이언트에서 처리해야 합니다.  

## 샌드박스 게임 결과를 인게임 서버로 전송

샌드박스 매칭이 된 유저는 일반 매칭과 동일하게 게임 결과 처리를 할 수 있습니다.  
클라이언트에서 **자체적으로 생성한 AI의 게임 결과를 제외한 모든 유저의 게임 결과**를 인게임 서버로 보내야 게임이 정상적으로 종료됩니다.  

- 매치 모드(랜덤, 포인트, MMR) 관계없이 승패 여부와 포인트, MMR 증감이 일반 매칭과 동일하게 적용됩니다.  
- 1:1, 팀전 게임의 경우 플레이어를 패배 처리할 수 있습니다.  
- 개인전의 게임의 경우 플레이어들만 순위를 정할 수 있습니다.  

### 샌드박스 매칭의 MatchGameResult 생성 예제

```js
// 1:1. 유저가 AI 플레이어에게 패배 한 경우
MatchGameResult oneGameResult = new MatchGameResult();
oneGameResult.m_winners = new List<SessionId>();
oneGameResult.m_losers = new List<SessionId>();
oneGameResult.m_losers.add(User01.SessionId);

// 2:2팀전. 각 팀의 유저가 한 명, AI 플레이어 한 명씩 존재하는 경우
MatchGameResult teamGameResult = new MatchGameResult();
teamGameResult.m_winners = teamOneList; // teamOneList에는 유저가 한 명의 세션 ID가 존재
teamGameResult.m_losers = teamTwoList; // teamTwoList에는 유저가 한 명의 세션 ID가 존재

// 4명 개인전. 3명이 유저. 1명이 AI 플레이어인 경우
MatchGameResult meleeGameResult = new MatchGameResult();
meleeGameResult.m_winners = new List<SessionId>();

meleeGameResult.add(User02.SessionId);
meleeGameResult.add(User03.SessionId);
meleeGameResult.add(User01.SessionId);

```
