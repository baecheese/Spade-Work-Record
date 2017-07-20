# Circular Reference

### 🕷  문제 상황
1. 특별하게 바꾼 것이 없던 것 같은데, 앱이 LaunchScreen에서 멈춘다.
2. 에러 코드가 찍히지 않는다.
3. appdelegate에서 didFinishLaunchingWithOptions에 breakpoint를 찍어도 들어오지 않는다.

### 📌 원인
- commit을 따라가보니 서로 순환참조(Circular Reference)를 하고 있었다.

### 📌  문제 코드

``` swift
class SpecialDayRepository: NSObject {

  private let wedgetManager = WedgetManager.sharedInstance // 🕷

  private override init() {
    super.init()
  }
  static let sharedInstance: SpecialDayRepository = SpecialDayRepository()
}

```

``` swift
class WedgetManager: NSObject {
  
  let specialDayRepository = SpecialDayRepository.sharedInstance // 🕷
    
  private override init() {
     super.init()
  }
  static let sharedInstance: WedgetManager = WedgetManager()
```

- 🕷 위 코드를 보면 SpecialDayRepository에서 WedgetManager 싱글톤 객체를 만들고, 반대로 WedgetManager에서 SpecialDayRepository 싱글톤 객체를 만든다. 순환참조로 인한 메모리 누수가 일어나는 것. 🕷

### 📌 해결
- 순환 참조가 일어나지 않는 구조로 바꾸었다. 한쪽을 삭제.
- Delete circular reference code.

### 📌 해결 코드

``` swift
class SpecialDayRepository: NSObject {

// 문제 코드 삭제 (code delete)

  private override init() {
    super.init()
  }
  static let sharedInstance: SpecialDayRepository = SpecialDayRepository()
}

```

``` swift
class WedgetManager: NSObject {
  
  let specialDayRepository = SpecialDayRepository.sharedInstance // ✨
    
  private override init() {
     super.init()
  }
  static let sharedInstance: WedgetManager = WedgetManager()
```

이 외에도 순환참조 문제를 해결하기 위한 방법들이 있다. 아래 링크 참고.
나는 weak로 변경했을 때, 에러가 나서 못했다. 할줄 몰라서 그런 거일수도..

    error message : 'weak' must be a mutable variable, because it may change at runtime


### 🐍 [etc] additional data about weak
- http://hiddenviewer.tistory.com/241
- http://frederiknakstad.com/2016/04/18/weak-singleton-in-swift/