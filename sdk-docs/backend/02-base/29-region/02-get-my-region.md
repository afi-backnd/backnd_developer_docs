---
sidebar_label: 내 위치 정보 확인
---

# LocationProperties
public string **Country** {get; private set;}  
public string **City** {get; private set;}  
public string **Region** {get; private set;}  
public string **Language** {get; private set;}

## 설명 
캐싱된 내 위치 정보를 확인합니다.  

다음 3가지의 경우는 위치 정보를 불러오지 못한 경우입니다.  
* 뒤끝의 인스펙터 값 Auto Load Location Properties가 체크 해제되어있을 경우
* BackendCustomSettings에서 autoLoadLocationProperties를 false로 선언할 경우
* 초기화동안 호출에 문제가 발생하여 내 위치 정보 불러오기 함수가 호출되지 않을 경우

위치를 불러오는 중 오류가 발생하면 다음과 같은 로그가 출력됩니다.  
> Backend LocationProperties Update : False

> <img src="https://developer.thebackend.io/static/img/unity/location/update-location-fail.png"/> 

국가 정보가 불러오지 못한 경우, 값들은 모두 string.Empty가 됩니다.  

### 국가
```js
Debug.Log("국가 이름 : " + Backend.LocationProperties.Country);
```

### 도시
```js
Debug.Log("도시 이름 : " + Backend.LocationProperties.City);
```

### 지역
```js
Debug.Log("지역 이름 : " + Backend.LocationProperties.Region);
```

### 언어
```js
Debug.Log("언어 이름 : " + Backend.LocationProperties.Language);
```
