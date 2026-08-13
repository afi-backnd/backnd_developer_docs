---
sidebar_label: "CountryCode"
description: "CountryCode"
---

# CountryCode

## 설명
CountryCode는 뒤끝 SDK에서 국가 코드를 등록할 때 사용하는 enum 값입니다.  
유저와 길드가 국가 코드를 가지고 있으면 콘솔에서 각 국가별로 필터링해서 검색할 수 있습니다.  
뒤끝은 총 196개국의 국가 코드를 제공합니다.  

CountryCode를 사용하기 위해서는 아래 구문을 스크립트 상단에 추가합니다.  
```csharp
using BackEnd.GlobalSupport;
```

CountryCode 값은 아래와 같습니다.  

|국가명(공식 한국어 명칭)|	국가명(영문)| CountryCode | 국가 코드(ISO 3166-1 alpha-2) |
| :------------ |:-------------|:-------------|
|가나	    |Ghana|Ghana|GH|
|가봉	    |Gabon|Gabon|GA|
|가이아나	| Guyana|Guyana|GY|
|감비아	| Gambia|Gambia|GM|
|과테말라	|    Guatemala|Guatemala|GT|
|그레나다	|    Grenada|Grenada|GD|
|그리스	|    Greece|Greece|GR|
|기니	    |    Guinea|Guinea|GN|
|기니비사우|	    Guinea-Bissau|GuineaBissau|GW|
|나미비아	|    Namibia|Namibia|NA|
|나우루	|    Nauru|Nauru|NR|
|나이지리아|	    Nigeria|Nigeria|NG|
|남수단	|    South Sudan|SouthSudan|SS|
|남아프리카|	South Africa|SouthAfrica|ZA|
|네덜란드	|Netherlands|Netherlands|NL|
|네팔	|Nepal|Nepal|NP|
|노르웨이	|Norway|Norway|NO|
|뉴질랜드	|New Zealand|NewZealand|NZ|
|니제르	|Niger|Niger|NE|
|니카라과|	Nicaragua|Nicaragua|NI|
|대한민국	|Korea, Republic of|SouthKorea|KR|
|덴마크	|Denmark|Denmark|DK|
|도미니카|	Dominica|Dominica|DM|
|도미니카 공화국|	Dominican Republic|DominicanRepublic|DO|
|독일	|Germany|Germany|DE|
|동티모르	|Timor-Leste|TimorLeste|TL|
|라오스	|Lao People's Democratic Republic|Laos|LA|
|라이베리아	|Liberia|Liberia|LR|
|라트비아|	Latvia|Latvia|LV|
|러시아	|Russian Federation|Russian|RU|
|레바논	|Lebanon|Lebanon|LB|
|레소토	|Lesotho|Lesotho|LS|
|루마니아	|Romania|Romania|RO|
|룩셈부르크|	Luxembourg|Luxembourg|LU|
|르완다	|Rwanda|Rwanda|RW|
|리비아	|Libya|Libya|LY|
|리투아니아|	Lithuania|Lithuania|LT|
|리히텐슈타인|	Liechtenstein|Liechtenstein|LI|
|마다가스카르|	Madagascar|Madagascar|MG|
|마셜 제도	|Marshall Islands|MarshallIslands|MH|
|말라위	|Malawi|Malawi|MW|
|말레이시아	|Malaysia|Malaysia|MY|
|말리	|Mali|Mali|ML|
|멕시코	|Mexico|Mexico|MX|
|모나코	|Monaco|Monaco|MC|
|모로코	|Morocco|Morocco|MA|
|모리셔스	|Mauritius|Mauritius|MU|
|모리타니	|Mauritania|Mauritania|MR|
|모잠비크	|Mozambique|Mozambique|MZ|
|몬테네그로	|Montenegro|Montenegro|ME|
|몰도바	|Moldova, Republic of|Moldova|MD|
|몰디브	|Maldives|Maldives|MV|
|몰타	|Malta|Malta|MT|
|몽골	|Mongolia|Mongolia|MN|
|미국	|United States of America|UnitedStates|US|
|미얀마	|Myanmar|Myanmar|MM|
|미크로네시아	|Micronesia(Federated States of)|Micronesia|FM|
|바누아투	|Vanuatu|Vanuatu|VU|
|바레인	|Bahrain|Bahrain|BH|
|바베이도스|	Barbados|Barbados|BB|
|바티칸	|Holy See|HolySee|VA|
|바하마	|Bahamas|Bahamas|BS|
|방글라데시|	Bangladesh|Bangladesh|BD|
|베냉	|Benin|Benin|BJ|
|베네수엘라|	Venezuela(Bolivarian Republic of)|Venezuela|VE|
|베트남	|Viet Nam|VietNam|VN|
|벨기에	|Belgium|Belgium|BE|
|벨라루스	|Belarus|Belarus|BY|
|벨리즈	|Belize|Belize|BZ|
|보스니아 헤르체고비나	|Bosnia and Herzegovina|BosniaAndHerzegovina|BA|
|보츠와나	|Botswana|Botswana|BW|
|볼리비아	|Bolivia(Plurinational State of)|Bolivia|BO|
|부룬디	|Burundi|Burundi|BI|
|부르키나파소	|Burkina Faso|BurkinaFaso|BF|
|부탄	|Bhutan|Bhutan|BT|
|북마케도니아	|North Macedonia|NorthMacedonia|MK|
|불가리아	|Bulgaria|Bulgaria|BG|
|브라질	|Brazil|Brazil|BR|
|브루나이	|Brunei Darussalam|BruneiDarussalam|BN|
|사모아	|Samoa|Samoa|WS|
|사우디아라비아	|Saudi Arabia|SaudiArabia|SA|
|산마리노	|San Marino|SanMarino|SM|
|상투메 프린시페	|Sao Tome and Principe|SaoTomeAndPrincipe|ST|
|세네갈	|Senegal|Senegal|SN|
|세르비아	|Serbia|Serbia|RS|
|세이셸	|Seychelles|Seychelles|SC|
|세인트루시아	|Saint Lucia|SaintLucia|LC|
|세인트빈센트 그레나딘	|Saint Vincent and the Grenadines|SaintVincentAndtheGrenadines|VC|
|세인트키츠 네비스	|Saint Kitts and Nevis|SaintKittsAndNevis|KN|
|소말리아	|Somalia|Somalia|SO|
|솔로몬 제도	|Solomon Islands|SolomonIslands|SB|
|수단	|Sudan|Sudan|SD|
|수리남	|Suriname|Suriname|SR|
|스리랑카	|Sri Lanka|SriLanka|LK|
|스웨덴	|Sweden|Sweden|SE|
|스위스	|Switzerland|Switzerland|CH|
|스페인	|Spain|Spain|ES|
|슬로바키아	|Slovakia|Slovakia|SK|
|슬로베니아	|Slovenia|Slovenia|SI|
|시리아	|Syrian Arab Republic|SyrianArabRepublic|SY|
|시에라리온	|Sierra Leone|SierraLeone|SL|
|싱가포르	|Singapore|Singapore|SG|
|아랍에미리트	|United Arab Emirates|UnitedArabEmirates|AE|
|아르메니아	|Armenia|Armenia|AM|
|아르헨티나	|Argentina|Argentina|AR|
|아이슬란드	|Iceland|Iceland|IS|
|아이티	|Haiti|Haiti|HT|
|아일랜드	|Ireland|Ireland|IE|
|아제르바이잔	|Azerbaijan|Azerbaijan|AZ|
|아프가니스탄	|Afghanistan|Afghanistan|AF|
|안도라	|Andorra|Andorra|AD|
|알바니아	|Albania|Albania|AL|
|알제리	|Algeria|Algeria|DZ|
|앙골라	|Angola|Angolav|AO|
|앤티가 바부다	|Antigua and Barbuda|AntiguaandBarbuda|AG|
|에리트레아	|Eritrea|Eritrea|ER|
|에스와티니	|Eswatini|Eswatini|SZ|
|에스토니아	|Estonia|Estonia|EE|
|에콰도르	|Ecuador|Ecuador|EC|
|에티오피아	|Ethiopia|Ethiopia|ET|
|엘살바도르	|El Salvador|ElSalvador|SV|
|영국	|United Kingdom of Great Britain and Northern Ireland|UnitedKingdom|GB|
|예멘	|Yemen|Yemen|YE|
|오만	|Oman|Oman|OM|
|오스트레일리아	|Australia|Australia|AU|
|오스트리아	|Austria|Austria|AT|
|온두라스	|Honduras|Honduras|HN|
|요르단	|Jordan|Jordan|JO|
|우간다	|Uganda|Uganda|UG|
|우루과이	|Uruguay|Uruguay|UY|
|우즈베키스탄	|Uzbekistan|Uzbekistan|UZ|
|우크라이나	|Ukraine|Ukraine|UA|
|이라크	|Iraq|Iraq|IQ|
|이란	|Iran(Islamic Republic of)|Iran|IR|
|이스라엘	|Israel|Israel|IL|
|이집트	|Egypt|Egypt|EG|
|이탈리아	|Italy|Italy|IT|
|인도	|India|India|IN|
|인도네시아	|Indonesia|Indonesia|ID|
|일본	|Japan|Japan|JP|
|자메이카	|Jamaica|Jamaica|JM|
|잠비아	|Zambia|Zambia|ZM|
|적도 기니	|Equatorial Guinea|EquatorialGuinea|GQ|
|조선민주주의인민공화국	|Korea(Democratic People's Republic of)|NorthKorea|KP|
|조지아	|Georgia|Georgia|GE|
|중국	|China|China|CN|
|중앙아프리카 공화국	|Central African Republic|CentralAfricanRepublic|CF|
|지부티	|Djibouti|Djibouti|DJ|
|짐바브웨	|Zimbabwe|Zimbabwe|ZW|
|차드	|Chad|Chad|TD|
|체코	|Czechia|Czechia|CZ|
|칠레	|Chile|Chile|CL|
|카메룬	|Cameroon|Cameroon|CM|
|카보베르데	|Cabo Verde|CaboVerde|CV|
|카자흐스탄	|Kazakhstan|Kazakhstan|KZ|
|카타르	|Qatar|Qatar|QA|
|캄보디아	|Cambodia|Cambodia|KH|
|캐나다	|Canada|Canada|CA|
|케냐	|Kenya|Kenya|KE|
|코모로	|Comoros|Comoros|KM|
|코스타리카	|Costa Rica|CostaRica|CR|
|코트디부아르	|Côte d'Ivoire|IvoryCoast|CI|
|콜롬비아	|Colombia|Colombia|CO|
|콩고	|Congo|Congo|CG|
|콩고 민주 공화국	|Congo, Democratic Republic of the|DRCongo|CD|
|쿠바	|Cuba|Cuba|CU|
|쿠웨이트	|Kuwait|Kuwait|KW|
|크로아티아	|Croatia|Croatia|HR|
|키르기스스탄	|Kyrgyzstan|Kyrgyzstan|KG|
|키리바시	|Kiribati|Kiribati|KI|
|키프로스	|Cyprus|Cyprus|CY|
|타이완	|Taiwan, Province of China|Taiwan|TW|
|타지키스탄	|Tajikistan|Tajikistan|TJ|
|탄자니아	|Tanzania, United Republic of|Tanzania|TZ|
|태국	|Thailand|Thailand|TH|
|터키	|Turkey|Turkey|TR|
|토고	|Togo|Togo|TG|
|통가	|Tonga|Tonga|TO|
|투르크메니스탄	|Turkmenistan|Turkmenistan|TM|
|투발루	|Tuvalu|Tuvalu|TV|
|튀니지	|Tunisia|Tunisia|TN|
|트리니다드 토바고	|Trinidad and Tobago|TrinidadAndTobago|TT|
|파나마	|Panama|Panama|PA|
|파라과이	|Paraguay|Paraguay|PY|
|파키스탄	|Pakistan|Pakistan|PK|
|파푸아뉴기니	|Papua New Guinea|PapuaNewGuinea|PG|
|팔라우	|Palau|Palau|PW|
|팔레스타인	|Palestine, State of|Palestine|PS|
|페루	|Peru|Peru|PE|
|포르투갈	|Portugal|Portugal|PT|
|폴란드	|Poland|Poland|PL|
|프랑스	|France|France|FR|
|피지	|Fiji|Fiji|FJ|
|핀란드	|Finland|Finland|FI|
|필리핀	|Philippines|Philippines|PH|
|헝가리	|Hungary|Hungary|HU|
