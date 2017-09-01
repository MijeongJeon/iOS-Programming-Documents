# App Extension Programming Guide_Today(위젯)
==
## 소개
애플에서 지원하는 다양한 extension 중 하나인 Today Extenstion에 관한 번역 문서입니다. 실제 애플리케이션에 위젯을 적용하기위해 공부하다 번역하게 되었습니다. 중간중간 실제 개발 스토리가 들어가있으니 함께 참고해주세요 :)

### App Extension Types

* Action  
* Audio Unit  
* Conent Blocker  
* Custom Keyboard  
* Documnet Provider  
* Finder Sync  
* Photo Editing  
* Share  
* **Today**

**App Extension 중 하나인 Today는 Today Extenstion/Today Widget이라 쓰이고, 흔히 Widgets(위젯)으로 불립니다.** 위젯은 사용자들이 중요한 정보에 빠르게 접근 할 수있도록 도와줍니다. 예를 들어 사용자는 위젯을 통해 현재 주가, 날씨 정보, 오늘의 일정을 확인하거나 해야 할 일을 완료했음을 체크할 수 있습니다. 사용자들은 위젯을 통해 관심있는 정보를 즉각 활용 할 수 있기 원합니다. 

위젯을 잠금 화면에서도 보여지게 할 수 있는데, Setting > Touch ID & Passcode > Notifications View에서 "Allow Access When Locked"를 체크하면 됩니다.

> **시작하기 전에**
> Today extension의 핵심은 빠르게 정보를 업데이트하고, 간단한 작업을 수행할 수 있게 하는 것입니다. 만약 사용자에게 멀티 테스크나 시간이 오래 걸리는 작업(업로딩, 다운로딩)을 제공하려고 한다면 Today extension은 적절한 선택이 아닐 수 있습니다. 


## 위젯 이해하기(Understand Today Widgets)
* iOS와 macOS 플랫폼의 Today Widget은 아래 사항을 준수해야 합니다.
	* 위젯의 내용은 항상 최신 상태를 유지해야 합니다.
	* 사용자 액션에 적절하게 반응해야합니다.
	* 작동 성능이 좋아야합니다. (특히, iOS 위젯은 메모리를 효율적으로 사용해야합니다. 그렇지 않으면 시스템에 의해 종료될 수 있습니다.)
	
위젯과 사용자간의 상호작용은 제한되어있기 때문에 단순하고 유연한 UI로 디자인하여 사용자가 관심있어 하는 정보를 부각 시킬 수 있도록 해야힙니다. 이를 위해 액션 가능한 항목 수를 제한하는 것이 좋습니다. 그리고 iOS 위젯은 키보드 입력을 지원하지 않습니다. 

> **NOTE**
> 위젯 뷰 내부에 스크롤 뷰(scroll view)를 두지 않도록 하십시오. 사용자가 위젯 화면 전체를 스크롤 하지 않고, 개별 위젯 뷰 내부를 스크롤하는 것은 어려운 일입니다.

사용자는 그들이 사용하는 플랫폼에 따라 그들이 사용하는 위젯을 각기 다르게 인식합니다.

**iOS** iOS 위젯에서는 키보드 입력이 지원되지 않기 때문에, 사용자는 위젯을 포함하는 애플리케이션을 통해 위젯에 보여질 내용을 구성 할 수 있어야 합니다. 예를 들어, Stocks 위젯에서 사용자는 값을 어떤 방식으로 볼지를 변경 할 수 있지만 위젯에서 보여질 목록을 관리하기 위해서는 Stocks 애플리케이션을 실행해야합니다.

**macOS** 위젯이 포함된 애플리케이션에서 어떤 기능을 제공하지 않을 수도 있기 때문에 위젯이 실행 중일때 사용자가 조절 할 수 있는 방법을 제공해야 합니다. 예를 들어 macOS의 Stocks 위젯에서 사용자는 원하는 항목을 추가할 수 있습니다. macOS의 Notification Center API는 사용자들이 위젯을 구성할 수 있는 method를 포함하고 있습니다.

Today Extension을 포함하는 애플리케이션을 설치하면 사용자가 Today View에 위젯을 추가 할 수 있습니다. Today View에서 'Edit' 버튼을 누르면 Notification Center가 위젯을 추가, 삭제 그리고 재정렬하는 기능을 제공합니다. 

## Xcode Today 템플릿
Xcode Today 템플릿은 핵심 class`TodayViewController`를 위한 기본 헤더 파일과 구현 파일 그리고 Info.plist, 인터페이스 파일`storyboard/xib`을 제공합니다. 

