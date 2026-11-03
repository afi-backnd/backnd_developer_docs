---
sidebar_label: "[Deprecated] String을 Stream으로 변환"
description: "String을 Stream으로 변환"
draft: true
unlisted: true
---

# StringToStream

```csharp
public static Stream StringToStream(string text);
```

## 파라미터

| Value | Type   | Description                            |
| ----- | ------ | -------------------------------------- |
| text  | string | stream으로 변환을 원하는 string 문자열 |

## 설명

string 타입의 문자열을 stream으로 변환합니다.  

## Example

```csharp
string text = "backend server";
var streamText = Backend.StringToStream(text);
```

## ReturnCase

stream 이 리턴됩니다.  
