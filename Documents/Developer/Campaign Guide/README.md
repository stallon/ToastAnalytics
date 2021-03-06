#Overview
Analytics 에서 캠페인이란 인하우스 캠페인을 의미합니다. 이는 앱 내에서 팝업이나 배너를 통해 사용자에게 이벤트(미션)를 노출하고, 사용자가 이벤트를 진행한 경우 보상을 지급할 수 있는 기능을 제공합니다.
Toast Analytics가 제공하는 인하우스 캠페인 기능은 캠페인 실행에 따른 이용자 증가, 매출 증가에 대한 성과 분석 지표를 자동으로 제공하며, 캠페인의 효율을 극대화하기 위하여 캠페인 목적에 맞는 대상자 타게팅을 쉽게 적용할 수 있습니다.

이 기능을 사용하기 위해서는 크게 아래와 같은 추가 작업이 필요하며 이 문서는 해당 작업들에 대한 자세한 설명을 담고 있습니다.
######1.캠페인 노출 및 보상 처리를 위해 앱 클라이언트에 Toast Analytics SDK의 적용이 필요합니다.
######2.사용자의 미션 달성 알림 및 보상 확인을 위해 게임 서버(혹은 애플리케이션 서버와 캠페인 서버간의 통신이 필요합니다.

##용어설명
캠페인 서비스에서는 다음 용어를 내부적으로 사용합니다.

|용어|설명|
|---|---|
|(인하우스) 캠페인|	앱 내에서 팝업이나 배너를 통해 사용자에게 이벤트(미션)을 노출하고, 사용자가 미션을 달성한 경우 보상을 지급할 수 있는 Toast Analytics의 기능을 의미한다|
|(캠페인) 미션|	캠페인의 보상 지급을 위해 사용자가 달성해야 하는 목표를 의미한다|
|커스텀 데이터|	커스텀 데이터|
|(캠페인) 보상|	사용자가 미션을 달성한 경우 지급해야 하는 앱 내의 재화를 의미한다|

##중요 연동 항목 설명
Campaign 서비스의 동작 흐름은 아래 그림과 같습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image003.png)
[그림 1 서비스 동작 흐름도]

아래는 연동 시 필요한 중요 항목에 대한 간단한 설명이며 이 문서 뒷 부분에서 좀 더 자세한 내용을 살펴볼 수 있습니다.
###1. TOAST Analytics SDK 적용
Analytics Campaign 기능을 사용하기 위해서는 Toast Analytics SDK를 앱에 적용해야 합니다.
적용과 관련된 내용은 이 문서 뒷 부분에서 확인할 수 있으며 별도로 제공되는 Toast Analytics API 레퍼런스 문서를 참조해도 됩니다.

###2. 캠페인 설정
신규 캠페인을 바로 생성할 수도 있지만 템플릿을 통해 효율적인 캠페인을 좀 더 빠르고 쉽게 만들 수 있습니다. 
Toast Analytics 사이트의 [캠페인 실행] 메뉴를 통하여 새로운 캠페인을 등록할 수 있습니다. 이에 대한 자세한 설명은 해당 페이지의 우측 상단에 위치한 [페이지 가이드]를 통하여 제공됩니다.

###3. 테스트 단말기 등록
캠페인이 일반 사용자에게 노출되기 전, 캠페인의 정상 노출 및 동작여부를 확인하기 위하여 테스트 과정을 거쳐야 하며, Toast Analytics SDK를 통해 Device ID와 Device Token을 획득한 후 테스트 단말기를 등록하면 됩니다.
Analytics 사이트 내 [앱 설정 버튼] > [캠페인 설정] > [테스트 디바이스 설정] 을 통해 테스트 단말기를 등록할 수 있습니다.

###4. 캠페인 테스트 진행
테스트 단말기가 정상적으로 등록되면, Toast Analytics 웹사이트 내의 [캠페인 실행 메뉴] > [캠페인 리스트] > [테스트 버튼]을 통해 테스트를 진행하실 수 있습니다.

###5. 캠페인 동작 확인
테스트 단말기를 통해 캠페인의 정상 동작 여부를 확인합니다. 아래와 같은 총 4단계의 진행 상태를 확인해야 하며 개별 단계에서의 자세한 사항은 Toast Analytics 사이트 우측 상단의 페이지 가이드에서에서 확인하시면 됩니다.
단, 미션 달성 & 보상 검증은 미션 및 보상을 설정한 캠페인의 경우에만 해당됩니다.
- 노출: 테스트 단말기로 캠페인 정보가 정상적으로 전달되어 노출준비가 된 상태를 의미합니다.
- 실행: 테스트 단말기에서 배너 혹은 팝업이 노출되어 캠페인에 참여되었음을 의미합니다.
- 미션: 게임 서버에서 캐Y인 서버로 미션 데이터가 정상적으로 전달되었음을 의미합니다.
- 보상: 캠페인 서버에서 게임 서버로 정보가 정상적으로 전달되었음을 의미합니다.

###6. 캠페인 노출
캠페인 테스트가 모두 완료된 경우 외부 사용자가 접근할 수 있도록 캠페인을 오픈할 수 있습니다. 캠페인이 오픈되고 앱이 캠페인 노출 지점(캠페인 설정 >노출위치 설정 참조)에 다다른 경우 캠페인이 노출됩니다.

