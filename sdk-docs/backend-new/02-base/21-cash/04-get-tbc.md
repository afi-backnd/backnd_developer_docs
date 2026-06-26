---
sidebar_label: 보유한 TBC 조회
---

# GetTBC

public Task< GetTBCResult > **GetTBCAsync**();

## 설명

게임 유저가 현재 보유한 TBC(The Backend Coin) 정보를 조회합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndTBC.Instance.GetTBCAsync();        
int amountTBC = int.Parse(reqResult.GetAmountTBC());

Debug.Log(amountTBC);
```

### Callback 방식

```js
BackndTBC.Instance.GetTBC((callback) =>
{
    int amountTBC = int.Parse(reqResult.GetAmountTBC());

    Debug.Log(amountTBC);
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

## ReturnValueJson

```js
{"amountTBC":4470}
```
