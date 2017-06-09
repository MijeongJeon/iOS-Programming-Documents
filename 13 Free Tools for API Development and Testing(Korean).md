<a name = "위"></a>

# API 디자인, 개발 그리고 테스트를 위한 13가지 무료 툴 📡
#### 작성자 : 전미정

> 본 문서에서는 API의 다양한 기능(개발, 테스트, 디자인, 관리 등)을 무료로, 손쉽게 다룰 수있는 여러가지 서비스를 안내합니다. **목적에 부합하는 툴을 이용해 API 개발 및 관리에 익숙해지고, 실제 개발에 적용해 봅시다 :)**

### 목차
[1. Amazon AWS Free Tier](#1)   
[2. IBM Bluemix API Management](#2)  
[3. Runscope](#3)  
[4. Restlet Studio](#4)  
[5. APImetrics](#5)  
[6. JsonStub](#6)  
[7. Mockable.io](#7)  
[8. Httpbin.org](#8)  
[9. Loader.io](#9)  
[10. BlazeMeter](#10)  
[11. Sandbox](#11)  
[12. Mocky.io](#12)  
[13. JOSN Server](#13)

<a name = "1"></a>
## 1. Amazon AWS Free Tier and Amazon API Gateway

### [Free Tier for AWS](http://www.infoworld.com/article/3046613/cloud-computing/make-the-most-of-free-amazon-web-services.html)
* 무료 요금제로 AWS에서 제공하는 거의 모든 기능들을 사용해 볼 수 있습니다. 
* 물론, 사용량에 제한이 있으므로 단순히 작업이 어떻게 이뤄지는지 파악하는 용도로 사용하는 것이 좋습니다.

### [Amazon API Gateway](http://aws.amazon.com/api-gateway/)
* Amazon API Gateway는 무료 요금제(free tier)와 함께 제공되는 API 관리 서비스입니다.

* 매달 1백만 건의 API 호출을 제공하므로,  Amazon에서 제공하는 API 관련 기능 및 방식이 구현하려는 목적에 부합하는지 확인하는 용도로 사용하면 좋습니다.


<a name = "2"></a>
## 2. IBM Bluemix API Management

* AWS와 마찬가지로, [IBM Bluemix](https://console.ng.bluemix.net/catalog/services/api-connect/) 또한 무료 요금제를 제공하고 있습니다.
* Bluemix는 개발자들이 원하는 다양한 기능을 제공하고 있지만, 무료 요금제로 실제 제품을 구축 하기는 힘듭니다.
* AWS와 마찬가지로, IBM은 API 관리 도구를 제공합니다.(API Connect, API Management, Conncect & Compose)
* 한달에 수만건의 API 호출이 가능하기 때문에 기능을 테스트해보는데 부족함이 없습니다.

<a name = "3"></a>
## 3. Runscope
* [Runscope](https://www.runscope.com/pricing-and-plans)는 웹 기반의 API 테스트 툴을 제공합니다.
* 작성된 API가 제대로 작동하는지, 유효한 값을 반환하는지 등을 디버깅 할 수 있습니다.
* 무료 요금제로 API 테스트, 가동시간 모니터링, 그리고 트래픽 디버깅 기능을 사용할 수 있으며, 한달에 최대 2.5만건  호출이 가능합니다.

<a name = "4"></a>
## 4. Restlet Studio
* [Restlet Studio](https://restlet.com/)는 "API 디자인을 위한 웹 IDE"로 불리며, 시각적 도구를 이용하고있습니다.
* 메서드와 쿼리 파라미터 등의 설정 뿐만 아니라, API를 위한 Jave / Node.js 코드를 자동 생성해줍니다.
* Swagger와 RAML을 모두 지원합니다.
* 무료 요금제로는 단 하나의 API를 구출 할 수 있지만, API 테스트시 호출 횟수에 제한이 없습니다. (만약 제품에 사용된다면 1,000건으로 제한)

<a name = "5"></a>
## 5. APImetrics
* [APImetrics](http://apimetrics.io/)는 API를 모니터링하고 변경하는 서비스를 제공합니다.
* APImetrics는 시각적인 API 디자인, 대시보드를 제공하며 REST, SOAP API를 모두 지원합니다.
* 무료 요금제로는 수동 호출만 할 수 있으며, 회사의 West Coast에 위치한 서버에서만 실행 가능합니다.

<a name = "6"></a>
## 6. JsonStub
* [JsonStub](http://jsonstub.com/)의 웹 인터페이스를 통해 고정적인 텍스트(JSON)를 반환하는 API를 손쉽게 만들 수 있습니다.(mockup)
* JsonStub 홈페이지에 "당신이 프론트 엔드를 개발하는 동안 백엔드를 조작하라. (Fake the back end while you develop the front end.)" 라고 나와있듯 프론트 엔드를 개발하는 동안 테스트용으로 사용하기에 유용합니다.

<a name = "7"></a>
## 7. Mockable.io
* [Mockable](https://www.mockable.io/) 역시 빠르고 간단하게 REST와 SOAP API를 제작할 수 있는 웹 사이트입니다.
* 3개월 이내에 사용되지 않는 경로는 삭제되지만, Mockable의 기본 계정(base tier)는 영원히 무료입니다.
* 로그는 24시간 동안만 유지되거나, 5MB를 초과하지 않는 한 영구 보존됩니다.
* 가장 좋은 점은 API 제작을 위해 사이트에 계정을 등록하지 않고, 임시계정을 이용 해도 된다는 점입니다.

<a name = "8"></a>
## 8. Httpbin.org
* [Httpbin](http://httpbin.org/)은 Runscope와 유사한 기능을 제공하는데, 요청을 보내는 프론트 엔드를 테스트하거나 디버깅하는데 유용한 HTTP API endpoint response를 제공합니다.
* 웹 인터페스를 통해 구성하는게 아니라, URL 파라미터를 이용해 구성할 수 있습니다.

<a name = "9"></a>
## 9. Loader.io
* 얼마나 많은 로드에 견딜 수 있는지 테스트하는 것은 public-facing API에 매우 중요한 일입니다. 
* [Loader.io](https://loader.io/)의 웹 인터페이스나 API를 이용해 실시간으로 API 테스트 결과를 관찰 할 수있습니다.
* 무료 버전에서는 최대 10,000명의 클라이언트가 동시에 싱글 타겟을 테스트할 수 있습니다.

<a name = "10"></a>
## 10. BlazeMeter
* [BlaserMeter](https://www.blazemeter.com/)는 Loader.io와 유사한 API 로드 테스트 서비스 입니다. 
* 특징적인 기능은 지역기반의 로드 테스팅이 가능하다는 것입니다. (여러 대륙의 서버에서 생성된 트래픽 활용 가능)
* Apache JMeter로 생성된 테스트 또한 가능합니다.
* 계정에 가입하면 처음 14일 동안 Pro 버전과 동일한 기능을 사용할 수 있으며, 이후 무료 요금제에서는 각종 기능이 제한됩니다.

<a name = "11"></a>
## 11. Sandbox
* [Sandbox]{https://getsandbox.com/features}는 가볍고, 독립적인 방식으로 API를 실행 할 수 있는 시스템입니다.
* REST와 SOAP 모두 지원합니다.
* Apiary, Swagger, RAML, WSDL 그리고 Blank를 지원합니다.
* Snadbox 회사에서 제공하는 클라우드의 무료 요금제를 이용하면 5,000건 까지 호출 할 수 있으며, Sandbox의 수는 무제한입니다.

<a name = "12"></a>
## 12. Mocky.io
* [Mocky.io](http://www.mocky.io)는 Runscope에서 제공하는 웹 기반 REST API 테스트 사이트입니다.
* 로그인이나 계정 생성없이 빠르고 간단하게 HTTP response를 확인 할 수 있습니다.

<a name = "13"></a>
## 13. JSON Server
* 작성한 프론트 엔드 코드가 정상적으로 작동하는지 테스트하기 위해 서버가 준비되길 무작정 기다릴 수만은 없습니다. `npm module`의 [JSON Server](https://github.com/typicode/json-server)를 이용하면 백엔드 프로토 타입을 생성해 REST API를 테스트 해 볼 수 있습니다.
* 자세한 방법 및 설명은 아래 링크를 참고하세요.
	* [JSON-Server as a Fave REST API in Fronted Devlopment](https://scotch.io/tutorials/json-server-as-a-fake-rest-api-in-frontend-development)
	* [Mock up your REST API with JSON Server](http://www.betterpixels.co.uk/projects/2015/05/09/mock-up-your-rest-api-with-json-server/)
	* [Create a Mock REST API in Seconds for Prototyping your Frontend](https://coligo.io/create-mock-rest-api-with-json-server/)

	
> 본 문서는 [[원문] 10 Free Tools for API Design, Development and Testing](http://www.infoworld.com/article/3060731/apis/10-free-tools-for-api-design-development-and-testing.html#slide1)을 바탕으로, 관련된 내용을 추가 조사하여 구성하였습니다.

[위로가기](#위)