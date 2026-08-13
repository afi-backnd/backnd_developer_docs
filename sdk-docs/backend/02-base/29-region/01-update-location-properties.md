---
sidebar_label: "내 위치로 국가 정보 동기화"
description: "UpdateLocationProperties"
---

# UpdateLocationProperties
public BackendReturnObject **UpdateLocationProperties**();

## 설명
현재 뒤끝 SDK 내부에 캐싱된 IP 기반의 국가정보를 업데이트합니다.  

> <img src="https://developer.thebackend.io/static/img/unity/guide/inspector/sdk-auto-location.png"/>

뒤끝의 인스펙터 값 Auto Load Location Properties가 체크되어있거나, BackendCustomSettings에서 autoLoadLocationProperties를 true로 선언할 경우 초기화시에 해당 비동기 함수를 호출하여 현재 국가 정보를 불러와 캐싱합니다.  

캐싱된 값은 **로그인 시(토큰 재발급 포함)에만 반영**이 되며, 로그인이 성공할 경우 해당 국가 정보로 변경됩니다.  

만약 초기화 시 서버 전송에 실패하여 에러가 발생하였을 경우, 해당 함수를 호출하여 다시 동기화할 수 있습니다.  
> VPN을 사용한 기기일 경우, 국가를 불러오지 못할 수도 있습니다.  

국가 정보는 Backend.LocationProperties 내부의 City, Country, Region, Language로 불러올 수 있습니다.  

## Example

### 동기
```js
Backend.LocationProperties.UpdateLocationProperties();

Debug.Log("국가 이름 : " + Backend.LocationProperties.Country);
Debug.Log("도시 이름 : " + Backend.LocationProperties.City);
Debug.Log("지역 이름 : " + Backend.LocationProperties.Region);
Debug.Log("언어 이름 : " + Backend.LocationProperties.Language);
```

### 비동기
```js
Backend.LocationProperties.UpdateLocationProperties(callback=>
{
    Debug.Log("국가 이름 : " + Backend.LocationProperties.Country);
    Debug.Log("도시 이름 : " + Backend.LocationProperties.City);
    Debug.Log("지역 이름 : " + Backend.LocationProperties.Region);
    Debug.Log("언어 이름 : " + Backend.LocationProperties.Language);
});
```

### SendQueue
```js
SendQueue.Enqueue(Backend.LocationProperties.UpdateLocationProperties, callback => 
{
    Debug.Log("국가 이름 : " + Backend.LocationProperties.Country);
    Debug.Log("도시 이름 : " + Backend.LocationProperties.City);
    Debug.Log("지역 이름 : " + Backend.LocationProperties.Region);
    Debug.Log("언어 이름 : " + Backend.LocationProperties.Language);
});
```

## ReturnCase

### Success cases

**동기화에 성공한 경우**  
statusCode : 204  
returnValue : Success

### Error cases

**VPN이 활성화되어있을 경우**  
statusCode : 400  
errorCode : HttpRequestException  
message : Response status code does not indicate success: 503(Service Temporarily Unavailable).  
