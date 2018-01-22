# In a current operating system, the presence of a fine-grained transport security key (NSAllowsArbitraryLoadsForMedia, NSAllowsArbitraryLoadsInWebContent, or NSAllowsLocalNetworking) overrides the value of the NSAllowsArbitraryLoads key to NO. 
## 문제
## When I called http, Some sites are responding, Some sites do not respond.
* http call을 했을 때 어떤 사이트는 response가 오고, 어떤 사이트는 response가 안온다.
	* naver, google은 안되고, facebook은 되었다.
		* error description : The resource could not be loaded because the App Transport Security policy requires the use of a secure connection.

## 해결
* info.plist 설정에
	- [ ] NSAllowsArbitraryLoads
	- [ ] NSAllowsArbitraryLoadsInWebContent
	* 이 두가지가 모두 체크되어있었는데, NSAllowsArbitraryLoadsInWebContent를 지워야 했다....
		* NSAllowsArbitraryLoadsInWebContent의 설정으로 설정 사항이 줄어서, NSAllowsArbitraryLoads은 NO가 되었던 것.
	* 현재 운영 체제에서 세분화 된 전송 보안 키 (NSAllowsArbitraryLoadsForMedia, NSAllowsArbitraryLoadsInWebContent 또는 NSAllowsLocalNetworking)가 있으면 NSAllowsArbitraryLoads 키 값이 NO로 대체된다…….

### ☀️ 관련 원문
> However, this fine-grained control is not available in older operating systems (iOS 10.0 and older, or macOS 10.12 and older). To maintain backward-compatibility when you use any of these keys, you must also set the value of the NSAllowsArbitraryLoads key to YES, taking advantage of its version-specific behavior.  
>   
> Version-specific ATS behavior: In a current operating system, the presence of a fine-grained transport security key (NSAllowsArbitraryLoadsForMedia, NSAllowsArbitraryLoadsInWebContent, or NSAllowsLocalNetworking) overrides the value of the NSAllowsArbitraryLoads key to NO. This allows you to set NSAllowsArbitraryLoads to YES if needed for your app in older operating systems, without disabling ATS generally in current operating systems  

[NSAppTransportSecurity](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/CocoaKeys.html#//apple_ref/doc/uid/TP40009251-SW68)

## 참고
### NSAllowsArbitraryLoadsInWebContent
An optional Boolean value that applies only to content to be loaded into an instance of the following classes:

* WKWebView
* UIWebView (iOS only)
* WebView (macOS only)

Set this key’s value to YES to obtain exemption from ATS policies in your app’s web views, without affecting the ATS-mandated security of your NSURLSession connections.
Default value is NO.
_Use of this key triggers App Store review and requires justification._
To support older versions of iOS and macOS, you can employ this key and still manually configure ATS. To do so, set this key’s value to YES and also configure the NSAllowsArbitraryLoads subkeys.
⭐️ If you add this key to your Info.plist file, then, irrespective of the value of the key, ATS ignores the value of the NSAllowsArbitraryLoads key, instead using that key’s default value of NO.

Available starting in iOS 10.0 and macOS 10.12.

### NSAllowsArbitraryLoads (Allow Arbitrary Loads)
An optional Boolean value that, when set to YES, disables App Transport Security (ATS) for all domains for which you do not explicitly reenable ATS by using an exception domain dictionary (as specified using the NSExceptionDomains key).

Use of this key triggers App Store review and requires justification.

Enable this key for cases where your app allows the user to specify connection to an arbitrary URL.

Enabling this key can also be useful for debugging and development.

In iOS 10 and later, and macOS 10.12 and later, the value of this key is ignored—resulting in an effective value for this key of its default value of NO—if any of the following keys are present in your app’s Info.plist file:

* NSAllowsArbitraryLoadsForMedia
* NSAllowsArbitraryLoadsInWebContent
* NSAllowsLocalNetworking

**NOTE** Disabling ATS allows connection regardless of HTTP or HTTPS configuration, allows connection to servers with lower Transport Layer Security (TLS) versions, and allows connection using cipher suites that do not support perfect forward secrecy (PFS).

This key’s default value of NO results in default ATS behavior for all connections except those for which you have specified an exception domain dictionary (see Table 3).

####  잡담
* facebook이 됬던 이유로 추측되는 것 (추측이라 아닐 수도 있음)
1. response http header를 본 결과, Strict-Transport-Security 설정에서 max-age차이가 있었다.
	* facebook은 1552000 / naver는 31536000 (둘다 preload)
	* 뭔가 설정이 옛날로 되어있어서 되나 싶기도 하고... (아닐 수도)
2. facebook은 response를 보니 Vary가 있었다. naver는 없고. 그래서 facebook에서는 문제 없이 되었을지도. 