---
sidebar_label: 길드 마스터 기능
---

# 길드 마스터 기능

:::danger 주의
* 길드 마스터 위임, 부 길드 마스터 지명/해제 함수 호출 시 inDate가 아닌 gamerIndate를 이용하여 함수를 호출해야 합니다.  

* GetGuildMemberListV3 함수를 호출하여 얻은 길드 멤버 리스트에는 inData 키와 gamerIndate 키가 들어있습니다.  

* inDate는 길드 가입 시점입니다. 해당 키를 이용하여 함수 호출 시 'statusCode  404 guildMember not found' 이 반환됩니다.  
* 권한이 없는 부 길드 마스터 혹은 길드원이 길드 마스터 함수를 호출할 경우 'statusCode 403, errorCode ForbiddenException'이 반환됩니다.  
:::

길드 마스터는 아래와 같은 기능을 이용할 수 있습니다.  

- 길드명 변경
- 길드 마스터 위임
- 부 길드 마스터 지명
- 부 길드 마스터 해제
- 길드 재화 사용
- 길드 즉시 가입 허용/해제
- 길드 국가 코드 등록/변경
