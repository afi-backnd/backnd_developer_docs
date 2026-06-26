---
sidebar_label: 성공 콜백
---

# 성공 콜백

채팅에서 처리 되는 기능이 성공 또는 완료 시에 오는 콜백 함수 및 Enum 정보 입니다.

### 콜백 함수

```csharp
// 채팅에서 처리 되는 기능이 성공 또는 완료 시에 오는 콜백 함수 입니다.
public void OnSuccess(SUCCESS_MESSAGE success, object param) { }
```

## SuccessCode(enum)

| Value         | Description      | Param |
| :------------ | :--------------- | :---- |
| REPORT        | 신고 처리 완료   | NULL  |
| ADD_BLOCK_PLAYER | 유저 차단 성공 | NULL  |
| REMOVE_BLOCK_PLAYER | 유저 차단 해제 성공 | NULL  |
