---
sidebar_label: "보유한 TBC 조회"
description: "GetTBC"
---

# GetTBC

public BackendReturnObject **GetTBC**();

:::warning TBC 기능 제공 중단 안내
스토어 영수증 검증 API의 최신화에 따라 기존 방식과의 호환성 문제 및 기능 전반에 대한 개선 필요 사항이 확인되어, 기능 제공이 중단되었습니다.

**SDK 5.18.7 이하 버전에서는 기존과 동일하게 TBC 기능을 계속 이용하실 수 있습니다.**
:::

## 설명

게임 유저가 현재 보유한 TBC(The Backend Coin) 정보를 조회합니다.  

## Example

### 동기

```js
var bro = Backend.TBC.GetTBC();

LitJson.JsonData json = bro.GetReturnValuetoJSON();
int amountTBC = int.Parse(json["amountTBC"].ToString());

Debug.Log(amountTBC);
```

### 비동기

```js
Backend.TBC.GetTBC((callback) =>
{
    LitJson.JsonData json = callback.GetReturnValuetoJSON();
    int amountTBC = int.Parse(json["amountTBC"].ToString());

     Debug.Log(amountTBC);
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.TBC.GetTBC, (callback) =>
{
    LitJson.JsonData json = callback.GetReturnValuetoJSON();
    int amountTBC = int.Parse(json["amountTBC"].ToString());

     Debug.Log(amountTBC);
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

## GetReturnValuetoJSON

```js
{"amountTBC":4470}
```
