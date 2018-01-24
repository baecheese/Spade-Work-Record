# 👾 What is the year-end Self Classification Report?
# iOS 앱 제출시 나오는 연말 자가 분류 보고서란
## 문제
* TestFlight 배포 중에 계속 앱 암호화를 사용할 경우, “ATS를 사용하고 있거나 HTTPS를 호출하는 경우 미국 정부에 연말 자가 분류 보고서를 제출해야 합니다.” 라는 알림이 뜬다.

## 해결
### What is the year-end Self Classification Report?
An annual Self Classification Report is a requirement for exports of certain encryption items. For example, if your app is making use of standard encryption algorithms available in Apple’s iOS or macOS, or simply making calls over HTTPS, you may be required to submit an annual Self Classification Report to two U.S. Government agencies with information about your app every January. For more detailed information, see the following encryption reporting guidance policy.

* 앱이 Apple의 iOS 또는 macOS에서 제공되는 표준 암호화 알고리즘을 사용하거나 단순히 HTTPS를 통해 Call을 하면 매년 1월에 자체 정보를 2 개의 미국 정부 기관에 제출해야 할 수 있다. 자세한 내용은 다음 암호화보고 지침을 참고 하라.

#### encryption reporting guidance policy (암호화보고 지침)
![](https://github.com/baecheese/Spade-Work-Record/blob/master/resource/encryption%20reporting%20guidance%20policy.png?raw=true)
* 위의 파란색 부분에 해당하는 경우 안내도 되는 걸로.. (번역은 잘 한건지 모르겠..)

## 기타
#### What are some examples of compliance requirements based on encryption use in an app?
##### 원문
![](https://github.com/baecheese/Spade-Work-Record/blob/master/resource/examples%20of%20compliance%20requirements%20based%20on%20encryption%20use%20in%20an%20app_en.png?raw=true)
##### 번역문 (원문 참고)
![](https://github.com/baecheese/Spade-Work-Record/blob/master/resource/examples%20of%20compliance%20requirements%20based%20on%20encryption%20use%20in%20an%20app.png?raw=true)

#### 그 외의 자세한 수출 규정 관련 문서
* 애플에 전화해본 결과 수출 규정과 관련된 설명이 적힌 페이지를 안내 받을 수 있었다. 
	* iTunes Connect - 참고 자료 및 도움말 - FAQ - Managing Your App - Export Compliance
	* 이 페이지에 가면 관련된 여러 사항을 자세히 읽을 수 있다
	* 더 모르는 것이 있으면 맨 아래에 있는 contact us로 가서 Export Compliance 토픽을 선택해서 물어보면 해당 부서에 문의가 된다고 한다.