기본 Today 템플릿의 Info.plist은 아래와 같은 key-value을 제공합니다.(macOS)

```
 <key>NSExtension</key>
    <dict>
        <key>NSExtensionPointIdentifier</key>
        <string>com.apple.widget-extension</string>
        <key>NSExtensionPrincipalClass</key>
        <string>TodayViewController</string>
    </dict>
```

만일 custom view controller subclass를 사용하려면`NSExtensionPrincipalClass` key의 값인 `TodayViewController`를 custom class의 이름으로 교체하면 됩니다. 

**iOS** 기본으로 제공되는 storyboard를 사용하지 않으려면 `NSExtentionMainStoryboard` key를 제거하고 `NSExtensionPrincipalClass` key를 추가하고 사용하려는 `view controller`(ex. TodayViewController)의 이름을 value에 입력하십시오. 
>**개발 실화** 
>
> 실제 애플리케이션에 스토리 보드를 사용하지 않고 Today Widget을 생성해 본 경험에 의하면 단순히 위와 같이 key-value를 수정하는 것 외에, 사용하려는 `view controller`의 상단에 `@objc` 키워드를 추가해야했습니다. 그렇지 않으면 다음과 같은 문구와 함께 애플리케이션 크래쉬를 피할 수 없었죠. 
> 
> `*** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: '*** setObjectForKey: object cannot be nil`
>
> 애플 문서만 보고 착실하게 따라했다가 아까운 시간을 얼마나 날려버렸는지...번역하면서 왠지 화가나는 포인트였습니다. 애플 문서에 왜 설명이 자세히 나와있지 않은지, 왜 `@objc`가 필요한지에 대해 한참동안 고민해본 결과🤔, *애플이 이런 방식으로 스토리보드를 사용하게 하는구나...*라는 추론에 이를 수 있었답니다 .
> 
> **결론** 
> 
> 스토리 보드를 사용하지 않으려면 `import` 아래에 `@objc (TodayViewController)`를 반드시 추가해주세요. (Xcode8, Swift3 기준)

## UI 디자인
> **IMPORTANT**  
> 최상의 결과를 위해 Today Widget 디자인시 오토 레이아웃(Auto Layout)을 사용하십시오.  (*MJ: 저는 오토 레이아웃 사용하지 않았는데, 최상의 결과는 아닐 수 있지만 별 문제는 없었어요 :)*)

Today 화면의 공간이 제한적이기 때문에 위젯을 너무 크게 생성하면 안됩니다. iOS와 macOS 두 플랫폼 모두 위젯의 너비는 Today View의 너비에 맞아야하며, 필요에 따라 높이는 높아질 수 있습니다. 

Xcode에서 제공하는 Today Extension의 템플릿은 기본 여백 insets값을 Auto Layouts로 포함하고 있습니다. Insets값을 확인하기 위해서는 NCWidgetProviding 프로토콜의 `widgetMarginInsetsForProposedMarginInsets:` 메서드를 이용하면 됩니다. 기본으로 제공되는 공간 안에 모든 위젯 컨텐츠를 담아야 합니다. 위젯의 모양에 대해 더 깊이 알고 싶다면 [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/)의 **Today Widgets** 파트를 참고하세요.

위젯에서 나타낼 컨텐츠가 많다면, 위젯의 적절한 높이를 맞추기 위해 Auto Layout constraints를 이용할 수 있습니다. 만일 Auto Layout을 사용하지 않는다면 아래와 같이 `UIViewController`의 `preferredContentSize` 속성을 이용하면 됩니다.

``` Objective-C
- (void)receivedAdditional Content {
     self.preferredContentSize = [self sizeNeededToShowAdditionalContent]
}
```

**iOS** 위젯 화면 크기가 재조정될때 애니메이션 효과를 주고 싶다면  `animateAlongsideTransition:completion:` 를 이용해 `viewWillTransitionToSize:withTransitionCoordinator:`를 구현하십니오.

Today view에서 아이템이 나타날때 진동 효과를 사용하려면 `notificationCenterVibrancyEffect`을 사용하세요.

**mac OS** 위젯은 `NSAppearanceNameVibrantDark`을 상속 받습니다. 기본값을 사용하면 자동적으로 적절한 형태를 갖추게 됩니다. 커스텀 컬러를 사용하려면 어두운 색에서 잘 보이는 색을 선택하도록 하십시오.

