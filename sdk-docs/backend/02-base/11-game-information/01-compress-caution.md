---
sidebar_label: 압축데이터 주의사항
---

# 압축데이터 주의사항

## 설명
콘솔에서 압축데이터를 설정할 경우, 압축데이터가 포함된 데이터를 삽입, 수정 시 자동으로 압축화가 진행됩니다.  
압축화는 List, Dictionary등과 같은 string으로 변환후 byte를 계산하였을 떄 데이터 크기가 큰 데이터 타입을 30~40% 정도로 절감시켜줍니다.  
압축할 데이터에 따라 압축률은 변할 수 있으니 **압축여부, 압축량**에 대한 측정이 필요합니다.

데이터의 읽기량/쓰기량에 대한 변화 측정은 다음과 같습니다.  
1. 압축화 컬럼을 가진 스키마 테이블 생성
2. 비스키마 테이블 생성
3. 스키마 테이블에 데이터 등록 후 GetWriteCapacity로 쓰기량 확인
4. 동일한 데이터를 비스키마 테이블에 데이터 등록 후 GetWriteCapacity로 쓰기량 확인

### 압축량 비교하기
```js
string compressData;
string column = "compressData";
Param param = new Param();
param.Add(column, compressData)

var normalInsertBro = Backend.PlayerData.Insert("normalTable", param);
var normalGetBro = Backend.PlayerData.GetMyData("normalTable", bro.GetInDate());

// 압축 테이블 테스트
var compressInsertBro = Backend.PlayerData.Insert("compressTable", param);
var compressGetBro = Backend.PlayerData.GetMyData("compressTable", bro.GetInDate());

// 각 쓰기량 측정
float normalInsertCapacity = normalInsertBro.GetWriteCapacity();
float compressInsertCapacity = compressInsertBro.GetWriteCapacity();

if(normalInsertCapacity > compressInsertCapacity) {
    Debug.Log($"압축된 데이터의 쓰기량이 감소했습니다 {normalInsertCapacity} -> {compressInsertCapacity}");
}
else {
    Debug.LogWarning($"압축된 데이터의 쓰기량이 변화하지 않았습니다.");
}

// 각 읽기량
float normalGetCapacity = normalGetBro.GetReadCapacity();
float compressGetCapacity = compressGetBro.GetReadCapacity();

if(normalInsertCapacity > compressInsertCapacity) {
    Debug.Log($"압축된 데이터의 읽기량이 감소했습니다 {normalGetCapacity} -> {compressGetCapacity}");
}
else {
    Debug.LogWarning($"압축된 데이터의 읽기량이 변화하지 않았습니다.");
}

// 각 데이터 접근 여부 확인 및 압축으로 인한 데이터 변경 확인
string normalData = normalGetBro.FlattenRows()[0][column].ToString();
string compressData = compressGetBro.FlattenRows()[0][column].ToString();

if(normalData == compressData) {
    Debug.Log("압축해제 이후의 데이터값이 동일합니다");
}
else {
    Debug.LogError("압축해제 이후의 데이터값이 일치하지 않습니다");
    Debug.LogError(compressData);
    Debug.LogError(normalData);
}

```

