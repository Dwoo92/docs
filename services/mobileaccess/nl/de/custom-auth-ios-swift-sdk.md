---

copyright:
  years: 2016
lastupdated: "2016-10-09"
---

# Angepasste Authentifizierung für {{site.data.keyword.amashort}}-iOS-App konfigurieren (Swift-SDK)
{: #custom-ios}

Konfigurieren Sie Ihre iOS-Anwendung, die mit der angepassten Authentifizierung arbeitet, zur Verwendung des {{site.data.keyword.amafull}}-Client-SDK und verbinden Sie Ihre Anwendung mit {{site.data.keyword.Bluemix}}.  Das neu freigegebene {{site.data.keyword.amashort}}-Swift-SDK ergänzt und verbessert die vom vorhandenen Mobile Client Access-Objective-C-SDK bereitgestellte Funktionalität.

**Hinweis:** Das Objective-C-SDK wird zwar weiterhin vollständig unterstützt und gilt noch als primäres SDK für {{site.data.keyword.Bluemix_notm}} Mobile Services, seine Verwendung und Unterstützung sollen jedoch zugunsten des neuen Swift-SDK noch dieses Jahr eingestellt werden.

## Vorbereitungen
{: #before-you-begin}

Sie müssen über eine Ressource verfügen, die durch eine Instanz des {{site.data.keyword.amashort}}-Service geschützt wird, die zur Verwendung eines angepassten Identitätsproviders konfiguriert ist.  Ihre mobile App muss außerdem mit dem {{site.data.keyword.amashort}}-Client-SDK instrumentiert sein.  Weitere Informationen finden Sie über die folgenden Links:
 * [Einführung in {{site.data.keyword.amashort}}](https://console.{DomainName}/docs/services/mobileaccess/index.html)
 * [iOS-Swift-SDK einrichten](https://console.{DomainName}/docs/services/mobileaccess/getting-started-ios-swift-sdk.html)
 * [Angepassten Identitätsprovider verwenden](https://console.{DomainName}/docs/services/mobileaccess/custom-auth.html)
 * [Angepassten Identitätsprovider erstellen](https://console.{DomainName}/docs/services/mobileaccess/custom-auth-identity-provider.html)
 * [{{site.data.keyword.amashort}} für die angepasste Authentifizierung konfigurieren](https://console.{DomainName}/docs/services/mobileaccess/custom-auth-config-mca.html)


## {{site.data.keyword.amashort}} für eine angepasste Authentifizierung konfigurieren
 {: #custom-auth-ios-configmca}

 1. Öffnen Sie Ihr Service-Dashboard.
 
 1. Klicken Sie auf **Mobile Systemerweiterungen** und notieren Sie die Werte für **Route** (*applicationRoute*) und **App-GUID/TenantId** (*serviceTenantID*). Sie benötigen diese Werte, wenn Sie das SDK initialisieren und Anforderungen an die Back-End-Anwendung senden.

 1. Klicken Sie auf die Kachel für {{site.data.keyword.amashort}}. Das {{site.data.keyword.amashort}}-Dashboard wird geladen.

 1. Klicken Sie auf die Kachel **Angepasst**.

 1. Geben Sie in **Realmname** Ihren angepassten Authentifizierungrealm an.

 1. Geben Sie in **URL** Ihre Anwendungsroute (applicationRoute) an.

 1. Klicken Sie auf **Speichern**.




### Client-SDK initialisieren
{: #custom-ios-sdk-initialize}

Initialisieren Sie das SDK, indem Sie den Parameter `applicationGUID` (tenantId) übergeben. Eine gängige, wenngleich nicht verbindliche, Position für den Initialisierungscode ist die Methode `application:didFinishLaunchingWithOptions` Ihres Anwendungsdelegats.

1. Importieren Sie die erforderlichen Frameworks in die Klasse, in der Sie das {{site.data.keyword.amashort}}-Client-SDK verwenden möchten.

	```Swift
	import UIKit
	import BMSCore
	import BMSSecurity
	```

1. Initialisieren Sie das {{site.data.keyword.amashort}}-Client-SDK, geben Sie `MCAAuthorizationManager` als Berechtigungsmanager an und definieren und registrieren Sie ein Authentifizierungsdelegat.

```Swift

	let tenantId = "<serviceTenantID>"
	let regionName = <applicationBluemixRegion>
	let customRealm = "<yourProtectedRealm>"

	func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: 
		[UIApplicationLaunchOptionsKey: Any]?) -> Bool {

		let mcaAuthManager = MCAAuthorizationManager.sharedInstance
	    		mcaAuthManager.initialize(tenantId: tenantId, bluemixRegion: regionName)
	 // Der Regionsname sollte eine der folgenden Konstanten für BMSClient.Region sein: BMSClient.Region.usSouth, BMSClient.Region.unitedKingdom oder BMSClient.Region.sydney
		BMSClient.sharedInstance.authorizationManager = mcaAuthManager

	//Authentifizierungsdelegat zum Verarbeiten der angepassten Anforderung
 class MyAuthDelegate : AuthenticationDelegate {
		func onAuthenticationChallengeReceived(_ authContext: AuthenticationContext, challenge: AnyObject){
		    print("onAuthenticationChallengeReceived")
		    let answer = try? Utils.parseJsonStringtoDictionary( "{\"userName\":\"" + "test" + "\",\"password\":\"" + "test" + "\"}")
			authContext.submitAuthenticationChallengeAnswer(answer)
		}

		func onAuthenticationSuccess(_ info: AnyObject?) {
		    print("onAuthenticationSuccess info = \(info)")
           print("onAuthenticationSuccess")
      }

		func onAuthenticationFailure(_ info: AnyObject?){
		     print("onAuthenticationFailure info = \(info)")
        print("onAuthenticationFailure")
      }
	     }

	     let delegate = MyAuthDelegate()
	     mcaAuthManager.registerAuthenticationDelegate(delegate, realm: customRealm)

	     return true
 }


```

Gehen Sie im Code wie folgt vor:

* Ersetzen Sie `<applicationBluemixRegion>` durch die Region, in der Ihre {{site.data.keyword.Bluemix_notm}}-Anwendung per Hosting bereitgestellt wird. Klicken Sie zur Anzeige der {{site.data.keyword.Bluemix_notm}}-Region auf das Symbol 'Avatar' ![Avatarsymbol](images/face.jpg "Avatarsymbol") in der Menüleiste, um das Widget **Konto und Unterstützung** zu öffnen.  Der Regionswert, der angezeigt wird, sollte einer der folgenden sein: **USA (Süden)**, **Vereinigtes Königreich** oder **Sydney**. Zudem sollte er den Konstanten entsprechen, die im Code erforderlich sind: `BMSClient.Region.usSouth`, `BMSClient.Region.unitedKingdom` oder `BMSClient.Region.sydney`.
* Ersetzen Sie `"<yourProtectedRealm>"` durch den Wert für **Realmname**, den Sie in der Kachel **Angepasst** des {{site.data.keyword.amashort}}-Dashboards definiert haben. 
* Ersetzen Sie `"<serviceTenantID>"` durch den Wert für **tenantId**, den Sie aus **Mobile Systemerweiterungen** abgerufen haben. Siehe [Mobile Client Access für angepasste Authentifizierung konfigurieren](#custom-auth-ios-configmca).

### Client-SDK initialisieren
{: #custom-ios-sdk-initialize}
   
  
## Authentifizierung testen
{: #custom-ios-testing}

Nachdem Sie das Client-SDK initialisiert und ein angepasstes Authentifizierungsdelegat registriert haben, können Sie mit dem Senden von Anforderungen an Ihre mobile Back-End-Anwendung beginnen.

### Vorbereitungen
{: #custom-ios-testing-before}

 Sie müssen eine Anwendung, die mit der {{site.data.keyword.mobilefirstbp}}-Boilerplate erstellt wurde, sowie eine Ressource, die durch {{site.data.keyword.amashort}} am Endpunkt `/protected` geschützt wird, haben.

1. Senden Sie in Ihrem Browser eine Anforderung an den geschützten Endpunkt Ihrer mobilen Back-End-Anwendung, indem Sie `{applicationRoute}/protected` öffnen. Ersetzen Sie `{applicationRoute}` durch den Wert, den Sie aus **Mobile Systemerweiterungen** abgerufen haben (siehe [Mobile Client Access für angepasste Authentifizierung konfigurieren](#custom-auth-ios-configmca)). Beispiel: `http://my-mobile-backend.mybluemix.net/protected`.

 Der Endpunkt `/protected` einer mobilen Back-End-Anwendung, die mit der {{site.data.keyword.mobilefirstbp}}-Boilerplate erstellt wurde, wird mit {{site.data.keyword.amashort}} geschützt. Auf den Endpunkt können nur mobile Anwendungen zugreifen, die mit dem {{site.data.keyword.amashort}}-Client-SDK instrumentiert sind. Daher wird eine Nachricht `Unauthorized` (Nicht autorisiert) in Ihrem Browser angezeigt.

1. Verwenden Sie Ihre iOS-Anwendung, um eine Anforderung an denselben Endpunkt zu senden. Fügen Sie den folgenden Code hinzu, nachdem Sie `BMSClient` initialisiert und Ihr angepasstes Authentifizierungsdelegat registriert haben:

	```Swift

	let protectedResourceURL = "<your protected resource absolute path>"
	let request = Request(url: protectedResourceURL, method: HttpMethod.GET)

	let callBack:BMSCompletionHandler = {(response: Response?, error: Error?) in
  if error == nil {
	       print ("response:\(response?.responseText), no error")
  } else {
	       print ("error: \(error)")
  }
	}

	request.send(completionHandler: callBack)
	 ```

1. Wenn Ihre Anforderung erfolgreich ist, wird die folgende Ausgabe in der Xcode-Konsole angezeigt:

	 ```
	 onAuthenticationSuccess info = Optional({
     attributes =     {
	     };
     deviceId = don;
     displayName = "Don+Lon";
     isUserAuthenticated = 1;
     userId = don;
 })
	 response:Optional("Hello Don Lon"), no error
	 ```

1. Durch Hinzufügen des folgenden Codes können Sie auch die Abmeldefunktion (logout) hinzufügen:

	 ```
	 MCAAuthorizationManager.sharedInstance.logout(callBack)
	 ```  

 Wenn Sie diesen Code aufrufen, nachdem sich ein Benutzer angemeldet hat, wird der Benutzer abgemeldet. Wenn der Benutzer versucht, sich wieder anzumelden, muss er auf die vom Server empfangene Anforderung erneut reagieren.

 Die Übergabe von `callBack` an die Abmeldefunktion ist optional. Sie können auch `nil` übergeben.
