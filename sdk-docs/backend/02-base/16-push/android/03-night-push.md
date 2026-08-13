---
sidebar_label: "야간 푸시 동의"
description: "AgreeNightPushNotification"
---

# AgreeNightPushNotification
public BackendReturnObject **AgreeNightPushNotification**(bool isAgree);


## 파라미터

| Value|  Type | Description |
| --- | --- | --- |
| isAgree| bool | 야간 푸시 동의 여부.<br />false : 미동의, 밤에 발송되는 푸시 알림 확인 불가<br />true : 동의, 밤에 발송되는 푸시 알림 확인 가능 |

## 설명
오후 8시 40분부터 오전 8시 사이에 발송되는 푸시알림의 수신 여부를 설정합니다.  
동의(true)시 해당 시간대에 발송되는 푸시 알림을 받을 수 있으며, 비동의(false)시 해당 유저는 푸시가 발송되지 않습니다.  

> 자신이 설정한 야간 푸시 동의 여부는 확인이 불가능합니다.  

## Example

### 동기
```js
Backend.Android.AgreeNightPushNotification(true);
```

### 비동기
```js
Backend.Android.AgreeNightPushNotification(true, (callback) => {
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.Android.AgreeNightPushNotification, true, (callback) => {
    // 이후 처리
});
```

## ReturnCase

### Success cases
**등록에 성공한 경우**  
statusCode : 204  
message : Success
