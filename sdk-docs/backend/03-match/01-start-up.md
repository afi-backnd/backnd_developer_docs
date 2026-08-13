---
description: "시작하기"
---

# 시작하기

:::info 뒤끝매치를 이용하려면 뒤끝베이스가 필요합니다!
뒤끝 매치 기능은 뒤끝 베이스 SDK에 포함되어 있으며 <a href="https://developer.thebackend.io/sdk-docs/backend/base/start-up">이곳</a>에서 다운받으실 수 있습니다.  
  

뒤끝 매치 서버에 접속하기 위해서는 뒤끝 베이스의 회원가입/로그인을 해야 하며 닉네임이 존재해야합니다.  

매칭 서버 접속 요청 함수(JoinMatchMakingServer)를 호출하기 전에 뒤끝 베이스의 <a href="https://developer.thebackend.io/sdk-docs/backend/base/user/signup-and-login">회원가입/로그인</a>과 <a href="https://developer.thebackend.io/unity3d/guide/bmember/nicknameUpdate/">닉네임 변경(생성) 함수</a>를 호출해주세요.  
:::



## 뒤끝매치 이용하기

뒤끝매치는 아래와 같은 조건을 만족하면 정상적으로 이용하실 수 있습니다.  
뒤끝매치 이용에는 별도의 추가 요금이 청구되지 않습니다.  
* 모든 요금제에서 동일한 기능을 제공합니다.  
* 뒤끝베이스와 별개로 뒤끝매치만 사용할 수 없습니다.  
* 유저는 반드시 닉네임을 가지고 있어야 합니다.  
* 닷넷4 환경에서만 서비스를 제공합니다.  

## 레이턴시

평균적으로 50ms 이하의 레이턴시가 측정됩니다.  
이 속도는 클라이언트에서 서버로 메시지를 전송한 후,
서버에서 클라이언트로 브로드캐스팅 메시지를 보내 클라이언트에 수신되었을 때의 레이턴시를 측정한 것입니다.  

측정하는 기기, 통신환경, 통신 상황에 따라 다르게 측정될 수 있습니다.