###7. 미션 완료 및 보상 처리
캠페인의 미션을 달성하기 위해 사용자가 특정 행동(예. 레벨, 랭킹의 변화 등)을 하였다면 게임 서버는 캠페인 서버 API(check mission)를 호출하여 미션달성 여부를 확인할 수 있습니다. 
미션이 달성된 경우에는 캠페인 서버에서 게임서버로 보상정보가 전달되며 이 정보를 근거로 게임서버에서는 클라이언트로 보상을 지급해주어야 합니다.

#게임 클라이언트 적용

Campaign 시스템의 동작 흐름은 아래 그림2와 같습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image004.png)

[그림 2 시스템 동작 흐름도]

##게임 클라이언트

개념 이해를 돕기 위해 Java Code로 설명합니다. 자세한 내용은 각 플랫폼 별로 제공되는 Programming Guide를 참고하세요.

###1. 초기화

캠페인 기능을 사용하기 위해서는 초기화 함수를 반드시 호출해주어야 합니다.

```
GameAnalytics.initializeSdk(getActivity(), appId, entId, appVer,  useLoggingUserId);
```


캠페인 정보 요청 전에 Campaign Listener를 등록합니다.
```
GameAnalytics.setCampaignListener(campaignListener); 
```

Listener에서는 아래의 Callback을 제공합니다.
- onCampaignVisibilityChanged : 캠페인 및 보상 관련 팝업/배너가 화면에 보이거나 사라질 때 호출됩니다.
- onCampaignLoadSuccess, onCampaignLoadFail : 캠페인 서버에서 가져 온 캠페인 정보 파싱 결과를 알려줍니다. 게임에서는 이 
Callback에서 특별한 처리를 할 필요는 없습니다. 로그 확인을 위해 제공하는 Callback입니다.

```
private CampaignListener campaignListener = new CampaignListener() {
@Override
public void onCampaignVisibilityChanged(String adSpaceName, boolean show) { ... }
@Override
public void onCampaignLoadSuccess(String adSpaceName) { ... }
@Override
public void onCampaignLoadFail(String adSpaceName, CampaignException exception) { ... }
}; </string>
```
###2. 사용자 ID등록
캠페인 진행 및 보상 처리를 위해서는 사용자 식별이 꼭 필요하며, 캠페인은 사용자 ID를 식별 값으로 이용합니다. 따라서 캠페인 기능을 사용하기 위해서는 사용자 ID 설정 함수를 반드시 호출해 주어야 합니다.
또한 이때 사용된 사용자 ID 값은 게임 서버에서 미션 달성 시 호출하는 API를 호출할 때 보내는 사용자 ID와 같은 값이어야 합니다.
사용자 ID는 사용자를 구분할 수 있는 유니크한 값이어야 하며 50Byte 이하의 문자열로 입력되어야 합니다.
``` 
GameAnalytics.setUserId(userId,true); 
```
###3. 캠페인 노출
onCampaignLoadSuccess 이후 캠페인을 노출시키고자 하는 지점에서 캠페인 노출 함수를 호출하고, 일정시간 사용자에게 노출한 이후 화면에서 제거하는 함수를 호출하여 캠페인 팝업/배너를 제거합니다.
아래와 같은 형태의 API를 제공합니다.
```
showCampaign(String adspaceName, Activity parent);
showCampaign(String adspaceName, Activity parent, int animation, int lifeTime);
hideCampaign(String adspaceName);
hideCampaign(String adspaceName, int animation);
```

###4. 보상정보 처리
캠페인이 미션/보상 정보를 포함한 경우, 게임서버는 사용자가 미션과 관련된 액션을 수행할 때마다 캠페인 서버에 미션 달성여부를 체크하게 되고 미션 달성이 확인되었다면 보상정보를 게임클라이언트로 전달하게 됩니다. 이 부분은 게임서버와 게임클라이언트 간의 프로토콜이기 문에 게임에서 자체적으로 처리해주시면 됩니다.

