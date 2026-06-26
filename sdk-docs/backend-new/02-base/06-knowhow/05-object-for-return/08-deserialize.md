---
description: "디시리얼라이즈"
---

# 디시리얼라이즈

언마샬 한 데이터를 다시 클래스로 디시리얼라이즈 하는 방법입니다.  
언마샬 한 데이터는 Newtonsoft Json 라이브러리를 이용하여 손쉽게 디시리얼라이즈 할 수 있습니다.  
> 디시리얼라이즈 기능은 뒤끝에서 제공하는 기능이 아닌 Newtonsoft Json 라이브러리를 이용한 기능입니다.  
자세한 설명은 [Newtonsoft API](https://www.newtonsoft.com/json/help/html/Overload_Newtonsoft_Json_JsonConvert_DeserializeObject.htm)를 확인해 주세요.  


정상적으로 클래스로 디시리얼라이즈 하기 위해서는 아래 조건을 만족해야 합니다.  
* 디시리얼라이즈 결과로 리턴되는 클래스에 포함되기를 원하는 변수가 **public**이어야 합니다.  
* 서버에서 조회한 데이터의 Key 값과 클래스의 변수가 **대소문자까지 동일**해야 합니다.  
조회한 데이터의 Key 값이 Name인데 클래스의 변수명은 name일 경우 실행환경에 따라 디시리얼라이즈가 되지 않을 수 있습니다.  

리턴 값에는 Key가 포함되어 있지만, 클래스에 해당 변수가 없거나 혹은 public이 아닌 경우 디시리얼라이즈 결과에 포함되지 않습니다.  



### example
```js
public class Item
{
    public string name = string.Empty;
    public int atk = 0;
    public int def = 0;
    public double critical = 1.0;
    public int[] socket = new int[3];
    public Dictionary<string, string> option = new Dictionary<string, string>();
    public override string ToString()
    {
        var arr = string.Join(", ", socket);
        string dic = string.Join(", ", option.Select(x => x.Key + ":" + x.Value).ToArray());
        return string.Format("name: {0}\tatk: {1}\tdef: {2}\tcriticla: {3}\tsocket: {4}\toption: {5}",
            name, atk, def, critical, arr, dic);
    }
}

public void Deserialize()
{
    // 인벤토리 테이블 100개 조회
    var reqResult = await BackndUserData.Instance.GetDataAsync("inventory", 100);
    if (reqResult.IsSuccess() == false)
    {
        return;
    }

    // 조회한 데이터를 언마샬
    var json = reqResult.GetRows();
    for (int i = 0; i < json.Count; ++i)
    {
        // 데이터를 디시리얼라이즈 & 데이터 확인
        var item = JsonConvert.DeserializeObject<Item>(json[i].ToString());
        Debug.Log(item.ToString());
    }
}
```
