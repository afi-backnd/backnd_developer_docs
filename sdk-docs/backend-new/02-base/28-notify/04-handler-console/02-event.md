---
sidebar_label: 이벤트 등록 핸들러
description: "이벤트 등록 핸들러"
---

# OnNewEventCreated

public OnNewEventCreateNotification **OnNewEventCreated**;

## 파라미터

| Value   | Type   | Description |
| ------- | ------ | ----------- |
| title   | string | - 이벤트 이름<br/>- (구버전 이벤트) 이벤트 제목 |
| content | string | - 폴백 언어 이벤트 내용<br/>- (구버전 이벤트) 이벤트 내용 |

## 설명

뒤끝 콘솔에서 이벤트 등록 시 호출되는 핸들러입니다.  
등록된 이벤트 이름과 폴백 언어 이벤트 내용을 핸들러에서 확인할 수 있습니다.  

실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 이벤트 즉시 등록
- 이벤트 예약 등록 (예약된 시간에 응답)
- 이벤트 수정 등록 (비공개 => 공개 전환 포함)

다음과 같은 이벤트는 발생하지 않습니다.  

- 이벤트 비공개 등록 (공개 => 비공개 전환 포함)
- 이벤트 삭제

## Example

```js
BackndBase.Notification.OnNewEventCreated= (string title, string content) => {
    Debug.Log(
        $"[OnNewEventCreated(새로운 이벤트 생성)]\n" +
        $"| title : {title}\n" +
        $"| content : {content}\n"
    );
};
```