###5. 푸쉬 연동
Analytics SDK에서 캠페인을 위해서 GCM을 사용합니다. GCM 사용을 위해서는 Analytics 관리자 페이지에 Project Number와 API Key를 등록해야 합니다. 또한 SDK에는 SenderID를 등록해야 합니다.
GCM에 관련된 전반적인 내용은 Google에서 제공하는 [Google Cloud Message](http://developer.android.com/google/gcm/gs.html) 문서를 참고하세요.
Analytics 관리자 페이지에 정보를 등록하는 방법은 “Getting Started” 문서에서 “푸쉬 설정” 항목을 참고하세요.
SDK에 SenderID는 “setGcmSenderId” API를 동해 등록할 수 있습니다.
```
 import com.nhnent.android.toast.analytics.GameAnalytics;
public class TestActivty extends Activity {
@Override
protected void onCreate(Bundle savedInstanceState)
{
super.onCreate();
……
int result = GameAnalytics.initializeSdk(getApplicationContext(), “AppID”, “CompanyID”, “AppVersion”, false);
if(result != GameAnalytics.S_SUCCESS)
	Log.d(TAG, "initialize error " + GameAnalytics.getResultMessage(result));
}
GameAnalytics.setGcmSenderId(“123456789012”);
……
} 
```

GCM 사용을 위해서는 AndroidManifest.xml 파일에 추가로 permission과 reciever를 등록해야 합니다.
추가로 필요한 permission은 아래와 같습니다. (여기에서 “APP.PACKAGE.NAME”은 사용자의 패키지명으로 변경해야 합니다.)
```
<!--  gcm 푸쉬 권한 설정 -->
<uses-permission android:name="android.permission.GET_ACCOUNTS"></uses-permission>
<uses-permission android:name="android.permission.WAKE_LOCK"></uses-permission>
<uses-permission android:name="com.google.android.c2dm.permission.RECEIVE"></uses-permission>
<uses-permission android:name="android.permission.VIBRATE"></uses-permission>
<permission android:name="APP.PACKAGE.NAME.permission.C2D_MESSAGE" android:protectionLevel="signature"></permission>
<uses-permission android:name="APP.PACKAGE.NAME.permission.C2D_MESSAGE"></uses-permission>
```

추가로 등록해야 하는 Receiver는 아래와 같습니다. (역시 “APP.PACKAGE.NAME”은 사용자의 패키지명으로 변경해야 합니다.)
```
<manifest>
……
<application>
……
<!-- gcm 리시버 설정 --> 
<receiver android:name="com.toast.android.analytics.gcm.GcmBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
	<intent-filter android:priority="”1”">
	  <action android:name="com.google.android.c2dm.intent.RECEIVE"></action>
	  <category android:name="APP.PACKAGE.NAME"></category>
	</intent-filter>
</receiver>
<service android:name="com.toast.android.analytics.gcm.GcmIntentService"></service>
……        
</application>
</manifest>
```

서버에서 푸쉬(GCM) 메시지를 받으면 Analytics SDK는 Notification Bar에 알림을 표시합니다. 이 알림을 터치하면 앱을 실햅합니다. 
이때 아무런 설정을 하지 않으면 해당 앱의 Main Activity를 띄웁니다. Main Activity가 아닌 다른 Activity를 실행하기 위해서는 “setPushIntent”함수를 통해서 명시적으로 지정해야 합니다.
```
import com.nhnent.android.toast.analytics.GameAnalytics;
public class TestActivty extends Activity {
@Override
protected void onCreate(Bundle savedInstanceState)
{
super.onCreate();
……
int result = GameAnalytics.initializeSdk(getApplicationContext(), “AppID”, “CompanyID”, “AppVersion”, false);
if(result != GameAnalytics.S_SUCCESS)
	Log.d(TAG, "initialize error " + GameAnalytics.getResultMessage(result));
}
GameAnalytics.setGcmSenderId(“123456789012”);
……
Intent intent = new Intent(getApplicationContext(), GCMActivity.class);
GameAnalytics.setPushIntent(intent);
……
} 
```

덧붙여 AndroidManifest.xml에 있는 Activity android:launchMode를 singeTop, singleTask, singleInstance로 설정하는 경우 새로운 Activity를 만들지 않고 기존 것을 재사용 합니다.
이때 Push에 대한 정보가 담긴 Intent 정보를 제대로 전달받기 위해서는 해당 Activity에서 “onNewIntent”를 명시적으로 Override 하여야 합니다.
```
public class NEAFlatSampleActivity extends Activity {
@Override
protected void onNewIntent(Intent intent)
{
super.onNewIntent(intent);
setIntent(intent);
}
} 
```

SDK 에서 기본으로 제공하는 GCM 푸쉬 리시버를 사용하지 않고, 사용자가 직접 구현하는 경우 Manifest의 설정은 아래와 같이 변경되어야 합니다.
또한 GCM 푸쉬를 받아 처리하는 로직(리시버, 서비스, Notification) 또한 사용자가 직접 구현해야 합니다.
```
<manifest>
……
<application>
……
<!-- gcm 리시버 설정 --> 
<receiver android:name="com.toast.android.analytics.gcm.GcmBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
<intent-filter android:priority="”1”">
  <action android:name="com.google.android.c2dm.intent.RECEIVE"></action>
  <category android:name="APP.PACKAGE.NAME"></category>
</intent-filter>
</receiver>
<service android:name="com.toast.android.analytics.gcm.GcmIntentService"></service>
……
<!--  gcm push 관련 사용자 정의 설정 -->
<receiver android:name=".GcmBroadcastReceiver" android:permission="com.google.android.c2dm.permission.SEND">
<intent-filter>
<action android:name="com.google.android.c2dm.intent.RECEIVE"></action>
<category android:name="com.example.aflatgametest"></category>
</intent-filter>
<receiver>
<service android:name=".GcmIntentService"></service>
……
</application>
</manifest>
```

위에서 보이는 것처럼 SDK 용으로 설정된 리시버, 서비스 설정 아래에 사용자 정의 설정을 신규로 추가하도록 합니다.
푸쉬 payload 구성시 “cid” 필드는 사용자 정의 푸쉬에서 사용해서는 안됩니다. “cid” 필드는 SDK 에서 제공하는 기본 푸쉬에서 내부적으로 사용하는 태그로 캠페인 정보를 전달하기 위해 사용하고 있습니다.

###6. Devide Id와 Push Token 확인
본 매뉴얼의 [적용 따라하기 (게임운영자)] > [환경설정] > [캠페인 설정] > [테스트 디바이스 설정] 부분을 진행하기 위해서는 테스트 단말기의 Device ID 와 Push Token 정보가 필요합니다. 
Toast Analytics SDK 에서는 Device ID와 Push Token을 확인을 위해서 각 플랫폼 별로 API를 제공합니다. 이 API는 initializeSDK 이후에 호출해야 합니다.
```
Android
public static String DEVICE_INFO_DEVICEID = "deviceId";
public static String DEVICE_INFO_TOKEN = "token";         
public static String DEVICE_INFO_CAMPAIGN_USERID = "campaignUserId";
public static String getDeviceInfo(String key)
iOS
#define DEVICE_INFO_DEVICEID (@"deviceId")
#define DEVICE_INFO_TOKEN (@"token")
#define DEVICE_INFO_CAMPAIGN_USERID (@"campaignUserId")
+(NSString*)deviceInfoWithKey:(NSString*)key
Unity
public const string DEVICE_INFO_DEVICEID = "deviceId";
public const string DEVICE_INFO_TOKEN = "token";         
public const string DEVICE_INFO_CAMPAIGN_USERID = "campaignUserId";
public static string getDeviceInfo(string key)
Cocos2d-x
#define DEVICE_INFO_DEVICEID_STR ("deviceId")
#define DEVICE_INFO_TOKEN_STR ("token")
#define DEVICE_INFO_CAMPAIGN_USERID_STR ("campaignUserId")
std::string getDeviceInfo(std::string key) 
```

아래는 Android 에서 Device ID 와 Push Token 출력을 위한 코드입니다.
```
// Device ID를 가져옵니다.
String deviceId = GameAnalytics.getDeviceInfo(GameAnalytics.DEVICE_INFO_DEVICEID);
// Push Token을 가져옵니다.
String pushToken = GameAnalytics.getDeviceInfo(GameAnalytics.DEVICE_INFO_TOKEN);
// 가져온 Device ID 와 Push Token 을 Logcat 콘솔에 출력합니다.
Log.d(“Sample”, “Device ID : “ + deviceId);
Log.d(“Sample”, “Push Token : “ + pushToken); 
```

로그 확인을 위해서 Android는 로그캣을, iOS는 iPhone Configurator를 이용하면 됩니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image005.png)
[그림 3 콘솔 로그에 출력된 디바이스 ID와 푸쉬토큰]

