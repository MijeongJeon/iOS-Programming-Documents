<a name = "위로가기"></a>

# Creating Independent watchOS Apps 
### 핸드폰 연결없이 단독으로 동작하는 watchOS 애플리케이션 만들기

**[Creating Independent watchOS Apps 원문 바로가기](https://developer.apple.com/documentation/watchkit/creating_independent_watchos_apps)**   

> 2019년에 발표된 watchOS 6부터 핸드폰과 전혀 상관없이 워치만을 위한 애플리케이션을 개발할 수 있게 되었습니다. 이 문서는 핸드폰과 연동하지 않고 독립적으로 설치, 실행하는 워치 애플리케이션을 개발하는 방법에 대해 소개합니다.
  
> 이 문서에서 pair로 연결되는 iOS - watchOS 애플리케이션 관계를 짝(👫) 으로 표현했습니다. 

> 원문을 Xcode 11.4 버전에 맞춰 용어를 업데이트했습니다.

---

<a name = "개요"></a>
## 1. Overview
> 애플 워치 사용자들은 핸드폰을 가지고 있지 않을 때에도 워치 애플리케이션이 정상적으로 작동하길 기대합니다. 애플 워치 사용자들에게 최상의 경험을 제공하기 위해 iOS를 필요하지 않는 독립적인 watchOS를 만들어보세요!

watchOS 프로젝트를 준비하는 방법에는 아래와 같은 2가지 방법이 있습니다. 

1. **Watch-only app**
	: 연결해야하는 iOS 애플리케이션 없이 애플워치에서만 사용할 수 있는 환경을 만들 때

2. **WatchOS with iOS app**
	: 짝이 되는 iOS 애플리케이션이 있고, iOS 애플리케이션과 유사하거나 연관된 사용자 경험을 watchOS 애플리케이션으로 제공하고 싶을 때   

watchOS 애플리케이션을 만들기 전에 위 두가지 중 어떤 유형으로 생성할지 먼저 결정하세요.
만일 iOS 애플리케이션과 watchOS 애플리케이션을 짝으로 묶을거라면, watchOS 애플리케이션이 iOS와 어떻게 상호작용할지 분명하게 정해야합니다.

* **Independent** 독립적인 워치 애플리케이션은 정상적으로 동작하기 위해 iOS 애플리케이션이 필요하지 않습니다. 사용자는 iOS 애플리케이션만 쓸지, watchOS 애플리케이션만 쓸지, 혹은 두 개 다 사용할지 선택할 수 있습니다.
* **Dependent**  짝이있는 워치 애플리케이션은 정상적으로 작동하기 위해 짝 iOS 애플리케이션이 필요합니다. iOS 애플리케이션과의 상호작용이 필요한 경우에만 독립적이지 않은 워치 애플리케이션을 생성하세요. 사용자는 watchOS를 구매하고 설치하기 위해 짝 iOS 애플리케이션을 반드시 함께 구매/설치해야합니다. 

> [참고] watchOS 5 이전 버전의 모든 watchOS 애플리케이션은 반드시 짝 iOS를 필요로합니다

watchOS 6 이후 버전부터 애플워치에 있는 AppStore에서 watchOS 애플리케이션을 구입하는게 가능합니다(그림1). watchOS 6.2 이후 버전부터는 애플 워치에서 인앱 결재를 하는 것도 가능합니다 💳.  만일 짝 iOS 애플리케이션이 있는 경우 인앱 결재를 하면 watchOS와 iOS에서 모두 해당 컨텐츠 사용이 가능합니다. 자세한 내용은 [인앱 결재 In-App Purchase](https://developer.apple.com/documentation/storekit/in-app_purchase)를 참고하세요.

![Appstore on Apple Watch](https://docs-assets.developer.apple.com/published/63fbc6d282/06d45110-1dd7-49a4-a413-9f5159ecdd0e.png)
그림 1. 애플 워치 속 앱 스토어 

짝 iOS 애플리케이션이 있는 경우나, 없는 독립적인 경우나 모두 watchOS 애플리케이션을 애플워치에서 바로 다운 받고 설치하는 것은 가능합니다. 하지만 짝 iOS 애플리케이션이 있는 경우에는 핸드폰에 iOS 애플리케이션 설치가 끝날때까지 watchOS 애플리케이션을 작동시킬 수 없습니다. 


<a name = "생성"></a>
## 2. Create a Watch-Only App
> 독립 워치 앱을 생성하는 방법입니다. 

 Xcode를 실행하고 `Create a New Xcode Project`를 선택한 뒤 `watchOS`에서 `Watch App` 템플릿을 선택하세요 (아래 그림에서는 `App`이지만 업데이트된 Xcode에서는 `Watch App`으로 변경되었습니다).
 
![figure2](https://docs-assets.developer.apple.com/published/fce218c141/af79db6d-02b1-4df0-9f46-6c3c9db12d76.png)
그림 2. Watch App 템플릿 선택
 
생성된 프로젝트는 그림 3과 같이 크게 2개의 섹션으로 나뉘어져있습니다 (혹시 3개의 섹션으로 나뉘어져있다면 독립 watch 앱이 아니라 쌍인 iOS 앱이 필요한 경우이니 다시한번 프로젝트를 생성해 보세요). 

첫번째, **WatchKit app**에는 애플리케이션에서 사용될 스토리보드와 해당 스토리보드에서 사용할 assets 폴더가 포함되어있습니다.

두번째, **WatchKit extension**에는 애플리케이션의 코드가 포함됩니다.

![image3](https://docs-assets.developer.apple.com/published/d19acfa47f/fa7719ce-5ea2-4292-810e-6c56e722f870.png)
그림 3. 프로젝트 네비게이션 화면

그림 4와 같이 targets은 3개가 생성되는 것을 확인할 수 있습니다.

![](https://docs-assets.developer.apple.com/published/52616fcb34/02ac2063-b61c-45ec-963a-f9e6e8f05cbe.png)
그림 4. 3개의 targets

3개의 targets 중 프로젝트를 대표하는 것은 첫번째 target으로, 이를 App Store에 제출하게 됩니다. 나머지 두개의 targets은 각각 WatchKit app과 WatchKit extension을 대표합니다.


<a name = "변환"></a>
## 3. Convert a Dependent App into an Independent App

만일 iOS와 쌍으로 연결하는 애플리케이션을 독립으로 존재하는 워치 애플리케이션으로 변환하고 싶다면 아래와 같이 하면 됩니다.

1. 세 가지 targets 중 `WatchKit Extesion`을 선택하고 `General` 탭으로 이동합니다.
2. 화면 중간 쯤 **Deplotment Info**로 이동합니다.
3. **Supports Running Without iOS App Installation**을 체크합니다.

![](https://docs-assets.developer.apple.com/published/a4dd468172/050ba8c6-8ea7-4a25-88af-0d616fd6b9a7.png)
그림 5. iOS 짝 애플리케이션 없이 동작 가능하도록 만들기

Xcode 11 이후 버전 부터는 새로운 watchOS를 만들면 위 체크박스가 기본으로 체크되어있습니다.

<a name = "확인"></a>
## 4. Ensure Your App Runs Independently
> 독립 애플리케이션으로서의 조건에 대해 알아봅시다.

독립 애플리케이션은 짝 iOS 애플리케이션이 없어도 정상적으로 작동해야합니다. **정상적인 작동**에는 데이터 다운 받기, 사용자 계정 설정 그리고 애플리케이션 설정 등이 포함됩니다. 

특히, 독립 watchOS 애플리케이션들은 아래 사항을 만족해야합니다.

* 애플 워치에서 사용자들의 새로운 계정을 생성하고, 인증 할 수 있어야합니다. 더 자세한 내용은 [Authenticating Users on Apple Watch](https://developer.apple.com/documentation/watchkit/authenticating_users_on_apple_watch)을 참고하세요.
* 시스템 서비스 활용을 위한 사용 권한을 아이폰이 아닌 워치 애플리케이션에서 요청해야합니다. 예를 들어 watchOW 6 이후 버전 부터는 `HealthKit`을 사용하는 애플리케이션은 사용자에게 애플 워치에서 `Health`에 대한 접근 권한을 요청할 수 있습니다.
* 데이터를 워치로 바로 다운로드 받을 수 있어야합니다. 독립 워치 애플리케이션은 iOS 애플리케이션으로 부터 데이터나 파일을 전송받기 위해 **WatchConnectivity**에 의존할 수 없습니다. 만일 디바이스간 데이터 싱크가 필요하다면. *CloudKit*이나 자체 서버를 활용하도록 하세요. 자세한 내용은 [Keeping Your watchOS Content Up to Date](https://developer.apple.com/documentation/watchkit/keeping_your_watchos_content_up_to_date)
* 워치로 push notification을 바로 보내야합니다. 자세한 내용은 [registerForRemoteNotifications()](complication)을 참고하세요.

iOS 쌍 애플리케이션이 있는 경우에는 iOS 디바이스가 가까이 있을 때 **WatchConnectivity**을 통해 자료를 전달 받을 수 있습니다. 하지만 독립 워치 애플리케이션은 **WatchConnectivity**을 주요 데이터 소스로 사용할 수 없으니 자체 정보에 접근할 수 있는 기능이 구현되어야합니다.

---

* 번역 및 정리: [미정](ninevincentg@gmail.com)
* 오타/오역 및 다양한 의견에 대한 PR은 언제나 환영입니다 🤗

[**위로가기**](#위로가기)
