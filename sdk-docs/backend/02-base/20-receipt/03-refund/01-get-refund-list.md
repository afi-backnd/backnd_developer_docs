---
sidebar_label: "환불 목록 조회"
description: "GetRefundList"
---

# GetRefundList

public BackendReturnObject **GetRefundList**(int **limit** = 10, int **offset** = 0, RefundSortOrder **orderBy** = RefundSortOrder.DESC);  
public BackendReturnObject **GetRefundList**(string **gamerId**, int **limit** = 10, int **offset** = 0, RefundSortOrder **orderBy** = RefundSortOrder.DESC);

## 설명

스토어(Google Play, Apple App Store, ONE Store)에서 환불이 이루어지면 뒤끝 콘솔에 환불 내역이 자동으로 등록됩니다.
`GetRefundList`를 호출하면 해당 유저의 환불 목록을 조회할 수 있습니다.

환불 목록을 통해 게임 내에서 다음과 같은 처리를 구현할 수 있습니다.

- 환불된 상품의 아이템 회수
- 악의적 환불 유저에 대한 계정 차단
- 환불 이력 UI 표시

:::info 사전 준비
뒤끝 콘솔에서 스토어 설정이 완료되어 있어야 합니다.
스토어 설정이 완료되면 스토어에서 발생한 환불이 자동으로 환불 목록에 노출됩니다.

- 구글: [구글 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/android/setting-google-console)
- 애플: [애플 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/ios/setting-google-console)
- 원스토어: [원스토어 결제 콘솔 설정](/sdk-docs/backend/base/receipt/receipt/onestore/setting-onestore-console)
:::

## 파라미터

| Value | Type | Description | default |
| ----- | ---- | ----------- | :-----: |
| gamerId | string | 뒤끝 로그인 없이 조회 시 게이머 ID <br/> (커스텀 인증 사용 시) | - |
| limit | int | 조회할 최대 건수 | 10 |
| offset | int | 조회 시작 위치 (페이징) | 0 |
| orderBy | RefundSortOrder | 정렬 순서 | DESC |

### RefundSortOrder

| 값 | 설명 |
| -- | ---- |
| DESC | 최신순 (내림차순) |
| ASC | 오래된순 (오름차순) |

## Example

### 기본 조회

```js
BackendReturnObject bro = Backend.Refund.GetRefundList();

if (bro.IsSuccess())
{
    var json = bro.GetReturnValuetoJSON();
    var rows = json["rows"];

    for (int i = 0; i < rows.Count; i++)
    {
        var row = rows[i];
        string orderId = row["orderID"].ToString();
        string orderStatus = row["orderStatus"].ToString();
        string refundAt = row["refundAt"].ToString();

        // details에는 영수증 검증 시 AddDetail로 저장한 정보가 포함
        var details = JsonMapper.ToObject(row["details"].ToString());
        string itemId = details["itemId"].ToString();
        string itemName = details["itemName"].ToString();

        Debug.Log($"환불 내역: {itemId}({itemName}) - 상태: {orderStatus}");
    }
}
else
{
    Debug.LogError("환불 목록 조회 실패: " + bro.GetMessage());
}
```

### 페이징 및 정렬

```js
// 최신순으로 20건 조회, 두 번째 페이지
BackendReturnObject bro = Backend.Refund.GetRefundList(
    limit: 20,
    offset: 20,
    orderBy: RefundSortOrder.DESC
);
```

### 커스텀 인증 사용 시

```js
string gamerId = "게이머ID";

BackendReturnObject bro = Backend.Refund.GetRefundList(gamerId);

if (bro.IsSuccess())
{
    var json = bro.GetReturnValuetoJSON();
    Debug.Log($"환불 목록: {json["rows"].Count}건");
}
```

## ReturnCase

### Success cases

| statusCode | message | returnValue |
| :--------: | ------- | ----------- |
| 200 | Success | GetReturnValuetoJSON 참조 |

### GetReturnValuetoJSON

```js
{
    "rows": [
        {
            "orderID": "GPA.1234-5678-9012-34567",
            "orderStatus": "REFUNDED",
            "refundAt": "2026-01-23T06:06:18.000Z",
            "details": "{\"itemId\":\"10001\",\"itemName\":\"골드 100개\",\"iconAddress\":\"icon_gold\"}"
        },
        {
            "orderID": "GPA.9876-5432-1098-76543",
            "orderStatus": "REFUND_COMPLETED",
            "refundAt": "2026-01-22T10:30:00.000Z",
            "details": "{\"itemId\":\"10002\",\"itemName\":\"빨간 보석 50개\"}"
        }
    ]
}
```

### 응답 필드

| 필드 | 설명 |
| ---- | ---- |
| orderID | 스토어 주문 ID |
| orderStatus | 환불 처리 상태 <br/> (아래 상태값 표 참조) |
| refundAt | 환불 발생 시각 |
| details | 영수증 검증 시 `AddDetail`로 저장한 상세 정보 <br/> (itemId, itemName 등이 JSON string으로 포함) |

### orderStatus 상태값

| 상태값 | 설명 |
| ------ | ---- |
| PURCHASED | 구매 완료 |
| REFUNDED | 환불됨 (스토어에서 환불 발생) |
| NOTIFICATION_SENT | 환불 알림 발송됨 |
| REFUND_COMPLETED | 회수 처리 완료 |
| REFUND_EXCLUDED | 환불 제외 처리 |
| BLOCKING_EXCLUDED | 차단 제외 처리 |
| OTHER | 기타 |

### Error cases

| statusCode | errorCode | 설명 |
| :--------: | --------- | ---- |
| 401 | InvalidSession | 인증 세션이 유효하지 않은 경우 <br/> 로그인이 필요합니다 |
