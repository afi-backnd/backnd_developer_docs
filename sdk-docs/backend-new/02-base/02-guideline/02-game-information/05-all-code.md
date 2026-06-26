---
description: "게임 정보 전체 코드"
---




# 게임 정보 전체 코드

## BackendGameData.cs
```js
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class UserData
{
    public int level = 1;
    public float atk = 3.5f;
    public string info = string.Empty;
    public Dictionary<string, int> inventory = new Dictionary<string, int>();
    public List<string> equipment = new List<string>();

    // 데이터를 디버깅하기 위한 함수입니다.(Debug.Log(UserData);)
    public override string ToString()
    {
        StringBuilder result = new StringBuilder();
        result.AppendLine($"level : {level}");
        result.AppendLine($"atk : {atk}");
        result.AppendLine($"info : {info}");

        result.AppendLine($"inventory");
        foreach (var itemKey in inventory.Keys)
        {
            result.AppendLine($"| {itemKey} : {inventory[itemKey]}개");
        }

        result.AppendLine($"equipment");
        foreach (var equip in equipment)
        {
            result.AppendLine($"| {equip}");
        }

        return result.ToString();
    }
}

public class BackendGameData
{
    private static BackendGameData _instance = null;

    public static BackendGameData Instance
    {
        get
        {
            if (_instance == null)
            {
                _instance = new BackendGameData();
            }

            return _instance;
        }
    }

    public static UserData userData;

    private string gameDataRowInDate = string.Empty;

    public async Task GameDataInsert()
    {
        if (userData == null)
        {
            userData = new UserData();
        }

        Debug.Log("데이터를 초기화합니다.");
        userData.level = 1;
        userData.atk = 3.5f;
        userData.info = "친추는 언제나 환영입니다.";

        userData.equipment.Add("전사의 투구");
        userData.equipment.Add("강철 갑옷");
        userData.equipment.Add("헤르메스의 군화");

        userData.inventory.Add("빨간포션", 1);
        userData.inventory.Add("하얀포션", 1);
        userData.inventory.Add("파란포션", 1);

        Debug.Log("뒤끝 업데이트 목록에 해당 데이터들을 추가합니다.");
        Param param = new Param();
        param.Add("level", userData.level);
        param.Add("atk", userData.atk);
        param.Add("info", userData.info);
        param.Add("equipment", userData.equipment);
        param.Add("inventory", userData.inventory);

        Debug.Log("게임 정보 데이터 추가를 요청합니다.");
        var reqResult = await BackndUserData.Instance.CreateDataAsync("USER_DATA", param);
        if (reqResult.IsSuccess())
        {
            Debug.Log("게임 정보 데이터 추가에 성공했습니다. : " + reqResult);

            //삽입한 게임 정보의 고유값입니다.  
            gameDataRowInDate = reqResult.GetRowInDate();
        }
        else
        {
            Debug.LogError("게임 정보 데이터 추가에 실패했습니다. : " + reqResult);
        }
    }

    public async Task GameDataGet()
    {
        Debug.Log("게임 정보 조회 함수를 호출합니다.");
        
        var reqResult = await BackndUserData.Instance.GetDataAsync("USER_DATA");
        if (reqResult.IsSuccess())
        {
            Debug.Log("게임 정보 조회에 성공했습니다. : " + reqResult);

            // Json으로 리턴된 데이터를 받아옵니다.  
            var gameDataJson = reqResult.GetRows();

            // 받아온 데이터의 갯수가 0이라면 데이터가 존재하지 않는 것입니다.  
            if (gameDataJson.Count <= 0)
            {
                Debug.LogWarning("데이터가 존재하지 않습니다.");
            }
            else
            {
                gameDataRowInDate = gameDataJson[0]["inDate"].ToString(); //불러온 게임 정보의 고유값입니다.  

                userData = new UserData();
                userData.level = int.Parse(gameDataJson[0]["level"].ToString());
                userData.atk = float.Parse(gameDataJson[0]["atk"].ToString());
                userData.info = gameDataJson[0]["info"].ToString();
                
                var inventoryJson = gameDataJson[0]["inventory"] as JObject;
                foreach (var property in inventoryJson.Properties())
                {
                    var itemKey = property.Name;
                    var itemValue = property.Value;
                    userData.inventory.Add(itemKey, int.Parse(itemValue.ToString()));
                }

                var equipmentJson = gameDataJson[0]["equipment"] as JArray;
                foreach (var equip in equipmentJson)
                {
                    userData.equipment.Add(equip.ToString());
                }

                Debug.Log(userData.ToString());
            }
        }
        else
        {
            Debug.LogError("게임 정보 조회에 실패했습니다. : " + reqResult);
        }
    }

    public void LevelUp()
    {
        Debug.Log("레벨을 1 증가시킵니다.");
        userData.level += 1;
        userData.atk += 3.5f;
        userData.info = "내용을 변경합니다.";
    }

    public async Task GameDataUpdate()
    {
        if (userData == null)
        {
            Debug.LogError("서버에서 다운받거나 새로 삽입한 데이터가 존재하지 않습니다. Insert 혹은 Get을 통해 데이터를 생성해주세요.");
            return;
        }

        Param param = new Param();
        param.Add("level", userData.level);
        param.Add("atk", userData.atk);
        param.Add("info", userData.info);
        param.Add("equipment", userData.equipment);
        param.Add("inventory", userData.inventory);

        RequestResult reqResult = null;
        if (string.IsNullOrEmpty(gameDataRowInDate))
        {
            Debug.Log("내 제일 최신 게임 정보 데이터 수정을 요청합니다.");

            reqResult = await BackndUserData.Instance.UpdateLatestDataAsync("USER_DATA", param);
        }
        else
        {
            Debug.Log($"{gameDataRowInDate}의 게임 정보 데이터 수정을 요청합니다.");

            reqResult = await BackndUserData.Instance.UpdateDataAsync("USER_DATA", gameDataRowInDate, param);
        }

        if (reqResult.IsSuccess())
        {
            Debug.Log("게임 정보 데이터 수정에 성공했습니다. : " + reqResult);
        }
        else
        {
            Debug.LogError("게임 정보 데이터 수정에 실패했습니다. : " + reqResult);
        }
    }
}
```

