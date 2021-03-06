---

copyright:
 years: 2015, 2016

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen:.screen}
{:codeblock:.codeblock}

# {{site.data.keyword.mobilepushshort}} 概説
{: #gettingstartedtemplate}
最終更新日: 2016 年 10 月 17 日
{: .last-updated}

{:shortdesc}

{{site.data.keyword.mobilepushshort}} サービスは、iOS と Android のモバイル・プラットフォーム、Google Chrome と Mozilla Firefox の Web ブラウザー、および Google Chrome アプリケーションおよびエクステンションをターゲットとする、モバイルと Web のプッシュ通知を送信および管理するための統一プラットフォームを提供します。{{site.data.keyword.mobilepushshort}}サービスは、デバイス、デバイス・プラットフォーム、および Web ブラウザーへのアプリケーション・ユーザーのマッピングを管理し、それらへのプッシュ通知のディスパッチを処理します。このサービスを使用して、モバイル・アプリケーション・ユーザーと Web ブラウザー・アプリケーション・ユーザーにブロードキャスト、ユニキャスト (deviceID に基づく) に加え、タグ (またはトピック) をプッシュ通知として送信できます。また、クライアント・アプリケーションをさらに開発するために SDK および [REST API](https://mobile.{DomainName}/imfpush/) を使用することもできます。

このセクションでは、基本のプッシュ通知のセットアップ方法を説明します。基本通知を使用すると、通知は、タグを使用して特定のユーザー集合に届くのではなく、ブロードキャストされます。


1. [通知プロバイダーの資格情報の構成](t__main_push_config_provider.html)を行います。
2. [モバイル・アプリによる通知受け取りの可能化](c_enable_push.html)を行います。
3. [基本通知の送信](t_send_push_notifications.html)を行います。

# 関連リンク
{: #rellinks}

* [概要](c_overview_push.md){: new_window}

## チュートリアルおよびサンプル{:id="samples"}
{: #samples}
* [Android helloPush サンプル・アプリケーション](https://github.com/ibm-bluemix-mobile-services/bms-samples-android-hellopush/){: new_window}
- [Cordova サンプル・アプリケーション](https://github.com/ibm-bluemix-mobile-services/bms-samples-cordova-hellopush){: new_window}
* [iOS helloPush サンプル・アプリケーション (Obj-C)](https://github.com/ibm-bluemix-mobile-services/bms-samples-ios-hellopush/){: new_window}
* [iOS helloPush サンプル・アプリケーション (Swift)](https://github.com/ibm-bluemix-mobile-services/bms-samples-swift-hellopush){: new_window}

## SDK
{: #sdk}
* [Android SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-android-push){: new_window}
* [Cordova SDK](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-cordova-plugin-push){: new_window}
* [iOS SDK (Obj-C)](https://hub.jazz.net/git/bluemixmobilesdk/imf-ios-sdk/archive?revstr=master){: new_window}
* [iOS SDK (Swift)](https://codeload.github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/zip/master){: new_window}

## API リファレンス
{: #api}
* [Push API リファレンス (Android)](https://classicdocs.ng.bluemix.net/docs/api/content/api/mobilefirst/android/push-api-doc/overview-summary.html){: new_window}
* [IMFPush API リファレンス iOS](https://classicdocs.ng.bluemix.net/docs/api/content/api/mobilefirst/ios/IMFPush_api-doc/html/index.html){: new_window}
* [BMSPush API リファレンス iOS (Swift)](https://github.com/ibm-bluemix-mobile-services/bms-clientsdk-swift-push/blob/development/Apple Docs/index.html){: new_window}
* [REST API リファレンス](https://mobile.{DomainName}/imfpush/){: new_window}
