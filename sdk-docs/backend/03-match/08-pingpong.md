---
sidebar_label: "메시지 송수신"
description: "Poll"
---

# Poll

public int **Poll**();

## 설명

클라이언트의 메시지를 서버로 송신하고, 수신한 데이터를 이벤트 형태로 호출시킵니다.  

- 서버에서 송신한 데이터는 SDK에서 재가공 후 이벤트를 발생시킵니다.  
- 항상 메시지를 송수신하기 위해 **Poll 함수는 주기적으로 호출**되어야 합니다.  

> 유니티 객체의 Update 함수에서 Poll을 호출하기
> 코루틴을 생성하고 해당 코루틴 내에서 Poll을 주기적으로 호출하기
> 별도의 스레드를 생성하고, 해당 스레드 내에서 Poll을 주기적으로 호출하기

## Example

```js
Backend.Match.Poll();
```

## Return Value

- 처리된 이벤트 개수

---

# OnException

public ExceptionEventHandler **OnException**;

## 전달인자

| Value | Type      | Description |
| :---- | :-------- | :---------- |
| e     | Exception | 발생한 예외 |

## 설명

이벤트를 수신 및 처리하면서 예외가 발생하는 경우, 개발자가 작성한 이벤트 핸들러 내에서 예외가 발생하는 경우 호출되는 이벤트 핸들러입니다.  

## Example

```js
Backend.Match.OnException(e) += {
    // TODO
};
```
