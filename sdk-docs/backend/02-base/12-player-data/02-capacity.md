---
sidebar_label: 쓰기량, 읽기량 확인
---

# 확인 방법

## 개요
Backend.PlayerData를 통해 게임 정보 관련 함수를 호출하여 성공 응답이 올 때에는 읽기량 혹은 쓰기량이 표시됩니다.  
(아래 읽기량과 쓰기량은 예시로, 불러오는 정보에 따라 달라질 수 있습니다.)

유저 데이터 등록 시
```js
returnValue : {"ConsumedCapacity":{"Write":{"CapacityUnits":3}},"inDate":"2023-10-28T11:00:38.611Z"}
```

유저 데이터 불러오기 시

```js
returnValue : {"serverTime":"2023-10-28T11:02:36.894Z","rows":[...],"firstKey":null,"ConsumedCapacity":{"Read":{"CapacityUnits":0.5}}}
```

유저 데이터 수정 시
```js
returnValue : {"ConsumedCapacity":{"Read":{"CapacityUnits":6},"Write":{"CapacityUnits":4}}}
```

유저 데이터 삭제 시
```js
returnValue : {"ConsumedCapacity":{"Write":{"CapacityUnits":3}}}
```

트랜잭션 쓰기 시
```js
returnValue : {"putItem":[...],"ConsumedCapacity":[{"Write":{"CapacityUnits":24},"Read":{"CapacityUnits":0}}]}
```

트랜잭션 읽기 시
```js
returnValue : {"Responses":[...],"ConsumedCapacity":[{"Read":{"CapacityUnits":27}}]}
```


## 파싱 방법
해당 returnValue를 확인할 수 있는 방법은 다음과 같습니다.

```js
var bro = Backend.PlayerData.InsertData("tableName");
if(bro.IsSuccess()) {
  Debug.Log("쓰기량 : " + bro.GetReturnValueToJson()["ConsumedCapacity"]["Write"]["CapacityUnits"]);
}
```

## GetWriteCapacity
위 파싱 방법을 통해 float으로 쓰기량을 함수로 쉽게 확인할 수 있습니다.  
만약 에러 시, 해당 함수를 호출할 경우 0.0f로 리턴됩니다.

```js
var bro = Backend.PlayerData.InsertData("tableName");
if(bro.IsSuccess()) {
  float capacity = bro.GetWriteCapacity();
  Debug.Log("쓰기량 : " + capacity);
}
```

## GetReadCapacity
위 파싱 방법을 통해 float으로 읽기량을 함수로 쉽게 확인할 수 있습니다.  
만약 에러 시, 해당 함수를 호출할 경우 0.0f로 리턴됩니다.

```js
var bro = Backend.PlayerData.GetMyLatestData("tableName");
if(bro.IsSuccess()) {
  float capacity = bro.GetReadCapacity();
  Debug.Log("읽기량 : " + capacity);
}
```
