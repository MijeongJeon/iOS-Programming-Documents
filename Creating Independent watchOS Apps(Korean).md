<a name = "위로가기"></a>

# Creating Independent watchOS Apps 
### 핸드폰 연결없이 단독으로 동작하는 watchOS 애플리케이션 만들기

**[Creating Independent watchOS Apps 원문 바로가기](https://developer.apple.com/documentation/watchkit/creating_independent_watchos_apps)**   

> 2019년에 발표된 watchOS 6부터 핸드폰과 전혀 상관없이 워치만을 위한 애플리케이션을 개발할 수 있게 되었습니다. 이 문서는 핸드폰과 연동하지 않고 독립적으로 설치, 실행하는 워치 애플리케이션을 개발하는 방법에 대해 소개합니다.
  
> 이 문서에서 pair로 연결되는 iOS - watchOS 애플리케이션 관계를 짝(👫) 으로 표현했습니다. 

## Contents
### [1. Overview](#개요)
>

### [2. Create a Watch-Only App](#생성)
> 문서의 목적 및 용도 설명
> 

### [3. Convert a Dependent App into an Independent App](#변환)
> 문서의 목적 및 용도 설명
> 

### [4. Ensure Your App Runs Independently](#확인)
> 문서의 목적 및 용도 설명
> 


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

짝 iOS 애플리케이션이 있는 경우나, 없는 독립적인 경우나 모두 watchOS 애플리케이션을 애플워치에서 바로 다운 받고 설치하는 것은 가능합니다. 하지만 짝 iOS 애플리케이션이 있는 경우에는 핸드폰에 iOS 애플리케이션 설치가 끝날때까지 watchOS 애플리케이션을 작동시킬 수 없습니다. 


<a name = "생성"></a>
## 2. Create a Watch-Only App

<a name = "변환"></a>
## 3. Convert a Dependent App into an Independent App

<a name = "확인"></a>
## 4. Ensure Your App Runs Independently















<a name = "위로가기"></a>
