---
title: Backend-5.15.1
date: 2024-11-14T10:00
slug: backend-5-15-1
---

:::danger 5.15.1 지원 종료
특정 상황에서 이용에 불편을 주는 에러가 발견되어 해당 버전의 지원이 종료되었습니다.  
- [뒤끝매치] GetMatchRecord 함수 사용 시, 에러 발생 이슈
- [Param] Param에서 간헐적으로 json parsing 오류 발생 이슈.

해당 이슈는 5.15.2 버전에서 수정이 되었으므로 5.15.2 버전을 사용해 주시기 바랍니다.
:::

:::info 업데이트 요약
iOS 환경에서 Select 절이 불가능한 문제를 해결했습니다.  
Param.Parse(LitJson.JsonData)가 정상적으로 작동하지 않는 문제를 해결했습니다.  
:::

<!--truncate-->

## Versions
- Backend-5.15.1.dll
- Backend-1.0.9.aar

## 5.15.1 Update

**[Fix]**
- [게임 데이터 관리] PlayerData.Get select 절 이용시 iOS 환경에서 에러가 발생하던 문제를 수정했습니다.
- [Param] Param.Add(LitJson JsonData) 호출 시, 에러가 발생하던 문제를 수정했습니다.
- [인스펙터] sha1 해시키 변환 인스펙터가 추가되었습니다.

## 5.15.0 Update
**[Updated]**
- Backend.GameLog.InsertLog 함수가 삭제되었습니다. Backend.GameLog.InsertLogV2를 사용해주세요.

**[Fix]**
- [Param] Param.Add(LitJson JsonData) 호출 시, 에러가 발생하던 문제를 수정했습니다.
- [차트 관리] Backend.CDN.Content.Get의 리턴값 중 contentJson을 호출할 경우, 무한 로딩이 되는 에러를 수정했습니다.
- [확률 관리] Backend.CDN.Probability.Get의 리턴값 중 contentJson을 호출할 경우, 무한 로딩이 되는 에러를 수정했습니다.
- [뒤끝 매치] OnChangeSuperGamer 핸들러의 응답값 MatchInGameChangeSuperGamerEventArgs에서 OldSuperUserRecord와 NewSuperUserRecord가 정상적으로 불러오지 않던 문제를 수정했습니다.(5.12.0부터 발생)
- [뒤끝 매치] OnMatchInGameAccess 핸들러의 응답값 MatchInGameSessionEventArgs에서  GameRecord가 정상적으로 불러오지 않던 문제를 수정했습니다.(5.12.0부터 발생)
- [뒤끝 매치] OnMatchMakingRoomSomeoneInvited 핸들러의 응답값 MatchMakingInvitedRoomEventArgs에서  InviteUserInfo가 정상적으로 불러오지 않던 문제를 수정했습니다.(5.12.0부터 발생)
- [뒤끝 매치] OnMatchMakingRoomJoin 핸들러의 응답값 MatchMakingGamerInfoInRoomEventArgs에서  UserInfo가 정상적으로 불러오지 않던 문제를 수정했습니다.(5.12.0부터 발생)
- [뒤끝 매치] OnMatchMakingRoomUserList 핸들러의 응답값 MatchMakingGamerInfoListInRoomEventArgs에서  UserInfo가 정상적으로 불러오지 않던 문제를 수정했습니다.(5.12.0부터 발생)



## SDK 포함 Nuget

| nuget 이름                     | 버전       | 라이센스                             |
| ----------------------------- | ---------- | ----------------------------------- |
| WebSocket4Net 0.14.1          | 0.14.1     | APACHE LICENSE, VERSION 2.0         |
| LitJson                       | 0.17.0     | The Unlicense                       |
| .NET Reactor                  | 7.9.0.0    | End-User License Agreement("EULA") |
