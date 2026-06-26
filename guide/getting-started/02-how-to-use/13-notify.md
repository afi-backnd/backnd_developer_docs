import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 실시간 알림

실시간 알림은 게임 내에서 실시간으로 이벤트를 발생시켜 유저가 바로 확인하도록 유도할 수 있는 기능입니다.  

실시간 알림은 아래 상황에서 이벤트가 호출됩니다.  

<ConsoleLinkButton text="실시간 알림 설정 바로가기" menu="settingSocial" feature="소셜 설정" title="실시간 알림" />

관련 콘솔 가이드 : [서버관리 - 소셜 - 실시간 알림](/guide/console-guide/server-setting/social)  
관련 SDK 개발자 문서 : [실시간 알림 - 실시간 알림 서버 연결하기](/sdk-docs/backend/base/notify/connect-to-notify-server)


## 유저 이벤트  

  * 유저 기능  
    * 특정 유저의 접속 여부 확인 알림

  - 친구 기능  
    * 친구 요청 도착 알림
    * 다른 유저에게 보낸 친구 요청 수락 알림
    * 다른 유저에게 보낸 친구 요청 거절 알림
    * 친구 유저의 게임 접속 알림
    * 친구 유저의 게임 종료 알림

  * 길드 기능  
    * 길드 운영진 대상 신규 가입 신청 알림
    * 길드 가입 신청 수락 알림
    * 길드 가입 신청 거절 알림
    * 길드 추방 알림

  - 쪽지 기능  
    * 새 쪽지 도착 알림

  * 유저 우편 기능  
    * 유저 우편 도착 알림

## 콘솔 이벤트

  * 공지시항 기능  
    * 공지사항 등록 알림

  - 이벤트 기능  
    * 이벤트 등록 알림

  * 프로젝트 상태  
    * 프로젝트 온/오프/점검 전환 알림

  - 관리자 우편 기능  
    * 관리자 우편 도착 알림
