# 🛠 HTTP/HTTPS Network - App transport security
## 문제
> Error Domain=NSURLErrorDomain Code=-1022 "The resource could not be loaded because the App Transport Security policy requires the use of a secure connection." UserInfo={NSUnderlyingError=0x600000e45970 {Error Domain=kCFErrorDomainCFNetwork Code=-1022 "(null)"}, NSErrorFailingURLStringKey=http://www.google.com/, NSErrorFailingURLKey=http://www.google.com/, NSLocalizedDescription=The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.}  

* network call 테스트 중에 http 링크로 call을 하면 위와 같은 에러가 났다.
* info.plist에 ATS관련 설정도 해둔 상태고, https링크가 아니라 무언가 했음

## 해결
* 애플이 ATS정책에 대해 새로 “새 앱을 개발하는 경우 HTTPS 만 사용해야합니다.”라고 했다. 암호화된 https통신만 허용하고 안전하지 않는 수준의 https/http통신을 차단하는 것이다. 백엔드와의 모든 통신을 HTTPS로 전환하는 것이 좋을 것.
* 비보안 도메인을 사용할 경우 info.plist 추가

## 기타
### 애플의 App Transport Security 관련 원문
> App Transport Security (ATS) enforces best practices in the secure connections between an app and its back end. ATS prevents accidental disclosure, provides secure default behavior, and is easy to adopt; it is also on by default in iOS 9 and OS X v10.11. You should adopt ATS as soon as possible, regardless of whether you’re creating a new app or updating an existing one.  
>   
> If you’re developing a new app, you should use HTTPS exclusively. If you have an existing app, you should use HTTPS as much as you can right now, and create a plan for migrating the rest of your app as soon as possible. In addition, your communication through higher-level APIs needs to be encrypted using TLS version 1.2 with forward secrecy. If you try to make a connection that doesn't follow this requirement, an error is thrown. If your app needs to make a request to an insecure domain, you have to specify this domain in your app's Info.plist file.  
[iOS 9.0 -  new features in more detail](https://developer.apple.com/library/content/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)