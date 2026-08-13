---
sidebar_label: "푸시 해제"
description: "DeleteDeviceToken"
---

# DeleteDeviceToken
public BackendReturnObject **DeleteDeviceToken**();

## 설명
푸시 설정을 OFF 합니다. 
푸시 설정이 OFF일 경우, 뒤끝 콘솔에서 푸시 기능을 사용해도 푸시 알람이 오지 않습니다.  

## Example

### 동기
```js
Backend.iOS.DeleteDeviceToken();
```

### 비동기
```js
Backend.iOS.DeleteDeviceToken((callback) => 
{
    // 이후 처리
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.iOS.DeleteDeviceToken, (callback) => 
{
    // 이후 처리
});
```

## ReturnCase

### Success cases
**삭제에 성공한 경우**  
statusCode : 204  
message : Success  
