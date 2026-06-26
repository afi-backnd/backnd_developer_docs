# 데이터의 자료형

뒤끝에 **저장된 모든 데이터는 string 형태**입니다.  
이때 해당 데이터의 자료형이 무엇인지 인식할 수 있도록 **모든 데이터는 자료형을 키 값**으로 가지고 있습니다.  

| 구분        | 자료형 | 설명  |
| :------------ | :------------- | :----- |
| BOOL | bool | boolean 형태의 데이터가 이에 해당됩니다. |
| N | numbers | int, float, double 등 모든 숫자형 데이터는 이에 해당됩니다. |
| S | string | string 형태의 데이터가 이에 해당됩니다. |
| L | list | list 형태의 데이터가 이에 해당됩니다. |
| M | map | map, dictionary 형태의 데이터가 이에 해당됩니다. |
| NULL | null | 값이 존재하지 않는 경우 이에 해당됩니다. |

## 리턴되는 json value의 자료형 알아내기

### Jsondata의 key에 해당하는 자료형을 리턴해주는 메소드
``` js
public static string WhichDataTypeIsIt(LitJson.JsonData data, string key)
{
    if(data.Keys.Contains(key))
    {
        if(data[key].Keys.Contains("S")) // string
            return "S";
        else if(data[key].Keys.Contains("N")) // number
            return "N";
        else if(data[key].Keys.Contains("M")) // map
            return "M";
        else if(data[key].Keys.Contains("L")) // list
            return "L";
        else if(data[key].Keys.Contains("BOOL")) // boolean
            return "BOOL";
        else if(data[key].Keys.Contains("NULL")) // null
            return "NULL";
        else
            return null;
    }
    else
    {
        return null;
    }
}
```

### list 형태의 JsonData의 n 번째 값에 해당하는 자료형을 리턴해주는 메소드

```js
public static string WhichDataTypeIsIt(LitJson.JsonData data, int n)
{
    if(data.IsArray)
    {
        if(data[n].Keys.Contains("S")) // string
            return "S";
        else if(data[n].Keys.Contains("N")) // number
            return "N";
        else if(data[n].Keys.Contains("M")) // map
            return "M";
        else if(data[n].Keys.Contains("L")) // list
            return "L";
        else if(data[n].Keys.Contains("BOOL")) // boolean
            return "BOOL";
        else if(data[n].Keys.Contains("NULL")) // null
            return "NULL";
        else
            return null;
    }
    else
    {
        return null;
    }
}
```
