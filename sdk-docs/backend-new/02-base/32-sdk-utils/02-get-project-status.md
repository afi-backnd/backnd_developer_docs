---
sidebar_label: 프로젝트 상태 조회
description: "프로젝트 상태 조회"
---

# GetProjectStatus

public Task< GetProjectStatusResult > **GetProjectStatusAsync**();

## 설명

뒤끝 콘솔 > 서버 설정 > 프로젝트 상태에 등록한 프로젝트 상태를 조회합니다.  

## Example

### Task 방식

```js
var statusResult = await BackndUtils.Instance.GetProjectStatusAsync();
```

### Callback 방식

```js
BackndUtils.Instance.GetProjectStatus((callback) =>
{
    // 이후 처리
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
    "serverStatus" : 1
}
```

| serverStatus | Description |
| :----------: | :---------: |
|      0       |   온라인    |
|      1       |  오프라인   |
|      2       |    점검     |