Device ID 와 Push Token 출력 코드는 테스트 시에만 사용할 것을 권장하며 실제 외부에 오픈되는 빌드에는 제거하시면 됩니다.

#게임서버

###1. 게임서버 인증
캠페인 서버에 대한 접근 제한 정책이 ACL에서 다른 방식으로 변경될 예정입니다. 
현재는 외부 유입에 대하여 ACL을 통한 제한을 하고 있지 않은 상태입니다.
향후 인증 관련 정책이 결정되면 추가 작업이 필요할 수 있습니다.

###2. 보상처리
캠페인과 관련되어 게임서버의 주된 작업은 보상처리입니다.
게임서버는 다음의 경우에 대해 캠페인 Server로 check-mission API를 호출하여 미션달성 여부를 확인해야 합니다.
- 캠페인이 진행 중일 때, 미션설정과 관련된 Factor(Level, Ranking, 게임실행횟수 등 게임에서 지정한 값)의 변화가 있는 경우

캠페인 서버에 check-mission API를 호출했을 때 보상이 있는 경우에는 보상코드를 전달 받게 되며 게임클라이언트로 보상을 전달하면 됩니다. 이 처리는 게임에서 구현합니다. (응답 데이터의 rewardList 필드를 통해 보상 정보가 전달되며 한번만 전달됩니다. 주의할 점은 보상이 없는 경우에도 rewardList 필드는 전달되며 빈 값이 전달됩니다. 

게임서버가 호출해야 하는 미션 달성 알림 API의 포맷은 아래와 같습니다.

###3. 요청 예시
```
Host: https://api-campaign-analytics.cloud.toast.com
POST /campaign/v1/server/check-mission
Content-Type: application/json
{
"header":
{
"transactionId" : 92348729384729,
},
"userId" : "23948234",
"appId" : "13",
"missionKey" : "LEVEL",
"missionValue" : 10
} 
```

###4. 요청 파라미터
요청 파라미터

|이름|자료형|설명|
|---|---|---|
|transactionId|	int64|	요청에 대한 추적을 위해 로깅 시에 사용되며 필수 값은 아닙니다. 여기에 입력된 값은 응답 데이터의 transactionId 필드에 동일하게 설정되어 반환됩니다|
|userId|	string|	게임에서 제공하는 사용자 unique ID|
|appId|	string|앱 등록 시 할당받은 앱 번호. Analytics 사이트의 [앱설정] > [AppKey]값을 입력하시면됩니다.|
|missionKey|	string|	특정 행동을 정의하는 Key 값 혹은 onMissionComplete(SDK) 를 통해 전달받은 값, Analytics 사이트의 [앱설정] > [캠페인 설정]> [미션 및 보상 아이템 설정]에서 등록한 미션 key 값|
|missionValue|	int64|	missionKey 에 대한 Value 값|

###5. 응답 예시
```
HTTP/1.1 200 OK
{
"header" :
{
"transactionId" : 92348729384729,
"isSuccessful" : "false",
"resultcode" : 9001,
"resultMessages" : ["Invalid input parameter.", "..."],
"serviceCode" : 10
},
"rewardList" : [
{
"campaignId" : 7,
"promoDateBegin" : "2014-10-10 00:00:00",
"promoDateEnd" : "2014-10-11 00:00:00",
"rewardDateBegin" : "2014-10-10 00:00:00",
"rewardDateEnd" : "2014-10-12 00:00:00",
"rewardCode" : "gem",
"rewardValue" : 100
},
...
]
}
```

###6. 응답 데이터
응답 데이터

|이름|자료형|설명|
|---|---|---|
|transactionId	|int64|	요청 시 전달받은 transactionId 를 동일하게 설정한다|
|isSuccessful	|string|	수행 성공 여부를 설정한다. (성공: "true", 실패: "false")|
|resultCode	|int|	리턴 코드를 작성한다. (성공인 경우 0)|
|resultMessages	|vector<string>|	복수 개의 리턴 메시지를 작성한다.|
|serviceCode	|int|	서비스 코드|
|campaignId	|int|	보상이 있는 캠페인 번호|
|promoDateBegin	|string|	캠페인 시작 시간 (UTC+0 기준)|
|promoDateEnd	|string|	캠페인 종료 시간 (UTC+0 기준)|
|rewardDateBegin	|string|	보상 시작 시간 (UTC+0 기준)|
|rewardDateEnd	|string|	보상 종료 시간 (UTC+0 기준)|
|rewardCode	|string|	보상 코드|
|rewardValue	|int|	보상 값|

#캠페인 환경설정

##환경설정: 캠페인 실행 전 설정하기

캠페인을 실행하기 위해서는 “앱설정>캠페인설정”과 “푸쉬설정”에 필요한 정보를 등록해야 합니다. 
“노출위치 설정”은 게임에서 캠페인 이미지가 노출될 위치를 등록하는 메뉴로서 필수 입력 값이고, “미션 및 보상 아이템 설정”은 보상이 있는 캠페인일 경우에만 등록하면 됩니다. 
“랜더링 설정”의 경우 사용자 정의 닫기 버튼을 사용하고자 할 경우에만 설정하면 됩니다. 시스템에서 제공하는 이미지를 사용할 경우 설정하지 않아도 됩니다. 
"테스트 디바이스 설정"항목에서는 캠페인 실행 테스트를 위해 한 개 이상의 테스트 디바이스를 반드시 등록하여야 합니다. 
"푸쉬설정"은 푸쉬를 활용할 캠페인을 진행하는 경우 필수로 입력해야 합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image4.png)
[그림 4 캠페인 설정메뉴 위치]
###1.캠페인 설정 > 노출위치 설정 및 테스트
애플리케이션에서 캠페인 팝업 혹은 배너가 노출될 위치에 대한 유니크한 식별자인 KEY를 설정합니다. 팝업과 배너를 등록하고 정상적으로 이미지가 노출되는지 OS 별로 테스트를 진행해야 합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image007.jpg)
[그림 5 캠페인 노출위치 설정]