## 컨텐츠 업데이트
Today Extenstion은 위젯의 상태와 컨텐츠 업데이트를 관리하는 API를 제공합니다(API에 대해 더 알아보려면 [Notification Center Framework Reference](https://developer.apple.com/documentation/notificationcenter)를 참고하세요). Today API는 플렛폼에 따라 약간의 차이는 있지만, 지원되는 기능은 iOS와 macOS에서 거의 유사합니다.

위젯을 최신 상태로 보여주기 위해, 시스템은 때때로 위젯 뷰의 스냅샷을 캡처합니다. 위젯이 보여질때 화면이 최신 상태를 반영하기 까지 시스템이 가장 최신 스냅샷을 보여줍니다.

스냅샷이 찍히기 전에 위젯의 상태를 업데이트 하려면 `NCWidgetProviding` 프로토콜을 준수하십시오. 위젯이 `widgetPerformUpdateWithCompletionHandler:` 호출을 받으면 위젯이 최신 내용으로 업데이트 되고, 아래 업데이트 결과중 하나와 함께 completion handler를 호출하게 됩니다. 

**업데이트 결과**  

* NCUpdateResultNewData—새로운 컨텐츠가 필요한 경우  
* NCUpdateResultNoData—업데이트할 필요가 없는 경우  
* NCUpdateResultFailed—업데이트 중 오류가 발생한 경우  

## 위젯이 나타날 시점 결정하기
Today view에서 위젯이 특정 시점에만 보여져야 한다면(새로운 내용이나 중요한 내용이 있을때) `NCWidgetController` 클래스의 `setHasContent:forWidgetWithBundleIdentifier:` 메서드를 사용하십시오. 이 메서드를 사용하면 위젯 컨텐츠의 상태를 선언하여 시스템이 위젯을 보일지 숨길지 결정하도록 합니다. 

위젯에 내용이 없음을 선언하고 숨기려면, `setHasContent:forWidgetWithBundleIdentifier:` 메서드를 호출하고 `flag` 파라미터 값을 `false`로 지정하십시오. 컨텐츠 나타내기 위해 위 메서드의 `flag` 파라미터의 값으로 `true`가 전달되기 까지 Notification Center는  위젯을 나타내지 않을 것입니다.

## Containing App 열기
위젯을 이용해 containig app(위젯과 연결된 본 애플리케이션)을 열어야 할때가 종종 있습니다. 예를 들어 캘린더 위젯에서 사용자가 이벤트를 터치하면 캘린더 애플리케이션으로 연동됩니다. 사용자가 선택한 작업과 동일한 부분의 containing app을 실행하기 위해서 애플리케이션과 위젯에서 함께 사용하는 `custom URL scheme`를 활용해야합니다.

위젯은 containing app에게 실행하라고 직접적으로 말하지 않습니다. `NSExtensionContext`의 `openURL:completionHandler:` 메서드를 통해 시스템에게 containing app을 실행시키라고 하며, 시스템은 유효성을 판단한 뒤 애플리케이션을 실행합니다.

## 수정 모드 지원(macOS)
macOS에서 수정 모드를 지원하려면 ` NCWidgetProviding` 프로토콜을 준수해야합니다. `widgetAllowsEditing` 속성을 `true`로 설정하면 위젯의 헤더 부분에 `Info` 버튼이 자동생성됩니다. 수정모드를 지원하기 위해 `NCWidgetProviding` 프로토콜을 준수하면 수정 모드에 진입 했을때 `Edit`, `Done` 그리고 `Cancel` 버튼을 자동 제공합니다.

위젯의 수정 모드/비수정 모드 변화를 파악하려면 `NCWidgetProviding` 프로토콜의 `widgetDidBeginEditing`와 `widgetDidEndEditing` 메서드를 사용하십시오. 

사용자가 위젯에서 수정을 하는 동안 검색 UI(search view controller)를 보여주려면, `NSViewController`의 `NCWidgetProvidingPresentationStyles` 카테고리를 사용하십시오. 사용자가 검색을 완료했음을 알리면 `dismissViewControllerAnimated:completion:` 메서드를 이용해 search view controller를 숨기십시오.

## 위젯 테스트
**iOS** iOS 시뮬레이터나 기기에서 위젯을 테스트 할 수 있습니다.

**macOS** macOS 위젯을 테스트하는 가장 손쉬운 방법은 Xcode Widget Simulator를 사용하는 것입니다. 이를 위해 widget target의 scheme에서 Wideget Simulator를 지정하면 됩니다.

App Extentsion에서의 디버깅에 대해 더 알아보려면 [ Debug, Profile, and Test Your App Extension](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/ExtensionCreation.html#//apple_ref/doc/uid/TP40014214-CH5-SW8)를 참고하세요.


* 번역 및 정리: 전미정
* 오타/오역 및 다양한 의견에 대한 PR은 언제든지 환영입니다.
