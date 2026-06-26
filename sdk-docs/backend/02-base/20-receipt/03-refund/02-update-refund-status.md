---
sidebar_label: 환불 상태 업데이트
---

# UpdateRefundStatus

public BackendReturnObject **UpdateRefundStatus**(string **orderId**, RefundStatus **refundStatus**, string **gamerId** = null);  
public void **UpdateRefundStatus**(string **orderId**, RefundStatus **refundStatus**, Backend.BackendCallback **callback**, string **gamerId** = null);

## 설명

환불 목록에서 조회한 환불 건에 대해 처리(아이템 회수, 환불 제외 등)를 완료한 후 환불 상태를 업데이트합니다.

환불 목록 조회 → 게임 내 처리(아이템 회수 등) → 상태 업데이트 순서로 진행합니다.

## 파라미터

| Value | Type | Description | default |
| ----- | ---- | ----------- | :-----: |
| orderId | string | 환불 목록에서 조회한 스토어 주문 ID (`orderID`) | - |
| refundStatus | RefundStatus | 업데이트할 환불 상태 | - |
| callback | Backend.BackendCallback | 응답을 받을 콜백 함수 (비동기) | - |
| gamerId | string | 뒤끝 로그인 없이 업데이트 시 게이머 ID <br/> (커스텀 인증 사용 시) | null |

### RefundStatus

| 값 | 설명 |
| -- | ---- |
| REFUND_COMPLETED | 회수 처리 완료 <br/> 환불된 아이템을 게임 내에서 회수한 경우 |
| REFUND_EXCLUDED | 환불 제외 처리 <br/> 해당 환불 건을 처리 대상에서 제외하는 경우 |
| OTHER | 기타 처리 |

## Example

### 동기

```js
// 환불 목록에서 조회한 환불 건
string orderId = "GPA.1234-5678-9012-34567";

// 1. 게임 내 아이템 회수 처리
RecoverItem(refundItem);

// 2. 환불 상태를 '회수 완료'로 업데이트
BackendReturnObject bro = Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_COMPLETED);

if (bro.IsSuccess())
{
    Debug.Log("환불 상태 업데이트 성공");
}
else
{
    Debug.LogError("환불 상태 업데이트 실패: " + bro.GetMessage());
}
```

### 비동기

```js
Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_COMPLETED, bro =>
{
    if (bro.IsSuccess())
    {
        Debug.Log("환불 상태 업데이트 성공");
    }
    else
    {
        Debug.LogError("환불 상태 업데이트 실패: " + bro.GetMessage());
    }
});
```

### 환불 제외 처리

```js
BackendReturnObject bro = Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_EXCLUDED);
```

### 커스텀 인증 사용 시

```js
string gamerId = "게이머ID";

// 동기
BackendReturnObject bro = Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_COMPLETED, gamerId);

// 비동기
Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_COMPLETED, bro =>
{
    if (bro.IsSuccess())
        Debug.Log("환불 상태 업데이트 성공");
}, gamerId);
```

## 환불 처리 전체 흐름

```
1. 스토어에서 환불 발생 (자동 감지)
2. 환불 목록 조회 (GetRefundList)
3. orderStatus가 "REFUNDED"인 건 확인
4. 게임 내 처리 (아이템 회수, 계정 차단 등)
5. 환불 상태 업데이트 (UpdateRefundStatus)
```

```js
BackendReturnObject bro = Backend.Refund.GetRefundList();

if (!bro.IsSuccess()) return;

var rows = bro.GetReturnValuetoJSON()["rows"];
for (int i = 0; i < rows.Count; i++)
{
    var row = rows[i];
    string orderId = row["orderID"].ToString();
    string status = row["orderStatus"].ToString();

    // REFUNDED 상태인 건만 처리
    if (status != "REFUNDED") continue;

    // details에서 게임 아이템 정보 추출
    var details = JsonMapper.ToObject(row["details"].ToString());
    string itemId = details["itemId"].ToString();

    // 아이템 회수
    RecoverItem(itemId);

    // 상태 업데이트
    BackendReturnObject updateBro = Backend.Refund.UpdateRefundStatus(orderId, RefundStatus.REFUND_COMPLETED);

    if (updateBro.IsSuccess())
        Debug.Log($"환불 회수 완료: {orderId}");
    else
        Debug.LogError($"환불 상태 업데이트 실패: {updateBro.GetMessage()}");
}
```

## 실시간 환불 알림 연동

뒤끝의 [실시간 알림](/sdk-docs/backend/base/notify/connect-to-notify-server) 기능을 활용하면, 스토어에서 환불이 발생했을 때 즉시 알림을 받아 처리할 수 있습니다.
환불 알림은 [OnRefundRequest](/sdk-docs/backend/base/notify/handler-user/post) 핸들러를 통해 수신됩니다.

```js
// 실시간 알림 연결
Backend.Notification.OnAuthorize = (bool result, string reason) =>
{
    if (result)
        Debug.Log("실시간 알림 연결 성공");
};

Backend.Notification.OnDisConnect = (string reason) =>
{
    Debug.Log($"실시간 알림 연결 끊김: {reason}");
};

// 환불 발생 시 알림 수신
Backend.Notification.OnRefundRequest = () =>
{
    Debug.Log("환불 알림 수신 → 환불 목록 조회");
    // 환불 목록을 조회하여 처리
    ProcessRefunds();
};

Backend.Notification.Connect();
```

:::info 실시간 알림과 환불 처리
실시간 알림은 환불 발생을 즉시 감지하는 용도입니다.
알림 수신 후 `GetRefundList`로 환불 목록을 조회하고, 처리가 완료되면 `UpdateRefundStatus`로 상태를 업데이트하는 흐름은 동일합니다.

실시간 알림을 사용하지 않는 경우, 앱 시작 시 또는 주기적으로 `GetRefundList`를 호출하여 환불 여부를 확인할 수 있습니다.
:::

:::danger Unity 메인 스레드 주의
실시간 알림 콜백은 백그라운드 스레드에서 호출될 수 있습니다.
UI 업데이트나 Unity API 호출이 필요한 경우, `Update()`에서 플래그를 확인하는 방식으로 메인 스레드에서 처리해야 합니다.

```js
private volatile bool _refundNotificationPending;

// 알림 수신 (백그라운드 스레드)
Backend.Notification.OnRefundRequest = () =>
{
    _refundNotificationPending = true;
};

// 메인 스레드에서 처리
private void Update()
{
    if (_refundNotificationPending)
    {
        _refundNotificationPending = false;
        ProcessRefunds();
    }
}
```
:::

## ReturnCase

### Success cases

| statusCode | message |
| :--------: | ------- |
| 200 | Success |

### Error cases

| statusCode | errorCode | 설명 |
| :--------: | --------- | ---- |
| 400 | BadParameterException | orderId가 유효하지 않은 경우 <br/> bad orderId, 잘못된 orderId 입니다 |
| 401 | InvalidSession | 인증 세션이 유효하지 않은 경우 <br/> 로그인이 필요합니다 |
| 404 | NotFoundException | 해당 orderId의 환불 내역을 찾을 수 없는 경우 |
