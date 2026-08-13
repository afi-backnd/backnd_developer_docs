---
sidebar_label: "레이턴시 확인"
description: "Latency"
---

# Latency

public Latency **Latency**();

## 설명

인게임 서버와 클라이언트 간의 레이턴시를 확인할 수 있습니다.  

- 일반적으로 50ms 이하의 레이턴시가 측정됩니다.  
- 매치 서버에서의 레이턴시는 제공하지 않습니다
- 인게임 서버와 접속이 완료된 이후 확인할 수 있습니다.  
  > 인게임 서버와 접속이 완료된 상황은 OnSessionJoinInServer 이벤트가 성공으로 호출된 상황을 뜻합니다.  

## Example

### long 타입으로 리턴

```js
// 현재 레이턴시
Backend.Match.Latency().NowLatency;
// 최대 레이턴시
Backend.Match.Latency().MaxLatency;
// 최소 레이턴시
Backend.Match.Latency().MinLatency;
// 평균 레이턴시
Backend.Match.Latency().AvgLatency;

// example
var latancy = Backend.Match.Latency().AvgLatency;
Debug.Log(latancy);

/// <ouput>
/// 유니티 콘솔에 출력된 데이터
/// </output>
7;
```

### string 타입으로 리턴

시간 뒤에 **ms**가 추가됩니다.  

```js
// 최소/평균/최대 레이턴시
Backend.Match.Latency().ToString();
// 현재 레이턴시
Backend.Match.Latency().ToString("now");
// 최대 레이턴시
Backend.Match.Latency().ToString("max");
// 최소 레이턴시
Backend.Match.Latency().ToString("min");
// 평균 레이턴시
Backend.Match.Latency().ToString("avg");

// example
var str = Backend.Match.Latency().ToString();
Debug.Log(str);

str = Backend.Match.Latency().ToString("now");
Debug.Log(str);

/// <ouput>
/// 유니티 콘솔에 출력된 데이터
/// </output>
RTT Min/Avg/Max: 7/8/8 ms
7 ms
```

## Return Case

** 인게임 서버에 접속 성공한 이후 레이턴시 측정**  
위 Example 참조

** 매칭 서버에만 접속하거나 혹은 인게임 서버 접속 완료 전 레이턴시 측정**  
최소 레이턴시가 Long.MaxValue(9223372036854775807)로 표시될 수 있습니다.  
