---
sidebar_label: 캐시 아이템 리스트 조회
---

# GetProducts

public Task< GetProductsResult > **GetProductsAsync**();

## 설명

뒤끝 콘솔에 등록한 게임 캐시 아이템 리스트를 조회합니다.  

## Example

### Task 방식

```js
var reqResult = await BackndTBC.Instance.GetProductsAsync();
```

### Callback 방식

```js
BackndTBC.Instance.GetProducts((callback) =>
{
  // 이후 처리
});
```

## ReturnCase

### Success cases

**조회에 성공한 경우**  
statusCode : 200  
message : Success  
returnValue : ReturnValueJson 참조

### Error cases

**뒤끝 콘솔에 등록한 제품이 없는 경우**  
statusCode : 404  
errorCode : NotFoundException  
message : product not found, product을(를) 찾을 수 없습니다

## ReturnValueJson

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
public async Task GetProductList()
{
    var reqResult = await BackndTBC.Instance.GetProductsAsync();
    var infoList = reqResult.GetInfoList();
    var productList = new List<ProductItem>();

    for (int i = 0; i < infoList.Count; i++)
    {
        var info = infoList[i];

        ProductItem product = new ProductItem();
        product.TBC = info.TBC;
        product.inDate = info.InDate;
        product.uuid = info.Uuid;
        product.name = info.Name;
        product.explain = info.Explain;

        productList.Add(product);
        Debug.Log(product.ToString());
    }
}
```
