# LitJSON

## 유니티 빌드 후 LitJSON 경고메시지가 표시되는 경우
유니티 빌드 후 **Assembly 'LitJSON' has non matching file name: 'LitJson.dll'. This can cause build issues on some platforms.** 경고 문구가 발생하는 경우 아래와 같이 하면 해결됩니다.  

1. Assets>TheBackend>Plugins 폴더로 이동
2. LitJson.dll의 파일명을 **LitJSON.dll**로 변경
3. 다시 빌드