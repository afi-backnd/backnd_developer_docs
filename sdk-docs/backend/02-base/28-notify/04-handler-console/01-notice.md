---
sidebar_label: "공지사항 등록 핸들러"
description: "OnNewNoticeCreated"
---

# OnNewNoticeCreated

public OnNewNoticeCreateNotification **OnNewNoticeCreated**;

## 파라미터

| Value   | Type   | Description   |
| ------- | ------ | ------------- |
| title   | string | - 공지사항 이름<br/>- (구버전 공지) 공지사항 제목 |
| content | string | - 폴백 언어 공지사항 내용<br/>- (구버전 공지) 공지사항 내용 |

## 설명

뒤끝 콘솔에서 공지사항 등록 시 호출되는 핸들러입니다.  
등록된 공지사항 이름과 폴백 언어 공지사항 내용을 핸들러에서 확인할 수 있습니다.  

실시간 알림이 발생하는 이벤트는 다음과 같습니다.  

- 공지사항 즉시 등록
- 공지사항 예약 등록(예약된 시간에 응답)
- 공지사항 수정 등록(비공개 => 공개 전환 포함)

다음과 같은 이벤트는 발생하지 않습니다.  

- 공지사항 삭제
- 공지사항 비공개 등록 (공개=>비공개 전환 포함)

## Example

```js
Backend.Notification.OnNewNoticeCreated = (string title, string content) => {
    Debug.Log(
        $"[OnNewNoticeCreated(새로운 공지사항 생성)]\n" +
        $"| title : {title}\n" +
        $"| content : {content}\n"
    );
};
```
