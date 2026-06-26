# 페이징 처리하기

뒤끝의 다양한 함수(게임 정보, 길드 등)를 이용하여 데이터를 조회하는 경우 한 번에 최대 100개의 데이터만 가져올 수 있습니다.  
하지만 데이터는 100개를 초과하여 존재할 수 있고, 이 경우 페이징 처리가 필요합니다.  
뒤끝의 함수를 이용하여 데이터를 조회할 때 조회 대상이 되는 데이터가 100개가 넘어가는 경우 결과의 returnValue에 `firstKey` 혹은 `LastEvaluatedKey`라는 데이터가 추가됩니다.  
이것을 GetOffsetKey() 함수로 가져와서 다음 offsetkey 값으로 넣어줄 수 있습니다.  

##  Example
아래는 "testPublic" 테이블 내 존재하는 모든 테이블을 조회하는 예제입니다.  

``` js
void GetTestPublic() {
    // 데이터 조회
var reqResult = await BackndUserData.Instance.GetDataAsync("testPublic", 10);
if (reqResult.IsSuccess() == false)
{
    // 실패 처리
    return;
}

while (reqResult.HasOffsetKey() == true)
{
    var offsetKey = reqResult.GetOffsetKey();

    // 다음 데이터 조회
    reqResult = await BackndUserData.Instance.GetDataAsync("testPublic", 10, offsetKey);
    if (reqResult.IsSuccess() == false)
    {
        // 실패 처리
        return;
    }
}
}
```
