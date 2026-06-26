---
sidebar_label: 인자 & 리턴 클래스 정보
---

# 인자/리턴 클래스 정보

뒤끝매치 이용 시 인자 값으로 주어지거나 리턴 값으로 리턴되는 클래스 들에 대한 정보입니다.  

## ErrorInfo

에러 정보를 표현하는 클래스입니다.  

| Value           | Type                                                                                                            | Description                    |
| :-------------- | :-------------------------------------------------------------------------------------------------------------- | :----------------------------- |
| Category        | ErrorCode                                                                                                       | ErrorCode 카테고리             |
| Detail          | ErrorCode                                                                                                       | ErrorCode 상세 내용            |
| SocketErrorCode | [SocketError](https://docs.microsoft.com/ko-kr/dotnet/api/system.net.sockets.socketerror?view=netframework-3.5) | Socket 클래스에 대한 오류 코드 |
| Reason          | string                                                                                                          | 성공/실패 사유                 |

## SessionInfo

세션 정보를 표현하는 클래스입니다.  
여기서 세션은 각각의 클라이언트를 의미합니다.  

| Value     | Type      | Description                                                   |
| :-------- | :-------- | :------------------------------------------------------------ |
| SessionId | SessionId | 해당 세션의 ID                                                |
| NickName  | string    | 해당 세션 유저의 닉네임                                       |
| IsRemote  | bool      | remote 여부(나의 세션인 경우 false, 타인의 세션인 경우 true) |

## MatchInGameRoomInfo

접속해야 할 인게임 서버와 게임방 정보가 포함되어 있습니다.  

| Value                  | Type        | Description                                             |
| :--------------------- | :---------- | :------------------------------------------------------ |
| m_inGameServerEndPoint | TcpEndPoint | 접속할 인게임 서버 앤드 포인트 정보                     |
| m_inGameRoomToken      | string      | 접속할 룸의 토큰                                        |
| m_enableSandbox        | bool        | [샌드박스 매칭](/sdk-docs/backend/match/using-sandbox) 여부 |

## TcpEndPoint

서버의 주소와 포트 번호가 포함되어 있습니다.  

| Value     | Type   | Description      |
| :-------- | :----- | :--------------- |
| m_address | string | 앤드 포인트 주소 |
| m_port    | ushort | 포트 번호        |

## MatchMakingUserInfo

매칭이 성사된 유저의 기본 정보를 표현하는 클래스입니다.  

| Value       | Type      | Description             |
| :---------- | :-------- | :---------------------- |
| m_sessionId | SessionId | 해당 세션의 ID          |
| m_nickName  | string    | 해당 세션 유저의 닉네임 |

## MatchUserGameRecord

매칭이 성사된 유저의 세부 정보를 표현하는 클래스입니다.  

| Value                  | Type      | Description                                                                             |
| :--------------------- | :-------- | :-------------------------------------------------------------------------------------- |
| m_sessionId            | SessionId | 해당 세션의 ID                                                                          |
| m_nickname             | string    | 해당 세션의 닉네임(Backend-4.6.0 이상 버전에만 존재)                                   |
| m_isSuperGamer         | bool      | 해당 세션의 슈퍼 게이머(방장) 여부(Backend-4.6.0 이상 버전에만 존재)                   |
| m_teamNumber           | byte      | 팀 정보(팀의 id)                                                                        |
| m_numberOfMatches      | int       | 해당 세션의 현재 매칭된 게임 모드의 총 매치 수                                          |
| m_numberOfWin          | int       | 해당 세션의 현재 매칭된 게임 모드의 내 승리 수                                          |
| m_numberOfDraw         | int       | 해당 세션의 현재 매칭된 게임 모드의 총 무승부 수                                        |
| m_numberOfDefeats      | int       | 해당 세션의 현재 매칭된 게임 모드의 패배 수                                             |
| m_numberOfChampionship | int       | 개인전일 경우 해당 세션의 현재 매칭된 게임 모드의 1등 횟수(개인전 아닐 시 0)           |
| m_points               | int       | 포인트 모드일 경우 해당 세션의 현재 매칭된 게임 모드의 포인트(포인트 모드가 아닐 시 0) |
| m_mmr                  | int       | MMR 모드일 경우 해당 세션의 현재 매칭된 게임 모드의 MMR(MMR 모드가 아닐 시 0)          |

## MatchGameResult

매칭의 결과를 서버로 보낼 때 사용하는 클래스입니다.  

| Value     | Type               | Description   |
| :-------- | :----------------- | :------------ |
| m_winners | List < SessionId > | 승자 리스트   |
| m_losers  | List < SessionId > | 패자 리스트   |
| m_draws   | List < SessionId > | 무승부 리스트 |
