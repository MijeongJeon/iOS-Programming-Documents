<a name = "위로가기"></a>
# iOS 앱 프로그래밍 가이드 번역문

**[iOS Programming Guide 원문 바로가기](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html)**   
  
## Contents
### [1. Introduction](#소개)
> 문서의 목적 및 용도 설명

### [2. Expected App Behaviors](#앱행동)
> 앱 기본 구동에 필요한 리소스, 사용자 개인정보 보호, 앱 지역화(현지화)에 관한 내용

* Providing the Required Resources
* Supporting User Privacy
* Internationalizing Your App

### [3. The App Life Cycle](#앱생명주기)
> iOS 기본 작동 구조 및 앱 생명 주기와 각 상태, 각 주기에서 시행해야하는 작업내용과 작업시점에 관한 내용

* The Main Function
* The Structure of an App
* The Main Run Loop
* Excution States for Apps
* App Termination
* Threads and Concurrency

### [4. Background Excution](#백그라운드실행)
> 백그라운드 상태에서 작업 실시하기, 백그라운 상태로 전환될때 작업 마무리하기, 백그라운 상태에서 되돌아오기 등 백그라운드 상태에서 이루어지는 작업에 관련된 내용

* Executing Finite-Length Tasks
* Downloading Content in the Background
* Implementing Long-Runnig Tasks
* Getting the User's Attention While in the Background
* Understanding When Your App Gets Launched into the Background
* Being a Responsible Background App
* Opting Out of Background Execution

### [5. Stragegies for Handling App Stage Transitions](#상태전환)
> 실행, 구동, 백그라운드, 종료, 일시 정지 등과 같은 앱의 실행 상태 변화에 따른 역할과 관련 메소드 사용법에 관한 내용

* What to Do at Launch Time
* What to Do When Your App Is Interrupted Temporily
* What to Do When Your App Enters the Background

### [6. Strategies for Handling App Features](#앱기능)
> 개인정보 활용, 등급제한, 여러 iOS 버전에 대응하기, 앱 상태 보존/복원, VoIP 활용 등 여러가지 앱에서 공통적으로 사용되는 기능에 대한 소개 및 사용법에 관한 내용

* Privacy Strategies
* Respecting Restrictions
* Supporting Multiple Versions of iOS
* Preserving Your App's Visual Appearance Across Launches
* Tips for Developing a VoIP App

### [7. Inter-App Communication](#앱통신)
>  iOS에서 이루어지는 앱간의 간접적 통신 방법(AirDrop, URLs)에 대한 소개와 데이터 송/수신 방법

* Supporting AirDrop
* Using URL Schemes to Communicate with Apps

### [8. Performance Tips](#성능향상)
> 전력 소모와 메모리 사용 등 앱을 실제로 구동할 때 성능 향상을 위해 고려해야할 여러가지 내용

* Reduce Your App's Power Consumption
* Use Memory Efficiently
* Tune Your Networking Code
* Improve Your Networking Code
* Improve Your File Management
* Make App Backups More Efficient
* Move Word off the Main Thread


<a name = "소개"></a>
## [1. Introduction](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007072-CH1-SW1)
>정상적인 앱 구동을 위한 iOS 코어 시스템 및 앱 설계에 관한 문서

