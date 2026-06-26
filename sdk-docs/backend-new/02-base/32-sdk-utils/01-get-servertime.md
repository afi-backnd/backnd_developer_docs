---
sidebar_label: 서버 시간 조회
---

# GetServerTime

public Task< GetServerTimeResult > **GetServerTimeAsync**();

## 설명

뒤끝 서버의 현재 시간을 받아옵니다.  
뒤끝 서버의 시간은 UTC(협정 세계표준시) 시간으로, 한국시간은 UTC + 9입니다. UTC에는 서머타임 등이 적용되지 않습니다.  

string형으로 사용하시려면 time 변수, DateTime형으로 사용하시려면 parsedDate 사용하시면 됩니다.  
[DateTime.Parse](https://docs.microsoft.com/ko-kr/dotnet/api/system.datetime.parse?view=netframework-4.7.2)는 현재 스레드 문화권의 규칙을 사용하여 날짜 및 시간의 문자열 표현을 해당 DateTime으로 변환합니다.  

> 해당 기능은 뒤끝 로그인(액세스 토큰) 없이 사용 가능한 기능입니다.  

## Example

### Task 방식

```js
var servertime = await BackndUtils.Instance.GetServerTimeAsync();

string time = servertime.GetUtcTime();
DateTime parsedDate = DateTime.Parse(time);
```

### Callback 방식

```js
BackndUtils.Instance.GetServerTime((callback) =>
{
    string time = servertime.GetUtcTime();
    DateTime parsedDate = DateTime.Parse(time);
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
{
    "utcTime": "2019-04-10T08:54:43.274Z"
}
```
