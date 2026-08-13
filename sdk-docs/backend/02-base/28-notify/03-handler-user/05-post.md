---
sidebar_label: "쪽지/우편/환불 기능 핸들러"
description: "쪽지/우편/환불 기능 핸들러"
---

# 쪽지/우편/환불 기능 핸들러

비동기로 작업이 진행되며 아래의 이벤트 핸들러로 쪽지/우편/환불 기능 관련 이벤트들이 콜백 됩니다.
실시간 알림이 발생하는 이벤트는 다음과 같습니다.

- 새 쪽지 도착
- 새 유저 우편 도착
- 환불 발생

---

# OnReceivedMessage

public OnNotification **OnReceivedMessage**;

## 설명

새 쪽지가 도착했을 때 호출되는 핸들러입니다.

## Example

```js
Backend.Notification.OnReceivedMessage = () => {
  Debug.Log("새 쪽지가 도착했습니다!");
};
```

---

# OnReceivedUserPost

public OnNotification **OnReceivedUserPost**;

## 설명

새 유저 우편이 도착했을 때 호출되는 핸들러입니다.

## Example

```js
Backend.Notification.OnReceivedUserPost = () => {
  Debug.Log("새 유저 우편이 도착했습니다!");
};
```

---

# OnRefundRequest

public OnNotification **OnRefundRequest**;

## 설명

스토어(Google Play, Apple App Store, ONE Store)에서 환불이 발생했을 때 호출되는 핸들러입니다.

## Example

```js
Backend.Notification.OnRefundRequest = () => {
  Debug.Log("환불이 발생했습니다!");
};
```

### 환불 알림 활용

알림 수신 후 [환불 목록 조회](/sdk-docs/backend/base/receipt/refund/get-refund-list)를 통해 환불 내역을 확인하고, 아이템 회수 등의 처리를 수행할 수 있습니다.

```js
Backend.Notification.OnRefundRequest = () => {
    // 환불 목록 조회하여 환불 건 처리
    BackendReturnObject bro = Backend.Refund.GetRefundList();
    if (!bro.IsSuccess()) return;

    var rows = bro.GetReturnValuetoJSON()["rows"];
    for (int i = 0; i < rows.Count; i++)
    {
        var row = rows[i];
        if (row["orderStatus"].ToString() == "REFUNDED")
        {
            // 환불 처리 (아이템 회수 등)
            Debug.Log($"환불 감지: {row["orderID"]}");
        }
    }
};
```

> 자세한 환불 처리 흐름은 [환불 관리](/sdk-docs/backend/base/receipt/refund/get-refund-list) 문서를 참고해 주세요.
