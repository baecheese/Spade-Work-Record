# 🛠 set navigation bar color without alpha

* 그냥 네비게이션 바의 배경 컬러 세팅으로 하면 자동으로 알파값이 넣어진 연한 색이 나온다.

* 문제의 코드

``` Swift
self.navigationController?.navigationBar.backgroundColor = .red
```

##  해결
* AppDelegate에서 barTintColor로 조정해주면 됨.


```Swift
func application(_ application: UIApplication, didFinishLaunchingWithOptions
                launchOptions: [UIApplicationLaunchOptionsKey: Any]?) -> Bool {
        UINavigationBar.appearance().barTintColor = .red
        return true
    }
```
