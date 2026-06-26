---
sidebar_label: 캐시 아이템 리스트 조회
---

# GetProductList

public BackendReturnObject **GetProductList**();

:::warning TBC 기능 제공 중단 안내
스토어 영수증 검증 API의 최신화에 따라 기존 방식과의 호환성 문제 및 기능 전반에 대한 개선 필요 사항이 확인되어, 기능 제공이 중단되었습니다.

**SDK 5.18.7 이하 버전에서는 기존과 동일하게 TBC 기능을 계속 이용하실 수 있습니다.**
:::

## 설명

뒤끝 콘솔에 등록한 게임 캐시 아이템 리스트를 조회합니다.  

## Example

### 동기

```js
Backend.TBC.GetProductList();
```

### 비동기

```js
Backend.TBC.GetProductList((callback) => {
  // 이후 처리
});
```

### SendQueue

```js
SendQueue.Enqueue(Backend.TBC.GetProductList, (callback) => {
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : GetReturnValuetoJSON 참조

### Error cases

**뒤끝 콘솔에 등록한 제품이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : product not found, product을(를) 찾을 수 없습니다

## GetReturnValuetoJSON

```js
{
    rows:
    [
        {
            TBC: { N : 3300 }, // TBC 값
            inDate: { S : "2018-03-06T02:26:09.526Z"}, // 캐시 아이템 indate
            uuid: { S : "bb35b960-20e5-11e8-8fdb-4928c1afeae2" }, // 캐시 아이템 uuid
            name: { S : "루덴의 메아리" }, // 캐시 아이템명
            explain: { S : "희귀템임" } // 캐시 아이템 설명
        },
        {
            TBC: [Object],
            inDate: [Object],
            uuid: [Object],
            name: [Object],
            explain: [Object]
        }
}
```

## Sample Code

```js
public class ProductItem
{
    public string TBC;
    public string inDate;
    public string uuid;
    public string name;
    public string explain;
    public override string ToString()
    {
        return $"TBC : {TBC}\ninDate : {inDate}\nuuid : {uuid}\nname : {name}\nexplain : {explain}\n";
    }
};
```

```js
public void GetProductList()
{
    var bro = Backend.TBC.GetProductList();

    LitJson.JsonData json = bro.FlattenRows();
    List<ProductItem> productList = new List<ProductItem>();

    for(int i = 0; i < json.Count; i++)
    {
        ProductItem product = new ProductItem();

        product.TBC = json[i]["TBC"].ToString();
        product.inDate = json[i]["inDate"].ToString();
        product.uuid = json[i]["uuid"].ToString();
        product.name = json[i]["name"].ToString();
        product.explain = json[i]["explain"].ToString();

        productList.Add(product);
        Debug.Log(product.ToString());
    }
}
```