“노출위치”는 중복되지 않으며, 하나의 노출위치에는 한 시점에 하나의 캠페인만 가능합니다.
한 개의 키로 안드로이드와 iOS를 공통으로 사용합니다. 캠페인 팝업/배너를 노출하고자 하는 애플리케이션의 특정 위치에서는 SDK의 캠페인 노출 API를 해당 KEY의 파라미터를 호출하여야 합니다.
노출위치의 상태항목에는 [테스트], [결과확인], [연동완료]가 있으며, [결과확인], [연동완료] 상태에서는 KEY값을 더 이상 수정, 삭제하실 수 없습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image008.jpg)
[그림 6 캠페인 노출위치 수정 팝업]

“노출위치 테스트”는 노출위치가 정상적으로 소스에 삽입되어 있는지 확인하는 기능입니다.
각 상태값은 다음과 같습니다.
- [테스트] 버튼
현재 테스트를 시작할 수 있는 상태를 의미합니다.
- [결과 확인] 버튼
해당 노출위치에 테스트 프로모션이 실행중이며, 실제 소스가 삽입된 애플리케이션을 통해 해당 노출위치에 프로모션이 정상적으로 노출되고 있는지 확인하실 수 있습니다. 새로 등록한 프로모션을 테스트하기 위해서는 앱을 재실행해야 합니다. 프로모션이 노출되는지 정상적으로 확인되면, 해당 버튼 클릭시 '연동 완료'상태로 변경됩니다.그렇지 않으면, 다시 [테스트]상태로 초기화됩니다.
- 연동완료
해당 노출위치가 정상적으로 소스에 삽입된 것을 확인 완료했음을 의미합니다.

###2. 캠페인 설정 > 미션 및 보상 아이템 설정
“캠페인 실행 > 캠페인 생성하기 > 기본 설정” 에서 미션 및 보상 여부를 “있음”에 체크되면 필수로 설정값이 입력되어야 합니다. 미션 및 보상 아이템은 OS 공통으로 사용됩니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image009.jpg)
[그림 7 캠페인 노출위치 수정 팝업]

캠페인을 등록하고 한 번 이상 테스트를 진행한 이후부터는 수정, 삭제가 되지 않으므로 신중히 입력해야 합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image010.jpg)
[그림 8 미션수정 팝업]

###3. 캠페인 설정 > 렌더링 설정
캠페인 팝업에 노출할 닫기(X) 버튼을 등록합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image011.jpg)
[그림 9 렌더링 설정 페이지]

등록한 닫기 버튼은 “캠페인 실행 > 캠페인 생성하기 > 기본 설정” 에서 “인앱 노출방식”을 팝업으로 선택합니다.
그 뒤, “상세 설정”에서 닫기버튼 옵션 중 “사용자지정”으로 선택하면 등록한 닫기 버튼을 캠페인에서 사용할 수 있습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image012.jpg)
[그림 10 등록한 닫기버튼 사용하기]

