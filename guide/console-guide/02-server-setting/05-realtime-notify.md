import ConsoleLinkButton from '@site/src/components/ConsoleLinkButton';

# 실시간 알림

<ConsoleLinkButton text="알림 발송 설정 바로가기" menu="settingPush" feature="알림 발송 설정" title="알림 발송 설정" />

실시간 알림은 유저 행동 및 시스템 이벤트 발생 시 이를 즉시 전달하여, 클라이언트에서 별도의 조회 없이 상태 변화를 반영할 수 있도록 지원합니다.

:::info
실시간 알림 SDK 연동 방법은 **[실시간 알림 SDK 문서](https://docs.backnd.com/sdk-docs/backend/base/notify/connect-to-notify-server/)** 를 참고해 주세요.
:::

## 활성화

실시간 알림을 사용하려면 활성화가 반드시 선행되어야 합니다. 
**비활성화 상태**에서는 우측에 **실시간 알림 활성화** 버튼이 표시됩니다.

![실시간 알림 비활성화](/img/docs/guide/base-serversetting/notify-realtime-inactive.png)


버튼을 눌러 활성화하면 상태가 **활성화**로 변경됩니다.

![실시간 알림 활성화](/img/docs/guide/base-serversetting/notify-realtime-active.png)
