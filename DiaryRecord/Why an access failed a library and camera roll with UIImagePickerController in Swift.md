# 🐥 Check the setting an access privacy data in Info.plist

![](https://github.com/baecheese/DiaryRecord/blob/master/Plan/note/Photo%20Library%20Usage%20Description/error.png?raw=true)

아무리 코드를 보아도 틀린 것이 없는데,

계속 라이브러리와 카메라롤 접근이 안되서 하루를 낭비했다.

에러 메세지도 안찍히고.. 너무 이상하다 싶어서 새로운 프로젝트를 만들어서

같은 걸 해보니 저렇게 로그가 찍혔다.

저 메세지를 바탕으로 아주 간단한 방법으로 해결을 할 수 있었다.

## 🐬 해결방법
### - Check the setting an access privacy data in Info.plist

![](https://github.com/baecheese/DiaryRecord/blob/writePage/imageBox/Plan/note/Photo%20Library%20Usage%20Description/setting.png?raw=true)


1. plist에 가서 information property list에서
2. **Privacy - Camera Usage Description** 와 **Privacy - Photo Library Usage Description**를 추가한다.

![](https://github.com/baecheese/DiaryRecord/blob/writePage/imageBox/Plan/note/Photo%20Library%20Usage%20Description/use.png)

해결~~