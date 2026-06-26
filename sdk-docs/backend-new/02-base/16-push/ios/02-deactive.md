---
sidebar_label: 푸시 해제
description: "푸시 해제"
---

# UnregisterPush
public Task&lt;RequestResult&gt; **UnregisterPushAsync**();

## 설명
푸시 설정을 OFF 합니다. 
푸시 설정이 OFF일 경우, 뒤끝 콘솔에서 푸시 기능을 사용해도 푸시 알람이 오지 않습니다.  

## Example

### Task 방식
```js
var reqResult = await BackndPushNotification.iOS.UnregisterPushAsync();
```

### Callback 방식
```js
BackndPushNotification.iOS.UnregisterPush((callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**삭제에 성공한 경우**  
statusCode : 204  
message : Success  
