# 시작하기

이 문서는 iOS에서 Analytics SDK를 연동하기 위한 방법에 대해 설명합니다. Analytics SDK를 사용하기 위해서는 먼저 앱을 등록해야 합니다. 앱 등록 방법은 링크 (http://cloud.toast.com/documents/2/)를 참고하세요. 
**캠페인 연동 관련된 내용은 별도의 문서를 제공합니다. 이 문서에서는 클라이언트 구현에 대한 부분만 설명합니다. 전체적인 내용은 "캠페인 연동 가이드"를 참고하세요.**

# 프로젝트 설정

## SDK 다운로드

http://cloud.toast.com/documents/6/에서 Android SDK 파일을 다운로드 합니다.

## 프로젝트설정

### 라이브러리 dependency 설정
다운로드 받은TAGAnalytics.h 파일을 프로젝트에 포함시키고, libTAGAnalytics.a 파일을 "Linked Frameworks and Libraries"에 추가합니다. "AdSuppert.framework", "libsqlite3.dylib", "CoreTelephony.framework"도 추가합니다.
![라이브러리링크](https://github.com/ToastAnalytics/ToastAnalytics/raw/master/Documents/Developer/iOS%20Developer%20Guide/images/pg_ios_001.png)

# 필수연동
기본적인 통계 데이터 수집을 위해서 필수로 연동해야 하는 API입니다. 필수 연동 항목만 구현해도 대부분의 데이터를 볼 수 있습니다.

필수 연동 항목은 아래와 같습니다.
* 초기화 : initializeSDK
* 세션추적 : traceActivation, traceDeactivation
* 구매 (In App Purchase) : tracePurchase
* 재화 획득/사용 : traceMoneyAcquisition, traceMoneyConsumption
* 레벨업 : traceLevelUp
* 친구수 : traceFriendCount

## 초기화

SDK를 사용하기 위해서는 앱 등록 후 발행되는 "앱 인증Key"와 "컴퍼니 아이디"가 필요합니다. 앱 등록 방법은 링크(http://cloud.toast.com/documents/2/)를 참고하세요.
![AppKey](https://github.com/ToastAnalytics/ToastAnalytics/raw/master/Documents/Developer/iOS%20Developer%20Guide/images/pg_ios_002.png)

GameAnalytics SDK를 사용하기 위해서는 SDK 초기화를 먼저 수행해야 합니다.<br>
GameAnalytics 클래스의 initializeSDK 함수는 SDK 초기화를 수행하는 함수입니다. 이 함수는 내부적으로 필요한 데이터(디바이스 정보, 앱 설정 정보)를 확인하고, 로그 전송을 위한 환경을 설정하는 작업을 수행합니다.
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  ……
  [TAGAnalytics initializeSdk:@"AppKey"
			        companyId:@"CompanyID"
			       appVersion:@"AppVersion"
		     useLoggingUserId:NO];
  ……
}
```

### 사용자 구분 기준 설정
**운영중에 사용자 구분 기준을 변경하면 변경 전/후 데이터의 연관 관계가 끊어지기 때문에 게임 오픈 이후에는 기준을 바꾸지 않아야 합니다.**<br>
Analytics는 사용자를 구분하는 기준으로 Advertise ID 또는 User ID를 사용합니다. 두가지를 모두 사용할 수는 없고, 게임 정책에 따라 한가지를 선택하여야 합니다.<br>
일반적으로 Advertise ID를 기준으로 사용합니다. 하지만 게임에서 특별한 요구사항이 있는 경우 User ID를 기준으로도 사용할 수 있습니다.<br>
예를들어 Advertise ID를 사용하는 경우 하나의 디바이스에서 탈퇴->재가입 하는 경우에도 기존과 동일한 사용자로 집계됩니다. 반면 User ID를 사용하는 경우에는 신규 사용자로 집계됩니다.<br> 
또는 한명의 사용자가 두개의 디바이스를 사용하는 경우 Advertise ID를 사용하면 각각 다른 사용자로 집계되는 반면 User ID를 사용하는 경우 한명의 사용자로 집계됩니다.<br>
이런 부분을 고려하여 게임에서 기준을 결정하여 사용합니다.<br>
초기화 함수(initializeSDK)의 마지막 인자(use logging userid flag)로 이 값을 설정할 수 있습니다. Flag가 true 인 경우 User ID를 사용자 구분 기준으로 사용합니다. False로 설정하면 Advertise ID를 구분 기준으로 사용합니다.<br>
아래 코드는 User ID를 사용자 구분 기준으로 사용하는 경우입니다.<br>
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  ……
  int result = [TAGAnalytics initializeSdk:@"AppKey"
		            			 companyId:@"CompanyID"
					            appVersion:@"AppVersion"
				          useLoggingUserId:YES];
  if (result != 0) {
	 // Initialize Fail
  }

  // 게임에서 로그인 처리 완료
  ……
  // User ID를 사용자 구분 기준으로 사용하는 경우 User ID를 등록하는 함수.
  [TAGAnalytics setLoggingUserId:@"user id"];
……
}
```

만약 초기화 함수의 마지막 인자(use logging userid flag)를 true로 설정한 경우 setLoggingUserId를 호출하여 User ID를 등록해야 합니다. Flag를 true로 설정하고 setLoggingUserId를 호출하지 않으면 이후에 호출하는 모든 API가 실패(E_LOGGING_USER_ID_EMPTY)를 Return 합니다.<br>
setUserId에서 "useCampaignOrPromotion"은 Promotion이나 Campaign을 사용하는 경우 true입니다. 그렇지 않은 경에는 false 입니다.<br>
setUserId 함수는 initializeSDK 호출 이후에 게임에서 로그인 성공한 후 게임에서 사용하는 userID를 획득한 직후에 호출하면 됩니다. userID는 게임에서 사용자를 구분하기 위해 사용하는 값을 사용하면 됩니다.<br>
Advertise ID 관련 내용은 아래 링크를 참고하세요.<br> 
https://developer.apple.com/LIBRARY/ios/documentation/AdSupport/Reference/ASIdentifierManager_Ref/index.html

## 세션 추적

DAU(Daily Active User)와 게임 체류 시간을 추적하기 위한 연동입니다.<br> 
App 시작/종료, Background/Foreground 이동시 해당 액션에 맞는 API를 호출하여 측정할 수 있습니다.<br>
App이 처음 실행될때(initializeSDK 이후) 또는 background에서 foreground로 이동시 traceActivation을 호출하여 세션 추적을 시작합니다. 이후 App이 background로 들어가는 시점에 traceDeactivation을 호출하여 세션 추적을 멈춥니다.<br>
traceDeactivation을 호출하면 traceActivation과 traceDeactivation 사이의 시간을 계산하여 게임 이용 시간을 측정합니다. 또한 SDK 내부적으로 수행하던 작업도 traceDeactivation에서 중단합니다.<br>
Background/Foreground 이동시 위 함수를 호출하지 않으면 정확한 게임 이용 시간을 측정할 수 없기 때문에 이 API는 반드시 호출해야 합니다.<br>
DAU는 하루동안 traceActivation을 호출한 사용자(Advertise ID 또는 User ID 기준)의 중복을 제거한 숫자로 계산합니다.
```
- (void)applicationWillResignActive:(UIApplication *)application
{
	[TAGAnalytics traceDeactivation];
}

- (void)applicationDidBecomeActive:(UIApplication *)application
{
	[TAGAnalytics traceActivation];
}
```
## 액션 추적

In-App Purchase, 머니 획득/사용, 레벨업, 친구 수 변경등 사용자의 Action에 대해 추적할 수 있습니다.

### 1. In-App Purchase
In-App Purchase가 발생한 후 tracePurchase를 호출하여 매출 정보를 전송합니다.<br>
Currency는 ISO-4217(http://en.wikipedia.org/wiki/ISO_4217)에서 정의한 코드를 사용합니다.<br>
$0.99의 보석을 구매하는 경우 아래와 같이 사용합니다.<br>
(여기에서 "GEM_10"은 게임에서 정의한 Item의 Code입니다. Unit Cost는 해당 아이템의 단위 가격, Payment는 실제 사용자가 사용한 금액입니다. Level은 구매한 사용자의 Level을 입력합니다.)
```
[TAGAnalytics tracePurchase:@"GEM_10"
                    payment:0.99
                   unitCost:0.99
                   currency:@"USD"
                      level:10];
```
### 2. 재화 획득/사용
게임내에서 재화의 획득/사용시 호출합니다. 1차 재화, 2차 재화의 변동량을 추적합니다. 일반적으로 1차 재화는 In-App Purchase를 통해서 구매하는 재화(ex. 보석, 루비등) 입니다. 2차 재화는 1차 재화를 이용하여 구매하는 재화(ex. 체리, 하트등) 입니다.<br>
IAP를 통해서 보석 10개를 구매한 경우 아래와 같이 사용합니다.<br>
("CODE_IAP"는 게임에서 정의한 Code입니다. 1차 재화인 경우 Type은 0, 2차 재화인 경우 1을 사용합니다)
```
[TAGAnalytics traceMoneyConsumption:@"CODE_IAP"
	                           type:@"0"
                  consumptionAmount:10
	                          level:10];
```

보석 10개를 이용하여 체리 100개를 구매한 경우는 아래와 같이 사용합니다.
```
// 1차 재화 사용
[TAGAnalytics traceMoneyConsumption:@"CODE_USE_GEM"
				               type:@"0"
		          consumptionAmount:10
				              level:10];

// 2차 재화 획득
[TAGAnalytics traceMoneyAcquisition:@"CODE_BUY_CHERRY"
				               type:@"0"
		          consumptionAmount:10
				              level:10];
```

1차 재화를 사용하여 2차 재화를 구입한 경우 실제 ‘1차 재화 감소’->‘2차 재화 증가’가 발생합니다. 하지만 2차 재화를 구입하기 위해서 1차 재화를 사용하는 경우 별도의 재화 소모로 판정하지 않고 싶은 경우 ‘2차 재화 획득’ 로그만 전송하여도 됩니다.

### 3. 레벨업
사용자 레벨이 변경되는 경우 traceLevelUp을 호출합니다. 참고로 대부분의 액션 추적 API는 레벨별 액션 추적을 위해서 사용자 Level을 같이 받습니다.<br>
사용자 레벨이 10으로 변경되는 경우 아래와 같이 호출합니다. 한 사용자의 레벨은 반드시 증가해야만 합니다. 감소하는 경우 정확한 데이터 측정이 불가능합니다.<br>
예를들어 "Candy Crush Sage"와 같이 스테이지로 진행 되는 게임에서 스테이지를 레벨로 사용하는 경우에는 해당 스테이지에 최초 진입할때만 레벨업 로그를 남겨야 합니다. 만약 이전 스테이지로 다시 돌아가서 플레이 하는 경우에는 레벨업 로그를 남기지 않습니다.<br>
또한 다른 API에 전달되는 level 값도 현재 진행중인 스테이지가 아닌 사용자의 최고 스테이지를 레벨 값으로 사용해야 합니다.
```
[TAGAnalytics traceLevelUp:10];
```

### 4. 친구
사용자의 친구 숫자를 등록합니다. 일반적으로 앱 실행 후 친구 정보 로딩이 완료된 시점에 호출하면 됩니다.
```
[TAGAnalytics traceFriendCount:100]; 
```

# 선택적 연동

기본연동을 통해 제공되는 기본 지표 이외에 게임에서 추가적으로 측정하고 싶은 지표가 있는경우, 혹은 Analytics에서 제공하는 Campaign 기능을 사용하는 방법에 대해 설명합니다.

추가적으로 연동할 수 있는 항목은 아래와 같습니다.
* Campaign 연동 : setCampaignListener, (show/hide)Campaign
* Custom Event 수집 : traceEvent
* 소요시간 측정 : traceStartSpeed, traceEndSpeed

## 캠페인 연동

### 1. 캠페인 연동 사전 준비
캠페인 연동 및 실행을 위해서는 별도의 가이드를 제공하고 있습니다.<br>
Toast Analytics의 "캠페인 실행" 메뉴의 "페이지 가이드"를 참고하세요. 
(http://analytics.toast.com/promotion/share/document/4.2_Campaign_run.pdf)

### 2. 푸시 연동
Analytics SDK에서 캠페인을 위해서 APNS를 사용합니다. APNS를 사용하기 위해서는 Analytics 관리자 페이지에 인증서를 등록해야 합니다.<br>
Analytics 관리자 페이지에 정보를 등록하는 방법은 "Getting Started" 문서에서 "푸시 설정" 항목을 참고하세요.
```
- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
{
  ……
  // SDK 초기화
  [TAGAnalytics initializeSdk:@"AppKey"
      	            companyId:@"CompanyID"
			       appVersion:@"AppVer"
		     useLoggingUserId:NO];
  ……

  // 푸시를 받은 경우 푸시 데이터를 SDK에 전달
  if (launchOptions != nil)
  {
	NSDictionary* dictionary = [launchOptions objectForKey:UIApplicationLaunchOptionsRemoteNotificationKey];
	if (dictionary != nil)
	  [TAGAnalytics setPushData:dictionary];
  }

  // 푸시 토큰을 APNS에 요청한다.
  if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 8.0)
	{
	[[UIApplication sharedApplication] registerUserNotificationSettings:[UIUserNotificationSettings 
	  settingsForTypes:(UIUserNotificationTypeSound | UIUserNotificationTypeAlert |
	UIUserNotificationTypeBadge) 
	categories:nil]];
	[[UIApplication sharedApplication] registerForRemoteNotifications];
  }
  else
  {
	[[UIApplication sharedApplication] registerForRemoteNotificationTypes:
	(UIUserNotificationTypeBadge | UIUserNotificationTypeSound | 
	UIUserNotificationTypeAlert)];
  }
  ……
}

- (void)application:(UIApplication *)application didRegisterForRemoteNotificationsWithDeviceToken:(NSData *) deviceToken
{
  // 푸시 토큰이 정상적으로 등록된 경우 토큰을 SDK에 전달한다.
  [TAGAnalytics tracePushToken:deviceToken];
}

- (void)application:(UIApplication *)application didReceiveRemoteNotification:(NSDictionary *)userInfo
{
  // 푸시를 받은 경우 푸시 데이터를 SDK에 전달
  [TAGAnalytics setPushData:userInfo];
}

```

### 3. 캠페인 Listener 구현 및 등록
SDK는 일정한 주기로 캠페인 서버와 통신하여 캠페인 및 보상정보를 가져옵니다. 만약에 현재 사용자에게 진행할 캠페인이 있거나 사용자가 받을 보상 정보가 있는 경우 TAGCampaignDelegate를 통해서 알려줍니다.<br>
따라서 캠페인 정보를 받기 위해서는 TAGCampaignDelegate를 구현해야 합니다.
```
@protocol TAGCampaignDelegate <NSObject>
-(void)analyticsDidMissionComplete:(NSArray*)missionList;
-(void)analyticsDidCampaignVisibilityChange:(NSString*)adspaceName show:(BOOL)show;
-(void)analyticsDidCampaignLoadSuccess:(NSString*)adspaceName;
-(void)analyticsDidCampaignLoadFail:(NSString*)adspaceName errorCode:(int)errorCode 
  errorMessage:(NSString*)errorMessage;
-(void)analyticsDidPromotionVisibilityChanged:(BOOL)show;
@end
```

각 Callback은 아래와 같은 경우 호출됩니다
* analyticsDidCampaignVisibilityChange: showCampaign, hideCampaign을 호출하여 캠페인 관련 팝업이나 배너가 보이거나 사라질때 호출됩니다.
* analyticsDidMissionComplete: 사용자가 캠페인/프로모션을 진행하고 정해진 미션을 달성하여 보상 정보가 있을때 호출됩니다. 여기에서 받은 정보를 이용하여 게임서버를 통해서 보상을 지급해야 합니다. 캠페인 보상 관련 프로세스는 "캠페인 적용 가이드"문서를 참고하세요
* analyticsDidCampaignLoadSuccess, analyticsDidCampaignLoadFail: 서버에서 가져 온 캠페인 정보 파싱 결과를 알려줍니다. 게임에서는 이 Callback에서 특별한 처리를 할 필요는 없습니다. 로그 확인을 위해 제공하는 Callback입니다. 
구현한TAGCampaignDelegate는 setCampaignDelegate를 이용하여 등록합니다.
* analyticsDidPromotionVisibilityChange: iOS에서는 현재 사용하지 않습니다.
```
- (void)viewDidLoad {
  ……
  [TAGAnalytics setCampaignDelegate:self];
  ……
}

……

-(void)analyticsDidMissionComplete:(NSArray *)missionList
{
  // missionList Array는 NSString으로 mission string입니다.
  // mission string은 key/value 정보가 구분자 ‘|’으로 제공됩니다.
  // 이 값을 가지고 게임서버를 통해 promotion server에서 검증하여 사용자에게
  // 보상을 지급합니다.
}

- (void)analyticsDidCampaignVisibilityChange:(NSString *)adspaceName show:(BOOL)show
{
  // 배너,팝업 Visibility Change
}

- (void)analyticsDidCampaignLoadSuccess:(NSString *)adspaceName
{
  // for Debugging
}

- (void)analyticsDidCampaignLoadFail:(NSString *)adspaceName errorCode:(int)errorCode errorMessage:(NSString *)errorMessage
{
  // for Debugging
}
```

### 4. 캠페인 Show/Hide
현재 사용자에게 진행 중인 캠페인이 있는 경우 Analytics 웹사이트에서 등록한 캠페인 팝업/배너를 보여주고, 노출된 팝업/배너를 숨기는 메소드입니다. 파라미터인 adspaceName은 Analytics 웹사이트에서 캠페인 등록시에 정의한 adspace 이름을 사용하면 됩니다. Adspace란 팝업/배너가 나타나는 게임 내의 특정 위치를 의미합니다.<br>
showCampaign() 메소드는 해당 adspace를 사용하는 캠페인이 없으면 아무런 동작도 하지 않으므로, 캠페인 팝업/배너를 노출할 것으로 예상되는 게임 내의 여러 지점에 각각 다른 adspaceName으로 함수를 호출해두면 이후에 별도의 게임 클라이언트 수정없이도 게임 운영자가 Analytics 웹사이트에서 캠페인을 등록하는 작업만으로 쉽게 팝업/배너를 노출할 수 있게 됩니다. 
Adspace 등록 방법은 "캠페인 테스트 가이드"를 참고하세요.
```
+(int)showCampaign:(NSString*)adspaceName parent:(UIView*)parent;
+(int)showCampaign:(NSString*)adspaceName parent:(UIView*)parent animation:(int)animation 
  lifeTime:(int)lifeTime;
+(int)hideCampaign:(NSString*)adspaceName;
+(int)hideCampaign:(NSString*)adspaceName animation:(int)animation;
```
## 커스텀 이벤트 사용

게임별로 특정 이벤트를 정의하여 분석하고 싶은 경우 사용합니다.<br>
예를들어 Fever Time Item을 사용하는 경우 아래와 같이 사용합니다. 사용된 모든 코드는 게임에서 정의하여 사용합니다. 아래 예제는 특정 스테이지에서 아이템 변동 내용을 추적하기 위해 정의한 코드입니다.
```
[TAGAnalytics traceEvent:@"ITEM"
               eventCode:@"ITEM_USE"
                  param1:@"FEVER"
                  param2:@"STAGE_10"
                   value:1
                   level:10];
```

특정 레벨에서 보스 배틀 결과를 추적할때도 사용할 수 있습니다.
```
[TAGAnalytics traceEvent:@"STAGE"
               eventCode:@"STAGE_BOSS_VICTORY"
                  param1:@"DRAGON_VALLEY"
                  param2:@"BOSS_MOB"
                   value:1
                   level:10];
```

이 외에도 다양한 용도로 게임에 특화된 이벤트 추적에 사용할 수 있습니다.<br>
traceEvent에 사용하는 String Type 파라미터(event type, event code, param1, param2)는 각각 50byte까지 사용할 수 있습니다. 그리고 event 하위에 발생 가능한 param1 은 300개까지, 또 param1 하위에 발생 가능한 param2는 200개까지 사용할 수 있습니다.<br> 
자세한 내용은 Toast Analytics 사이트에서 가이드를 참고하세요. (커스텀 이벤트 페이지 우측 상단의 "페이지 가이드"를 클릭하면 다운받을 수 있습니다)

## 소요시간 측정

특정 구간에 소요되는 시간을 측정할 수 있습니다. 예를들어 튜토리얼에 소요되는 시간을 측정하고 싶은경우, Scene 전환에 걸리는 시간을 측정하고 싶은경우등 시간 측정이 필요한 임의의 구간에 사용할 수 있습니다.<br>
Intro Scene 로딩 시간을 측정하고 싶은 경우 아래와 같이 사용합니다. "INTRO_LOADING"은 특정 구간에 대해서 게임에서 정의하는 값입니다.
```
- (void) onStart() 
{
  [TAGAnalytics traceStartSpeed:@"INTRO_LOADING"];
}

- (void) onLoadCompleted() {
  [TAGAnalytics traceEndSpeed:@"INTRO_LOADING"];
}
```

# SDK 설정

### 1. 디버그 모드 활성화
개발중에 SDK 로그 확인을 위해서 로그 출력 여부를 설정할 수 있습니다.<br>
이 함수는 initializeSDK 이전/이후 모두 호출 가능합니다. 기본 값은 setDebugMode(false)입니다. 
Log tags는 "Analytics:"로 시작합니다. Eclipse에서 log cat filter를 "Analytics"로 지정하면 SDK에서 발생하는 로그를 확인할 수 있습니다.)
```
- (void) Start () 
{
  ……
 [TAGAnalytics setDebugEnabled:YES];
  ……
}
```

디버그 모드가 활성화된 경우 로그 전송 내용을 확인할 수 있습니다. 로그를 전송하고 그에 대한 응답 로그를 확인하여 로그가 정상적으로 전송되었는지 확인할 수 있습니다. 아래와 같은 로그 스트링이 있으면 수집된 데이터가 정상적으로 서버로 전송된 것입니다. (***은 상황에 따라서 다른 값입니다)
```
iOS : RequestWorkerThread::didReceiveResponse - <NSHTTPURLResponse: ***> { URL: *** } { status code: 200,
```

### 2. 디바이스 정보 확인
SDK에서 수집하는 Device 정보를 확인할 수 있습니다.<br>
현재 확인 가능한 값은 Device ID, Push Token, Campaign User ID입니다. 이 값들은 캠페인 연동 테스트시 필요합니다. 자세한 내용은 "캠페인 연동 가이드"를 참고하세요.
```
void printDeviceInfo() 
{
  NSString *deviceID = [TAGAnalytics deviceInfoWithKey:DEVICE_INFO_DEVICEID];
  NSString *pushToken = [TAGAnalytics deviceInfoWithKey:DEVICE_INFO_TOKEN;
  NSString *cUserID = [TAGAnalytics deviceInfoWithKey:DEVICE_INFO_CAMPAIGN_USERID];
  ……
}
```

### 3. SDK 버전 확인
SDK 버전은 "getVersion()" 함수를 통해 확인할 수 있습니다.
```
[TAGAnalytics version];
```
