---
sidebar_label: "우편 발송 핸들러"
description: "OnNewPostCreated"
---

# OnNewPostCreated

public OnNewPostCreateNotification **OnNewPostCreated**;

## 파라미터

| Value          | Type           | Description |
| -------------- | -------------- | ----------- |
| postRepeatType | PostRepeatType | 우편 종류   |
| title          | string         | 우편 제목   |
| content        | string         | 우편 내용   |
| author         | string         | 우편 발송인 |

### PostRepeatType

| enum        | 설명      |
| ----------- | --------- |
| repeat      | 반복 발송 |
| immediately | 즉시 발송 |
| reservation | 예약 발송 |

## 설명

뒤끝 콘솔의 우편 관리에서 우편 발송 시 호출되는 핸들러입니다.  
등록된 제목과 내용, 우편 종류와 발송인을 핸들러에서 확인할 수 있습니다.  

우편은 발송될 우편의 수에 따라 발송이 지연되므로 정시보다 몇 분 후에 도착 할 수 있습니다.  

우편 발송이 발생하는 이벤트는 다음과 같습니다.  

- 우편 즉시 발송
- 우편 예약 발송(예약된 시간에)
- 우편 반복 발송(예약된 시간에)
- 우편 개인 발송(등록된 유저에게만)

다음과 같은 이벤트는 발생하지 않습니다.  

- 랭킹 보상 우편
- 쿠폰 우편

## Example

```js
Backend.Notification.OnNewPostCreated = (PostRepeatType postRepeatType, string title, string content, string author) => {
    Debug.Log(
        $"[OnNewPostCreated(새로운 우편 생성)]\n" +
        $"| postRepeatType : {postRepeatType}\n" +
        $"| title : {title}\n" +
        $"| content : {content}\n" +
        $"| author : {author}\n"
    );
};
```