* 본 문서는 iOS 앱을 처음 제작하는 사람들을 위해서가 아니라, 이미 앱을 개발하여 다듬고 향상시키려는 개발자들을 위한 가이드입니다.
* 만약 iOS 개발이 처음이라면 [Start Developing iOS Apps_Swift](https://developer.apple.com/library/content/referencelibrary/GettingStarted/DevelopiOSAppsSwift/index.html#//apple_ref/doc/uid/TP40015214)를 먼저 읽어보세요.
* iOS 기술에 대해 심도있게 알아보려면 [About the iOS Technologies](https://developer.apple.com/library/content/documentation/Miscellaneous/Conceptual/iPhoneOSTechOverview/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007898)를 참고하세요.

<a name = "앱행동"></a>
## [2. Expected App Behaviors](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/ExpectedAppBehaviors/ExpectedAppBehaviors.html#//apple_ref/doc/uid/TP40007072-CH3-SW2)
>Xcode를 이용해 제작된 모든 프로젝트는 즉각 장비에서 구동 될 수 있지만, 그렇다고 App Store에 등록될 준비가 완료된 것은 아닙니다. 모든 앱은 사용자에게 좋은 사용 경험을 전달하기 위해 어느 정도의 시스템 설정이 필요합니다. 앱 아이콘 제공에서부터 어떻게 정보를 표시하고 사용하게 할 것인지에 대한 설정에 대해 알아봅니다.
 
### Providing the Requred Resources
>모든 어플은 아래의 리소스 및 메타데이터를 포함해야합니다.

* 정보 속성 목록 파일(Info.plist)
* 앱 실행 하드웨어 사양
* 한개 이상의 앱 아이콘
* 한개 이상의 앱 런칭 이미지

#### The App Bundle
>iOS 앱을 생성하면 Xcode는 앱을 _bundle_로 묶는데, _bundle_은 앱과 관련된 자원을 그룹화해놓은 파일 시스템의 디렉토리입니다. _bundle_에는 앱 실행 파일, 앱 아이콘, 이지미 파일 및 현지화(localize)된 콘텐츠등의 리소스 파일이 포함되어있습니다. 

* **정보 속성 목록 파일(Info.plist)** : Info.plist 파일은 앱 구성에 관한 중요한 정보가 포함된 구조화된 파일입니다. App Store 및 iOS에서 앱의 기능을 확인하는데 사용되며, 모든 앱에 Info.plist. 파일이 포함되어야합니다.
	* 기본으로 제공되는 Info.plist 파일에는 필수항목에 대한 기본값이 설정되어있으며, 특정 기능을 위한 항목값 변경 및 항목 추가가 가능합니다.
	* Wi-Fi 연결, 맞춤 URL 스키마 지원, 사진 앨범 접근등을 위해서는 Info.plist에 적절한 키를 설정해줘야합니다.
	* Info.plist의 다양한 키와 값에 대해 자세히 알아보려면 [About Info.plist Keys and Valuse](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)를 참고하세요.

* **앱 아이콘** : 모든 앱은 기기의 홈 화면과 App Store에 표시할 아이콘을 제공해야 합니다. 
	* 앱 아이콘은 Image Assets에 포함됩니다.
	* 아이콘의 크기를 포함하여 아이콘을 디자인 하는 방법에 대한 자세한 내용은 [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/)을 참고하세요.

* **런칭 이미지** : 런칭 이미지는 앱을 처음 구동할때 화면에 잠깐 보이는 이미지로, 앱 화면을 구성하고 사용자에게 보여줄 준비가 완료되면 런칭 이미지가 사라집니다.
	* 앱이 foregrounde에서 background로 들어갈때는 사용중인 앱의 스냅 샷이 생성되고, 다시 foreground로 되돌아 올대는 런칭 이미지가 아닌 스냅 샷을 활용합니다. 오래도록 앱을 실행하지 않은 경우에는 스냅 샷을 삭제하고 기존의 런칭 이미지를 활용하게 됩니다.
	* 런칭 이미지의 크기 및 디자인 방법에 대해서는  [iOS Human Interface Guidelines](https://developer.apple.com/ios/human-interface-guidelines/)을 참고하세요.

---

### Supporting User Privacy
>사용자 개인 정보 보호 및 사용을 위한 앱 설계는 대단히 중요합니다. 대부분의 iOS 장비에는 사용자가 외부에 공개하고 싶지 않은 개인 정보가 포함되어있습니다. 개인 정보를 사용하기 위해서는 반드시 해당 법률에 준수하여 사용자의 동의를 얻은 후에 접근해야합니다.

* 보호된 데이터 및 리소스의 경우 인증확인 및 요청을 위한 전용 키와 API를 각각 제공합니다.
* 사용자는 접근 권한을 시스템 설정(Setting)에서 언제든지 변경할 수 있으므로, 해당 항목에 접근하기 전에 권한 부여 상태를 확인하십시오.
* 접근시 사용자 권한이 필요한 항목에는 다음과 같은 것이 있습니다. 
	* 블루투스, 카메라, 달력, 연락처, 건강 정보, 홈킷, 위치정보, 마이크, 기기 모션, 음악, 사진, 미리 알림, 시리, 음성인식, 티비 등

**\* 중요사항 : 앱에서 보호된 항목에 접근하려고 하면 시스템에서 사용자에게 엑세스 권한을 요청하는 메시지(alert)를 표시합니다. iOS 10 부터는 Info.plist에 각각의 개인 정보를 활용하려는 *목적 문구*를 명시하여 액세스 권한 요청 메시지(alert)에 보이도록 해야합니다.**

---

### Internationalizing Your App
>iOS 앱은 많은 국가에 배포될 수 있기 때문에 앱 콘텐츠를 지역화하면 더 많은 사용자에게 다가 갈 수 있습니다. 콘텐츠를 현지화하는 과정은 간단한 과정입니다.

* 지역화 과정을 시작하면 Xcode에서 앱을 현지화 가능한 리소스 파일로 분리하고, 각 리소스를 언어 별 프로젝트(.lproj) 디렉토리를 제공합니다.
* 일반적으로, iOS 앱에서는 다음 유형의 리소스 파일의 현지화가 필요합니다.
	* 스토리 보드 파일, 런칭 이미지 파일: 문자열이 포함된 경우 
	* 오디오 파일: 음성 해설이 포함된 경우
	* 언어 별 또는 문화별 컨텐츠가 포함되어 있지 않은 경우 멀티 미디어 파일의 지역화는 하지 않습니다.


<a name = "앱생명주기"></a>
## [3. The App Life Cycle](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/TheAppLifeCycle/TheAppLifeCycle.html#//apple_ref/doc/uid/TP40007072-CH2-SW1)
>앱은 작성된 코드와 시스템 프레임 워크간의 정교한 상호작용입니다. 시스템 프레임 워크는 모든 앱의 실행에 필요한 기본 인프라를 제공하며, 개발자는 앱에 어울리는 모양과 느낌을 코드로 구현합니다. 이 둘의 효과적인 상호작용을 위해 iOS 작동원리에 대한 이해가 필요합니다. 

### The Main Function
> 모든 C 기반의 앱과 마찬가지로, iOS 앱의 진입점 역시 main 함수 입니다. iOS에서의 다른 점은 main 함수를 직접 작성하지 않는다는 점입니다. Xcode가 기본으로 제공해주며, 제공해주는 기본 main 함수를 수정해서는 안됩니다.

* main 함수는 UIKit 프레임 워크에 제어권을 넘겨주며, UIApplicationMain 함수가 앱의 핵심 객체를 만들며, 스토리 보드로 UI를 로드하고, 초기 설정을 수행하는 코드를 호출합니다.
* 개발자가 제공해야하는 부분은 스토리 부분과 초기화 코드입니다.

---

### The Structure of an App
>  앱을 시작하는 동안 UIApplicationMain 함수는 핵심 객체를 설정하고, 앱 실행을 준비합니다. 모든 iOS앱의 핵심은 UIApplication 객체로, 시스템과 응용 프로그램 객체 사이의 상호작용을 원활하게 하는 역할을 합니다. 

* iOS 앱은 Model View Controller 아키텍처(MVC 패턴) 를 사용합니다. MVC 패턴은 앱의 데이터와 로직 그리고 시각적 화면을 구분합니다. 이러한 패턴은 화면 크기가 다른 다양한 장치에서 앱을 구동하는데 매우 중요한 역할을 합니다.

---

### The Main Run Loop
> 앱의 main run loop는 모든 사용자 관련 이벤트를 처리합니다. UIApplication 객체는 실행시 main run loop를 설정하고, 이를 사용해 이벤트를 처리하고 UI 업데이트를 처리합니다. 이름에서 알 수 있듯이, main run loop는 앱의 main thread에서 실행되며, 사용자 이벤트가 입력되면 순차적으로 처리합니다.

<center>
<img src="https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/event_draw_cycle_a_2x.png" width="400px"/>
</center>


* 사용자가 장비와 상호 작용하며 발생된 이벤트는 UIKit에 의해 설정된 특정 포트를 통해 앱에 전달됩니다. 이벤트들은 내부 queue에서 대기하고 있다가 main run loop에 하나씩 전달 됩니다. 
* 다양한 유형의 이벤트를 iOS 앱에 전달 할 수 있습니다. 가장 일반적인 이벤트에는 터치, 원격 제어, 모션, 가곡도계 및 자이로 스코프 이벤트 등이 있습니가. 각 이벤트에 대한 자세한 내용은 [Event Handling Guid for UIKit Apps](https://developer.apple.com/library/content/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/index.html#//apple_ref/doc/uid/TP40009541)에서 확인 할 수 있습니다.

---

### Execution States for Apps
> iOS 앱은 언제나 다음 중 하나의 상태값을 지니게됩니다.

	* Not running : 앱이 실행되지 않았거나 혹은 시스템에 의해 종료된 상태
	* Inactive : 앱이 foreground에서 실행 중이지만, 일시적으로 이벤트를 받지 못하는 상태
	* Active : 앱이 foreground에서 실행 중이며, 사용자 이벤트를 받을 수 있는 상태
	* Background : 앱이 백그라운드에서 코드를 실행 중인 상태
	* Suspended : 백그라운드에 있는 앱이 더이상 코드를 실행하지 않는 정지된 상태 

<center>
<img src="https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/high_level_flow_2x.png" width="300px"/>
</center>


* iOS 시스템은 전체 시스템에서 발생하는 작업을 처리하며 해당 앱을 여러가지 상태로 이동시킵니다. 예를 들어 사용자가 홈 버튼을 누르거나 전화가 걸려오면 현재 실행 중인 앱이 비활성 상태로 변하게 됩니다.
* 대부분의 상태 전환에는 appDelegate 메소드가 호출 됩니다. 각 메소드를 적절히 사용하면 상태변화에 대응할 수 있습니다.
	* `application:willFinishLaunchingWithOptions:` 앱에서 처음으로 코드를 실행 할 수 있습니다.
	* `application:didFinishLaunchingWithOptions:` 사용자에게 앱의 화면이 보여지기 전에 마지막으로 초기화를 수행할 수 있습니다.
	* `applicationDidBecomeActive:` 앱이 foreground로 전환되는 시점을 알려줍니다.
	* `applicationWillResignActive:` 앱이 foreground 상태에서 inactive 되는 시점입니다.
	* `applicationDidEnterBackground:` 앱이 background에서 실행되고 있으며, 언제든지 정지될수 있음을 알립니다.
	* `applicationWillEnterForeground:` 앱이 background에서 foreground로 진입하고 있지만 아직 active 된 상태는 아닙니다.
	* `applicationWillTerminate:` 앱이 종료되고 있음을 알리는 메소드 입니하. 하지만 앱이 시스템에 의해 정지되었을 경우에는 호출 되지 않습니다.

---

### App Termination
> 앱은 언제든지 종료될 수 있어야 하며, 종료하기 전에 사용자 정보를 저장하거나 특별한 기능을 수행을 위해 기다리지 않습니다. 시스템에 의한 앱 종료는 앱 수명 주기에서 정상적인 부분입니다. 보통은 시스템이 사용하지 않는 메모리를 회수하여 다른 앱을 실행 할 수 있는 공간을 확보하기 위해 종료하지만, 오작동을 하거나 앱이 적절하게 응답하지 않는 경우에도 앱을 종료 시킬 수 있습니다.

* 시스템에 의해 정지된 앱은 종료 될때 알림을 받지 않지 않고 메모리를 회수당하게 됩니다. 
* 사용자가 의도적으로 앱을 종료하는 경우 역시 앱 종료가 앱에 전송되지 않습니다.

---

### Threads and Concurrency
> 시스템은 기본적으로 앱의 main thread를 생성하는데, 필요에 따라 추가 thread를 생성하여 다른 작업을 수행 할 수 있습니다. iOS 앱의 경우, 직접 thread를 만들고 관리하는 대신 `Grand Central Dispatch(GCD)`, `operation objects`, `asynchronous` 프로그래밍 기법을 사용하는 것이 좋습니다.

* GCD를 이용하면 수행하고 싶은 작업과, 작업의 순서를 정할 수 있지만, 시스템이 CPU에서 작업을 가장 효과적으로 수행할 수 있는 방법을 결정 할 수 있도록 하는 것이 전반적인 성능을 향상시키고, 코드를 단순화 시킬 수 있습니다.
* Thread와 concurrncy(동시성)를 사용할때 다음 사항을 고려하세요.
	* View, Core Animation 그리고 UIKit 클래스와 고나련된 작업은 main thread에서 실행되어야 합니다. 
	* 오래 걸리는 작업은 백그라운드 스레드에서 수행해야합니다. 네트워크 작업을 포함하거나, 파일에 접근하거나, 많은 양의 데이터를 처리할 때는 GCD를 이용하여 비동기로 수행해야합니다.
	*  GCD 및 operation object를 사용한 작업 방법에 대해서는 [Concurrency Programming Guide](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)를 참고하세요.

<a name = "백그라운드실행"></a>
## [4. Background Execution](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/BackgroundExecution/BackgroundExecution.html#//apple_ref/doc/uid/TP40007072-CH4-SW1)
> 사용자가 앱을 사용하지 않을 경우 시스템은 앱을 백그라운드 상태로 전환시킵니다. 일반적으로 백그라운드 상태는 정지 상태로 가는 길입니다. 앱을 정지 시키는 일을 배터리 수명을 향상 시키기는 일이며, 다른 앱이 foreground에서 실행 될수 있는 리소스를 제공해줍니다. 하지만 모든앱들이 백그라운드에서 정지되는 것은 아닙니다. 예를 들어 하이킹 앱은 백그라운드에서도 사용자의 위치를 추적해야하며, 오디오 앱은 잠금화면에서도 계속 음악을 재생할 수 있어야 합니다. 이렇게 백그라운드에서 앱을 실행하는 것이 필요하다고  판단되면 iOS는 배터리를 많이 소모하지 않고 효율적으로 수행 할수 있도록 아래와 같은 기술을 제공합니다.

* foreground에서 짧은 작업을 하는 앱은 백그라운드로 전환될때 해당 작업을 완료할 시간을 요청할 수 있습니다.
* foreground에서 다운로드를 시작하는 앱은 다운로드 관리를 시스템에 전할 할 수 있으므로, 다운로드가 되는 동안 앱이 중지되거나 종료되지 않습니다.
* 특정 유형의 작업을 지원하기 위해 백그라운드에서 실행하는 앱은 하나 이상의 백그라운드 실행 모드에 대한 지원을 선언 할 수 있습니다.

### Executing Finite-Length Tasks
> 백그라운드로 이동한 앱이 작업 중인 일을 완료하기 위해 추가적인 시간이 필요한경 경우, `UIApplication` 객체의 `beginBackgroundTaskWithName : expirationHandler :` 또는 `beginBackgroundTaskWithExpirationHandler :` 메소드를 호출하여 작업이 완료되기 까지의 추가 시간을 요청 할수 있습니다. 위 메소드 중 하나를 하나를 호출하면 앱은 일시적으로 중지가 지연되고, 작업을 마저 실행 할수 있습니다. 작업이 끝난 앱은 `endBackgroundTask:`메소드를 호출하여 시스템에 작업을 마쳤음을 알립니다.

* `expirationHandler :` 에 종료하기 전에 수행해야하는 코드를 추가 할 수 있지만, 이때 실행되는 작업이 너무 오래 걸리지 않아야합니다. 
* 작업을 처리하는데 남은 시간을 알고 싶다면 `UIApplication`의 `backgroundTimeRemaining` 값을 확인하세요.

---

### Downloading Content in the Background
>파일을 다운로드 할 때는 `NSURLSession` (`URLSession`)객체를 이용해 다운로드를 시작해야 앱이 중지 되거나, 종료 될 경우 시스템에서 다운로드 프로세스를 제어 할 수 있습니다. 백그라운드 다운로드를 지원하는 `NSURLSessionConfiguration` 객체를 통해 작업이 시작되면 적절한 시간에 업로드 또는 다운로드 작업을 시스템에 전달합니다. 

* 앱이 실행중일때 작업이 완료되면 세션 객체는 일반적인 방식으로 `delegate`에 알려줍니다. 
* 작업이 끝나지 않은 상태에서 시스템이 앱을 종료하면 시스템은 백그라운드에서 작업을 계속 관리합니다.
* 사용자가 앱을 강제로 종료하면 보류중인 작업이 취소됩니다.

---

### Implemneting Long-Runnig Tasks
> 구현하는데 많은 시간이 필요한 작업의 경우, 백그라운드에서 실행 될 수있도록 특정 권한을 요청해야합니다. 백그라운드 실행 기능을 위해서는. `Project Setting` - `Capabilities` - `Background Modes` 옵션을 선택해야하며, 이렇게하면 Info.plist 파일에 `UIBackgroundMoes` 키가 추가됩니다. iOS에서는 아래와 같은 특정 유형의 기능만 백그라운드에서 실행 할 수 있습니다.

	* 음악 플레이어 앱과 같이 백그라운드에서 사용자에게 미디어 콘텐츠를 재생하는 앱
	* 백그라운드에서 오디오 콘텐츠를 녹음하는 앱
	* 내비게이션 앱과 같이 항상 사용자에게 위치 정보를 제공하는 앱
	* VoIP (Voice over Internet Protocol)를 지원하는 응용 프로그램
	* 새로운 콘텐츠를 정기적으로 다운로드하고 처리해야하는 앱
	* 외부 액세서리를 정기적으로 업데이트하는 앱

---

### Getting the User's Attention While in the Background
>알람은 앱이 중지되었거나 백그라운드에 있을때 사용자의 신선을 끌수 있는 방법입니다. 로컬 알림의 사운드, 배지, 알림 기능을 이용하여 사용자에게 알릴 수 있으며 사용자는 앱을 foreground로 되돌려 놓을지 경정해야합니다. 앱이 이미 foreground에서 실행중인 경우, 로컬 알림은 사용자에게 전달되지 않습니다.

* 로컬 알림을 예약하려면 `UILocalNotification` 클래스의 인스턴스를 만들고 매개변수를 구성한 다음  `UIApplication` 클래스의 메소드를 이용해 일정을 예약 할 수 있습니다. 
* 로컬 알림은 알림의 유형(소리, 알림, 배지) 및 알림 시각에 대한 셋팅을 할 수 있습니다.
* `UIApplication` 클래스의 메소드를 사용하여 예약된 알림을 취소하거나 알림 목록을 가져올 수 있습니다. 자세한 내용은 [Local and Remote Notification Programming Guide](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/index.html#//apple_ref/doc/uid/TP40008194)를 참고하세요.

---

### Understanding When Your App Gets Launched into the Background
>백그라운드 실행을 지원하는 앱은 이벤트 처리를 위해 시스템에 의해 재실행 될 수 있습니다. 사용자에 의해 강제로 앱이 종료되지 않은 이상 다음과 같은 이벤트 중 하나가 발생하면 시스템이 임의로 앱을 재실행하게됩니다.

	* 위치 앱 : 앱에 지정된 특정 지역에 들어왔음을 인지했을때
	* 오디오 앱 : 일부 데이터를 처리하기 위해(오디오 재생 혹은 마이크 사용)
	* 블루투스 앱 : 연결된 주변 기기에서 데이터를 수신하거나, 중앙으로부터 명령을 받을 때
	* 백그라운드 다운로드 앱 : 다운로드를 성공적으로 완료했거나 오류가 발생했을 때

*  사용자가 강제로 앱을 종료한 경우에는 시스템이 앱을 재실행 하지 않지만, iOS8 이상의 장비에서 위치앱은 재실행됩니다. 
*  만일 기기가 비밀번호(지문)으로 보호중이면 사용자가 먼저 기기를 잠금해제 해야 백그라운드 상태에서 앱이 실행됩니다.

---


### Being a Responsible Background App
>Foreground 상태의 앱은 시스템 리소스 및 하드웨어 사용과 관련해 항상 백그라운드 상태의 앱보다 우선시 됩니다. 백그라운드에서 실행되는 앱은 이러한 불균등을 대비하여 백그라운드에서 실행될 동작을 제어해야합니다. 특히 백그라운드로 이동하는 앱은 다음 가이드를 준수해야 합니다.

* **코드에서 OpenGL ES를 호출하지 마십시오.** 백그라운드에서 실행되는 동안 EAGLContext객체를 만들거나 OpenGL ES 드로잉 명령을 실행하면 앱은 즉시 종료됩니다. 백그라운드에서 OpenGL ES를 처리하는 방법에 대해서는 [OpenGL Programming Guide](https://developer.apple.com/library/content/documentation/3DDrawing/Conceptual/OpenGLES_ProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008793)를 참고하세요.
* **앱이 정지 되기 전에 Bonjour 관련 서비스를 취소하십시오.**  앱이 백그라운드로 이동해 정지되기 전에 Bonjour를 취소하고 네트워크 서비스와 관련된 수신 대기 소켓을 닫아야 합니다. 만약 Bounjour 서비스를 직접 취소하지 않으면 시스템이 서비스를 종료시키게됩니다.
* **네트워크 기반 소켓의 연결 오류를 처리 할 수 있도록 준비하십시오.** 시스템은 여러가지 이류로 앱이 정지된 동안 소켓 연결을 해제할 수 있습니다. 신호 손실이나 네트워크 전환 오류에 대비되어있어야 예상치 못한 문제가 발생하지 않습니다.
* **백그라운드로 이동하기 전에 앱 상태를 저장하십시오.** 정지된 앱은 메모리에서 제거되기 전에 앱에 알림이 제공되지 않이 때문에 상태보존 메커니즘을 활용해 앱의 UI 상태를 디스크에 저장해야합니다. 이 기능에 대한 자세한 내용은 [Preserving Your App’s Visual Appearance Across Launches](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforImplementingYourApp/StrategiesforImplementingYourApp.html#//apple_ref/doc/uid/TP40007072-CH5-SW2)을 참고하세요.
* **백그라운드로 이동하기 전에 불필요한 객체에 대한 강한 참조를 해제하세요.** 앱에서 용량이 큰 객체(이미지)를 관리하는 경우, 백그라운드로 이동하기 전에 해당 캐시에 대한 강한 참조를 해제해야합니다.
* **정지되기 전에 공유 시스템 자원 사용을 중지하십시오.** 주소록, 캘린더, 데이터 베이스와 같이 공유 시스템 리소스와 상호작용하는 앱은 정지되기 전에 해당 리소스 사용을 중지해야합니다.
* **View 업데이트를 하지 마십시오.** 앱이 배경에 있을때는 View가 표시되지 않으므로 업데이트를 하지않는 것이 좋습니다.
* **외부 액세서리에 대한 연결 알림 및 연결 해제 알림에 응답하세요.** 
* **백그라운드로 이동할 때 활성화된 경고 창을 정리하세요.** 앱을 백그라운드로 전환할때 시스템이 자동으로 `UIActionSheet` 또는 `UIAlertView`를 닫지 않습니다. 백그라운드로 이동하기 전에 적절한 정리를 해야합니다.
* **백그라운드로 이동하기 전에 View에서 민감한 정보를 제거하십시오.** 앱이 백그라운드로 전환되면 시스템은 앱 화면을 스냅 샷으로 찍은 다음, 다시 foreground로 전환 할때 간단히 표시하게 됩니다. `applicationDidEngetBackground:` 메서드에서 돌아오기 전에 비밀 번호나 개인 정보는 숨기도록 해야합니다.
* **백그라운드에 있는 동안 최소한의 작업을 수행하십시오.**

---

### Opting Out of Backgound Execution
> 백그라운드에서 앱이 전혀 실행하지 않게하려면 Info.plist 파일의 `UIApplicationExitsOnSuspend` 키의 값을 true로 추가하여 백그라운드를 명시적으로 선택해제 할 수 있습니다. 앱이 선택 해제되면 앱 생명 주기가 not-running, inactive, active 상태를 순환하며 백그라운드라 정지 상태로 들어가지 않습니다. 사용자가 홈 버튼을 눌러 앱을 종료하면 `appDelegate`의 `applicationWillTermintae:` 메소드 호출되고 앱이 종료되기 전에 약 5초간 정리되고 not-running 상태로 이동하게됩니다.

* 백그라운드 실행을 선택해제 하는 것은 권장하지는 않지만, 특정 조건에서는 기본 옵션이 될 수 있습니다. 특히 백그라운드에서 실행되는 코드가 복잡성을 대단히 증가시키는 경우 앱을 종료시키는 것이 간단한 해결책일 수 있습니다. 
* 앱이 많은 메모리를 소비하고, 쉽게 메모리를 해제 할 수 없는 경우 시스템이 앱을 빠르게 종료하여 다른 앱을 위한 공간을 확보 할 수 있습니다. 

<a name = "상태전환"></a>
## [5. Strategies for Handling App State Transitions](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforHandlingAppStateTransitions/StrategiesforHandlingAppStateTransitions.html#//apple_ref/doc/uid/TP40007072-CH8-SW1)
> 앱의 실행 상태에 따라 시스템은 어느정도의 기대치를 가지고 있으며, 상태 전환이 발생하면 시스템이 app delegate에게 알림을 줍니다. `UIApplicationDelegate` 프로토콜을 이용해 앱의 상태 변화를 감지하고, 각 상화에 맞는 적절한 대응을 할 수 있습니다. 예를 들어 foreground에서 background로 전환할때 저장되지 않은 데이터를 기록하고, 진행중인 작업을 중지 할 수 있습니다. 이번 세션에서는 상태전환코드를 어떻게 구현하면 좋을지에 대한 팁과 지침을 제공합니다.

### What to Do at Launch Time
> 앱이 시작되면(launched) app delegate의 `application:willFinishLaunchingWithOptions:` 와`application:didFinishLaunchingWithOptions:` 메소드를 사용해 다음과 같은 작업을 할 수 있습니다.

	 * 앱이 시작된 이유에 대한 정보를 확인하고, 적절하게 대응하십시오.
	 * 앱의 중요한 데이터 구조를 초기화 하십시오.
	 * 앱으로 보여줄 Window 및 view를 준비하십시오. 

* 앱이 시작 될때 시스템은 자동으로 `main stroyboard`와 `initial view controller`를 로드합니다.
* `state restoration`(상태복원)을 지원하는 앱의 경우 `willFinishLaunchingWithOptions :` `application : didFinishLaunchingWithOptions :` 두가지 메소드를 이용해 이전 상태 인터페이스로 복원합니다.
* 위 두가지 메소드에서 실행되는 작업은 가능한 가벼워야합니다.
*  앱은 초기화 작업은 5초 이내에 완료되어야 하며, 초기화 작업이 너무 오래 걸리면 시스템이 응답없음으로 간주하고 앱을 종료시킬 수 있습니다. 
*  실행 속도를 늦출 수 있는 작업(네트워크 액세스)은 보조 thread에서 실행 되도록 조정해야합니다.

#### The Launch Cycle 
* 앱이 foreground 상태로 들어올때 시스템이 main thread를 생성하고 앱의 main 함수를 호출합니다. 기본 main 함수는 UIKit 프레임워크를 제어하고, 앱이 구동되는데 필요한 초기화 작업을 해줍니다.
* 앱이 background 상태로 시작할때는 foreground 상태로 시작될때와 실행주기에 약간의 차이가 생기는데, 가장 큰 차이점은 작업을 처리한 뒤, 정지(suspend) 될 수 있다는 점입니다. 백그라운 상태로 실행될때에도 시스템은 UI 파일을 로드하지만, 사용자 화면에 표시하지는 않습니다.
* 앱이 어느 상태로 시작되는지 확인하려면 `UIApplication` 객체의 `applicationState` 속성값을 확인해 보면 알 수 있습니다. 

#### Launch in Landscape Mode
* 일반적으로 앱은, 세로 모드로 실행되며 필요한 경우 기기 방향에 맞게 인터페이스를 회전합니다. 만약 가로 방향만 사용하려면 다음과 같은 작업을 수행하여 시스템에 가로 방향으로만 앱이 실행되도록 설정해야합니다.
	* Info.plist에 `UIInterfaceOrientation` 키를 추가하고 값을 `UIInterfaceOrientationLandscapeLeft` `UIInterfaceOrientationLandscapeRight` 둘 중 하나로 설정하세요.
	* `layout`이나 `autosizing` 옵션이 설정되어있는지 확인하세요.
	* `shouldAutorotateToInterfaceOrientation:` 메소드가 `true`를 반환하도록 오버라이드 하세요.(세로모드일 경우 `false`)

#### Installing App-Specific Data Files at First Launch
* 앱의 구동에 요구되는 데이터나 설정 값 셋팅을 위해 첫 실행 주기(first launch cycle)를 이용할 수 있습니다. 
* 앱의 특정 데이터 파일은 Library/Application Support/<bundle ID>/directory에 위치해야합니다.
* 앱 번들 내에 있는 파일은 code signed 되어있어 수정을 할 수 없기때문에, 만약 수정이 필요한 경우에는 반드시 해당 파일을 샌드박스의 다른 폴더로 복사한 다음 수정해야합니다.


---

### What to Do When Your App Is Interrupted Temporaily
> 시스템 알림이 전달되면 앱은 일시적으로 제어권을 잃게되고, 사용자 터치 이벤트를 받아 들이지 않습니다. 이러한 상황을 대비해 앱은 `applicationWillResignActive` 메소드에서 아래와 같은 작업을 수행해야 합니다.

	* 데이터 및 상태를 저장합니다.
	* 타이머 및 기타 주기적 작업을 중지합니다.
	* 메타데이터 쿼리를 모두 중지합니다.
	* 새로운 작업을 시작하지 않도록 합니다.
	* 동영상 재생을 중지합니다.(AirPlay를 통한 재생은 제외)
	* OpenGL ES 프레임 속도를 재조절합니다.

* 앱이 일시 정지 상태에서 벗어나 다시 활성 상태로 되돌아 오면 `applicationWillResignActive:` 메소드에서 했던 작업들을 `applicationDidBecomeActive:` 메소드에서 되돌려야합니다. 
* 만약 앱에 `NSFileProtectionComplete` 옵션으로 보호된 파일이 있다면, 화면이 잠겨있는동안 해당 파일에 접근하지 못하도록 `applicationWillResignActive:` 메소드에서 해당 파일에 대한 참조를 닫아줘야합니다.

<center>
<img src="https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/app_interruptions_2x.png
" width="400px"/>
</center>


### What to Do When Your App Enters the Foreground
>Foreground로 들어가면 백그라운드 상태로 이동하면서 중지된 작업을 재개할 수 있습니다. `applicationDidEnterBackground :` 메소드에서 수행 된 작업을 `applicationWillEnterForeground` 메소드에서 실행취소해야하며 `applicationDidBecomeActive:` 메소드는 실행때와 동일한 작업을 계속 수행할 수 있도록 해야합니다.

* `UIApplicationWillEnterForegroundNotification` 알림을 이용해 앱이 foreground로 다시 들어오는 때를 추측 할 수있습니다. 

#### Be Prepared to Process Queued Notifications
* 정지 상태의 앱이 foreground 또는 background 실행 상태가 될때, 대기 중인(queued) 알람을 처리할 준비가 되어야합니다. 앱이 정지되어 있는 동안에는 앱이 코드를 실행하지 못하므로, 시스템이 관련 알람을 대기열에 추가해두고 코드 실행이 시작되는 순간 앱에 알람을 전달합니다.
* 앱에 전달되는 알람의 내용과 키에는 아래와 같은 것이 있습니다.
	* 악세서리 연결 등록 / 연결 해제(`EAAccessoryDidConnectNotification`, `EAAccessoryDidDisconnectNotification`)
	* 기기 화면 방향 전환(`UIDeviceOrientationDidChangeNotification`)
	* 다량의 시간 변화(`UIApplicationSignificantTimeChangeNotification`)
	* 배터리 상태 및 잔량 변화(`UIDeviceBatteryLevelDidChangeNotification`, `UIDeviceVatteryStateDidChangeNotification`)
	* 근접 상태 변화(`UIDeviceProximityStateDidChangeNofitication`)
	* 보호된 파일의 상태 변화(`UIApplicationProtectedDataWillBecomeUnavailable`, `UIApplicationProtectedDataDidBecomeAvailable`)
	* 외부 화면 연결 / 연결 해제(`UIScreenDidConnectNotification`, `UIScreenDidDisconnectNotification`)
	* 화면의 스크린 모드 변화(`UIScreenModeDidChangeNotification`)
	* Settings에서 변경 가능한 앱 설정값의 변화(`NSUserDefaultsDidChangeNotification`)
	* 사용 언어 및 지역 변경(`NSCurrentLocaleDidChangeNotification`)
	* 사용자 iCloud 계정 변경(`NSUbiquityIdentityDidChangeNotification`)

* 대기 중인 알람(queued notification)은 사용자 터치나 입력 전에 앱의 main run loop로 전달됩니다. 
* 앱이 재개 될때 눈에 띄는 지체가 발생하지 않도록 이러한 이벤트를 빠르게 처리 할 수 있어야합니다.

#### Handle iCloud Changes
* iCloud 상태 변화에 따라 앱은 iCloud 관련 UI를 업데이트하거나, 관련문서에 대한 참조 상태를 변경해야합니다.
* 앱에서 iCloud 사용 여부를 묻는 메시지가 이미 표시되는 경우, iCloud 상태가 변경되었다고 해서 다시 메시지를 띄우지 마십시오. 관련된 설정은 앱 환결 설정이나, 시스템 Settings에서 사용자가 지정 할수 있도록 하십시오.

#### Handle Locale Changes
* 앱이 일시 정지된 상태에서 사용자가 Locale(지역)을 변경하면 날짜, 시간 및 숫자와 같은 정보를 강제 업데이트 할 수 있습니다.
* Locale 변경에 대등하기 위한  코드 국제화와 관련된 자세한 내용은 [Internationalization and Localization Guide](https://developer.apple.com/library/content/documentation/MacOSX/Conceptual/BPInternational/Introduction/Introduction.html#//apple_ref/doc/uid/10000171i)를 참고하세요.

#### Handle Changes to Your App's Settings
* Settings를 통해 관리되는 설정값이 있다면 `NSUserDefaultsDidChangeNotification` 알람을 확인해야 합니다. 앱이 일시 정지되거나, 백그라운드 상태일때 설정을 수정할 수 있으므로 해당 알람을 사용하면 설정 변경에 따른 적정한 반응을 할 수 있습니다.
* 경우에 따라 해당 알림에 응답하는 것은 잠재적인 보안 허점을 줄일 수 있습니다. 암호 또는 기타 보안 관련 정보가 변경된 경우 인터페이스를 적절히 업데이트 해야합니다.

---

### What To Do When Your App Enters the Bakcground
> Foreground에서 백그라운드로 이동할때는 `applicationDidEnterBackground:`  메소드를 사용하여 다음과 같은 작업을 수행할 수 있습니다. 

	* 앱 스냅샷 준비
	* 메모리 확보 

* `applicationDidEnterBackground:` 메소드는 약 5초 동안 모든 작업을 수행하게 되는데, 작업 수행 이내에 메소드가 return 되지 않으면 앱에 종료되고 메모리에서 제거됩니다. 
* 작업 수행에 더 많은 시간이 필요한 경우 `beginBackorundTaskWithExpirationHandler:` 메소드를 호출하여 백그라운드 실행 시간을 요청한 다음, 보조 스레드에서 장시간 작업을 수행합니다.
* 백그라운드 작업을 시작하는지 여부에 관계없이 `applicationDidEnterBackground:` 메소드는 5초 이내에 종료되어야 합니다.

#### The Background Transition Cycle
* 사용자가 홈 버튼을 누르거나, Sleep/Wake 버튼을 눌렀을 때, 시스템에 다른 앱을 실행 시킬때 foreground에 있던 앱을 비활성 상태(inactive state)가 되었다가 백그라운드 상태로 진입하게 됩니다. 이러한 상태 변화는 `applicationWillResignActive:` 와 `applicationDidEnterBackground:` 메서드를 호출하게 됩니다.
* 대부분의 앱은 `applicationDidEnterBackground:` 메소드가 호출 되고, 잠시후 일시 중지 상태로 전환됩니다.  

<center>
<img src="https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/app_bg_life_cycle_2x.png" width="450px"/>
</center>

#### Prepare for the App Snapshot
* app delegate의 `applicationDidEnterBackground:` 메소드가 반환 되고 잠시 후 시스템은 앱의 화면을 스냅샷을 촬영합니다. 
* 백그라운드 작업 수행이 진행되어 화면에 변화가 생기면, 시스템은 스냅샷에 변화 사항을 반영합니다.
* 촬영된 스냅샷은 멀티 태스킹 UI에 사용됩니다.
* 백그라운드에 진입할때 view에 변화를 주는 경우, `snapshotViewAfterScreenUpdates:` 메소드를 호출하여 변경 사항을 렌더링 할 수 있습니다. 
* `snapshotViewAfterScreenUpdates:` 메소드의 매개변수를 `true` 값으로 설정하면 강제로 스냅샷을 업데이트 합니다.

#### Reduce Your Memory Footprint
* 시스템은 최대한 많은 메모리를 유지하려고 시도하며, 메모리가 부족한 경우에는 일시 중지된 앱을 종료하여 메모리를 회수합니다.
* 백그라운드에서 대용량의 메모리를 사용하는 경우, 종료 1순위 대상이 됩니다.
* 백그라운드에 들어갈때, 앱은 더 이상 필요없는 개체에 대한 강한 참조를 해제해야합니다. 
* 가능한 빨리 강한 참조를 해제해야 하는 객체에는 아래와 같은 예가 있습니다.
	* 생성한 이미지 객체
	* 디스크에서 다시 로드할 수 있는 대형 미디어 또는 데이터 파일
	* 앱에 당장 필요하지 않으며, 나중에 쉽게 다시 만들 수 있는 모든 객체

<a name = "앱기능"></a>
## [6. Strategies for Implementing Specific App Features](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/StrategiesforImplementingYourApp/StrategiesforImplementingYourApp.html#//apple_ref/doc/uid/TP40007072-CH5-SW1)
>각각의 앱은 서로 다른 기능과 필요성을 지니지만, 일부 특징은 다양한 앱에는 공통적입니다. 이번 섹션에서는 특정  유형의 기능을 어떻게 앱에서 구현할 수 있는지에 대한 가이드라인을 제시합니다.

### Privacy Strategies
> 사용자의 개인 정보를 보호하는 것은 앱을 디자인할때 고려해야할 주요사항 입니다. 보호되어야 할 개인정보에는 사용자의 신원 및 개인 정보와 사용자의 데이터가 포함됩니다. 시스템 프레임워크는 연락처와 같은 데이터를 관리하기 위해 개인 정보 제어를 기본적으로 제공하지만, 앱은 로컬에서 사용하는 데이터를 보호하기위해 개별 적인 조치를 취해야 합니다.

#### Protecting Data Using On-Disk Encryption
> 데이터 보호는 내장된 하드웨어에 암호화 된 형식으로 파일을 저장하고, 필요할때 암호를 해독하는 방식을 사용합니다. 사용자긔 기기가 잠겨있는 동안에는 해당 파일을 만든 앱도 파일에 접근 할 수 없습니다. 데이터 보호는 대부분의 iOS 기기에서 사용할 수 있습니다.

* 파일을 보호하려면 원하는 보호 수준을 나타내는 속성을 파일에 추가하십시오. 
* `NSData` 클래스 또는 `NSFileManager`클래스를 사용하여 보호 여부를 지정할 수 있습니다.
	*  `NSData`의 `writeToFile: options: error:` 메소드를 사용하여 새로 작성하는 파일에 보호 설정을 할수 있습니다.
	*  `NSFileManager` 의 `setAttributes: ofItemAtPath: error:` 메소드를 사용해 기존 파일의 보호 값을 설정 할 수 있습니다.
* 모든 개체는 `UIApplication`의 `protectedDataAvailable` 속성 값을 검사하여 현재 파일에 접근이 가능한지 여부를 확인 할수 있습니다.
* 새롭게 작성하는 파일의 경우, 데이터를 작성하기 전에 보호하는 것이 좋습니다.

#### Identifying Unique Users of Your App
> 앱에서 사용자를 식별하는 작업을 수행 할때는 항상 투명하게 작업해야합니다. 사용자를 식별하는데 사용되는 몇가지 방법에는 다음과 같은 방법이 있습니다.
 
 * 사용자 정보로 로그인하는 화면을 구현해 서버의 특정 계정에 연결합니다.
 * `UIDevice`의 `identifierForVendor` 속성을 이용해 서로다른 장치 사용자를 구분합니다.
 * 광고 목적으로 사용자를 식별하기 위해서는 `ASIdentifierManager` 클래서의 `advertisingIdentifier` 값을 이용해 사용자 ID를 확보 할 수 있습니다.

---

### Respecting Restrictions
> 사용자는 엡에 사용되는 미디어의 등급을 지정하여 제한 설정할 수 있습니다. 만약 앱이 미디어를 포함하거나 수정하는 경우, 제한 설정을 확인하고 그 변경사항에 맞는 응답을 해야합니다.

* 제한 설정 값을 가져오려면 `standardUserDefaults` 객체의 `objectForKey`메서드를 사용해 각 값을  확인 할 수 있습니다.
	* `com.apple.content-rating.ExplicitBooksAllowed`
	* `com.apple.content-rating.ExplicitMusicPodcastsAllowed`
	* `com.apple.content-rating.AppRating`
	* `com.apple.content-rating.MovieRating`
	* `com.apple.content-rating.TVShowRating`
* `objectForKey`가 위 특정 키에 대해 `nil`을 반환하면 해당 정보를 사용할 수 없음을 의미합니다. 이런 경우에는 앱 자체 정책을 사용하여 적절한 등급을 결정 할 수 있습니다.

---

### Supporting Multiple Versions of iOS
>  최신버전의 iOS와 이전 버전을 함께 지원하는 앱의 경우, 이전 버전의 iOS에서 최신 API를 사용하지 못하도록 런타임 검사를 실시해야합니다. 런타임 검사는 현재 OS에서 사용할 수 없는 기능을 사용하려고 할때 발생하는 앱 충돌(crash)를 방지해줍니다.

* 해당 class가 존재하는지 검사하기 위해 Class 객체가 nil인지 아닌지 확인하는 방법이 있습니다.
* 해당 메소드를 사용할 수 있는지 확인하기 위해 `instancesRespondToSelector:` 또는 `respondsToSelector:`를 사용할 수 있습니다.
* 여러가지 대상을 지원하는 코드를 작성하는 방법에 대해서는 [SDK Compatibility Guide](https://developer.apple.com/library/content/documentation/DeveloperTools/Conceptual/cross_development/Introduction/Introduction.html#//apple_ref/doc/uid/10000163i)를 참고하세요.

---

### Preserving Your App's Visual Appearance Across Launches
>  백그라운드로 전환된 앱은 현재 foreground에서 구동되는 앱의 메모리 확보를 위해 언제든지 시스템에 의해 종료 될수 있습니다. 하지만 사용자는 앱을 전환하거나 할때 앱이 일시 종료되었다고 생각하고, 완전히 종료되었다고 생각하지 않기 때문에 마지막 사용시점으로 되돌아오는 것이 좋습니다. 이러한 동작은 사용자에게 더 나은 UX를 제공할 수 있습니다. 이번 세션에서는 iOS의 infrastrucure를 활용해 상태 보존/복원(state preservation, restroration)을 구현 하는 방법을 알아봅니다.

* `UIKit`의 상태 보존 시스탬은 `View Controller`와 `View`의 상태를 보존, 복원하기 위한 간단하고 유연한 infrastructure을 제공합니다.
* Infrastructure는 적절한 시점에 보존 및 복구 프로세스를 추진합니다.
* 앱의 상태 보존을 위해 고려해야하는 위치는 다음 세가지가 있습니다.
	* App delegate 객체 (앱의 최상위 레벨 상태 관리)
	* View Controller 객체 (앱의 전반적인 UI 관리)
	* Custom Views (보존해야할 custom data가 있는 경우)

#### Enablikng State Preservation and Restoration in Your App
* 앱의 상태 보존/복원은 자동기능이 아니므로, 앱에서 사용하도록 선택해야합니다. 
* App delegate에서 다음 메소드를 구현하여 `true`값이 반환되도록 합니다.
	 * `application: shouldSaveApplicationState:` 
	 * `application: shouldRestoreApplicationState:`

#### The Preservation and Restoration Process
> 앱 상태 보존 / 복원 기능을 이용하기 위해서는 앱에서 보존되어야할 객체를 식별해줘야 `UIKit`에서 적절한 시점에 객체를 보존 / 복원하는 작업을 수행 할 수 있습니다. `UIKit` 은 보존 / 복원 프로세스의 많은 부분을 처리하므로, 어떻게 작업을 하는지 알아두면 전반적인 코드 작성과 흐름이해에 용이합니다.

* 상태 보존 / 복원을 고려할때, 두가지 프로세스를 분리하는 것이 좋습니다.
	* 보존:  앱이 foreground와 background를 오고갈때 `UIKit`은 앱의 상태를 적절히 보존합니다. 상태 보존에 필요한 관련 데이터를 암호화된 디스크 상의 파일에 쓰고, 다음번에 앱이 launch될때 `UIKit`이 해당 파일을 찾아 앱 상태 복원 시도합니다. 물론, 이때 파일은 암호화 되어있기 때문에 기기가 잠금해제 되어있는 경우에만 발생합니다.
	* 복원: 상태를 복원하는 동안 `UIKit`은 보존 된 데이터를 사용하여 UI를 재구성 하지만, 실제 객체 생성은 코드에 의해 처리됩니다. Storyboard에 접근해 객체를 로드하는 것은 가능하지만, 실제로 어느 객체가 필요한지, 어느 객체가 생성되었는지는 코드를 통해서 알 수 있기 때문입니다. 객체가 생성되면 `UIKit`이 보존해 놓은 자료를 이용해 객체를 초기화 합니다.
* 각각의 상태에서 앱은 다음과 같은 역할을 하게됩니다.
	* 보존
		*  `UIKit`에 상태 보존을 해야한다고 알립니다.
		* `UIKit`에 어떤 `ViewController`와 `View`가 보존 될지 알립니다.
		* 보존되는 객체에 대한 데이터를 인코딩해야합니다.
	* 복원
		*  `UIKit`에 상태 복원을 해야한다고 알립니다.
		*  `UIKit`에서 요청한 객체를 제공(생성) 해야 합니다.
		*  복원할 객체의 상태를 디코딩하고, 이를 이용해 객체를 이전 상태로 되돌립니다.
	*  가장 중요한 부분은 `UIKit`에게 보존 / 복원할 객체를 알려주는 것으로, 관련 코드를 디자인 할때 대부분의 시간을 소비해야합니다. 
*  몇몇 `ViewContrller`는 앱의 `main storyboard`에서 자동으로 로드되지만 `push`되거나 `present`되는 `ViewController`들은 접근이 불가능 하기 때문에 `restoreation idientifier`를 지정해줘야합니다.

<center>
<img src="https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Art/state_vc_hierarchy_preserve_2x.png" width="450px"/>
</center>

* 보존하려는 `view controller`에 대해 추후 어떤 방식으로 복원될지 결정해야합니다. `UIKit`는 두가지 방식을 제공합니다. App delegate를 이용해 재구성하거나 view controller에 `restoration class` 를 구현하는 방식이 있습니다. 각 방식에 대한 사용팁은 아래와 같습니다.
	* 만약 main storyboard를 통해 view controller가 항상 로드된다면, `restore class` 를 구현하지 마십시오. 대신 app delegate를 이용해 객체를 복원하십시오.
	* 만약 main storyboard를 통해 view controller가 로드되지 않는다면, `restore class`를 구현하는 것이 좋습니다.

#### What Happnes When You Exclude Groups of View Controllers?
> 어떤 view controller의 restore identifier가 `nil`이면 해당 view controller와 이의 child view controllers은 보존 데이터에서 제외됩니다. 만일 child view controller를 수동으로 보존하려면 childe view controller에 개별적으로 rester identifier를 지정해주면됩니다.

#### Checklist for Implementing State Presevtion and Restoration
> 앱의 상태 보존 / 복원을 지원하려면 app delegate와 view controller가 상태 정보를 인코딩/ 디코딩 할 수 있도록 수정해야합니다. 상태 보존 / 복원 기능 추가를 위한 코드 작성시, 아래 체크리스트를 확인하십시오.

* (필수)  `application:shouldSaveApplicationState:`, `application:shouldRestoreApplicationState:`를 구현하십시오.
* (필수) 보존하려는 각각의 view controller의 `restorationIdentifier`에 빈값이 아닌 String값 입력하십시오.
* (필수) `application:willFinishLaunchingWithOptions:` 에서 복원할 스크롤 위치와 관련 UI 설정을 위해 window를 살피십시오.
* 필요하다면 view controller에 `restoration class`를 구현하십시오.
* 보존 / 복원 하려는 view controller 혹은 view에 `encodeRestorableStateWithCoder:`와 `decodeRestorableStateWithCoder:`메소드를 추가하십시오.
* 테이블 뷰나 콜렉션 뷰에서 data soucre역할을 하는 객체는 `UIDataSourceModelAssociation protocol`을 구현해야합니다.  필수사항은 아니지만 이 프로토콜은 해당 뷰에서 선택되거나 보이는 영역의 복원을 도와줍니다.

#### Enabling State Preservation and Restoration in Your App
> 앱 상태 보존 / 복원을 자동 기능이 아니므로 app delegate에서 아래 두 메소드를 구현해 기능 지원을 명시해야합니다.
 
* `application:shouldSaveApplicationState:`
* `application:shouldRestoreApplicationState:`

#### Preserving the State of Your View Controllers
> View controller의 상태 보존을 위해서는 다음 작업을 해야합니다.

 * (필수) `restore identifier` 할당
 * (필수) 앱 실행 시점(launch)에서 새로운 view controller 를 생성할 코드 작성
 * (옵션) 앱을 다시 실행했을때 재생성 되지 않는 상태 정보를 위해 `encodeRestorableStateWithCoder:`와 `decodeRestorableStateWithCoder:` 메소드 구현

#### Preserving the State of Your Views
> 특정 view에 보존할만한 가치가 있는 정보가 있다면, view가 포함된 view controller의 나머지 부분과 함께 보존 될 수 있습니다. view의 상태를 저장하는 경우는 view 자신이 속한 view controller와 독립적으로 사용자가 변경할 수 있는 경우입니다. 예를들어 스크롤뷰는 스크롤의 현재 위치값(position)을 저장하게되는데 이는 view controller는 관심이 없는 값입니다. 뷰의 상태값을 저장하기 위해서는 다음과 같은 작업을 해야합니다.
 
 * view의 `restoretionIdentifier`값을 할당합니다.
 * view가 속해있는 view controller도 유효한 `restorationIdentifier`값을 지녀야합니다.
 * tableView와 collectionView의 경우 `UIDataSourceModelAssociation protocol`을 준수하십시오.

#### Preserving Your App's High-Level State
> view controller와 view에 보존 되는 데이터가 아니라도, 다음 두 메소드를 이용해 앱에 필요한 기타 데이터를 저장 할 수 있습니다. 
 
 * `application:willEncodeRestorableStateWithCoder:`
 * `application:didDecodeRestorableStateWithCoder:`
 
> 위 메소드는 보존 프로세스의 맨 처음에 호출 되므로 UI의 현재 버전 등과 같은 high-level의 앱 상태를 저장 할 수 있습니다.

#### Tips for Saving and Restore State Information
> 앱 상태 보존 / 복원을 구현하는 경우 다음 지침을 고려하십시오.

* **버전 정보를 앱 상태와 함께 인코딩 합니다.** 보존을 할때에는 앱의 현재 UI 버전을 식별할 수 있는 버전 문자열 또는 숫자를 인코딩 하는 것이 좋습니다. 버전 정보는 app delegate의 `application:willEncodeRestorableStateWithCoder:` 메소드에서 인코딩 할수 있으며, `application:shouldRestoreApplicationState:` 메소드가 호출 될때 인코딩 되었던 정보를 확인할 수 있습니다.
* **앱 상태 정보에 데이터 모델 객체를 포함하지 마십시오.** 앱은 iCloud 또는 디스크의 로컬 파일에 데이터를 별도로 저장해야합니다. 상태 복원 메커니즘을 사용하여 데이터 저장시, 복원 작업중에 문제가 발생하면 보존 된 데이터가 삭제 될 수 있습니다. 
* **앱 상태 보존은 기존에 디자인된 방식으로 view controller를 사용할 것을 기대합니다.** 앱의 view controller 계층 구조는 기존에 설계된 방식을 유지해야합니다. view controller간의 계층/포함 관계와 상과없이 view를 보여주는 경우 보존 시스템이 보존할 view controller를 제대로 찾아내지 못할 수도 있습니다.
*  **모든  view controller를 보존하는 것은 의도에 맞지 않을 수있습니다.** 어떤 경우의 view controller는 보존하는 것이 적절하지 않을 수 있습니다. 예를 들어 사용자의 민감한 정보가 있는 화면은 보존 / 복원하지 않습니다.
*  **복원 프로세스 중에 view controller 클래스를 변경하지 마십시오.** 복원 하는 동안 앱의 클래스가 보존했던 객체의 클래스와 일치 하지 않거나 하위 클래스가 아니면 복원 작업을 하지 않습니다.
*  **사용자가 강제로 앱을 종료하면 시스템에서 앱의 보존 상태를 자동으로 삭제합니다.** 앱이 종료될때 복원 상태를 삭제하는 것은 일종의 안정 장치입니다. 보존 / 복원 기능을 테스트 하려면 멀티태스킹 상태를 사용하지 말고 Xcode를 이용해 임시 명령이나 제스처로 exit를 호출하도록 하십시오.

---

### Tips for Developing a VoIP App
> VoIP(Voice over Internet Protocol) 앱을 통해 사용자는 기기의 셀룰러 서비스 대신 인터넷 연결을 이용해 전화를 걸 수 있습니다. iOS 8 이상에서는 APN(Apple Push Notification Serviece) 및 PushKit 프레임 워크의 API를 사용하여 VoIP 앱을 만들 수 있습니다. 푸쉬기능을 기용하면 VoIP 사용을 위한 지속적인 네트워크 연결이나 소켓 구성을 하지 않아도 됩니다. VoIP 푸시가 도착하면 앱이 푸시를 처리할 시간이 주어집니다(앱이 종료된 상태에도). 

* VoIP 구현을 위해 다음과 같은 요구사항들이 있습니다.
	* **앱에 VoIP 백그라운드 모드를 사용해야합니다.** Xcode project Capabilities 탭에서 백그라운드 모드를 활성화 합니다. 보통 VoIP 앱은 오디오 내용을 포함하기 때문에 Audio 및 AirPlay 백그라운드 모드도 사용하는것이 좋습니다.
	* PushKit API를 사용하여 VoIP 푸시 알림을 수신하고, 수신 알람을 처리하도록 하십시오.
	* 오디오 세션을 구성하고 활성화 시키십시오.
	* iPhone에서 더 나은 UX를 보장하려면 `Core Telephony` 프레임 워크를 이용하십시오.
* VoIP 백그라운드 모드를 활성화 하면 백그라운드에서 오디오를 재생할 수도 있고, 시스템 부팅 직후 백그라운드에서 재시작 되어 VoIP 서비스를 항상 사용 할수 있습니다.


#### Using the Reachability Interfaces to Improve the User Experience
> VoIP 앱은 네크워크 환경에 크게 의존하기 때문에, `System Configuration framework`의 `reachability interface`를 활용해 네크워크 가능성을 확인하고 결과에 맞게 동작을 조정해야합니다. `reachability interface`를 사용하면 네트워크 상태가 변경 될때마다 앱에 알림을 보낼 수 있습니다. 변경 사항을 감지하여 사용자에게 VoIP 연결 상태에 대해 알려주도록 해야합니다.

* 네트워크 가용성을 파악해 앱의 동작을 조정하면, 기기의 배터리 수명을 향상 시킬 수 있으며 앱이 더 자주 잠자기 상태로 있을 수 있습니다.
* `reachability interface`에 대한 자세한 내용은 [System Configuration Framework Reference](https://developer.apple.com/reference/systemconfiguration)를 참고하세요.

<a name = "앱통신"></a>
## [7. Inter-App Communication](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Inter-AppCommunication/Inter-AppCommunication.html#//apple_ref/doc/uid/TP40007072-CH6-SW2)
>  기기내 앱간 통신은 간접적인 방식을 통해서만 가능합니다. AirDrop을 사용하여 다른 앱과 파일 및 데이터를 공유 할 수 있습니다. 또한 앱 URL을 사용하여 앱에 정보를 보낼 수 있도록 custom URL Schemes을 정의할 수 있습니다.
> * 참고: `UIDocumentInteractionController` 객체 또는 `document picker`를 사용하여 앱간 파일 교환도 가능합니다. `UIDocumentInteractionController`에 대한 정보는 [Document Ineraction Programming Topics for iOS](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/DocumentInteraction_TopicsForIOS/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010403)를, `document picker`에 관한 정보는 [Document Picker Programming Guide](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/DocumentPickerProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014451)를 참고하세요.

### Supporting AirDrop
> AirDrop을 통해 근처에 있는 기기와 사진, 문서, URL 그리고 각종 데이터들을 공유 할 수 있습니다. AirDrop은 peer-to-peer 네트워킹을 통해 주변 기기를 찾고 연결합니다.

#### Sending Files and Data to Another App
> AirDrop을 이용해 파일과 데이터를 전송하려면 `UIActivityViewController` 객체를 사용하십시오. `UIActivityViewController`객체를 사용하면 현재 보여지는 UI 화면에 activity sheet를 보여줍니다. 해당 객체를 생성할때 전송하려는 데이터를 지정하십시오. `UIActivityViewController`는 특정 데이터를 지원 가능한 activities만 표시해줍니다. AirDrop의 경우 이미지, 문구, URL 그리고 몇몇 다른 타입의 데이터 전송이 가능합니다. `UIActivityItemSource` 프로토코를 채택한 custom 객체 전송도 가능합니다.

* 더 많은 정보를 보려면 [UIActivityViewController Class Reference](https://developer.apple.com/reference/uikit/uiactivityviewcontroller)와 [UIActivity Class Reference](https://developer.apple.com/reference/uikit/uiactivity)를 참고하세요.

#### Receiving Files and Data Send to Your App
> AriDrop을 통해 수신한 파일을 받으려면 아래와 같은 작업을 하십시오.

* Xcode에서 앱에서 열 수 있는 문서 타입을 명시하십시오.
* app delegate에 ` application:openURL:sourceApplication:annotation:` 메소드를 구현하여 다른 앱을 통해 받은 데이터를 받으십시오.

* Xcode project Info 탭에 앱에서 지원가능한 문서 타입을 지정할 수 있는 문서 유형 섹션이 있습니다. 문서 유형의 이름과 데이터 유형을 나타내는 하나 이상의 UTI를 지정해야합니다. 예를 들어 png 파일에 대한 지원을 선언하려면 public.png를 UTI 문자열로 포함합닏. iOS는 지정된 UTI를 사용하여 앱이 주어진 문서를 열 수 있는지 확인합니다.
* 문서를 앱 컨테이너에 전송 받게 되면 (필요한 경우)시스템이 앱을 launch 하고 app delegate의 `application:openURL:sourceApplication:annotation:` 메소드를 호출하게 됩니다. 이때 앱이 foreground 상태라면 이 메소드를 사용해 유저에게 파일을 보여주고, background 상태라면 나중에 열 수 있는 파일이 있음을 노트해둬야합니다.
* AirDrop을 통해 전송 된 파일은 암호화 되기 때문에 장치가 잠금 해제 되어있지 않으면 파일을 열 수 없습니다.
* AirDrop을 이용해 전송받은 파일은 앱에서 읽고, 삭제하는 것은 가능하지만 편집은 불가능 합니다.

---

### Using URL Schemes to Communicate with Apps
> URL scheme을 사용하면 정의해둔 프로토콜을 통해 다른 앱과 통신 할 수 있습니다. Scheme을 구축해둔 앱과 통신 하려면 적절한 형식의 URL를 구성하고 시스템에 열어달라고 요청해야합니다. 특정 sheme을 사용하려면 해당 scheme에 대한 지원여부를 선언하고, 해당 sheme을 통해 들어오는 URLs을 처리해야합니다.

* Apple은 http, mailto, tel 그리고 sms URL scheme에 대한 내장 지원을 제공합니다. 또한 Maps, YouTube 그리고 iPod 앱을 대상으로 하는 http기반 URL도 지원합니다. 
* 기본 scheme에 대한 핸들러는 고정되어있으며 변경 할 수 없습니다. (Apple에서 지원하는 것과 동일한 scheme을 사용하면 시스템에서는 기본 앱을 실행합니다.)
* Apple에서 지원하는 기본 scheme을 확인하려면 [Apple URL Scheme Reference](https://developer.apple.com/library/content/featuredarticles/iPhoneURLScheme_Reference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40007899)를 참고하세요.

#### Sending a URL to Another App
> 앱에 구현해 놓은 custom URL scheme을 이용해 데이터를 보내려면 적절한 URL을 먼저 생성하고 `openURL:` 메소드를 호출 하면 됩니다.

#### Implementing Custom URL Schemes
> 앱에서 특별한 형태의 URL을 수신 하려면, 해당 URL scheme를 시스템에 등록해야합니다. 앱은 종종 custom URL scheme를 이용해 다른 앱에서 서비스를 이용 할수 있도록 합니다. 예를 들어 지도 앱은 특정 위치를 지도에 표시하기 위한 URL을 지원합니다. 

* Registering Custom URL Schems: 앱에 custom URL을 등록하려면 Info.plist 파일에 `CFBundleURLTypes` 키를 포함하십시오. `CFBundleURLTypes` 키에는 dictionary가 array로 들어가 있습니다. dictionary는 `CFBundleURLName`, `CFBunldeURLSchemes` 키로 이루어져 있습니다.

* Handling URL Requests: custom URL scheme를 갖는 앱은 전달되는 URLs을 잘 처리 할 수 있어야 합니다. 모든 URLs은 app delegate를 통해 전달 됩니다. 전달된 URLs을 핸들하기 위해 아래 메소드들을 구현해야합니다.
	* `application:willFinishLaunchingWithOptions:`, `application:didFinishLaunchingWithOptions:` : 전달 받은 URL에 대한 정보를 파악하고, URL을 오픈할지 결정합니다. 둘 중 하나의 앱이 `false`를 반환하면 핸들링 코드는 호출되지 않습니다.
	* `application:openURL:sourceApplication:annotation:` : 전달받은 파일을 열기 위한 메소드입니다.

#### Displaying a Custom Launch Image When a URL is Opened
> Custom URL schemes을 지원하는 앱은 각 scheme에 custom launch image를 제공 할 수 있습니다. URL을 처리하기 위해 앱을 실행하고 관련 스냅 샷을 사용 할 수 없는 경우 제공한 launch image가 표시됩니다. launch image를 지정하려면 다음 규칙을 사용하는 이름의 png 이미지를 제공하십시오.

	  \<basename>-\<url_scheme>\<other_modifiers>.png
 
 * **basename**: Info.plist 파일에 있는 UILaunchImageFile키로 지정된 기본 이미지 이름을 나타냅니다.
 * **url_scheme**: URL sheme의 이름입니다.
 * **other_modifiers**: 이미지에 덧붙일수 있는 수식어는 [Information Property Key Reference](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009247)를 참고하십시오.

<a name = "성능향상"></a>
## [8. Performance Tips](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/PerformanceTips/PerformanceTips.html#//apple_ref/doc/uid/TP40007072-CH7-SW1)
> 앱 설계(design)가 전반적인 성능에 미치는 영향에 대해 개발 각 단계 마다 고려해야합니다. 전력 사용량과 메모리 사용량은 iOS 앱에 매우 중요한 고려 사항이며, 이 외에도 고려할 사항이 많이 있습니다. 이번 세션에서는 개발 프로세스 전반에서 앱 성능을 위해 고려해야할 요소에 대해 설명합니다.

### Reduce Your App's Power Cunsumption
> 모바일 기기에서 전력 소모는 항상 이슈입니다. iOS의 전원 관리 시스템은 현재 사용되지 않는 하드웨어 기능을 종료하여 전력을 절약합니다. 아래의 기능 사용을 최적화 하여 배터리 수명을 향상 시킬 수 있습니다.
 
	 CPU, Wi-Fi, 블루투스, 셀룰러 데이터(4G, 3G), 라디오, 위치 시스템, 가속도계, 디스크

* 앱 최적화의 목표는 최대한 효율적으로 가능한 많은 작업을 수행하는 것입니다. Instruments를 사용하여 앱의 알고리즘을 항상 최적화 해야합니다. 그러나 가장 최적화 된 알고리즘 조차도 기기 배터리에 부정적인 영향을 미칠 수 있으므로 코드를 작성할때 다음 지침을 고려하십시오.
	* Polling이 필요한 작업을 하지 마십시오. Polling 작업은 CPU가 잠자기 모드가 되는 것을 방지 합니다. Polling 대신 NSRunLoop 또는 NSTimer 클래스를 사용하여 필요에 따라 작업을 예약하십시오.
	* 가능하면 `UIApplication`의 `shared` 객체의 `idleTimerDisabled` 속성을 `flase`로 설정하십시오. ildeTimer는 일정 시간 동안 사용되지 않는 기기의 화면을 끄는 기능을 하는데, 앱 화면을 계속 켜둘 필요가 없는 경우 시스템이 종료시킬 수 있도록 하십시오. 
	* 가능한 작업을 병합하는 것이 좋습니다. 오랜 시간 동안 작은 chunk로 작업을 수행하는 것 보다 한번에 작업을 수행하는 것이 더 적은 전력을 소비하기 때문입니다.
	* 디스크에 너무자주 access하지 마십시오. 예를 들어 앱이 상태 정보를 디스크에 저장하는 경우, 상태에 변경이 생길때만 정보를 저장하고 가능하면 변경 사항을 통합하여 너무 빈번하게 변경 사항을 기록하지 않도록 합니다. 
	* 필요이상으로 빠르게 화면을 그리지 마십시오. 그리기(draw) 작업은 전력이 많이 소모되는 작업입니다. 
	* `UIAccelerometer` 클래스를 사용하여 가속도계 이벤트를 수신하는 경우, 필료하지 않을때는 이벤트 전달을 비활성화 하십시오. 자세한 내용은 [Event Handling Guide for UIKit Apps](https://developer.apple.com/library/content/documentation/EventHandling/Conceptual/EventHandlingiPhoneOS/index.html#//apple_ref/doc/uid/TP40009541)를 참고하십시오.
	* 네트워크로 전송하는 데이터가 많을 수록 많은 전력을 소모하게 되며, 네트워트를 사용하는 작업은 전력 소모에 가장 영향을 많이 기치는 작업입니다. 다음 지침을 따르면 네트워크에 소모되는 시간을 최소화 할 수 있습니다.
		* 필요한 경우에만 네트워크 서버에 연결하고 해당 서버를 polling하지 마십시오. 
		* 네트워트에 연결해 작업을 할때는 압축된 데이터 형식을 사용하여 최소량의 데이터를 전송하십시오. 
		* `NSURLSession` 클래스를 사용하여 여러개의 파일을 업로드하거나 다운로드 할때, 다음 작업을 하기 위해 이전 작업이 완료되기를 기다리지 말고 queue에 추가하십시오. 시스템이 자동적으로 가장 효율적으로 작업을 실행합니다.
		* 가능하다면 Wi-Fi를 사용해 네트워크에 연결하십시오. Wi-Fi는 셀룰러보다 전력 소모가 적습니다.
		* `Core Location` 클래스를 이용해 위치 데이터를 수집하는 경우, 최대한 빨리 위치 업데이트를 비활성화 하고 distance filter와 accuracy level을 적절한 값을 설정하십시오. 관련된 자세한 내용은 [Location and Maps Programming Guide](https://developer.apple.com/library/content/documentation/UserExperience/Conceptual/LocationAwarenessPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40009497)를 참고하십시오.
* Instruments는 전력 관련 정보 수집을 위한 여러가지 계측기가 포함되어 있습니다. 이 계측기를 사용하여 전력 소비에 대한 여러가지 정보와 여러 하드웨어(Wi-Fi, Blutooth, GPS, 디스플레이, CPU 등)에 대한 측정 값을 수집 할 수 있습니다. 

---

### Use Memory Efficiently
> 앱은 가능한 적은 메모리를 사용하여 시스템이 많은 메모리를 확보할 수 있도록 해야합니다. 시스템의 사용가능한 여유 메모리와 앱의 성능에는 직접적인 상관 관계가 있습니다. 사용 가능한 메모리가 적으면 이후 메모리 요청에 따른 작업을 수행할때 문제을 일으킬 가능성이 높습니다.

#### Obseve Low-Memory Warnings
>  시스템에서 앱에 메모리 부족 경고를 보내면 즉시 응답하십시오. 메모리 부족 경고는 필요없는 객체에 대한 참조를 제거할 수 있는 기회입니다. 이 경고에 대해 응답하다 실패하는 경우 앱이 종료 될 수 있기 때문에 주의해야합니다. 시스템은 아래 API를 사용해 메모리 경고를 앱에 전달합니다.

 * `applicationDidReceiveMemoryWarning:` : app delegate 메소드
* `didReceiveMemoryWarning`: UIViewController 메소드
* `UIApplicationDidReceiveMemoryWarningNotification`: 노티피케이션
 
* 이러한 경고를 받으면 불필요한 메모리를 즉시 해제해야합니다. 사용용하지 않는 거대 데이터 구조가 있는 경우, 해당 구조를 디스크에 저장하고 메모리에서 사본을 제거하십시오.

* 참고: iOS 시뮬레이터의 `Simulate Memory Warning` 명령을 사용하여 메모리 부족 상태의 앱 동작을 테스트 할 수 있습니다.

#### Reduce Your App's Memory Footprint
> 초기에 낮은 설치 공간으로 시작해 추후에 앱을 확장 할 수있습니다. 초기 설치 공간을 줄이는 방식에는 다음과 같은 방법이 있습니다.

* **메모리 릭(leak) 제거** Instruments를 사용하여 실제 디바이스와 시뮬레이터에서 메모리 릭을 추적하십시오.
* **리소스 파일 줄이기** 리소스 파일을 가능한 작게 만들기 위해 모든 이미지 파일을 압축하십시오. (iOS 앱 기본 이미지 포맷인 PNG 이미지를 압축하려면 pngcrush 도구를 사용하십시오.)
* **속성 목록 파일 줄이기** NSPropertyListSerialization` 클래스를 사용하여 속성 목록을 이진법으로 작성할 수 있습니다.
* **대규모 데이터 관리** 대규모 데이터를 관리하는 경우 `Core Data` 혹은 `SQLite`를 사용하십시오. 이는 전체 세트를 한번에 메모리에 저장하지 않고 대용량 데이터를 효율적으로 관리 할수 있는 방법을 제공합니다.
* **느린 리소스 로드** 실제로 필요할 때까지 리소스 파일을 로드하지 마십시오. 리소스 파일을 미리 가져오는 것은 시간을 절약 시키는 방법처럼 보일 수 있지만, 결국은 앱의 속도를 저하시키게 됩니다. 

#### Allocate Memory Wisely
> 다음은 현명하게 메모리를 할당할 수 있는 팁입니다.

* **리소스에 크기 제한을 두십시오.** 고해상도 이미지를 사용하는 대신 iOS 기기에 적합한 크기의 이미지를 사용하십시오. 대용량 리소스 파일을 사용해야하는 경우에는 파일의 일부분만 로드하는 방법을 사용하세요.(`mmap`, `munmap`) 
* **무제한 문제를 다루지 않도록 하십시오.** 제한이 없는 문제는 많은 양의 데이터 계산이 필요할 수도 있습니다. 앱에서 사용 가능한 메모리보다 문제 해결에 더 많은 메모리가 필요 한 경우 계산을 완료하지 못할 수 있습니다. 

---

### Tune Your Networking Code
> iOS의 네트워킹 스택에는 iOS 기기의 무선 통신을 위한 몇가지 인터페이스가 포함되어있습니다. 주요 인터페이스는 `CFNetwork` 프레임 워크이며, `Core Entity`프레임 워크와 `BSD` 소켓을 기반으로 되어있습니다. 
> 네트워크 통신을 위해 `CFNetwork` 프레임 워크를 사용하는 방법에 대한 정보는 [ CFNetwork Programming Guide](https://developer.apple.com/library/content/documentation/Networking/Conceptual/CFNetwork/Introduction/Introduction.html#//apple_ref/doc/uid/TP30001132)와 [CFNetwork Framework Reference](https://developer.apple.com/reference/cfnetwork)를 참고하십시오.

#### Tips for Efficient Networking Using Wi-Fi
> 네트워크를 통해 데이터를 송수신하는 작업은 기기에서 가장 전력을 많이 소모하는 작업 중 하나입니다. 데이터 송수신에 소요되는 시간을 최소화하여 배터리 수명을 향상시키기 위해 아래 팁을 고려하십시오.

* 데이터 형식을 가능한 컴팩트하게 정의하십시오.
* 수다스러운 프로토콜 사용을 피하십시오.
* 셀룰러 및 Wi-Fi는 활동이 없을때 자동적으로 꺼지도록 설계되어있지만, 앱이 주기적으로 작은 데이터를 전송하는 경우에는 Wi-Fi가 활동하지 않을때도 계속 켜져 전력을 소비할 수 있습니다. 소량의 데이터를 자주 송신하기 보다 대량의 데이터를 긴 간격으로 전송하는 것이 좋습니다.
* 네트워크 동신을 할때 패킷은 언제든지 손실 될 수 있습니다. 따라서 네트워킹 코드를 작성할때 장애 처리와 관련해서는 강력하게 대응하는 것이 좋습니다. 

#### Using Wi-Fi
> 앱이 Wi-Fi를 사용하여 네트워크에 액세스 하는 경우 앱의 Info.plist 파일에 `UIRequiresPersistentWiFi` 키를 포함시켜 시스템에 알려줘야합니다. 이 키가 포함되어 있으면 앱이 실행되는 동안 Wi-Fi 하드웨어를 종료하지 말아야 한다는 것을 의미합니다. 

#### The Airplane Mode Alert
> 기기가 비행기 모드에 있는 동안 앱이 시작되면, 시스템에서 사용자에게 비행모드임을 알리기 위해 alert를 표시할 수 있습니다. 시스템은 다음 조건이 모두 충족되면 alert를 표시합니다.
 
	* Info.plist 파일에 `UIRequiresPersistentWiFi` 키가 포함되어있고, 해당 키의 값이 `true`일때
	* 기기가 비행기 모드에 있는 동안 앱이 실행됨
	* 비행기 모드로 전환 한 후, 장치의 Wi-Fi가 수동으로 활성화 됨

---

### Imporve Your File Management
> 파일 관리를 위해 디스크에 쓰는 데이터 양을 최소화 하는 것이 좋습니다. 파일 관련 작업을 최소하하는 데 도움이 되는 몇가지 팁은 다음과 같습니다.

* 파일 전체를 저장하는 것이 아니라, 변경된 파일의 일부분만 저장할 수 있도록 하십시오.
* 데이터가 randomly 접근가능하고, 양이 이 몇 MB이상인 경우 `Core Data` 혹은 `SQLite`와 같은 영구 저장소에 저장하십시오.
* 캐시파일을 디스크에 쓰지 않도록 하십시오. 유일한 예외 상황은 앱을 종료했을때와 동일한 상태로 되돌릴 수 있도록 상태 정보를 저장해야 할때 입니다.

---

### Make App Backups More Efficient
>  백업은 사용자가 iTunes와 기기를 동기화 할때나 iCloud를 통해 무선으로 실행되고, 백업이 진행되는 동안 기기에서 사용자의 컴퓨터 또는 iCloud 계정으로 파일이 전송됩니다. 앱 샌드박의 파일 위치에 따라 해당 파일을 백업/복원할지 여부가 결정되는데, 앱에서 주기적으로 많은 양의 데이터를 만들고 변경하는 경우에는 백업 속도가 느려질 수 있습니다. 파일 관리 코드를 작성할때는 이 사실을 꼭 염두해 두어야합니다.

#### App Backup Best Practices
> 백업/복원 작업을 위해 앱에서 준비해야 하는 것은 없습니다. iClound나 iTunes에 연결되면 앱 데이터 파일의 변경된 부분을 백업합니다. 하지만 아래 디렉토리의 내용은 백업하지 않습니다.

	 \<Application_Home>/AppName.app
	 \<Application_Data>/Library/Caches
	 \<Application_Data>/tmp
	 
**동기화 프로세스에 걸리는 시간과 앱이 차지하는 용량을 줄이기 위해 다음 가이드 라인에 따라 앱 데이터를 저장하는 것이 좋습니다.**

* 중요한 데이터는 <Application_Data>/Documents 에 저장해야합니다. 중요데이터란 사용자 문서 및 기타 사용자가 생성한 콘텐츠와 같이 앱에서 다시 생성 할 수 없는 데이터입니다.
* Support file에는 앱에서 다운로드하거나 재생성가능한 파일이 포함됩니다. 
* 캐시된 데이터는 <Application_Data>/Library/Caches 디렉토리에 저장되어야 합니다. 캐시 디렉토리에 저장해야하는 파일의 예로는 데이터베이스 캐시 파일, 잡지, 신문, 지도 앱 등에서 다운로드 받은 파일입니다.
* 임시 데이터는  <Application_Data>/tmp 디렉토리에 저장해야합니다. 임시 데이터는 오랜 시간 동안 유지할 필요가 없는 데이터로 구성됩니다. 기기에서 사용된 후에는 공간을 차지하지 않도록 파일을 삭제해야합니다.
* 응용 프로그램에서 디렉토리를 사용하는 자세한 방법은 [File Systeom Programming Guide](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/FileSystemProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40010672)를 참고하세요.

#### Files Saved During App Updates
> 사용자가 앱 업데이트를 다운로드하면 iTunes는 새로운 앱 디렉토리에  업데이트를 설치합니다. 그런 다음 이전 앱을 삭제하기 전에 사용자 데이터 파일을 새 앱의 디렉토리로 이동시킵니다. 아래 디렉토리의 파일은 업데이트 프로세스에서 보존됩니다.
 
	\<Application_Data>/Documents
	\<Application_Data>/Library

**업데이트 후에, 다른 디렉토리의 파일도 옮겨질 수 있지만 파일이 존재한다고 믿어서는 안됩니다.**

---

### Move Work off the Main Thread
> 앱의 main thread에서 수행되는 작업의 유형을 제한해야합니다. main thread는 앱의 터치 이벤트 및 기타 사용자 입력을 처리하는 곳입니다. 앱이 항상 사용자에게 응답할 수 있도록 하려면 네트워크에 액세스하는 작업이나 장시간이 걸리는 작업을 main thread에서 실행 해서는 안됩니다. 대신 백그라운드 스레드에서 옮겨서 작업해야합니다. 이를 수행하는 가장 좋은 방법은 GCD(Grand Center Dispatch) 또는 NSOperation 객체를 사용하여 작업을 비동기(async)로 수행하는 것입니다.

* 작업을 백그라운드에서 실행시키면 main thread가 자유롭게 사용자 입력을 처리할 수 있게되며, 이는 앱이 시작/종료될때 특히 중요합니다.
	* 앱이 실행(launch)될때 main thread가 차단 된 경우 시스템은 실행이 완료되기 전에 앱을 종료(kill)시킬 수 있습니다.
	* 앱이 종료될때 main thread가 차단되어있으면 사용자의 중요한 데이터를 기록하기 전에 시스템이 앱을 종료(kill)할 수 있습니다.
	* CGD, operation object, thread에 관한 자세한 정보는 [Concurrency Programming Guide](https://developer.apple.com/library/content/documentation/General/Conceptual/ConcurrencyProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40008091)를 참고하세요.

---
---

* 원문에서 중복되는 내용이나 예제 코드는 생략했습니다. 정확한 내용은 [원문](https://developer.apple.com/library/content/documentation/iPhone/Conceptual/iPhoneOSProgrammingGuide/Introduction/Introduction.html)을 확인해주세요.
* **번역 : 전미정(ninevinentg@gmail.com)**

[**위로가기**](#위로가기)