---
sidebar_label: 바이너리 데이터 송신
---

# SendDataToInGameRoom

public void **SendDataToInGameRoom**(byte[] **data**);  
public void **SendDataToInGameRoom**(byte[] **data**, int **offset**, int **length**);

## 파라미터

| Value  | Type   | Description                                   |
| :----- | :----- | :-------------------------------------------- |
| data   | byte[] | 바이트 배열                                   |
| offset | int    | 바이트 배열에서 데이터를 읽기 시작할 지점     |
| length | int    | 바이트 배열에서 offset부터 데이터를 읽을 길이 |

## 설명

키 입력 값 혹은 일렬의 게임 로직의 결괏값을 인게임 서버로 전송합니다.  
송신된 데이터는 서버에서 **현재 게임방에 참여한 모든 클라이언트에게 브로드캐스팅**합니다.  

- 모든 클라이언트에는 서버로 데이터를 송신한 클라이언트도 포함됩니다.  
  > 보내고자 하는 데이터를 바이트 배열로 바꾸는 방법은 [패킷 디자인 문서](/sdk-docs/backend/match/note/packet-design)를 참고해 주세요.  
  > 데이터를 서버로 전송할 때 쓰로틀링이 적용되어 있습니다. 쓰로틀링 관련해서는 [쓰로틀링 문서](/sdk-docs/backend/match/note/throttling)를 참고해 주세요.  

## Example

```js
Backend.Match.SendDataToInGameRoom(data);
```
