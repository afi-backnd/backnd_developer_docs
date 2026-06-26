---
sidebar_label: 게임방 폐기
---

# 게임방 폐기

매칭 성사 후 유저들이 1분 안에 모두 게임방에 접속하지 않으면 해당 게임방은 폐기됩니다.  
폐기되었을 때 이미 게임방에 접속해 있던 유저들에게는 아래 이벤트 핸들러가 모두 호출됩니다.  

[게임 결과 이벤트 핸들러](/sdk-docs/backend/match/ingame-server/result/match-result)  
**게임 시작 실패(룸 생성 후 모든 유저가 게임에 접속하지 않은 경우)**  
ErrInfo : Match_InGame_Timeout  
Reason : Some gamers are not connected.(0)  

[인게임 서버 접속 종료 이벤트 핸들러](/sdk-docs/backend/match/ingame-server/leave-game-server/leave-server)  
**게임방이 폐기되고, 서버에서 접속을 끊었을 경우**  
ErrInfo : ErrorCode.Success