###4. 캠페인 설정 > 테스트 디바이스 설정
노출위치, 미션 및 보상 아이템, 푸쉬, 캠페인 등을 테스트 하기 위해 디바이스를 등록합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image11.png)
[그림 11 테스트 디바이스 설정 페이지]

사용하실 디바이스ID와 GCM 혹은 APNS에서 발급받은 토큰 정보를 테스트 디바이스 등록팝업에서 OS별로 입력합니다.
Toast Analytics SDK의 initializeSdk API 호출 시 useLoggingUserId 값을 true 로 설정한 경우에는 ‘디바이스ID’ 입력 란에 사용자 ID를 입력하여야 합니다.
```
int result = initializeSdk(context,  appId, companyId, appVersion, true);
```

Toast Analytics SDK 설치된 앱을 통해 Device ID 와 Device Token 을 전달받아 입력합니다.
본 매뉴얼의 [게임 클라이언트 적용] > [게임 클라이언트] > [Device ID 와 Push Token 확인] 부분을 참고하시기 바랍니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image12.png)
[그림 12 테스트 디바이스 등록 및 수정 팝업]

###5. 푸쉬 설정
푸쉬발송 기능을 사용하기 위해서는 APNS 인증서 (iOS) 또는 GCM API Key (안드로이드)를 발급받아 푸쉬설정 페이지에 등록해주어야 합니다. 플랫폼 별로 인증서, API Key를 확인하여 등록하는 방법은 다음 내용을 참조해 주십시오.

######iOS 푸쉬 설정

iOS의 푸쉬 알림은 APNS(Apple Push Notification Service)를 활용합니다. 따라서 푸쉬발송 기능을 사용하기 위해서는 Apple 개발자 센터에 앱 등록 및 푸쉬 사용활성화가 완료되어 있어야 합니다.
앱 등록과 푸쉬 사용활성화가 되어있다면 앱 선택시 아래와 같은 정보을 확인할 수 있습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image015.png)
[그림 13 APNS에서 앱 등록 및 푸쉬 사용 활성화 확인 (iOS)]

앱 등록과 푸쉬 사용활성화가 되어있다면 Analytics에 푸쉬 인증서를 등록해야 합니다. Analytics에서 사용하는 iOS 푸쉬 인증서는 .p12 인증서로 발급과정은 다음과 같습니다.
####Step 1. 개발자 센터에서 [Certificates, Identifiers & Profiles] 메뉴를 선택한 후 [Certificates] 메뉴에서 등록 된 앱을 확인합니다.
정상적으로 등록이 되어있다면 앱 이름과 APNs Development iOS, APNs Production iOS 인증서를 찾을 수 있습니다. APNs Production iOS 인증서를 선택하면 인증서를 다운로드 할 수 있습니다. Analytics Service에서 푸쉬기능을 사용하려면 Development 인증서가 아닌 Production 인증서가 반드시 필요합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image016.png)
[그림 14 인증서 다운로드 (iOS)]

####Step 2. 다운로드 받은 인증서를 더블 클릭하면 키체인 접근 앱에 등록이 됩니다. 키체인 접근 앱을 실행하고 로그인 항목에서 인증서 카테고리를 선택하면 설치한 인증서를 찾을 수 있습니다. 
목록에서 인증서를 선택하고 펼치면 개인키가 함께 노출됩니다. 인증서와 개인키를 함께 선택한 후 마우스 오른쪽버튼을 클릭하면 팝업메뉴가 노출되는데, [2개 항목 보내기…] 메뉴를 선택합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image017.jpg)
[그림 15 인증서 보내기 (iOS)]

####Step 3. .p12 파일을 생성할 위치를 지정하고, 내보내기 위한 암호를 설정합니다. 
이 암호는 Analytics에서 인증서 등록 시에 필요하므로 잘 기억해둡니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image018.jpg)
[그림 16 인증서 암호설정 (iOS)]

####Step 4. 생성한 인증서를 Analytics 서비스 [APP SETTING] > [푸쉬설정] 메뉴에서 등록합니다.
위에서 생성한 .p12 인증서와 인증서 생성시 입력한 암호를 함께 입력합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image17.png)
[그림 17 Analytics 푸쉬설정 메뉴에서 인증서 등록 (iOS)]

######Android 푸쉬 설정

Android의 푸쉬 알림은 GCM(Google Cloud Messaging)을 활용합니다. 따라서 푸쉬발송 기능을 사용하기 위해서는 Google 개발자 콘솔에 앱 등록 및 GCM 사용활성화가 완료되어 있어야 합니다.
Analytics Service에서 Android 푸쉬발송기능 사용을 위해서는 ‘Project Number’와 ‘API Key’가 필요합니다.
####Step 1. [Google Developers Console](https://console.developers.google.com/project)에서 프로젝트 생성이 완료되었다면 프로젝트 대시보드에서 [Project Number]를 확인할 수 있습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image020.png)
[그림 18 Project Number 확인 (안드로이드)]

####Step 2. 프로젝트 메뉴에서 [APIs & auth] > [APIs] 메뉴를 선택합니다.
Google Cloud Messaging for Android 항목이 비활성화 상태이면 활성화(ON) 시킵니다.
####Step 3. 프로젝트 메뉴에서 [APIs & auth] > [Credentials] 메뉴를 선택합니다.
API KEY 발급을 위해 [Public API access] 섹션에서 [Create new Key] 버튼을 클릭합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image021.png)
[그림 19 API Key 생성 (안드로이드)]