## 주의사항
다음의 주의사항이 존재합니다.  
1. 짧은 데이터, 규칙적이지 않은 데이터가 많은 데이터일 경우, 압축량이 줄어들 수 있습니다.(반복되는 데이터가 많을수록 압축효율이 증가합니다.)
2. 데이터를 암호화, 난독화, 압축화를 하여 string으로 만들어 보낼 경우, 압축 효율이 10% 미만이 발생할 수 있습니다.
3. 압축화하려는 데이터가 {\"column\":\"데이터 이름\", \"gold\", \"100\"} 와 같이 json을 string으로 바꾼 값일 경우, 데이터를 불러올 때에는 \" 값이 사라진 값으로 불러와집니다.  
4. 압축데이터가 존재하는 테이블을 불러올 때에는 언마샬링이 진행되서 불러옵니다. `["S"]`, `["N"]`, `["L"]`등의 데이터 타입을 나타내는 컬럼이 삭제됩니다.  
5. 트랜잭션 이용 시, 압축데이터가 들어간 테이블이 포함되어있다면 해당 리턴값 전부 언마샬링되어 리턴됩니다.  
6. 따라서 압축데이터를 이용하고자 할 경우, 게임 정보 불러오기 관련 함수는 GetFlattenJSON() 혹은 FlattenRows()를 통해 Json을 파싱해주시기 바랍니다.  

### [언마샬](/sdk-docs/backend/base/knowhow/object-for-return/unmarshalled)이란?
서버에는 별도의 자료형이 존재하지 않아, 데이터 앞에 구분값인 "S", "N", "L"등의 데이터 타입을 나타냅니다.  
아래 기본데이터를 보시면 inDate는 string형이므로, "S"로 데이터형이 표시되었습니다.  
그리하여 해당 데이터를 가져오려면 `json["Responses"][0]["inDate"]["S"].ToString()`과 같이 접근해야합니다.  
그러나 클라이언트에서 inDate가 string이 아닌 number형 혹은 list형으로 바뀌는 것이 아니라고 하면, 해당 값은 언제나 string으로 바꿔주면 됩니다. 데이터 타입을 가르키는 "S"는 불필요하게 됩니다.  

언마샬은 이런 데이터 타입을 지우는 기능입니다.  
언마샬이 진행되면 기본 데이터는 아래 언마샬된 데이터처럼 변경됩니다.  
그리고 inDate의 값을  `json["Responses"][0]["inDate"].ToString()`과 같이 좀 더 쉽게 접근할 수 있습니다.  

압축화된 테이블을 불러올 때에는 언마샬이 이미 진행되어 리턴이 됩니다. 압축화된 테이블이 없다면 데이터 타입이 존재하는 값이 리턴됩니다.  
이러한 리턴 케이스에 따른 상황 처리를 줄이고자 언마샬로 리턴되는 GetFlattenJson() 혹은 FlattenRows()를 이용해주시면 좀 더 편하게 이용하실 수 있습니다. 

또한 압축형 데이터의 원본이 "{\"column\":\"데이터 이름\", \"gold\", \"100\"}"와 같이 json을 string으로 바꾼 값이라면 이후 데이터를 불러올 경우 \" 형식이 사라지고 "{"column":"데이터 이름", "gold", "100"}"로 표시됩니다.  

**기본 데이터**
```js
{
    "Responses": [
        {
            "inDate": {
                "S": "2023-07-21T10:33:34.160Z"
            }
        }
    ]
}
```

**언마샬된 데이터**
```js
{
    "Responses": [
        {
            "inDate": "2023-07-21T10:33:34.160Z"
        }
    ]
}
```

## 1. 압축량
뒤끝의 압축화는 서버에 전송하려는 데이터의 공통된 문자열을 하나의 문자로 치환하여 데이터 길이를 줄이는 방식입니다.  
만약 데이터의 대부분이 공통되는 문자라면 압축률이 더욱 상승합니다.  
그러 데이터의 크기가 작을 경우에는 치환되는 문자가 적어 데이터 크기의 큰 폭을 기대하기 어렵습니다.  

### Good Cases

데이터의 큰 폭을 기대할 수 있는 데이터 예시는 다음과 같습니다.  
* List, Dictionary

#### 200글자, 200byte 이상의 데이터
```js
[12345, 67890, 23456, 78901, 34567, 89012, 45678, 90123, 56789, 01234, 67890, 23456, 78901, 34567, 89012, 45678, 90123, 56789, 01234, 67890, 23456, 78901, 34567, 89012, 45678, 90123, 56789, 01234, 67890, 23456]
```

#### 공통된 데이터가 많은 경우
```js
[1,1,1,1,1,1,1,0,0,0,3,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0]
```

```js
[
  {"jewel_pack": 872},
  {"diamond_box": 321},
  {"gold_bar": 582},
  {"mystery_box": 250},
  {"rare_gem": 749},
  {"epic_boost": 527},
  {"health_potion": 302},
  {"magic_scroll": 429},
  {"armor_upgrade": 691},
  {"sword_polish": 174},
  {"shield_enhancer": 463},
  {"rare_seed": 844},
  {"legendary_key": 516},
  {"mystic_feather": 287},
  {"sacred_totem": 668},
  {"power_orb": 253},
  {"moon_pearl": 796},
  {"sun_crystal": 369},
  {"star_fragment": 143},
  {"void_stone": 621},
  {"elixir_life": 395}
]
```

### Bad Cases
다음은 압축데이터에 부적절한 데이터 케이스입니다.

#### 데이터 크기가 작을 경우(200 byte 미만)
```js
[1,0,0,0,0,0,0,1,1,2,0]
```

```js
24133232412325234002321230000235123421
```

```js
{
  "username": "Player123",
  "level": 25,
  "class": "warrior",
  "guild": "DragonSlayers",
  "last_active": "2023-07-25"
}
```

#### 암호화,난독화,압축화가 진행된 경우
압축 로직은 중복되는 문자열이 많을수록 많이 압축이 됩니다. 따라서 비슷한 key가 나열되거나 대괄호가 있는 List, Dic의 경우 압축량이 뛰어납니다.  
다만 암호화, 난독화가 진행될 경우, 중복되지 않는 문자열이 표시되어 압축량이 줄어들 수 있습니다.  

```js
{
    userInfo : "eyJpc3MiOiJodHRwczovL2FjY291bnRzLmdvb2dsZS5jbdaskjdfjfjkadiOiI0OTMyMDYzOTIyMzMtYzQ0bmlnb3BkNG5tZ2ltaGtyamQ3b2o0YjZ2cnBuYnIuYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJhdWQiOiI0OTMyMDYzOTIyMzMtbjI0ZGgyYXV0N2s1dWttMmsyYXVubnIxcnY5MHBlbWouYXBwcy5nb29nbGV1c2VyY29udGVudC5jb20iLCJzdWIiOiIxMTEyMTUyNjI5NzA0OTg2MTE4OTEiLCJlbWFpbCI6ImhocnNzMDQxN0BnbWFpbC5jb20iLCJlbWFSdgsdjlsdjfklkdjks1ZSwicGljdHVyZSI6Imh0dHBzOi8vbGgzLmdvb2dsZXVzZXJjb250ZW50LmNvbS9hLS9BTFYtVWpXUDZXbVdZdkRYMWx5OXlrVEsxYnhxel9LM0hXX0lEN0JiZndndmRCYk01NlU9czk2LWMiLCJpYXQiOjE3MDI4NzMyNTIsImV4cCI6MTcwMjg3Njg1Mn0",
    nickname : "backend"
}
```
#### json을 string으로 변환한 값을 사용하고자 할 경우
압축형 데이터가 다음과 같은 json을 string으로 변환한 경우, 압축된 데이터가 풀어지는 로직에서 자동으로 이스케이프 문자열이 해제되면서, **Object 형태로 변환**이 됩니다.  

**기존 데이터 접근 방식**
```js
JsonData["rows"][0]["list"]["S"].ToString();
```

**기존 데이터 JSON 데이터**
```js
{
    "serverTime": "2023-12-20T12:03:25.944Z",
    "rows": [
        {
            "inDate": {
                "S": "2023-12-20T12:03:25.901Z"
            },
            "owner_inDate": {
                "S": "2023-03-14T05:17:08.383Z"
            },
            "list": {
                "S": "{\"updated\":\"2010-01-07T19:58:42.949Z\",\"totalItems\":800,\"startIndex\":1,\"itemsPerPage\":1,\"items\":[{\"id\":\"hYB0mn5zh2c\",\"uploaded\":\"2007-06-05T22:07:03.000Z\",\"updated\":\"2010-01-07T13:26:50.000Z\",\"uploader\":\"GoogleDeveloperDay\",\"category\":\"News\",\"title\":\"Google Developers Day US - Maps API Introduction\",\"description\":\"Google Maps API Introduction ...\",\"tags\":[\"GDD07\",\"GDD07US\",\"Maps\"],\"duration\":2840,\"aspectRatio\":\"widescreen\",\"rating\":4.63,\"ratingCount\":68,\"viewCount\":220101,\"favoriteCount\":201,\"commentCount\":22,\"status\":{\"value\":\"restricted\",\"reason\":\"limitedSyndication\"},\"accessControl\":{\"syndicate\":\"allowed\",\"commentVote\":\"allowed\",\"rate\":\"allowed\",\"list\":\"allowed\",\"comment\":\"allowed\",\"embed\":\"allowed\",\"videoRespond\":\"moderated\"}}]}"
            }
        }
    ]
}
```

**동일한 데이터를 압축형으로 지정했을 경우**
```js
JsonData["rows"][0]["list"]["apiVersion"].ToString();

JsonData["rows"][0]["list"]["data"]["updated"].ToString();
```

**동일한 데이터의 압축 이후 JSON 표시 방식**
```js
{
    "serverTime": "2023-12-20T12:07:56.878Z",
    "rows": [
        {
            "inDate": "2023-12-20T12:07:56.795Z",
            "owner_inDate": "2023-03-14T05:17:08.383Z",
            "list": {
                "apiVersion": "2.0",
                "data": {
                    "updated": "2010-01-07T19:58:42.949Z",
                    "totalItems": 800,
                    "itemsPerPage": 1,
                    "items": [
                        {
                            "id": "hYB0mn5zh2c",
                            "uploaded": "2007-06-05T22:07:03.000Z",
                            "updated": "2010-01-07T13:26:50.000Z",
                            "uploader": "GoogleDeveloperDay",
                            "category": "News",
                            "title": "Google Developers Day US - Maps API Introduction",
                            "description": "Google Maps API Introduction ...",
                            "tags": [
                                "GDD07",
                                "GDD07US",
                                "Maps"
                            ],
                            "duration": 2840,
                            "aspectRatio": "widescreen",
                            "rating": 4.63,
                            "ratingCount": 68,
                            "viewCount": 220101,
                            "favoriteCount": 201,
                            "commentCount": 22,
                            "status": {
                                "value": "restricted",
                                "reason": "limitedSyndication"
                            }
                        }
                    ]
                }
            }
        }
    ]
}
```


## 2. Json 파싱 언마샬로 진행하기
압축데이터를 사용할 경우, 압축 여부에 따라 데이터 리턴 방식이 달라지는 것에 대한 대책으로, 모든 데이터를 언마샬링하여 진행하는 방식이 있습니다.  

### 예시1. 기존 제공하던 데이터형, 압축데이터가 존재하지 않는 테이블 불러오기
```js
{
    "Responses": [
        {
            "inDate": {
                "S": "2023-07-21T10:33:34.160Z"
            },
            "updatedAt": {
                "S": "2023-07-21T10:33:34.161Z"
            },
            "owner_inDate": {
                "S": "2023-07-20T10:34:14.843Z"
            },
            "list": {
                "L": [
                    {
                        "S": "SJE2MDUFZHCH"
                    },
                    {
                        "S": "ARM27CDGLPFT"
                    },
                    {
                        "S": "PWR44N90ZDAB"
                    }
                ]
            },
            "key": {
                "M": {
                    "key1": {
                        "S": "8EWRPEASL4R5"
                    },
                    "key2": {
                        "S": "C145FSBDDVMZ"
                    },
                    "key5": {
                        "S": "TEUGC77N164Z"
                    },
                    "key3": {
                        "S": "WZLMIMS7QK33"
                    },
                    "key4": {
                        "S": "QV1GOXHVFV2D"
                    }
                }
            }
        }
    ]
}
```

접근하는 방법은 다음과 같습니다.  

**일반 Json**
```js
string data = bro.GetReturnValuetoJSON()["Responses"][0]["list"]["L"][0]["S"].ToSTring();
Debug.Log("list의 첫번쨰 배열값 : :" + data);
```

**언마샬된 Json**
```js
string data = bro.GetFlattenJson()["Responses"][0]["list"][0].ToSTring();
Debug.Log("list의 첫번쨰 배열값 : :" + data);
```

### 예시2. 압축데이터가 존재하는 테이블 불러오기

```js
{
    "Responses": [
        {
            "inDate": "2023-07-21T08:55:52.972Z",
            "updatedAt": "2023-07-21T08:55:52.975Z",
            "owner_inDate": "2023-07-20T10:33:26.32Z",
            "map": {
                "key1": "VqHgm87lMAvLZd9GgraW",
                "key2": "fNc8KFnZ8m0GlXYziAzt",
                "key5": "PGKKeeG4h6Yjqy7kwpf9",
                "key3": "a8xPJv5W372T5hXF0I4O",
                "key4": "yto9H0p3s1r1yJxQKWFo"
            },
            "list": [
                "VqHgm87lMAvLZd9GgraW",
                "fNc8KFnZ8m0GlXYziAzt",
                "yto9H0p3s1r1yJxQKWFo",
                "uGZVN6rAgsUrZRNbEAEg",
                "PGKKeeG4h6Yjqy7kwpf9",
                "a8xPJv5W372T5hXF0I4O",
                "J14wpMYHPbcrTXRA5fIk",
                "rSs0OC4NVdJbfdZxZWLb",
                "X5tBLDnQgg91PabRLNnm",
            ]
        }
    ]
}
```

**일반 Json**
```js
string data = bro.GetReturnValuetoJSON()["Responses"][0]["list"][0].ToSTring();
Debug.Log("list의 첫번쨰 배열값 : :" + data);
```

**언마샬된 Json**
```js
string data = bro.GetFlattenJson()["Responses"][0]["list"][0].ToSTring();
Debug.Log("list의 첫번쨰 배열값 : :" + data);
```

이처럼 언마샬된 데이터로 접근을 한다면 데이터 타입에 관계없이 동일한 로직으로 동일한 접근을 하실 수 있습니다.
