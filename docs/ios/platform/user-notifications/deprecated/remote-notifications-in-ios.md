---
title: "Nabízená oznámení v iOS"
description: "Tato část se bude zabývat nabízená oznámení v iOS. Zavádí službu Apple Push brány oznámení a roli, které splňuje při publikování oznámení pro aplikace iOS. Se vysvětluje, jak vytvořit potřeba povolte nabízená oznámení a popisují certifikáty zabezpečení. Nakonec bude v této části popisují některé housekeeping úlohy, které můžete sledovat mobilní zařízení klientů musí provést aplikační servery."
ms.topic: article
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: 8e90bc3974247066a714cb44b6648a83cdb58cf5
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="push-notifications-in-ios"></a>Nabízená oznámení v iOS

_Tato část se bude zabývat nabízená oznámení v iOS. Zavádí službu Apple Push brány oznámení a roli, které splňuje při publikování oznámení pro aplikace iOS. Se vysvětluje, jak vytvořit potřeba povolte nabízená oznámení a popisují certifikáty zabezpečení. Nakonec bude v této části popisují některé housekeeping úlohy, které můžete sledovat mobilní zařízení klientů musí provést aplikační servery._

> [!IMPORTANT]
> **Poznámka:** informace v této části se vztahují na iOS 9 a předchozí, ho byla ponechána zde k podpoře starší verze iOS. IOS 10 a novější, najdete v tématu [uživatele oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní i vzdálené oznámení na zařízení s iOS.

Nabízená oznámení by měly být udržovány stručný a obsahovat pouze dostatek dat oznámení mobilní aplikace, jestli ho měli kontaktovat serverová aplikace pro aktualizaci. Například při doručení nových e-mailů, je serverová aplikace by upozornit pouze mobilních aplikací, který byl doručen nových e-mailů. Oznámení nebude obsahovat nových e-mailů sám sebe. Mobilní aplikace by pak nové e-maily ze serveru načíst, že je vhodné

V centru nabízených oznámení v iOS se *nabízených oznámení Apple brány Service (APNS)*. To je služba poskytovaných společností Apple, která je odpovědná za směrování oznámení z aplikační server na zařízení s iOS.
Následující obrázek ilustruje topologii nabízená oznámení iOS: ![ ] (remote-notifications-in-ios-images/image4.png "tento obrázek ilustruje topologii nabízená oznámení iOS")

Vzdálená oznámení sami jsou formátu řetězce, které splňovat do formátu JSON a protokoly v [The datová část oznámení](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) části [nabízená oznámení průvodci programováním místních a](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)v [dokumentaci pro vývojáře iOS](https://developer.apple.com/devcenter/ios/index.action).

Apple udržuje dvě prostředí služby APN: *izolovaného prostoru* a *produkční* prostředí. Prostředí izolovaného prostoru jsou určené výhradně pro testování v průběhu fáze vývoje a najdete na `gateway.sandbox.push.apple.com` na TCP portu 2195. V provozním prostředí je určen pro použití v aplikacích, které byly nasazeny a najdete na `gateway.push.apple.com` na TCP portu 2195.

## <a name="requirements"></a>Požadavky

Nabízená oznámení musí odpovídat následujícím pravidlům, které vyplývají architektuře služby APN:

-  **256 bajtů Limit zprávy** – velikost celé zprávy oznámení nesmí překročit 256 bajtů.
-  **Žádné potvrzení přijetí** -APNS neposkytuje odesílatele se všechna oznámení, která zprávu dostal zamýšlený příjemce. Pokud zařízení nedostupné a více po sobě jdoucích oznámení se odesílají, všechna oznámení s výjimkou nejnovější se ztratí. Jenom nejnovější oznámení bude doručen do zařízení.
-  **Každá aplikace vyžaduje certifikát zabezpečení** -komunikace se službou APNS je třeba provést přes protokol SSL.


## <a name="creating-and-using-certificates"></a>Vytváření a používání certifikátů

Každá z prostředí uvedených v předchozí části vyžadovat vlastní certifikát. Tato část popisuje postup vytvoření certifikátu, jeho přidružení k profilu pro zřizování a potom získat certifikát Personal Information Exchange pro použití s PushSharp.

1.  Chcete-li vytvořit certifikáty přejít na iOS Provisioning Portal na webu společnosti Apple, jak je znázorněno na následujícím snímku obrazovky (upozornění ID aplikace položky nabídky na levé straně):

    [![](remote-notifications-in-ios-images/image5new.png "IOS Provisioning Portal na webu jablka")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Dále přejděte do části ID aplikace a vytvořte nové ID aplikace, jak je znázorněno na následujícím snímku obrazovky:

    [![](remote-notifications-in-ios-images/image6new.png "Přejděte do části ID aplikace a vytvoření nového ID aplikace")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Po kliknutí na  **+**  tlačítko, nebudete moci zadat popis a identifikátor svazku pro dané ID aplikace, jak ukazuje následující snímek obrazovky:

    [![](remote-notifications-in-ios-images/image7new.png "Zadejte popis a identifikátor svazku pro ID aplikace")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Je nutné vybrat **explicitní ID aplikace** a zda není končí identifikátor svazku `*` . Tím se vytvoří identifikátor, který je vhodný pro více aplikací a nabízených oznámení certifikáty musí být pro jednu aplikaci.

1. V části aplikační služby, vyberte **nabízená oznámení**:

    [![](remote-notifications-in-ios-images/image8new.png "Vyberte nabízená oznámení")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. A stiskněte klávesu **odeslání** k potvrzení registrace nové ID aplikace:

    [![](remote-notifications-in-ios-images/image9new.png "Potvrzení registrace nové ID aplikace")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Dále je třeba vytvořit certifikát pro ID aplikace. V levém navigačním panelu, přejděte do **certifikáty > všechny** a vyberte `+` tlačítko, jak je znázorněno na následujícím snímku obrazovky:

    [![](remote-notifications-in-ios-images/image10new.png "Vytvořit certifikát pro ID aplikace")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Vyberte, zda chcete použít certifikát pro vývoj nebo produkčního prostředí:

    [![](remote-notifications-in-ios-images/image11new.png "Vyberte certifikát, vývoj nebo produkčního prostředí")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. A pak vyberte nové ID aplikace, kterou jsme právě vytvořili:

    [![](remote-notifications-in-ios-images/image12new.png "Vyberte nové ID aplikace právě vytvořili")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Tato akce zobrazí pokyny, které vás provede procesem vytvoření *žádost o podepsání certifikátu* pomocí **přístup do řetězce klíčů** aplikace na vašem Mac.

7.  Teď, když certifikát byl vytvořen, musíte ho použít jako součást procesu sestavení k podepsání aplikace tak, aby se může zaregistrovat pomocí služby APNs. To vyžaduje vytvoření a instalace profil zřizování, která certifikát používá.

8.  Pokud chcete vytvořit profil pro zřizování vývoj, přejděte na **profily zřizování** části a postupujte podle kroků pro vytvoření, pomocí Id aplikace, který jsme právě vytvořili.

9.  Po vytvoření profilu pro zřizování, otevře **Xcode organizátora** a jej aktualizovat. Když vytvoříte profil zřizování nezobrazí, že může být potřeba stáhnout profil z iOS Provisioning Portal a ji manuálně naimportovat. Následující snímek obrazovky ukazuje příklad Organizátoru s profilem zřídit přidán:

    [![](remote-notifications-in-ios-images/image13new.png "Tento snímek obrazovky ukazuje příklad Organizátoru s profilem zřídit přidán")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  V tomto okamžiku je potřeba nakonfigurovat Xamarin.iOS projekt, který používá to nově vytvořený profil pro zřizování. To se provádí v **možnosti projektu** dialogové okno, v části **iOS podepisování sady** karta jako zobrazující na následujícím snímku obrazovky:

    [![](remote-notifications-in-ios-images/image11.png "Konfigurace projektu Xamarin.iOS využít nově vytvořený profil pro zřizování")](remote-notifications-in-ios-images/image11.png#lightbox)



Aplikace je nyní nakonfigurováno pro práci s nabízenými oznámeními. Existují však stále několik další kroky potřebné k certifikátu. Tento certifikát je ve formátu DER, který není kompatibilní s PushSharp, který vyžaduje certifikát Personal Information Exchange (PKCS12). Chcete-li převést certifikát tak, aby byla použitelná pro PushSharp, proveďte tyto kroky poslední:

1.  **Stáhněte si soubor certifikátu** -přihlášení do systému iOS Provisioning Portal zvolili kartu certifikáty, vyberte certifikát přidružený k správné zřizovací profil a pokusit **Stáhnout** .
1.  **Otevřete přístup do řetězce klíčů** -Toto je aplikace je grafického uživatelského rozhraní pro systém správy heslo OS X.
1.  **Import certifikátu** – Pokud certifikovaných ještě není nainstalovaný, **souboru... Import položek** z nabídky přístup do řetězce klíčů. Přejděte na certifikát, který exportovat výše a vyberte jej.
1.  **Export certifikátu** – rozbalte certifikát s přidruženým privátním klíčem je viditelná, klikněte pravým tlačítkem na klíč a zvolili Export. Zobrazí se výzva pro název souboru a heslo pro exportovaný soubor.

V tuto chvíli jsme se provádějí pomocí certifikátů. Jsme vytvořili certifikát, který se použije k podepisování aplikací pro iOS a převést tento certifikát do formátu, který lze použít s PushSharp v serverové aplikaci. Další podíváme, jak aplikace pro iOS komunikovat s služby APN.

## <a name="registering-with-apns"></a>Registrace služby APN

Před iOS aplikace může přijímat vzdáleného oznámení, která musíte zaregistrovat u služby APN. Služby APN se vygenerování tokenu jedinečný zařízení a vrátit, do aplikace systému iOS. Aplikace systému iOS se pak proveďte token zařízení a potom registraci se serverem aplikace. Poté tento všechny registrace je dokončena a aplikační server může nabízená oznámení na mobilní zařízení.

Teoreticky token zařízení může změnit pokaždé, když aplikace pro iOS registruje služby APN, ale v praxi se tato situace, často. Aplikace může jako optimalizace nejnovější token zařízení do mezipaměti a při změně aktualizovat pouze aplikační server. Následující diagram znázorňuje proces registrace a získat token zařízení:

 ![](remote-notifications-in-ios-images/image12.png "Tento diagram znázorňuje proces registrace a získat token zařízení")

Registrace pomocí služby APNS je zpracována v `FinishedLaunching` metoda delegát třídy aplikace voláním `RegisterForRemoteNotificationTypes` na aktuální `UIApplication` objektu. Když aplikace pro iOS zaregistruje u služby APN, je také třeba určit, jaké typy vzdáleného oznámení, ho chcete dostávat. Tyto typy vzdáleného oznámení jsou deklarované v výčtu `UIRemoteNotificationType`. Následující fragment kódu je příkladem jak aplikace pro iOS můžete zaregistrovat pro příjem oznámení vzdálené výstrahy a oznámení "BADGE":

```csharp
if (UIDevice.CurrentDevice.CheckSystemVersion (8, 0)) {
    var pushSettings = UIUserNotificationSettings.GetSettingsForTypes (
                       UIUserNotificationType.Alert | UIUserNotificationType.Badge | UIUserNotificationType.Sound,
                       new NSSet ());

    UIApplication.SharedApplication.RegisterUserNotificationSettings (pushSettings);
    UIApplication.SharedApplication.RegisterForRemoteNotifications ();
} else {
    UIRemoteNotificationType notificationTypes = UIRemoteNotificationType.Alert | UIRemoteNotificationType.Badge | UIRemoteNotificationType.Sound;
    UIApplication.SharedApplication.RegisterForRemoteNotificationTypes (notificationTypes);
}
```

Žádost o registraci služby APN se odehrává na pozadí - při obdržení odpovědi iOS bude volat metodu `RegisteredForRemoteNotifications` v `AppDelegate` třídy a předejte token registrovaných zařízení. Bude obsahovat token `NSData` objektu. Následující fragment kódu ukazuje, jak získat token zařízení této služby APN poskytuje:

```csharp
public override void RegisteredForRemoteNotifications (
UIApplication application, NSData deviceToken)
{
    // Get current device token
    var DeviceToken = deviceToken.Description;
    if (!string.IsNullOrWhiteSpace(DeviceToken)) {
        DeviceToken = DeviceToken.Trim('<').Trim('>');
    }

    // Get previous device token
    var oldDeviceToken = NSUserDefaults.StandardUserDefaults.StringForKey("PushDeviceToken");

    // Has the token changed?
    if (string.IsNullOrEmpty(oldDeviceToken) || !oldDeviceToken.Equals(DeviceToken))
    {
        //TODO: Put your own logic here to notify your server that the device token has changed/been created!
    }

    // Save new device token
    NSUserDefaults.StandardUserDefaults.SetString(DeviceToken, "PushDeviceToken");
}
```

Pokud se registrace nezdaří z nějakého důvodu (například zařízení není připojen k Internetu), iOS bude volat `FailedToRegisterForRemoteNotifications` na aplikaci delegovat třídy. Následující fragment kódu ukazuje, jak zobrazit výstrahu o uživateli informací, které se nezdařily, registrace:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Token Housekeeping zařízení

Tokeny zařízení bude vypršení platnosti nebo časem změnit. Z tohoto důvodu serverové aplikace muset provést některé úklidové čištění a mazání tyto tokeny vypršela platnost, nebo změněné. Pokud aplikace odešle jako nabízených oznámení do zařízení, které má tokenu vypršela platnost, budou služby APN zaznamenejte a uložte tento vypršela platnost tokenu. Servery může potom budete dotazovat služby APN a zjistěte, jaké tokeny vypršela.

APNS použitý k poskytnutí *zpětné vazby služby* -koncový bod HTTPS, který ověřuje prostřednictvím certifikát, který byl vytvořen k odesílání nabízených oznámení a odesílá zpět dat o jaké tokeny mají platnost. Toto je zastaralé společností Apple a odebrat.

Místo toho je nové stavový kód HTTP pro případ, která byla dříve hlášena zpětné vazby služby:

> 410 - token zařízení už není aktivní pro téma.

Kromě toho nové `timestamp` klíč data JSON bude v textu odpovědi:

> Pokud je hodnota v: Stav záhlaví je 410, hodnota tohoto klíče je poslední čas, kdy APNs potvrzeno, že token zařízení již nebude platný pro téma.
>
> Zastavte odesílání nabízených oznámení, dokud se zařízení zaregistruje token s novější časové razítko u svého poskytovatele.

## <a name="summary"></a>Souhrn

Tato část zavést klíčové koncepty, které obaluje nabízená oznámení v iOS. Je vysvětlené roli z APNS Apple Push Notification brány služby (). Potom zahrnutých, vytváření a použití certifikátů zabezpečení, které jsou nezbytné pro služby APN. Nakonec tento dokument se dokončila a informace o tom, jak můžete použít aplikační servery *zpětné vazby služby* zastavit sledování platnost tokenů zařízení.


## <a name="related-links"></a>Související odkazy

- [Oznámení – ukázka místních a vzdálených oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Místní a nabízená oznámení pro vývojáře](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