####Step 4. Server key를 선택하고, 허용 IP를 반드시 빈칸으로 둡니다. 이는 Analytics Service에서 푸쉬 메시지를 발송할 때 제한이 걸리는 것을 막기 위함이며, 허용 IP를 특정 IP로 지정하면 푸쉬 발송을 할 수 없게 됩니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image022.png)
[그림 20 허용 IP 공란 유지 (안드로이드)]

####Step 5. 발급한 API KEY는 Analytics Service 푸쉬설정에서 입력하기 위해 필요한 값입니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image023.png)
[그림 21 API Key 확인 (안드로이드)]

####Step 6. [APP SETTING] > [푸쉬설정] 메뉴에서 Project Number와 API Key를 등록합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image22.png)
[그림 22 Project Number 및 API Key 등록 (안드로이드)]

#캠페인 생성

캠페인 생성방법에는 템플릿을 활용하거나 사용자가 모든 항목을 입력하여 생성하는 방법이 있습니다.
주기적으로 실행되는 캠페인은 템플릿으로 등록하면 쉽고 빠르게 캠페인을 생성할 수 있습니다.

##1. 캠페인 템플릿 생성

자주 실행되거나 특정한 목표가 있는 캠페인에 대해 캠페인 템플릿을 제공합니다. "초기이탈방지, 이탈방지, 복귀유도, 첫구매유도, 재구매유도"의 5가지 유형을 제공합니다. 
시스템에서 제공되는 기본 템플릿에 대해서는 사용자가 삭제할 수 없습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image23.png)
[그림 23 템플릿 리스트]

사용자가 실행했던 캠페인을 템플릿으로 저장하고자 할 때에 캠페인 생성 3단계인 상세설정에서 템플릿으로 저장할 수 있습니다. 생성된 템플릿은 “템플릿으로 캠페인 생성하기” 목록에 표시되며, 사용자가 삭제할 수 있습니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image24.png)
[그림 24 템플릿 생성하기]

##템플릿으로 캠페인 생성하기

템플릿으로 캠페인을 생성하고자 할 때는 “템플릿으로 캠페인 생성하기’ 목록에서 캠페인 생성 버튼을 클릭합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image25.png)
[그림 25 템플릿으로 캠페인 생성하기]

캠페인 생성 화면에서 템플릿으로 관리되는 대상선택, 플랫폼, 진행 국가, 인앱 노출방식, 푸쉬 사용여부, 미션 및 보상 여부 항목이 설정되어 있습니다. 그 외 일정 및 미션 및 보상 설정, 상세 설정 정보는 해당 캠페인 내용에 맞게 입력한 후 캠페인을 등록 합니다. (※ 캠페인실행 - 캠페인 생성하기 Guide 참조)
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image26.png)
[그림 26 템플릿 생성시 설정되는 항목]

##캠페인실행

템플릿을 활용하지 않고, 사용자가 원하는 모든 항목을 입력하여 새로운 캠페인을 생성할 수 있습니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image27.png)
[그림 27 캠페인 생성하기 버튼]

기본설정, 미션 및 보상설정, 상세설정 3단계로 정보를 입력하고 캠페인을 등록하면 캠페인이 생성됩니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image030.jpg)
[그림 28 캠페인 생성 단계]

###1. 기본설정
캠페인의 대상선택, 일정, 플랫폼 등 기본정보를 입력합니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image29.png)
[그림 29 캠페인 기본설정]

###2. 미션 및 보상설정
캠페인을 진행 시 미션 및 보상을 "있음"으로 선택했을 경우 미션과 보상에 대한 달성값, 보상종류, 지급될 보상값을 입력합니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image30.png)
[그림 30 캠페인 미션 및 보상설정]

###3. 상세설정
기본설정에서 선택한 “인앱 노출방식”과 “푸쉬 사용 여부”, “미션 및 보상”에 따라 상세 설정을 합니다. “인앱 노출방식”을 팝업으로 선택한 경우 “팝업 설정”을 합니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image31.png)
[그림 31 캠페인 상세설정 중 팝업설정]

"인앱 노출방식"을 배너로 선택 한 경우 "배너 설정"에 대한 상세설정을 입력합니다.
![Project Setting](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image32.png)
[그림 32 캠페인 상세설정 중 배너설정]

“푸쉬를 사용할 경우, “푸쉬 설정”에 대한 상세설정을 입력합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image33.png)
[그림 33 캠페인 상세설정 중 푸쉬설정]


발송일시는 캠페인 실행 시 한번만 발송되는 1회 발송과 매일 또는 매주 특정 시간에 주기적으로 발송되는 반복발송을 선택할 수 있습니다. (1회 발송의 경우 발송까지 최대 1시간 정도 걸릴 수 있습니다.)

#캠페인 테스트

캠페인 실행 전 설정된 내용이 정상적으로 노출되는지 확인하기 위해 테스트를 실행할 수 있습니다. 테스트가 정상적으로 완료된 이후에 캠페인을 배포할 수 있습니다.

##캠페인 상태