## BackendManager.cs
```js
using UnityEngine;
// 뒤끝 SDK namespace 추가
using BACKND.Base;

public class BackendManager : MonoBehaviour
{
    // Start is called before the first frame update
    async void Start()
    {
        // 뒤끝 초기화
        var reqResult = await BackndBase.InitializeAsync();
        // 뒤끝 초기화에 대한 응답값
        if (reqResult.IsSuccess())
        {
            Debug.Log("초기화 성공 : " + reqResult); // 성공일 경우 statusCode 204 Success
        }
        else
        {
            Debug.LogError("초기화 실패 : " + reqResult); // 실패일 경우 statusCode 400대 에러 발생
        }

        Test();
    }


    async void Test()
    {        
        await BackendLogin.Instance.CustomSignIn("user1", "1234"); // 뒤끝 로그인
        await BackendGameData.Instance.GameDataGet(); //[추가] 데이터 불러오기 함수

        // [추가] 서버에 불러온 데이터가 존재하지 않을 경우, 데이터를 새로 생성하여 삽입
        if (BackendGameData.userData == null)
        {
            await BackendGameData.Instance.GameDataInsert();
        }

        BackendGameData.Instance.LevelUp(); // [추가] 로컬에 저장된 데이터를 변경
        await BackendGameData.Instance.GameDataUpdate(); //[추가] 서버에 저장된 데이터를 덮어쓰기(변경된 부분만)

        Debug.Log("테스트를 종료합니다.");
    }
}
```

<div className="linked_button">
    <a href="/sdk-docs/backend/base/guideline/rank/before">다음 챕터로</a>
</div>
