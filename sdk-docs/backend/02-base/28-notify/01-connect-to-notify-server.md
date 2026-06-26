---
sidebar_label: 실시간 알림 서버 연결
---

# Connect

public void **Connect**(bool reconnection = true, long reconnectionDelay = 1000, long reconnectionDelayMax = 5000, int reconnectionAttempts = 1000);

## 설명

실시간 알림 서버에 연결합니다. 뒤끝 콘솔에서 활성화하지 않은 경우에도 연결이 되지만, 알림을 받을 수 없습니다.  
연결 후 인증 과정을 거치며, 인증 결과가 아래의 `OnAuthorize` 핸들러로 리턴됩니다.  

> 기존의 뒤끝 베이스 함수들과는 달리 반환되는 값을 핸들러를 통해 얻을 수 있습니다.  

## 파라미터
| Value        | Type           | Description  | Default |
| :------------ |:-------------| :----- | :----- |
| reconnection | bool | 연결이 끊긴 후, 재접속 여부 | 
| reconnectionDelay | long | 연결이 끊긴 후, 다시 재접속할 딜레이(ms) |
| reconnectionDelayMax | long | 연결이 끊긴 후, 최대 다시 재접속할 딜레이(ms) |
| reconnectionAttempts | long | 최대 재접속 시도 수 |

## Example

```js
// 접속 시 반응하는 핸들러 설정
Backend.Notification.OnAuthorize = (bool result, string reason) => {
  Debug.Log("실시간 서버 성공 여부 : " + result);
  Debug.Log("실패 시 이유 : " + reason);
};

// 실시간 알림 서버로 연결
Backend.Notification.Connect();
```

---

# OnAuthorize

public OnNotificationAuthorize **OnAuthorize**;

## 파라미터

| Value  | Type   | Description                                           |
| ------ | ------ | ----------------------------------------------------- |
| result | bool   | 연결 성공 여부(true 시 연결 성공, false 시 연결 실패) |
| reason | string | 실패한 이유(성공 시 successed w/ socket.id가 출력)    |

## 설명

Connect 호출 시, 자동으로 인증 과정을 거치며, 인증 결과가 OnAuthorize 핸들러를 통해 콜백 됩니다.  

## Example

```js
Backend.Notification.OnAuthorize = (bool result, string reason) => {
  Debug.Log("실시간 알림 서버 접속 시도!");

  //접속 이후 처리
  if(result) {
    Debug.Log("실시간 알림 서버 접속 성공!");
  } else {
    Debug.Log("실시간 알림 서버 접속 실패 : 이유 : " + reason);
  }
};
```

## ResponseCase

### Success cases

<table>
    <thead>
        <tr>
            <th>result(bool)</th>
            <th>reason(string)</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>true</td>
            <td>successed w/ <code>socket.id</code></td>
            <td>ErrorCode</td>
        </tr>
    </tbody>
</table>

### Fail cases

<table>
    <thead>
        <tr>
            <th>result(bool)</th>
            <th>reason(string)</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan = "3">false</td>
            <td>bad clientAppId</td>
            <td>ErrorCode</td>
        </tr>
        <tr>
            <td>bad accessToken</td>
            <td>ErrorCode</td>
        </tr>
        <tr>
            <td>bad expired accessToken</td>
            <td>ErrorCode</td>
        </tr>
    </tbody>
</table>
