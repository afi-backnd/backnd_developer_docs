---
sidebar_label: RequestResult
description: "RequestResult"
---

# RequestResult(리턴 값)

RequestResult는 **통신의 결과로 넘어오는 값을 저장하는 클래스**입니다.  
뒤끝 SDK를 이용하여 **서버로 요청한 모든 기능은 RequestResult 클래스 형태로 리턴**됩니다.  

| Value | Type | Description |
|---|---|---|
| statusCode | string | 서버에서 넘겨주는 상태 코드 |
| errorCode | string | 에러 정보를 알려주는 코드 |
| message | string | 서버에서 넘겨주는 성공 / 실패 세부 정보 |
| returnValue | string | **요청 성공 시** 서버에서 넘겨준 json 형태의 데이터 |

RequestResult는 returnValue에 요청에 대한 응답을 담고 있습니다.  이는 Json 형태입니다.  
>Json 라이브러리는 Newtonsoft Json을 사용하며,  [이곳](https://www.newtonsoft.com/json)에서 api를 확인하실 수 있습니다.  

## Example
```js
var reqResult = await BackndAuth.Instance.SignInWithProviderAsync("AccessToken", AuthProvider.Google);
if (reqResult.IsSuccess() == false)
{
    // 서버에 에러가 리턴되었다면
    Debug.Log(reqResult.ToString()); // 에러 정보 확인
    return;
}

// 서버의 응답을 json 객체로 변환
var json = reqResult.GetJObject();
```

## RequestResult 메소드 리스트

| 메소드        | 반환 타입           | Description  |
| :------------ |:-------------| :----- |
| Clear()     | void | 정보 초기화 |
| StatusCode     | int | statusCode 값 받아오기 |
| ErrorCode     | string | errorCode 값 받아오기 |
| Message     | string | message 값 받아오기 |
| HasReturnValue() | bool | returnValue가 존재하는지 확인 |
| ReturnValue     | string | returnValue 값 받아오기 |
| GetJObject()     | JsonData | returnValue 값을 JsonData로 변환하여 받아오기 |
| ToString()     | string | statusCode, errorCode, message, returnValue가 존재하는 경우, 해당 내용을 모두 string형으로 반환  |
| HasRows()     | bool | returnValue에 rows 존재 여부 확인 |
| GetRows()    | JsonData | returnValue 내부에 rows를 JsonData로 변환하여 반환 |
| HasInDate()     | bool | returnValue에 inDate 존재 여부 확인 |
| GetInDate()     | string | returnValue 내부에 inDate를 반환 |
| HasOffsetKey()     | bool | returnValue에 firstKey 나 LastEvaluatedKey 존재  여부 확인 |
| GetOffsetKey()     | string | returnValue 내부에 firstKey 나 LastEvaluatedKey 를 string 으로 변환하여 반환 |
| IsSuccess()     | bool | 요청이 성공했는지 실패했는지 확인 (성공: 200번대 statusCode, 실패: 300 이상의 statusCode) |
| IsServerError()     | bool | statusCode가 서버 에러(500번대)인지 판별하여 반환 |


