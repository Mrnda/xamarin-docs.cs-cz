---
title: Nabízená oznámení v iOS
description: Tento dokument popisuje, jak pracovat s nabízenými oznámeními v systému iOS 9 a starší. Tento článek popisuje certifikáty, zaregistrovali Apple Push oznámení brány Service (APNS) a další.
ms.prod: xamarin
ms.assetid: 64B3BE6A-A3E2-4B1B-95ED-02D27A8FDAAC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f11f5d1cbde0f5eae27215af8eb6544be46c0206
ms.sourcegitcommit: ef04a4ae1b19c1854a8e4e8315516d4030f4bbd6
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/08/2018
ms.locfileid: "39654812"
---
# <a name="push-notifications-in-ios"></a>Nabízená oznámení v iOS

> [!IMPORTANT]
> Informace v této části se vztahují na iOS 9 a předchozí, to byla ponechána zde pro podporu staršími verzemi Iosu. IOS 10 a novější, najdete v tématu [uživatel oznámení Framework – průvodce](~/ios/platform/user-notifications/index.md) pro podporu místní a Vzdálená oznámení na zařízení s Iosem.

Nabízená oznámení se uchovávají (BRIEF) a obsahovat pouze dostatek dat k mobilní aplikaci oznámilo, že obraťte serverová aplikace pro aktualizaci. Například při přijetí nového e-mailu, serverová aplikace by jenom mobilní aplikaci oznámilo, že dorazila nového e-mailu. Oznámení by obsahovat nový e-mail, samotného. Mobilní aplikace by pak načte nových e-mailů ze serveru, že je vhodné

V centru nabízených oznámení v iOS je *Apple Push Notification brány Service (APNS)*. Jedná se o službu poskytovaných společností Apple, která zodpovídá za směrování oznámení z aplikační server na zařízení s Iosem.
Následující obrázek ilustruje topologii nabízená oznámení iOS: ![](remote-notifications-in-ios-images/image4.png "tento obrázek ilustruje topologii nabízená oznámení iOS")

