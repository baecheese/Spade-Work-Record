# [swift] Change UIButton image tint color
## 📌 문제상황
- 네비게이션 아이템 안에 customview로 이미지 버튼을 넣었는데, tintcolor가 설정이 안되었다.

1. 첫 번째 착각
처음에는 navigation item tint color를 계속 설정하다가 왜 안되지 생각해보니,
navigation item 안에 넣은 UIButton의 tint color를 설정해야하는 것이었다. (....바보냐..)

2. 두 번째 착각
button의 tint color설정을 해도 안되어서 찾아보니,
button의 image tint color를 바꾸기 위해서는, UIImageRenderingMode를 **.alwaysTemplate** 로 설정해줘야 한다.

### 📌 UIImageRenderingMode

``` swift
@available(iOS 7.0, *)
public enum UIImageRenderingMode : Int {
  case automatic // Use the default rendering mode for the context where the image is used
  case alwaysOriginal // Always draw the original image, without treating it as a template
  case alwaysTemplate // Always draw the image as a template image, ignoring its color information
}

```

```
case automatic // 이미지가 사용 된 컨텍스트에 기본 렌더링 모드 사용
case alwaysOriginal // 항상 원본 이미지를 템플릿으로 처리하지 않고 그립니다.
case alwaysTemplate // 항상 색상 정보를 무시하고 이미지를 템플릿 이미지로 그립니다.
```


### 🐬 해결 code

``` swift

func makeNavigationItem()  {
  let backBtn = UIButton(frame: CGRect(x: 0, y: 0, width: 20, height: 20))
  let back = UIImage(named: "back")?.withRenderingMode(.alwaysTemplate) //🌟
  backBtn.setImage(back, for: .normal)
  backBtn.tintColor = .white
  backBtn.addTarget(self, action: #selector(WriteViewController.back), for: .touchUpInside)
  let item2 = UIBarButtonItem(customView: backBtn)       
  navigationItem.leftBarButtonItem = item2
}

```


