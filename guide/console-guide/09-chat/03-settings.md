import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 설정

요금제, 서버 버전, 메시지, 도배, 대화 내역, 부적절 표현, 신고, 기타 설정을 할 수 있습니다.

<ConsoleLinkButton text="설정 바로가기" menu="chatSetting" feature="설정" title="설정" />

### 프로젝트

![project settings](/img/docs/guide/chat/console/settings-customauth.png)
![project settings](/img/docs/guide/chat/console/settings-customauth2.png)

- 채팅 프로젝트 ID(Chat UUID)는 채팅 프로젝트를 식별하기 위한 고유 값이며, 채팅 서버를 연동할 때 사용됩니다.
- 유저 인증 방법은 2가지 중에서 선택할 수 있습니다.
    - 뒤끝 베이스의 유저 인증 서버를 사용하면, 유니티 프로젝트에서 Client App Id와 Signature Key를 입력하여 곧바로 유저 인증 기능을 사용할 수 있습니다.
    - 커스텀 인증 서버를 사용하면, 유저 관리 호출 및 유저 DB 비용이 0원으로 처리되어 관련 비용을 지불하지 않아도 됩니다. 유저 서버를 이미 구축해둔 팀에게 추천합니다. 연결하고자 하는 서버의 엔드 포인트를 입력하고, 토큰과 함께 요청을 보냈을 때 형식에 맞는 응답을 반환하도록 구성하여 커스텀 인증 서버를 연결할 수 있습니다. 자세한 사용 방법은 [커스텀 인증 문서](/sdk-docs/chat/custom-auth)를 확인하세요.

### 유저 간 차단

![ban user settings](/img/docs/guide/chat/console/settings-ban-user.png)
![ban user settings](/img/docs/guide/chat/console/settings-ban-user2.png)

- 베이스 인증을 사용하는 경우, 별도의 설정이 필요 없습니다.
- 커스텀 인증을 사용하는 경우, 유저 차단하기 및 차단 해제하기 API를 설정하면 특정 유저가 다른 유저의 채팅을 보지 않기 위해 차단 여부를 설정할 수 있습니다. 특히 게임 클라이언트에서 상대방을 특정하기 어려운 경우에 유용합니다. 예를 들어 닉네임만으로 차단할 경우, 상대방이 닉네임을 변경하면 차단이 제대로 적용되지 않을 수 있습니다. 하지만 이 API를 설정하면 변경이 불가능한 유저 ID 정보를 기반으로 서버에서 자동으로 차단을 관리합니다. 자세한 설정 방법은 [유저 간 차단 문서](/sdk-docs/chat/user)를 확인하세요.

### 요금

- 사용 중인 요금제를 확인할 수 있습니다.
- 요금제 변경하기 버튼을 클릭하여 요금제를 설정할 수 있습니다.  
요금제 변경 권한이 있는 관리자만 접근할 수 있습니다.
- 현재 적용된 요금제에 대한 사용량 현황을 확인할 수 있습니다.  
이달 CCU 최고 기록, 트래픽 사용량, 유저 관리 호출 사용량을 확인할 수 있습니다.

![pricing settings 1](/img/docs/guide/chat/console/settings-pricing.png)
![pricing settings 2](/img/docs/guide/chat/console/change-plans.png)

### 서버 버전

![server settings](/img/docs/guide/chat/console/settings-server.png)

- 현재 사용 중인 서버 버전을 확인할 수 있습니다. 해당 버전과 호환되는 가장 최신 SDK를 다운로드 받을 수 있습니다.
- 사용자가 직접 서버 버전을 변경할 수 있습니다. 적용할 수 있는 버전 목록을 확인하고, 특정 버전을 선택하여 업데이트를 진행하세요. 서버는 하위 호환성을 유지하기 때문에, 이미 배포된 빌드에서도 별도의 수정 없이 그대로 사용할 수 있습니다. 하위 호환이 되지 않는 경우 별도로 표시됩니다.

### 메시지

![messages settings](/img/docs/guide/chat/console/settings-message.png)

- 메시지 길이는 최소 2byte, 최대 1000byte까지 설정할 수 있습니다.

### 도배

![spam settings](/img/docs/guide/chat/console/settings-spam.png)

- 도배 방지 방식은 ‘도배 예방하기’와 ‘도배 시 패널티 적용하기’ 중에 선택할 수 있습니다. '도배 예방하기'는 메시지를 보내고 최소 N 초 이후에 메시지를 보낼 수 있도록 도배를 예방하는 방식이며, '도배 시 패널티 적용하기'는 N 초 안에 메시지를 N 번보다 많이 전송하면, N 초 동안 채팅을 금지하는 패널티를 주는 방식입니다.

### 대화 내역

![messages settings](/img/docs/guide/chat/console/settings-history.png)

- Pro 기능을 지원하는 요금제에서 사용할 수 있습니다.
- 채널 입장 시 최근 대화 내역 보여주기를 활성화하면 채널에 새로 입장하는 유저가 정해진 조건에 따라 과거 채팅 내역을 볼 수 있게 됩니다.
- 메시지 저장 기간은 최소 1일, 최대 90일까지 설정할 수 있습니다.

### 부적절 표현

![profanity settings](/img/docs/guide/chat/console/settings-profanity.png)

- Pro 기능을 지원하는 요금제에서 사용할 수 있습니다.
- 필터 방식은 ‘\*로 대체하기’와 ‘메시지 보내지 않기’ 중에 설정할 수 있습니다.
- 부적절 표현 리스트를 통해 필터 할 문구를 설정할 수 있습니다.  
'모든 언어 공통' 항목은 언어와 상관없이 모든 유저에게 적용됩니다.  
특정 언어에 대한 설정이 추가되는 경우, 해당 유저에게는 공통 및 해당 언어에 대한 설정이 모두 적용됩니다.  
언어는 5개까지 설정할 수 있습니다.  
[뒤끝에서 제공하는 표준 리스트로 덮어씌우기]를 클릭하면 작성한 내용이 사라지고 뒤끝 표준 리스트로 덮어씌워집니다.  
최대 100,000byte까지 작성할 수 있습니다.

### 신고

![reports settings](/img/docs/guide/chat/console/settings-reports.png)

- Pro 기능을 지원하는 요금제에서 사용할 수 있습니다.
- 언어별로 신고 사유를 지정할 수 있습니다. 특정 언어에 대한 설정이 존재하지 않은 경우 그 언어를 사용하고 있는 해당 유저에게는 자동으로 ‘FALLBACK 언어’ 항목이 노출됩니다. Key 값은 영어와 “-”만 사용해서 48 byte 까지 입력할 수 있으며, 중복을 허용하지 않습니다.
- 신고 횟수 제한은 유저 한 명이 하루에 신고할 수 있는 최대 신고 횟수입니다.
- 신고 기간 제한은 일정 시간이 지난 메시지에 대해서 신고를 막는 기능입니다. 메시지 저장 기간을 넘을 수 없습니다.

### 번역

![translations settings](/img/docs/guide/chat/console/settings-translation.png)

- 자동 번역 기능을 활성화할 수 있습니다. 자동 번역을 허용할 언어를 설정하여 자동 번역 기능을 효율적으로 사용할 수 있습니다.



### Platform API

![api settings](/img/docs/guide/chat/console/settings-api.png)

- API를 통해 채팅의 라이브 옵스 기능을 사용할 수 있습니다. 자세한 방법은 [Platform API 문서](/api-docs/chat/intro)를 확인하세요.


### 비활성화

![deactivate chat](/img/docs/guide/chat/console/settings-deactivate.png)

- 채팅 기능을 비활성화할 수 있습니다.