Vzdálená oznámení samotných jsou řetězce, které dodržují formát ve formátu JSON a protokoly zadané v [The datová část oznámení](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/CreatingtheNotificationPayload.html#//apple_ref/doc/uid/TP40008194-CH10-SW1) část [nabízená oznámení průvodci programováním místních a](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)v [dokumentaci pro vývojáře iOS](https://developer.apple.com/devcenter/ios/index.action).

Apple udržuje dvě prostředí APNS: *izolovaného prostoru* a *produkční* prostředí. Prostředí izolovaného prostoru je určená pro testování během fáze vývoje a lze nalézt v `gateway.sandbox.push.apple.com` na portu TCP 2195. Produkčním prostředí je určen pro použití v aplikacích, které jsou nasazené a lze nalézt v `gateway.push.apple.com` na portu TCP 2195.

## <a name="requirements"></a>Požadavky

Nabízené oznámení, musí odpovídat následujícím pravidlům, které vyplývají z architektury služby APN:

-  **256 bajtů Limit zprávy** – velikost celé zprávy oznámení nesmí překročit 256 bajtů.
-  **Žádné potvrzení o příjmu** – APNS neposkytuje odesílatele se všechna oznámení, která zprávu dostal zamýšlený příjemce. Pokud zařízení nedostupné a více sekvenčních oznámení se posílají, se ztratí všechna oznámení s výjimkou nejnovější. Jenom nejnovější oznámení bude doručen do zařízení.
-  **Každá aplikace vyžaduje zabezpečené certifikát** – komunikace se službou APNS je třeba provést přes protokol SSL.


## <a name="creating-and-using-certificates"></a>Vytváření a používání certifikátů

Každá z uvedených v předchozí části prostředí vyžadovat vlastní certifikát. Tato část se zabývá vytvořit certifikát přidružit zřizovací profil a potom získá certifikát Personal Information Exchange pro použití s PushSharp.

1.  Chcete-li vytvořit certifikáty přejít na iOS Provisioning Portal na webu společnosti Apple, jak je znázorněno na následujícím snímku obrazovky (Všimněte si, že aplikace ID položky nabídky na levé straně):

    [![](remote-notifications-in-ios-images/image5new.png "Portál zřizování na webu jablka iOS")](remote-notifications-in-ios-images/image5new.png#lightbox)

2.  Potom přejděte do části ID aplikace a vytvořte nové ID aplikace, jak je znázorněno na následujícím snímku obrazovky:

    [![](remote-notifications-in-ios-images/image6new.png "Přejděte do části ID aplikace a vytvořte nové ID aplikace")](remote-notifications-in-ios-images/image6new.png#lightbox)

3.  Po kliknutí na **+** tlačítko, bude možné zadat popis a identifikátoru sady prostředků pro ID aplikace, jak je znázorněno snímku obrazovky:

    [![](remote-notifications-in-ios-images/image7new.png "Zadejte popis a identifikátoru sady prostředků pro ID aplikace")](remote-notifications-in-ios-images/image7new.png#lightbox)

4. Je nutné vybrat **explicitní ID aplikace** a že identifikátor sady prostředků nekončí `*` . Tím se vytvoří identifikátor, který je vhodný pro několik aplikací a nabízených oznámení certifikáty musí být pro jednu aplikaci.

1. V části App Services vyberte **nabízená oznámení**:

    [![](remote-notifications-in-ios-images/image8new.png "Vyberte nabízená oznámení")](remote-notifications-in-ios-images/image8new.png#lightbox)

2. A stiskněte klávesu **odeslat** potvrďte registraci nového ID aplikace:

    [![](remote-notifications-in-ios-images/image9new.png "Potvrdit registraci nového ID aplikace")](remote-notifications-in-ios-images/image9new.png#lightbox)

3.  Dále musíte vytvořit certifikát pro ID aplikace. V levém navigačním panelu vyhledejte **certifikáty > všechny** a vyberte `+` tlačítko, jak je znázorněno na následujícím snímku obrazovky:

    [![](remote-notifications-in-ios-images/image10new.png "Vytvořit certifikát pro ID aplikace")](remote-notifications-in-ios-images/image8.png#lightbox)

4.  Vyberte, zda chcete použít certifikát pro vývojové nebo produkční prostředí:

    [![](remote-notifications-in-ios-images/image11new.png "Vyberte certifikát, který vývojové nebo produkční prostředí")](remote-notifications-in-ios-images/image11new.png#lightbox)

5. A pak vyberte nového ID aplikace, kterou jsme právě vytvořili:

    [![](remote-notifications-in-ios-images/image12new.png "Vyberte nového ID aplikace právě vytvořili.")](remote-notifications-in-ios-images/image12new.png#lightbox)

6.  Zobrazí se pokyny, které vás provedou procesem vytvoření *žádost o podepsání certifikátu* pomocí **přístup do řetězce klíčů** aplikaci na vašem počítači Mac.

7.  Teď, když se vytvořil certifikát, musí být používá jako součást procesu sestavení k podepsání aplikace tak, aby se může zaregistrovat s APNs. To vyžaduje vytvoření a instalace zřizovacího profilu, který používá certifikát.

8.  Chcete-li vytvořit vývojový profil pro zřizování, přejděte na **zřizovací profily** části a postupujte podle kroků k vytvoření, pomocí Id aplikace, který jsme právě vytvořili.

9.  Po vytvoření zřizovacího profilu, otevřete **Xcode médií** a obnovovat je. Když vytvoříte profil zřizování nezobrazuje, že může být potřeba stáhnout profil z iOS Provisioning Portal a ji manuálně naimportovat. Na následujícím snímku obrazovky ukazuje příklad Organizátor profilu zřizování přidán:

    [![](remote-notifications-in-ios-images/image13new.png "Tento snímek obrazovky ukazuje příklad Organizátor profilu zřizování přidán")](remote-notifications-in-ios-images/image13new.png#lightbox)

10.  V tomto okamžiku musíme konfigurace projektu Xamarin.iOS na tuto nově vytvořenou zřizovací profil použít. To provedete z **možnosti projektu** dialogového okna, v části **podepsání sady prostředků aplikace pro iOS** kartu, jak ukazuje následující snímek obrazovky:

    [![](remote-notifications-in-ios-images/image11.png "Konfigurace projektu Xamarin.iOS používat tuto nově vytvořenou zřizovací profil")](remote-notifications-in-ios-images/image11.png#lightbox)

Aplikace je teď nakonfigurováno pro práci s nabízenými oznámeními. Existují však stále s certifikátem vyžaduje několik kroků. Tento certifikát je ve formátu kódování DER, který není kompatibilní s PushSharp, která vyžaduje certifikát Personal Information Exchange (PKCS12). Chcete-li převést certifikát tak, aby je použitelný pro PushSharp, proveďte tyto kroky poslední:

1.  **Stáhněte si certifikát, který** -přihlášení do systému iOS Provisioning Portal, zvolit kartu certifikáty, vyberte certifikát přidružený k správné zřizovací profil a vyberte možnost **Stáhnout** .
1.  **Otevřete přístup do řetězce klíčů** – Toto je aplikace je grafického uživatelského rozhraní pro systém správy hesel v OS X.
1.  **Importujte certifikát** – Pokud je certifikovaných ještě není nainstalovaná, **souboru... Import položek** nabídce přístup do řetězce klíčů. Přejděte na certifikát, který exportovat výše a vyberte ji.
1.  **Exportujte certifikát** – rozbalit certifikát přidružený privátní klíč je viditelné, klikněte pravým tlačítkem na klíč a zvolili exportu. Zobrazí se výzva k zadání názvu souboru a heslo pro exportovaný soubor.

V tuto chvíli jsme hotovi s certifikáty. Jsme vytvořili certifikát, který se použije k podepsání aplikace pro iOS a převést tento certifikát do formátu, který jde použít s PushSharp v serverové aplikaci. Další Podívejme se na interakci aplikací pro iOS s APNS.

## <a name="registering-with-apns"></a>Registrace pomocí APNS

Před iOS aplikace může přijímat Vzdálená oznámení, které se musí zaregistrovat s APNS. APNS bude generovat token jedinečná zařízení a, vraťte se do aplikace pro iOS. Aplikace pro iOS se pak proveďte token zařízení a pak sám zaregistruje aplikačního serveru. Jakmile všechny v takovém případě registrace je dokončena a aplikačním serverem může nabízená oznámení do mobilních zařízení.

Teoreticky vzato token zařízení může změnit pokaždé, když aplikace pro iOS se zaregistruje s APNS, však v praxi to nestalo, který často. Optimalizace aplikace může nejnovější token zařízení do mezipaměti a aktualizovat pouze aplikační server při změně. Následující obrázek znázorňuje proces registrace a získání tokenu zařízení:

 ![](remote-notifications-in-ios-images/image12.png "Tento diagram znázorňuje proces registrace a získání tokenu zařízení")

Probíhá registrace s APNS v `FinishedLaunching` metoda třídy delegáta aplikace po zavolání `RegisterForRemoteNotificationTypes` na aktuální `UIApplication` objektu. Pokud se aplikace pro iOS se registruje pomocí APNS, kromě toho musí určovat typy Vzdálená oznámení, je vhodné pro příjem. Tyto typy Vzdálená oznámení jsou deklarovány ve výčtu `UIRemoteNotificationType`. Následující fragment kódu je příklad, jak se aplikace pro iOS můžete zaregistrovat pro příjem Vzdálená oznámení výstrahy a oznámení "BADGE":

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

Žádost o služby APN registrace probíhá na pozadí – při přijetí odpovědi iOS bude volat metodu `RegisteredForRemoteNotifications` v `AppDelegate` třídy a předat token registrovaných zařízení. Token, který bude součástí `NSData` objektu. Následující fragment kódu ukazuje, jak načíst token zařízení tento APNS k dispozici:

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

Pokud se registrace nezdaří z nějakého důvodu (například zařízení není připojené k Internetu), iOS, bude volat `FailedToRegisterForRemoteNotifications` třída delegáta na aplikaci. Následující fragment kódu ukazuje, jak zobrazit upozornění pro uživatele informuje o tom, které se nepodařilo registrace:

```csharp
public override void FailedToRegisterForRemoteNotifications (UIApplication application , NSError error)
{
    new UIAlertView("Error registering push notifications", error.LocalizedDescription, null, "OK", null).Show();
}
```

### <a name="device-token-housekeeping"></a>Token údržbu zařízení

Tokeny zařízení se vypršení platnosti nebo v průběhu času měnit. Z důvodu tuto skutečnost serverových aplikací muset provést některé house čištění a odstranění těchto tokenů vypršela platnost nebo se změnily. Když aplikace odešle jako nabízené oznámení do zařízení, která má tokenu vypršela platnost, APNS se zaznamenejte a uložte tento vypršela platnost tokenu. Servery mohou pak dotazovat služby APNS a zjistěte, jaké tokenů vypršela.

APNS používají k zajištění *zpětnou vazbu služby* – koncový bod HTTPS, který ověřuje prostřednictvím certifikátu, který byl vytvořený k odesílání nabízených oznámení a odešle zpět data o jaké tokenů vypršela. To byl zastaralý společností Apple a odebrat.

Místo toho je nový stavový kód HTTP pro případ, která byla dříve ohlášena služba zpětnou vazbu:

> 410 - token zařízení není už aktivní pro téma.

Kromě toho nový `timestamp` bude klíč dat JSON v textu odpovědi:

> Pokud hodnota v: Stav záhlaví je 410, hodnota tohoto klíče, je čas posledního jakou APNs potvrzuje, že token zařízení již není platný pro téma.
>
> Zastavte odesílání nabízených oznámení, dokud se zařízení zaregistruje novější časové razítko se svým poskytovatelem tokenu.

## <a name="summary"></a>Souhrn

V této části představují klíčové koncepty okolní nabízených oznámení v iOS. To je vysvětleno roli z Apple Push Notification brány Service (APNS). Potom zahrnutých, vytváření a používání certifikátů zabezpečení, které jsou nezbytné pro služby APN. Nakonec tento dokument dokončení představíme na použití aplikačních serverů *zpětnou vazbu služby* zastavit sledování vypršení platnosti tokenů zařízení.


## <a name="related-links"></a>Související odkazy

- [Oznámení – ukázka místních a vzdálených oznámení (ukázka)](https://developer.xamarin.com/samples/monotouch/Notifications/)
- [Místní a nabízenými oznámeními pro vývojáře](https://developer.apple.com/notifications/)
- [UIApplication](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIApplication)
- [UIRemoteNotificationType](http://iosapi.xamarin.com/?link=T%3aMonoTouch.UIKit.UIRemoteNotificationType)
