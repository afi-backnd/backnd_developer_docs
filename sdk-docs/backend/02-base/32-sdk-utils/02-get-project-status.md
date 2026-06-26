---
sidebar_label: 프로젝트 상태 조회
---

# GetServerStatus

public BackendReturnObject **GetServerStatus**();

## 설명

뒤끝 콘솔 > 서버 설정 > 프로젝트 상태에 등록한 프로젝트 상태를 조회합니다.  

## Example

### 동기

```js
Backend.Utils.GetServerStatus();
```

### 비동기

```js
Backend.Utils.GetServerStatus((callback) =>
{
    // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.Utils.GetServerStatus, (callback) =>
{
    // 이후 처리
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
{
    "serverStatus" : 1
}
```

| serverStatus | Description |
| :----------: | :---------: |
|      0       |   온라인    |
|      1       |  오프라인   |
|      2       |    점검     |
