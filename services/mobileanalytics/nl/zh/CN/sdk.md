---

copyright:
  years: 2015, 2016
lastupdated: "2016-10-18"

---

# 检测应用程序以使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK
{: #mobileanalytics_sdk}

上次更新时间：2016 年 10 月 19 日
{: .last-updated}

使用 {{site.data.keyword.mobileanalytics_full}} SDK，您可以检测移动应用程序。
{: shortdesc}

使用 {{site.data.keyword.mobileanalytics_short}}，您可以收集两种<!--three-->类别的数据，每一种需要不同程度的检测：

1. 预定义的数据：这种类别包含适用于所有应用程序的通用使用情况和设备信息。在这种类别中存在设备元数据（操作系统和设备模型）和使用情况数据（活动用户和应用程序会话），可指示应用程序使用的数量、频率和持续时间。当您在应用程序中初始化 {{site.data.keyword.mobileanalytics_short}} SDK 之后，会自动收集预定义的数据。

2. 应用程序日志消息：这种类别使得开发人员可以在整个应用程序中添加记录定制消息的代码行，以协助开发和调试。开发人员会为每一则日志消息分配严重性/详细程度级别，这样就可以根据所分配的级别对消息进行过滤，或者通过将应用程序配置为忽略低于给定日志级别的消息来保留存储空间。要收集应用程序日志数据，您必须在应用程序内初始化 {{site.data.keyword.mobileanalytics_short}} SDK，并对每一则日志消息，添加一行代码。

<!--2. Custom events - This category includes data that you define yourself and that is specific to your app. This data represents events that occur within your app, such as page views, button taps, or in-app purchases. In addition to initializing the {{site.data.keyword.mobileanalytics_short}} SDK in your app, you must add a line of code for each custom event that you want to track. -->

目前，Android、iOS 和 WatchOS 可以使用 SDK。

## 识别服务凭证 API 密钥值
{: #analytics-clientkey}

在设置客户端 SDK 之前，先识别您的 **API 密钥**值。初始化客户端 SDK 时，需要 API 密钥。

1. 打开 {{site.data.keyword.mobileanalytics_short}} 服务仪表板。
2. 单击**服务凭证**选项卡。
3. 展开**查看凭证**以显示 API 密钥值。当您初始化 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK 时，将需要 API 密钥值。


## 初始化 Android 应用程序以收集分析
{: #initalize-ma-sdk-android}

初始化应用程序以启用发送日志到 {{site.data.keyword.mobileanalytics_short}} 服务。

1. 通过将以下 `import` 语句添加到项目文件的开头，导入客户端 SDK：

  ```
  import com.ibm.mobilefirstplatform.clientsdk.android.core.api.*;
import com.ibm.mobilefirstplatform.clientsdk.android.analytics.api.*;
import com.ibm.mobilefirstplatform.clientsdk.android.logger.api.*;
  ```
  {: codeblock}

2. 通过在 Android 应用程序主要活动的 `onCreate` 方法中，或者在适合您项目的位置，添加初始化代码，以在 Android 应用程序中初始化 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK。

	```Java
	BMSClient.getInstance().initialize(this.getApplicationContext(), BMSClient.REGION_US_SOUTH); // Make sure that you point to your region
	```
  {: codeblock}

  要使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK，您必须使用 **bluemixRegion** 参数，初始化 ``BMSClient``。在初始化程序中，**bluemixRegion** 值指定您使用的是哪一个 {{site.data.keyword.Bluemix_notm}} 部署，例如 ``BMSClient.REGION_US_SOUTH` 和 ``BMSClient.REGION_UK`。<!-- , or `BMSClient.REGION_SYDNEY`.-->  <!-- Set this value with a `BMSClient.REGION` static property. -->  <!--You can optionally pass the **applicationGUID** and **applicationRoute** values if you are using another {{site.data.keyword.Bluemix_notm}} service that requires these values, otherwise you can pass empty strings.-->

3. 使用 Android 应用程序对象并为其提供应用程序名称，来初始化 Analytics。您还需要 [**API 密钥**](#analytics-clientkey)值。
	
	```Java
	// In this code example, Analytics is configured to record lifecycle events.
	Analytics.init(getApplication(), "your_app_name_here", apiKey, hasUserContext, Analytics.DeviceEvent.LIFECYCLE);
	```
  {: codeblock}
	
	您针对应用程序选择的名称 (`your_app_name_here`) 会在 {{site.data.keyword.mobileanalytics_short}} Console 中显示为应用程序名称。应用程序名称用作过滤器，在仪表板中搜索应用程序日志。当您跨平台（例如 Android 和 iOS）使用相同的应用程序名称时，您可以在相同的名称下，查看该应用程序的所有日志，而无论日志发自哪个平台。
	
	**注**：将 `hasUserContext` 的值设置为 **true** 或 **false**。如果是 false（缺省值），那么在计数时，每个设备都会被计为活动用户。通过 [`Analytics.setUserIdentity("username")`](sdk.html#android-tracking-users) 方法，您可以跟踪当前使用应用程序的每个装置的用户数，但是当 `hasUserContext` 为 false 时无法使用。如果是 true，那么每次使用 [`Analytics.setUserIdentity("username")`](sdk.html#android-tracking-users) 都会在计数时被计为活动用户。当 `hasUserContext` 为 true 时，没有缺省用户身份，因此必须设置该身份以填充活动用户图表。
	
4. [发送分析数据](sdk.html#app-monitoring-gathering-analytics)至 (sdk.html#app-monitoring-gathering-analytics) 服务。

## 初始化 iOS 应用程序以收集分析
{: #init-ma-sdk-ios}

初始化应用程序以启用发送日志到 {{site.data.keyword.mobileanalytics_short}} 服务。iOS 和 watchOS 可以使用 Swift SDK。

1. 通过将以下 `import` 语句添加到 `AppDelegate.swift` 项目文件的开头，导入 `BMSCore` 和 `BMSAnalytics` 框架：

  ```Swift
  import BMSCore
  import BMSAnalytics
  ```
  {: codeblock}

2. 要使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK，您必须先使用下列代码，初始化 `BMSClient` 类。

  将初始化代码置于应用程序代表的 `application(_:didFinishLaunchingWithOptions:)` 方法中，或者置于适合您项目的位置。
	
    ```Swift 
    BMSClient.sharedInstance.initialize(bluemixRegion: BMSClient.Region.usSouth) // Make sure that you point to your region
    ```
    {: codeblock}

    要使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK，您必须使用 **bluemixRegion** 参数，初始化 `BMSClient`。在初始化程序中，**bluemixRegion** 值指定您使用的是哪一个 {{site.data.keyword.Bluemix_notm}} 部署，例如 `BMSClient.REGION_US_SOUTH` 或 `BMSClient.REGION_UK`。
    <!-- , or `BMSClient.REGION_SYDNEY`. -->
   
   <!-- Set this value with a `BMSClient.REGION` static property. -->

   <!-- You can optionally pass the **applicationGUID** and **applicationRoute** values if you are using another {{site.data.keyword.Bluemix_notm}} service that requires these values, otherwise you can pass empty strings.-->

3. 通过为 Analytics 提供您的移动应用程序名称，对其进行初始化。您还需要 [**API 密钥**](#analytics-clientkey)值。

 应用程序名称用作过滤器，在 {{site.data.keyword.mobileanalytics_short}} 仪表板中搜索应用程序日志。通过跨平台（例如 Android 和 iOS）使用相同的应用程序名称，您可以在相同的名称下，查看该应用程序的所有日志，而无论日志发自哪个平台。

  可选 `deviceEvents` 参数会自动收集设备级别事件的分析。

 ### iOS
 {: #ios-initialize-analytics}
	
 ```Swift
 Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false, deviceEvents: DeviceEvent.lifecycle)
 ```
 {: codeblock}
  
 ### watchOS
 {: #watchos-initialize-analytics}
	
 ```Swift
 Analytics.initialize(appName: "your_app_name_here", apiKey: "your_api_key_here", hasUserContext: false)
  ```
 {: codeblock}
	
 您针对应用程序选择的名称 (`your_app_name_here`) 会在 {{site.data.keyword.mobileanalytics_short}} Console 中显示为应用程序名称。应用程序名称用作过滤器，在仪表板中搜索应用程序日志。当您跨平台（例如 Android 和 iOS）使用相同的应用程序名称时，您可以在相同的名称下，查看该应用程序的所有日志，而无论日志发自哪个平台。
	
 **注**：将 `hasUserContext` 的值设置为 **true** 或 **false**。如果是 false（缺省值），那么在计数时，每个设备都会被计为活动用户。通过 [`Analytics.userIdentity = "username"`](sdk.html#ios-tracking-users) 方法，您可以跟踪当前使用应用程序的每个装置的用户数，但是当 `hasUserContext` 为 false 时无法使用。如果 `hasUserContext` 为 true，那么每次使用 [`Analytics.userIdentity="username"`](sdk.html#ios-tracking-users) 都会在计数时被计为活动用户。当 `hasUserContext` 为 true 时，没有缺省用户身份，因此必须设置该身份以填充活动用户图表。

 您可以使用 `Analytics.recordApplicationDidBecomeActive()` 和 `Analytics.recordApplicationWillResignActive()` 方法，来记录 WatchOS 上的设备事件。
  
 将下列一行添加到 ExtensionDelegate 类的 `applicationDidBecomeActive()` 方法中。

	```
	Analytics.recordApplicationDidBecomeActive()
	```
  {: codeblock}

 将下列一行添加到 ExtensionDelegate 类的 applicationWillResignActive() 方法中：
	```
	Analytics.recordApplicationWillResignActive()
	```
  {: codeblock}
  
4. [发送分析数据](sdk.html#app-monitoring-gathering-analytics)至 {{site.data.keyword.mobileanalytics_short}} 服务。

## 收集使用情况分析
{: #app-monitoring-gathering-analytics}

  您可以配置 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK，以记录使用情况分析，并将记录的数据发送到 {{site.data.keyword.mobileanalytics_short}} 服务。

  使用下列 API，开始记录和发送使用情况分析：

#### Android
{: #android-usage-api}
	
```
// 禁用记录使用情况分析（例如，节省磁盘空间）
// 缺省情况下会启用记录
Analytics.disable();
	
// 启用记录使用情况分析
Analytics.enable();
	
// Send recorded usage analytics to the Mobile Analytics Service
Analytics.send(new ResponseListener() {
            @Override
            public void onSuccess(Response response) {
                // Handle Analytics send success here.
            }

            @Override
            public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
                // Handle Analytics send failure here.
            }
        });
```
{: codeblock}

<!-- removed: Analytics.log(eventJSONObject); -->

<!--	
Sample usage analytics for logging an event:
	
```
// Log a custom analytics event for custom charts, which is represented by a JSON object:
JSONObject eventJSONObject = new JSONObject();
	
eventJSONObject.put("customProperty" , "propertyValue");
```
{: codeblock}
-->

#### iOS - Swift
{: #ios-usage-api}

```
// Disable recording of usage analytics (for example, to save disk space)
// Recording is enabled by default
Analytics.isEnabled = false

// Enable recording of usage analytics
Analytics.isEnabled = true

// Send recorded usage analytics to the Mobile Analytics Service
Analytics.send(completionHandler: { (response: Response?, error: Error?) in

    if let response = response {
        // Handle Analytics send success here.
    }
    if let error = error {
        // Handle Analytics send failure here.
    }
})
```
{: codeblock}

<!--
Sample usage analytics for logging an event:

#### Swift
{: customchartsswift}

```Swift
// Log a custom analytics event for custom charts
let eventObject = ["customProperty": "propertyValue"]
Analytics.log(eventObject)
```
{: codeblock}

-->

  <!--Removing Cordova for experimental-->
  <!--### Cordova-->
  <!--{: #usage-analytics-cordova}-->

  <!--```JavaScript-->
  <!--// Enable usage analytics recording-->
  <!--Analytics.enable();-->

  <!--// Send recorded usage analytics to the {{site.data.keyword.mobileanalytics_short}} Service-->
  <!--Analytics.send();-->
  <!--```-->
  <!--**Note:** When you are developing Cordova applications, use the native API to enable application lifecycle event recording.-->
  
## 启用、配置和使用记录器
{: #app-monitoring-logger}

  {{site.data.keyword.mobileanalytics_full}} 客户端 SDK 提供了一种日志记录框架，与您可能熟悉的其他日志框架（如 `java.util.logging` 或 `log4j`）类似。此日志记录框架支持每个数据包多个记录器实例，支持不同的日志级别，支持捕获对应用程序崩溃的堆栈跟踪，等等。

  您还可以将记录的数据配置为存储在应用程序运行所在的设备上，并在以后将这些设备日志发送到 {{site.data.keyword.mobileanalytics_short}} 服务。

  <!-- Initialization has to happen first to be able to collect logs and send them to the {{site.data.keyword.mobileanalytics_short}} service. -->

  {{site.data.keyword.mobileanalytics_short}} 客户端 SDK 日志记录框架支持以下日志级别，按详细程度从低到高的顺序与其建议的使用准则一起列出：

  * `FATAL` - 用于不可恢复的崩溃或挂起。`FATAL` 级别保留用于记录不可恢复的错误，这些错误对于用户显示为应用程序崩溃
  * `ERROR` - 用于意外异常或意外网络协议错误
  * `WARN` - 用于记录不视为严重错误的使用情况警告，例如使用了不推荐的 API 或网络响应速度慢
  * `INFO` - 用于报告初始化事件以及其他可能重要但不紧急的数据
  * `DEBUG` - 用于报告调试语句，以帮助开发者解决应用程序缺陷

    #### 日志级别方案
    {: #log-level-scenario}

    当记录器级别配置为 `FATAL` 时，记录器会捕获未捕获的异常，但是不会捕获导致崩溃事件的任何日志。您可以设置更详细的记录器级别，以确保同时捕获可能导致 `FATAL` 记录器条目的日志，如 `WARN` 和 `ERROR`。

  <!--**Note:** Find full Logger API references for each platform at [SDKs, samples, API reference](sdks-samples-apis.html). The Logger API is part of the--> <!--{{site.data.keyword.mobileanalytics_short}} Client SDK Core.-->

  <!--### Cordova-->


  <!--```JavaScript-->

  <!--var logger = MFPLogger.getInstance("myLogger");-->

  <!--logger.debug("debug info");-->
  <!--logger.info("info message");-->
  <!--logger.warn("warning message");-->
  <!--logger.fatal("fatal message");-->

  <!--```-->


### 记录器使用情况示例
{: #sample-logger-usage}

**注**：在使用此日志记录框架之前，请确保已检测应用程序，可使用 {{site.data.keyword.mobileanalytics_short}} 客户端 SDK。
 
  以下代码片段显示了样本记录器用法：

#### Android
{: #android-logger-sample}

```
// 配置记录器以将日志保存到设备，这样稍后
// 就可以将其发送到 Mobile Analytics 服务
// 缺省情况下，处于禁用状态；设置为 true 可启用
Logger.storeLogs(true);

// 更改最低日志级别（可选）
// 缺省设置为 Logger.LEVEL.DEBUG
Logger.setLogLevel(Logger.LEVEL.INFO);

// 创建两个记录器实例
// 您可以创建多个日志实例，以组织您的日志
Logger logger1 = Logger.getLogger("logger1");
Logger logger2 = Logger.getLogger("logger2");

// 具有不同级别的日志消息
// 功能 1 的调试消息
// 功能 2 的信息消息
logger1.debug("debug message");
// 未记录 logger1.debug 消息，因为 logLevelFilter 设置为 Info
logger2.info("info message");
// Send logs to the Mobile Analytics Service
        Logger.send(new ResponseListener() {
                    @Override
                    public void onSuccess(Response response) {
                        // Handle Logger send success here.
                    }

                    @Override
                    public void onFailure(Response response, Throwable throwable, JSONObject jsonObject) {
                        // Handle Logger send failure here.
                    }
                });        
```
{: codeblock}

#### iOS - Swift
{: #ios-logger-sample-swift2}

```
// Configure Logger to save logs to the device so that they can later be sent to the Mobile Analytics service
// Disabled by default; set to true to enable
Logger.isLogStorageEnabled = true

// Change the minimum log level (optional)
// The default setting is LogLevel.debug
Logger.logLevelFilter = LogLevel.info

// Create two logger instances
// You can create multiple log instances to organize your logs
let logger1 = Logger.logger(name: "feature1Logger")
let logger2 = Logger.logger(name: "feature2Logger")

// 具有不同级别的日志消息
logger1.debug(message: "debug message for feature 1")
//the logger1.debug message is not logged because the logLevelFilter is set to info
logger2.info(message: "info message for feature 2")

// Send logs to the Mobile Analytics Service
Logger.send(completionHandler: { (response: Response?, error: Error?) in
    
    if let response = response {
        logger.debug(message: "Status code: \(response.statusCode)")
        logger.debug(message: "Response: \(response.responseText)")
    }
    if let error = error {
        logger.error(message: "Error: \(error)")
    }
})
```
{: codeblock}

**提示**：出于隐私考虑，您可以对在发布方式下构建的应用程序，禁用记录器输出。缺省情况下，记录器类会将日志打印到 Xcode 控制台。在目标的构建设置中，将 `-D RELEASE_BUILD` 标记添加到发布构建配置的**其他 Swift 标记**部分。
    

  <!-- ### Cordova-->
  <!--{: #enable-logger-sample-cordova}-->

  <!--// Enable persisting logs-->
  <!--MFPLogger.setCapture(true);-->

  <!--// Set the minimum log level to be printed and persisted-->
  <!--MFPLogger.setLevel(MFPLogger.INFO);-->

  <!--var logger1 = MFPLogger.getInstance("logger1");-->
  <!--var logger2 = MFPLogger.getInstance("logger2");   -->

  <!--// Log messages with different levels-->
  <!--logger1.debug ("debug message");-->
  <!--logger2.info ("info message");-->

  <!--// Send persisted logs to the {{site.data.keyword.mobileanalytics_short}} Service-->
  <!--```-->

<!--## Enabling the {{site.data.keyword.mobileanalytics_short}} Client SDK internal logs
{: #enable-logger-sdklogs}

  The {{site.data.keyword.mobileanalytics_short}} Client SDK provides a better development experience by not unnecessarily increasing the console output with its internal debug messages. Therefore, by default, internal log messages that are produced by the {{site.data.keyword.mobileanalytics_short}} SDK with the DEBUG level are not printed. You can enable printing of all internal log messages of the {{site.data.keyword.mobileanalytics_short}} Client SDK with the following API:

#### Android
{: #enable-logger-print-android}

```
{: codeblock}
Logger.setSDKDebugLoggingEnabled(true);
```
{: codeblock}

#### iOS - Swift
{: #enable-logger-print-swift}

```
Logger.sdkDebugLoggingEnabled = true
```
{: codeblock}
-->

## 报告崩溃分析
{: #report-crash-analytics}

您可以通过将分析和日志信息发送到 {{site.data.keyword.mobileanalytics_short}} 来查看[应用程序崩溃数据](app-monitoring.html#monitor-app-crash)。

`Analytics.send()` 方法会在**崩溃**页面填充**崩溃概述**和**崩溃**表。本部分的图表通过初始化和发送过程启用以供分析之用；无需任何特殊配置。

`Logger.send()` 方法会在**故障诊断**页面填充**崩溃摘要**和**崩溃详细信息**表。您必须使您的应用程序能够储存和发送日志，才能通过在应用程序代码中添加额外的语句，来填充本部分的图表。

#### Android
{: #android-crash-statement}

* `Logger.storeLogs(true);`
<!-- * `Logger.setLogLevel(Logger.LEVEL.FATAL); // or greater` -->

查看[样本记录器用法](sdk.html#android-logger-sample)。

#### iOS
{: #ios-crash-statement}

* `Logger.isLogStorageEnabled = true`
<!-- * `Logger.logLevelFilter = LogLevel.Fatal // or greater` -->

查看[样本记录器用法](sdk.html##ios-logger-sample-swift2)。

## 跟踪活动用户
{: #trackingusers}

如果您的应用程序可以识别设备上的非重复用户，那么通过将活动用户的用户名传递到 {{site.data.keyword.mobileanalytics_short}} ，您可以选择跟踪有多少用户正在积极使用您的应用程序。 

通过使用 `hasUserContext=true` 初始化 {{site.data.keyword.mobileanalytics_short}} 来启用用户跟踪。否则，{{site.data.keyword.mobileanalytics_short}} 仅会从每个设备捕获一个用户。 

#### Android
{: #android-tracking-users}

添加下列代码，以在用户登录时进行跟踪：

```
Analytics.setUserIdentity("username");
```
{: codeblock}

<!--Add the following code for when the user logs out:

```
Analytics.clearUserIdentity();
```
{: codeblock}
-->

#### iOS - Swift
{: #ios-tracking-users}

添加下列代码，以在用户登录时进行跟踪：

```
Analytics.userIdentity = "username"
```
{: codeblock}

<!--
Add the following code for when the user logs out:

```
Analytics.userIdentity = nil
```
{: codeblock}
-->

<!--## Configuring MobileFirst Platform Foundation servers to use the {{site.data.keyword.mobileanalytics_short}} service (optional)
{: #configmfp}
  If you are using MobileFirst Platform Foundation Server V7 and higher, you can configure it to use the {{site.data.keyword.Bluemix_notm}} {{site.data.keyword.mobileanalytics_short}} service to store analytics and logged data. 
  
  Configure MobileFirst Platform V7.0, V7.1, and V8.0 Beta servers to send analytics data to the {{site.data.keyword.mobileanalytics_short}} service on {{site.data.keyword.Bluemix_notm}}.

  1. Visit the dashboard for the {{site.data.keyword.mobileanalytics_short}} service where you want to send analytics data and note the browser URL for the dashboard.
  2. Determine the value for reporting analytics, as follows:
  	1. Get the {{site.data.keyword.mobileanalytics_short}} service route from the dashboard URL. The {{site.data.keyword.mobileanalytics_short}} service route is the part of the URL before ``/analytics/console/dashboard``.  

  		For example, if the dashboard URL is: `http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde`
      {: screen}

  		then the Analytics Service Route is
      `http://mobile-analytics-dashboard.ng.bluemix.net`
      {: screen}

  	2. Get the [Client API Key](#analytics-clientkey) value from the Analytics dashboard.

  	You will use the {{site.data.keyword.mobileanalytics_short}} service route and API Key to construct a URL to report analytics data, depending on the version of your MobileFirst Platform server. -->

<!--  3. Copy the URL for your {{site.data.keyword.mobileanalytics_short}} service dashboard. Include the `instanceId` query parameter, for example:
      `http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=1212345abcde`
      {: screen}

  4. Configure the values for the MobileFirst Platform server JNDI properties, as follows:-->

<!--    ### MobileFirst Platform V7.0
    {: #mfp70}

        The format of the `wl.analytics.url` JNDI property is: `<Analytics Service Route>/analytics-service/rest/data?tenant=<Client API Key>`
        {: screen}

        The `wl.analytics.console.url` JNDI property is the Analytics service dashboard URL.

        #### Example of MobileFirst Platform V7.0 JNDI property values
        {: #example-mfp70-jndi-values}

        For a MobileFirst Platform project named `Myproj7000`, based on the examples in steps 2 and 3, replace the current JNDI property values in the `server.xml` file with:
        ```
        <jndiEntry jndiName="MyProj7000/wl.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/data?tenant=12345abcde"'/>
        ```
        {: screen}
        ```
        <jndiEntry jndiName="MyProj7000/wl.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
        ```
        {: screen}

    ### MobileFirst Platform V7.1
    {: #mfp71}

      The format of the `wl.analytics.url` JNDI property is: `<Analytics Service Route>/analytics-service/rest/v2/<Client API Key>`
      {: screen}

      #### Example of MobileFirst Platform JNDI V7.1 property values
      {: #example-mfp71-jndi-values}

      For a MobileFirst Platform project named `MyProj7101`, replace the current JNDI property values in the `server.xml` file with:
      ```
      <jndiEntry jndiName="MyProj7101/wl.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/v2/data?tenant=12345abcde"'/>
      ```
      {: screen}
      ```
      <jndiEntry jndiName="MyProj7101/wl.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
      ```
      {: screen}

      ### MobileFirst Platform V8.0
      {: #mfp80}

      The format of the `mfp.analytics.url` JNDI property is:
      `<Analytics Service Route>/analytics-service/rest/v2/<Client API Key>`  

    #### Example MobileFirst Platform V8.0 Beta JNDI property values
      {: example-mfp80b-jndi-values}

      For a MobileFirst Platform project named `mfp`, which is the default, replace the current JNDI property values in the `server.xml` file with:
      ```
      <jndiEntry jndiName="mfp/mfp.analytics.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics-service/rest/data?tenant=12345abcde"'/>
      ```
      {: screen}
      ```
      <jndiEntry jndiName="mfp/mfp.analytics.console.url" value='"http://mobile-analytics-dashboard.ng.bluemix.net/analytics/console/dashboard?instanceId=12345abcde"'/>
      ```
      {: screen}
-->
<!-- #### Ignored events and event retention
{: #ignored-events}
The {{site.data.keyword.mobileanalytics_short}} service saves the following data for ignored events and event retention:
  
<dl>	
<dt>Ignored events</dt>
	<dd>`ANALYTICS_SDK_CFG_IGNORE_AppPushAction: true`
		  `ANALYTICS_SDK_CFG_IGNORE_ServerLog: true`
		  `ANALYTICS_SDK_CFG_IGNORE_NetworkTransaction: true`
		  `ANALYTICS_SDK_CFG_IGNORE_NetworkTransactionSummarizedHourly: true`
		  `ANALYTICS_SDK_CFG_IGNORE_PushNotification: true`
		  `ANALYTICS_SDK_CFG_IGNORE_PushSubscription: true`</dd>
</dt>
<dt>Event retention</dd>
<dd>
`ANALYTICS_TTL_AlertNotification: 14d`<br>
`ANALYTICS_TTL_CustomData: 7d`<br>
`ANALYTICS_TTL_Device: 90d`<br>
`ANALYTICS_TTL_AppLog: 3d`<br>
`ANALYTICS_TTL_AppSession: 7d`
</dd>
</dt>
</dl>
  
-->

## 后续操作
{: #what-to-do-next}

您现在可以转到 {{site.data.keyword.mobileanalytics_short}} **Console**，以查看使用情况分析，如使用应用程序的新设备和设备总数。您还可以通过<!--[creating custom charts](app-monitoring.html#custom-charts),-->[设置警报](app-monitoring.html#alerts)和[监视应用程序崩溃](app-monitoring.html#monitor-app-crash)来监视应用程序。

# 相关链接

## API 参考
{: #api}
* [REST API](https://mobile-analytics-dashboard.{DomainName}/analytics-service/){:new_window}