템플릿으로 또는 사용자 입력으로 생성된 캠페인은 “캠페인 리스트”에 노출됩니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image34.png)
[그림 34 캠페인 리스트에 노출된 신규생성 캠페인]
“상태” 컬럼은 1)임시저장, 2)등록완료[테스트], 3)테스트완료[실행], 4)실행 대기[정지], 5)실행중[정지], 6)종료의 6가지 항목으로 상태가 변경됩니다.
1) 임시저장: 캠페인의 일부 항목만 입력한 상태에서 [임시저장] 버튼을 누르면 ‘임시저장’상태로 저장됩니다. 임시저장 상태인 경우에는 캠페인 설정항목들을 수정하거나 캠페인을 삭제할 수 있습니다.
2) 등록완료[테스트]: 캠페인의 모든 항목을 입력하고 [생성]버튼을 누르면 캠페인이 생성되어 테스트 준비가 된 ‘저장완료’ 상태가 됩니다. 이때 [테스트] 버튼을 누르면 생성한 캠페인에 대한 테스트를 진행할 수 있습니다. 캠페인 테스트 진행방법은 2. 테스트하기 항목을 참조해 주세요.
3) 테스트완료[실행]: 캠페인 테스트가 완료된 경우 노출되는 상태입니다.
4) 진행대기[중지]: 캠페인 시작일 이전 [실행] 버튼을 누르면 진행대기 상태가 되며, 캠페인 시작일이 되면 자동으로 ‘실행중[중지]’ 상태로 변경됩니다. 진행대기 상태에서 [중지] 버튼을 누르면 ‘테스트완료 [실행]’ 상태로 돌아갑니다.
5) 실행중[중지]: 캠페인 시작일 이후 [실행] 버튼을 누르면 캠페인이 실행되어 사용자에게 캠페인 노출을 시작합니다. 이 상태에서 [중지] 버튼을 누르면, 캠페인 생성시 설정한 종료일 이전이더라도 캠페인을 종료합니다.
##테스트 하기

등록된 캠페인이 정상적으로 작동되는지 테스트 하기 위해서 "캠페인실행 > 캠페인리스트"의 "상태"에 있는"등록완료[테스트]" 에서 "테스트" 버튼을 클릭합니다. 
* 테스트는 팝업/배너 또는 푸쉬 클릭으로 캠페인이 시작되는 "캠페인 진행 테스트"와 디바이스에서 푸쉬를 받을 수 있는 지 확인하는 "푸쉬 발송 테스트"로 구분됩니다. 

####1) 푸쉬가 없는 경우 캠페인 진행 테스트하기
푸쉬가 없는 경우 테스트 버튼을 클릭하면 아래와 같은 팝업이 노출됩니다. 
팝업에서 "테스트 시작하기" 버튼을 클릭하면 테스트가 시작됩니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image35.png)
[그림 35 캠페인 테스트 팝업 (푸쉬 없는 경우)]

“환경설정 > 캠페인 설정 > 테스트 디바이스 설정”에 등록한 기기에 설치된 애플리케이션을 실행하여 캠페인이 정상적으로 노출되는지 확인하거나 푸쉬 메시지를 눌러 앱을 실행합니다.

######캠페인이 노출되지 않을 경우
- “테스트 디바이스 설정”에서 디바이스 정보가 정상인지 확인합니다.
- “노출위치”가 정확히 애플리케이션에 입력되어 있는지 확인합니다.

######캠페인이 정상종료된 경우 
팝업/배너에 대한 노출 테스트 종료 후 결과보기를 클릭하면 노출과 실행의 상태가 “완료”로 변경이 됩니다. 푸쉬 클릭으로 실행된 경우는 실행의 상태가 “완료”로 변경이 됩니다.
미션 및 보상이 있는 캠페인일 경우 등록한 미션을 달성하고 보상을 확인합니다. 테스트 종료 후 결과보기에서 미션과 보상이 “완료”로 변경된 것을 확인할 수 있습니다.

######미션이 실패로 노출된 경우
- 사용자의 미션 달성여부와 달성정보가 캠페인 REST API를 정상 호출했는지 확인합니다.
- 캠페인 REST API를 호출할 때 서버간 통신에 문제가 없는지 확인합니다.

######보상이 실패로 노출된 경우
- 미션 완료 이후, 애플리케이션에서 보상정보가 제대로 수신되었는지 확인합니다.
- 이 과정은 미션 완료 후 최대 5분 정도 소요될 수 있으며, 애플리케이션 종료 후 재 로그인 할 경우 바로 확인 가능합니다.
- 보상을 지급받은 이후에는 지급 정보가 캠페인 REST API 를 정상 호출했는지 확인합니다.
- 보상을 지급하기 전 캠페인 REST API 를 통해 유효성 여부를 확인한 후 보상을 지급해야 합니다.


####2) 푸쉬가 있는 경우 캠페인 진행 테스트하기 
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image36.png)
[그림 36 캠페인 테스트 팝업 (푸쉬 있는 경우)]

"푸쉬발송 테스트”는 푸쉬 발송 성공 여부와 상관없이 “환경 설정 > 캠페인 설정 > 테스트 디바이스 설정”에서 등록된 기기로 푸쉬가 발송 됩니다.

######푸쉬를 테스트 기기에서 못 받았을 경우 
- “환경 설정 > 푸쉬 설정”에서 인증서 정보가 정상인지 확인합니다.
- “환경 설정 > 캠페인 설정 > 테스트 디바이스 설정”에서 디바이스 토큰이 정상인지 확인합니다.
- 이후에도 발송이 되지 않을 경우 “문의하기”를 통해 문의합니다.
![Project Sett](https://raw.githubusercontent.com/ToastAnalytics/ToastAnalytics/master/Documents/Developer/Campaign%20Guide/images/image039.jpg)
[그림 37 푸쉬요청 발송알림 얼럿]
