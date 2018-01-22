# 🛠 AdMob banner Request Error: No ad to show
## 문제
```
Request Error: No ad to show
```
* App ID와 Banner ID를 모두 등록했는데 광고가 뜨지 않는다.
* 테스트용 App ID, Banner ID로 하면 잘됨

## 해결
* 처음에 등록을 하고 두 시간 정도 기다려야 한다.
* 그래도 안되서 찾아보니 지급 정보를 적어야 한다고 한다.
	* admit 마이 페이지에서 **지급** 메뉴에 설정을 적어주면 됨 (이름 주소 등등)
* Pod update를 해주고 배포할 것

## 기타
* 이후에도 TestFlight 배포 후, 테스터 마다 광고가 보이는 사람있고, 아닌 사람 있었는데, AppStore 등록 전에는 좀 그럴 수 있나보다. 왤까..
	* [ios - AdMob banner shows in Simulator but not on device/TestFlight - Stack Overflow](https://stackoverflow.com/questions/44478282/admob-banner-shows-in-simulator-but-not-on-device-testflight)
* 코드에는 문제가 없었다. 나중에 쓰기 위한 코드 메모.

### AppID - AppDelegate
``` swift
func application(_ application: UIApplication, 
					didFinishLaunchingWithOptions launchOptions:
					[UIApplicationLaunchOptionsKey: Any]?) -> Bool {
	GADMobileAds.configure(withApplicationID: AdMobInfomation.appID.value())
	return true
}
```

### BannerID - Use Class
``` swift
@IBOutlet weak var banner: GADBannerView!
override func viewDidLoad() {
	loadAD()
}

func loadAD() {
	banner.adUnitID = AdMobInfomation.bannerID.value()
	banner.rootViewController = self
	banner.delegate = self
	let request = GADRequest()
	banner.load(request)
}
```