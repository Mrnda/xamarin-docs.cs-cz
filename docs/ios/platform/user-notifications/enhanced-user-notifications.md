---
title: "Rozšířené uživatelská oznámení"
description: "Tento článek se týká všech způsobů, jak uživatelé oznámení vylepšily iOS 10 a jejich použití v aplikaci pro Xamarin.iOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 4E1FF652-28F0-4566-B383-9D12664401A4
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: a5dbd65cc32ed63c0fa6f8abe3a13ffee4e9df63
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="enhanced-user-notifications"></a>Rozšířené uživatelská oznámení

_Tento článek se týká všech způsobů, jak uživatelé oznámení vylepšily iOS 10 a jejich použití v aplikaci pro Xamarin.iOS._

Nový iOS 10, framework umožňuje doručení a zpracování místní a vzdálené oznámení oznámení pro uživatele. Pomocí toto rozhraní, aplikace nebo rozšíření aplikace můžete naplánovat doručování oznámení místní zadáním sadu podmínek, jako je například umístění nebo denní dobu.

## <a name="about-user-notifications"></a>O upozornění uživatele

Jak jsme uvedli výše, nové architektury oznámení uživateli umožňuje doručení a zpracování místní a vzdálené oznámení. Pomocí toto rozhraní, aplikace nebo rozšíření aplikace můžete naplánovat doručování oznámení místní zadáním sadu podmínek, jako je například umístění nebo denní dobu.

Kromě toho aplikace nebo rozšíření může přijímat (a potenciálně upravit) místních i vzdálených oznámení dodaným do zařízení iOS uživatele.

Nové architektury uživatelského rozhraní oznámení uživatele umožňuje aplikace nebo rozšíření aplikace k přizpůsobení vzhledu místních i vzdálených oznámení poté, co se zobrazí uživateli.

Toto rozhraní nabízí tyto způsoby, které aplikace může poskytovat oznámení pro uživatele:

- **Visual výstrahy** – kde oznámení vrátí dolů z horní části obrazovky v záhlaví.
- **Zvuk a vibrace** -může být přidružen oznámení.
- **Badging ikona aplikace** – kde se zobrazí ikona aplikace oznámení "BADGE", zobrazující, že nový obsah je k dispozici, například počet nepřečtená e-mailové zprávy.

Kromě toho v závislosti na kontextu aktuálního uživatele, existují různé způsoby, které se zobrazí oznámení:

- Pokud se zařízení odemkne, oznámení bude vrátit dolů z horní části obrazovky v záhlaví.
- Pokud je zařízení uzamčené, zobrazí se oznámení na zamykací obrazovce uživatele.
- Pokud uživatel byl vynechán oznámení, mohou otevřete Centrum oznámení a zobrazit všechny dostupné, čekání na oznámení existuje.

Aplikace Xamarin.iOS má dva typy oznámení uživateli, aby bylo možné odeslat:

- **Místní oznámení** -tyto odesílá aplikace nainstalované lokálně na zařízení uživatelů.
- **Vzdálená oznámení** -odešlou ze vzdáleného serveru a buď zobrazovat uživateli, nebo se aktivovala aktualizace na pozadí obsahu aplikace.

### <a name="about-local-notifications"></a>O místní oznámení

Místní oznámení, které mohou odesílat aplikace pro iOS mají tyto funkce a atributy:

- Se odešlou aplikace, které jsou místní na zařízení uživatele. 
- Se dá nakonfigurovat k použití čas nebo umístění na základě aktivační události. 
- Aplikace plány oznámení s zařízení uživatele a se zobrazí, když je splněna podmínka aktivace.
- Když uživatel pracuje s oznámení, aplikace se zobrazí zpětné volání.

Některé příklady místní oznámení zahrnují:

- Výstrahy kalendáře
- Připomenutí výstrahy
- Umístění vědět aktivační události

Další informace najdete v tématu společnosti Apple [vzdáleného oznámení průvodci programováním místních a](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentace.

### <a name="about-remote-notifications"></a>O vzdáleného oznámení

Vzdálené oznámení, které mohou odesílat aplikace pro iOS mají tyto funkce a atributy:

- Serverové komponenty, která komunikuje s má aplikace.
- Apple Push Notification Service (APNs) se používá k přenosu typu best effort doručení vzdáleného oznámení do zařízení uživatele ze serverů cloudové pro vývojáře.
- Když aplikace obdrží vzdálené oznámení se zobrazí uživateli.
- Když uživatel pracuje s oznámení, aplikace se zobrazí zpětné volání.

Některé příklady vzdáleného oznámení zahrnují:

- Zprávy výstrah
- Aktualizace sportu
- Zasílání rychlých zpráv

Aplikace pro iOS k dispozici jsou dva druhy vzdáleného oznámení:

- **Uživatel přístupem** – ty se zobrazí uživatelům na zařízení.
- **Bezobslužné aktualizace** -tyto poskytují mechanismus aktualizovat obsah aplikace pro iOS na pozadí. Po přijetí bezobslužné aktualizace aplikace oslovení pro akce pull serverů odebrat dolů nejnovější obsah.

Další informace najdete v tématu společnosti Apple [vzdáleného oznámení průvodci programováním místních a](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentace.

### <a name="about-the-existing-notifications-api"></a>O existující oznámení rozhraní API

Před iOS 10, využije aplikace pro iOS `UIApplication` registraci oznámení v systému a naplánovat, jak tohoto oznámení by měl být vyvolány (buď čas nebo umístění).

Existuje několik problém, který vývojář může dojít při práci s existující oznámení rozhraní API:

- Nebyly k dispozici různé zpětná volání, vyžaduje se pro místní nebo vzdálené oznámení, což může vést k duplikaci kódu.
- Aplikace měla omezené řízení oznámení po byla naplánoval systémem.
- Nebyly různé úrovně podpory napříč všemi existujícími platformami společnosti Apple.

### <a name="about-the-new-user-notification-framework"></a>O nové architektury oznámení uživatele

S iOS 10, má společnost Apple vydala nové architektury oznámení pro uživatele, která nahrazuje stávající `UIApplication` metoda uvedených výše.

Rozhraní framework oznámení pro uživatele obsahuje následující informace:

- Známé rozhraní API, které zahrnuje parity funkcí s předchozích metod a usnadňuje port kódu z existující rozhraní.
- Zahrnuje rozšířené sadu možností obsahu, která umožňuje bohatší oznámení pro uživatele.
- Místní a Vzdálená oznámení mohou být zpracována stejný kód a zpětná volání.
- Zjednodušuje proces zpracování zpětná volání, které se odesílají do aplikace, když uživatel pracuje s oznámení.
- Vylepšení správy čekající a doručené oznámení, včetně možnosti odstranit nebo aktualizovat oznámení.
- Přidá tuto schopnost prezentace v aplikaci oznámení.
- Přidá možnost naplánovat a zpracovat oznámení z rozšíření aplikace.
- Přidá nový bod rozšíření pro oznámení, sami. 

Nové architektury oznámení pro uživatele poskytuje jednotné oznámení API napříč více platformami, Apple podporuje, včetně: 

- **iOS** -úplné podpory pro správu a naplánovat oznámení.
- **tvOS** -přidává možnost k oznámení "BADGE" ikon aplikace pro místní a vzdálené oznámení.
- **watchOS** – přidá možnosti předávat oznámení z iOS spárované zařízení uživatele k jejich Apple Watch a dává možnost provést místní oznámení přímo na kukátko samotné aplikace sledovat.

Další informace najdete v tématu společnosti Apple [UserNotifications Framework – referenční informace](https://developer.apple.com/reference/usernotifications) a [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui) dokumentaci.

## <a name="preparing-for-notification-delivery"></a>Příprava pro doručení

Před iOS aplikace může odesílat oznámení pro uživatele, které aplikace musí být zaregistrován u systému a oznámení je přerušení uživateli, a proto aplikace musí explicitně žádala o oprávnění před jejich odesláním.

Existují tři různé úrovně oznámení požadavků, které můžete schválit uživatele pro aplikaci:

- Zobrazí se informační zpráva.
- Zvukové výstrahy.
- Badging ikona aplikace.

Kromě toho musí být tyto úrovně schválení požadovaná a nastavit pro místní i vzdálené oznámení.

Oznámení oprávnění byste měli požadovat, jakmile se aplikace spustí přidáním následující kód do `FinishedLaunching` metodu `AppDelegate` a nastavení typu požadované oznámení (`UNAuthorizationOptions`):

```csharp
using UserNotifications;
...

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    return true;
}
```

Kromě toho uživatel může vždy změnit oprávnění oznámení pro aplikaci na současně pomocí **nastavení** aplikace na zařízení. Aplikace by měl zkontrolovat oprávnění požadovaná oznámení uživatele před prezentace oznámení pomocí následujícího kódu:

```csharp
// Get current notification settings
UNUserNotificationCenter.Current.GetNotificationSettings ((settings) => {
    var alertsAllowed = (settings.AlertSetting == UNNotificationSetting.Enabled);
}); 
``` 

### <a name="configuring-the-remote-notifications-environment"></a>Konfigurace prostředí vzdáleného oznámení

Nové na iOS 10 vývojář musí informovat o tom, jaké prostředí nabízená oznámení jsou spuštěny v jako vývoj nebo produkční operačního systému. Důsledkem selhání poskytnout tyto informace může být aplikace odmítnuta při odeslání iTune obchodu s aplikacemi s oznámení podobný následujícímu:

> Chybějící nabízená oznámení nárocích - vaše aplikace obsahuje rozhraní API pro službu Apple Push Notification, ale `aps-environment` nárocích chybí podpis aplikace.

K poskytování požadované nárok, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte `Entitlements.plist` souboru v **řešení Pad** otevřete pro úpravy.
2. Přepnout **zdroj** zobrazení: 

    [![](enhanced-user-notifications-images/setup01.png "Zobrazení zdroje")](enhanced-user-notifications-images/setup01.png#lightbox)
3. Klikněte  **+**  tlačítko přidejte nový klíč.
4. Zadejte `aps-environment` pro **vlastnost**, ponechte **typ** jako `String` a zadejte buď `development` nebo `production` pro **hodnotu**: 

    [![](enhanced-user-notifications-images/setup02.png "Vlastnost přístupových bodů prostředí")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Uložte změny do souboru.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte `Entitlements.plist` souboru v **Průzkumníku řešení** otevřete pro úpravy.
3. Klikněte  **+**  tlačítko přidejte nový klíč.
4. Zadejte `aps-environment` pro **vlastnost**, ponechte **typ** jako `String` a zadejte buď `development` nebo `production` pro **hodnotu**: 

    [![](enhanced-user-notifications-images/setup02w.png "Vlastnost přístupových bodů prostředí")](enhanced-user-notifications-images/setup02.png#lightbox)
5. Uložte změny do souboru.

-----

### <a name="registering-for-remote-notifications"></a>Registrace vzdáleného oznámení

Pokud aplikace bude odesílání a příjem vzdáleného oznámení, bude stále nutné udělat _tokenu registrace_ pomocí stávající `UIApplication` rozhraní API. Tato registrace vyžaduje, aby zařízení tak, aby měl přístup k živé síti připojení služby APN, které způsobí vygenerování nezbytné token, který se odesílají do aplikace. Aplikace potřebuje pak předávat tento token do aplikace straně serveru pro vývojáře k registraci pro vzdálené oznamování událostí:

[![](enhanced-user-notifications-images/token01.png "Přehled tokenu registrace")](enhanced-user-notifications-images/token01.png#lightbox)

Použijte následující kód k inicializaci požadované registrace:

```csharp
UIApplication.SharedApplication.RegisterForRemoteNotifications ();
```

Token, který se odešlou do aplikace pro vývojáře server straně muset být zahrnuty jako součást oznámení datové části tohoto get na odeslaných ze serveru pro služby APN při odesílání oznámení o vzdálené:

[![](enhanced-user-notifications-images/token02.png "Token, jsou součástí datová část oznámení")](enhanced-user-notifications-images/token02.png#lightbox)

Token funguje jako klíčem, který přiřazuje společně oznámení a aplikace použít k otevření nebo reakce na oznámení.

Další informace najdete v tématu společnosti Apple [vzdáleného oznámení průvodci programováním místních a](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/) dokumentace.

## <a name="notification-delivery"></a>Doručení oznámení

V aplikaci plně zaregistrován a požadovaná oprávnění vyžadovaná z a uděleno uživatelem, aplikace je nyní připraven k odesílání a přijímání oznámení. 

### <a name="providing-notification-content"></a>Poskytuje obsah oznámení

Nové na iOS 10 obě obsahovat všechna oznámení **název** a **Subtitle** , se vždy zobrazí **textu** obsahu oznámení. Nové, je také možnost přidávat **média přílohy** obsahu oznámení.

Chcete-li vytvořit obsah místního oznámení, použijte následující kód:

```csharp
var content = new UNMutableNotificationContent();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
```

Vzdálená oznámení je podobný proces:

```csharp
{
    "aps":{
        "alert":{
            "title":"Notification Title",
            "subtitle":"Notification Subtitle",
            "body":"This is the message body of the notification."
        },
        "badge":1
    }
}
```

### <a name="scheduling-when-a-notification-is-sent"></a>Plánování při oznámení se odesílají.

S obsahem oznámení vytvořit aplikace potřebuje pro plánování, když oznámení bude zobrazovat uživatelům nastavením *aktivační událost*. iOS 10 obsahuje čtyři různé typy aktivační události:

- **Nabízená oznámení** – slouží výhradně pomocí vzdáleného oznámení a se aktivuje, když balíček služby APN zasílá oznámení do aplikace spuštěné na zařízení.
- **Časový Interval** -umožňuje místního oznámení k naplánování ze času intervalu začínat teď a ukončení některých budoucí bod. Třeba `var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);`.
- **Kalendáře datum** -umožňuje místní oznámení naplánovaných pro určité datum a čas.
- **Na základě umístění** -umožňuje místní oznámení nutné naplánovat, kdy je zařízení s iOS zadáním nebo ponechat určitou zeměpisnou polohu nebo je daný blízko k žádné signály Bluetooth.

Když místního oznámení je připraven, aplikace potřebuje k volání `Add` metodu `UNUserNotificationCenter` objekt, který chcete naplánovat jeho zobrazení pro uživatele. Vzdáleného oznámení aplikace na straně serveru odešle datová část oznámení APNs, který pak odešle paket na zařízení uživatele.

Spojením všechny vytvořené, ukázku místního oznámení může vypadat:

```csharp
using UserNotifications;
...

var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

var trigger =  UNTimeIntervalNotificationTrigger.CreateTrigger (5, false);

var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

## <a name="handling-foreground-app-notifications"></a>Zpracování oznámení o aplikaci popředí

Nové na iOS 10 aplikace dokáže zpracovat oznámení jinak když je aktivní a aktivaci oznámení. Tím, že poskytuje `UNUserNotificationCenterDelegate` a implementace `UserNotificationCenter` metody aplikace můžete převzít odpovědnost za zobrazení oznámení. Příklad:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        #region Constructors
        public UserNotificationCenterDelegate ()
        {
        }
        #endregion

        #region Override Methods
        public override void WillPresentNotification (UNUserNotificationCenter center, UNNotification notification, Action<UNNotificationPresentationOptions> completionHandler)
        {
            // Do something with the notification
            Console.WriteLine ("Active Notification: {0}", notification);

            // Tell system to display the notification anyway or use
            // `None` to say we have handled the display locally.
            completionHandler (UNNotificationPresentationOptions.Alert);
        }
        #endregion
    }
}
```

Tento kód je jednoduše zápis se obsah `UNNotification` výstupní aplikace a požádat systému zobrazíte standardní výstrahy pro oznámení. 

Pokud aplikace chtěli zobrazit vlastního oznámení, že je v popředí a ne použít výchozí systémové nastavení, předat `None` obslužné rutině dokončení. Příklad:

```csharp
completionHandler (UNNotificationPresentationOptions.None);
```

S tímto kódem na místě, otevřete `AppDelegate.cs` souboru pro úpravy a změňte `FinishedLaunching` metodu vypadat podobně jako následující:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    // Request notification permissions from the user
    UNUserNotificationCenter.Current.RequestAuthorization (UNAuthorizationOptions.Alert, (approved, err) => {
        // Handle approval
    });

    // Watch for notifications while the app is active
    UNUserNotificationCenter.Current.Delegate = new UserNotificationCenterDelegate ();

    return true;
}
```

Tento kód je připojení vlastní `UNUserNotificationCenterDelegate` z výše na aktuální `UNUserNotificationCenter` tak aplikace dokáže zpracovat oznámení, když je aktivní a v popředí.

## <a name="notification-management"></a>Správa oznámení

Nové na iOS 10 oznámení správu poskytuje přístup k čekající a doručené oznámení a přidává možnost odebrat, aktualizovat nebo povýšit tato oznámení.

Je důležitou součástí správy oznámení _identifikátor žádosti_ , byl přiřazen oznámení, když byl vytvořen a naplánované systémem. Pro vzdálené oznámení, to je přiřazen prostřednictvím nové `apps-collapse-id` pole v hlavičce požadavku HTTP.

Identifikátor žádosti slouží k výběru oznámení, že aplikace, které chcete provádět správu oznámení na.

### <a name="removing-notifications"></a>Odebrání oznámení

Chcete-li odebrat čekající oznámení ze systému, použijte následující kód:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemovePendingNotificationRequests (requests);
```

K odebrání již doručené oznámení, použijte následující kód:

```csharp
var requests = new string [] { "sampleRequest" };
UNUserNotificationCenter.Current.RemoveDeliveredNotifications (requests);
```

### <a name="updating-an-existing-notification"></a>Aktualizace stávající oznámení

Pokud chcete aktualizovat existující oznámení, jednoduše vytvořit nové oznámení s požadované parametry upravit (jako je například nový čas aktivace) a přidejte ho do systému se stejným identifikátorem žádosti jako oznámení, které má být změněn. Příklad:


```csharp
using UserNotifications;
...

// Rebuild notification
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;

// New trigger time
var trigger = UNTimeIntervalNotificationTrigger.CreateTrigger (10, false);

// ID of Notification to be updated
var requestID = "sampleRequest";
var request = UNNotificationRequest.FromIdentifier (requestID, content, trigger);

// Add to system to modify existing Notification
UNUserNotificationCenter.Current.AddNotificationRequest (request, (err) => {
    if (err != null) {
        // Do something with error...
    }
});
```

Již doručené oznámení bude získat aktualizovat existující oznámení a povýšen na horní části seznamu na Home a uzamčení obrazovky a v centru oznámení, pokud je již přečtena uživatelem.

## <a name="working-with-notification-actions"></a>Práce s akcí oznámení

V iOS 10 oznámení, které jsou uživateli poskytovaná nejsou statické a zadejte několik způsobů, které uživatel může komunikovat s nimi (z předdefinované na vlastní akce).

Existují tři typy akcí, které může reagovat aplikaci pro iOS:

- **Výchozí akce** – to je, když uživatel klepnutím oznámení otevřete aplikaci a zobrazit podrobnosti o daném oznámení.
- **Vlastní akce** -těchto byly přidány v iOS 8 a poskytují rychlý způsob, jak uživatelům provádět vlastní úlohu přímo z oznámení bez nutnosti spustit aplikaci. Se může zobrazovat jako seznam tlačítka s přizpůsobitelné názvy nebo na text vstupní pole, které můžete spustit buď na pozadí (kde aplikace je zadána malé množství času ke splnění tohoto požadavku) nebo popředí (kde je aplikace spuštěná v popředí fu lfill požadavek). Jsou k dispozici na iOS a watchOS vlastní akce.
- **Zrušit akci** – tato akce je odeslána do aplikace, pokud uživatel zavře daného oznámení.

### <a name="creating-custom-actions"></a>Vytváření vlastní akce

Vytvoření a registraci vlastní akce v systému, použijte následující kód:

```csharp
// Create action
var actionID = "reply";
var title = "Reply";
var action = UNNotificationAction.FromIdentifier (actionID, title, UNNotificationActionOptions.None);

// Create category
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);
    
// Register category
var categories = new UNNotificationCategory [] { category };
UNUserNotificationCenter.Current.SetNotificationCategories (new NSSet<UNNotificationCategory>(categories)); 
```

Při vytváření nového `UNNotificationAction`, má přiřazené jedinečné ID a název, který se zobrazí na tlačítko. Ve výchozím nastavení akce vytvoří jako pozadí akce, ale můžete zadat možnosti upravit chování typu akce (například jeho nastavení na popředí akce).

Všechny akce vytvoření musí být přidružen kategorii. Při vytváření nového `UNNotificationCategory`, je přiřazen jedinečný ID, a seznam akcí se může provádět, seznam identifikátorů záměr poskytovat další informace o záměr akcí v kategorii a některé možnosti, které řídí chování kategorie.

Nakonec všechny kategorie jsou registrovány systému pomocí `SetNotificationCategories` metoda.

### <a name="presenting-custom-actions"></a>Představuje vlastní akce

Jakmile sadu kategorií a vlastní akce byly vytvořeny a zaregistrována v systému, lze zobrazit z místní nebo vzdálené oznámení.

Pro vzdálené oznámení, nastavení `category` v datové části vzdáleného oznámení, která odpovídá jednomu z kategorie vytvořili výše. Příklad:

```csharp
{
    aps:{
        alert:"Hello world!",
        category:"message"
    }
}
```

Pro místní oznámení, nastavte `CategoryIdentifier` vlastnost `UNMutableNotificationContent` objektu. Příklad:

```csharp
var content = new UNMutableNotificationContent ();
content.Title = "Notification Title";
content.Subtitle = "Notification Subtitle";
content.Body = "This is the message body of the notification.";
content.Badge = 1;
content.CategoryIdentifier = "message";
...
```

Toto ID znovu, musí odpovídat některou z kategorií, které byla vytvořena výše.

### <a name="handling-dismiss-actions"></a>Zpracování zavřít akce

Jak jsme uvedli výše, můžete zrušit akci odesílají do aplikace, když uživatel zavře oznámení. Vzhledem k tomu, že to není standardní akce, jako možnost muset nastavit při vytváření kategorii. Příklad:

```csharp
var categoryID = "message";
var actions = new UNNotificationAction [] { action };
var intentIDs = new string [] { };
var categoryOptions = new UNNotificationCategoryOptions [] { };
var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.CustomDismissAction);

```

### <a name="handling-action-responses"></a>Zpracování odpovědi na akce

Když uživatel pracuje s vlastní akce a kategorie, které byly vytvořeny výše, aplikace potřebuje ke splnění požadovanou úlohu. To se provádí zadáním `UNUserNotificationCenterDelegate` a implementace `UserNotificationCenter` metoda. Příklad:

```csharp
using System;
using UserNotifications;

namespace MonkeyNotification
{
    public class UserNotificationCenterDelegate : UNUserNotificationCenterDelegate
    {
        ...

        #region Override Methods
        public override void DidReceiveNotificationResponse (UNUserNotificationCenter center, UNNotificationResponse response, Action completionHandler)
        {
            // Take action based on Action ID
            switch (response.ActionIdentifier) {
            case "reply":
                // Do something
                break;
            default:
                // Take action based on identifier
                if (response.IsDefaultAction) {
                    // Handle default action...
                } else if (response.IsDismissAction) {
                    // Handle dismiss action
                }
                break;
            }

            // Inform caller it has been handled
            completionHandler();
        }
        #endregion
    }
}
```

Předaný v `UNNotificationResponse` třída má `ActionIdentifier` vlastnost, která buď může být výchozí akce nebo akci zrušit. Použití `response.Notification.Request.Identifier` k testování pro všechny vlastní akce.

`UserText` Vlastnost uchovává hodnotu všechny uživatele textový vstup. `Notification` Vlastnost obsahuje původní oznámení, které zahrnuje požadavek s aktivační události a oznámení obsahu. Aplikace se můžete rozhodnout, pokud byl místní nebo vzdálené oznámení na základě typu aktivační události.

## <a name="working-with-service-extensions"></a>Práce s rozšíření služeb

Při práci se službou vzdálené oznámení _rozšíření služeb_ poskytují způsob, jak povolit šifrování klient server uvnitř datová část oznámení. Rozšíření služeb jsou (k dispozici v iOS 10) uživatelské rozhraní příponu, která běží na pozadí s hlavním účelem rozšířit nebo výměna viditelný obsah oznámení předtím, než se zobrazí uživateli. 

[![](enhanced-user-notifications-images/extension01.png "Služby rozšíření – přehled")](enhanced-user-notifications-images/extension01.png#lightbox)

Rozšíření služeb, která jsou určená ke spuštění rychle a jsou uvedeny pouze za krátkou dobu doba provádění v systému. V případě, že rozšíření služby selže a dokončení úkolu v přiděleném množství času, bude mít název záložní metodu. V případě selhání záložního obsahu původní oznámení se zobrazí uživateli.

Některé potenciální použití rozšíření služeb patří:

- Poskytuje šifrování klient server obsahu vzdáleného oznámení.
- Přidání přílohy do vzdáleného oznámení je zlepšit komunikaci.

### <a name="implementing-a-service-extension"></a>Implementace rozšíření služby

Pokud chcete implementovat rozšíření služby v aplikaci Xamarin.iOS, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřít řešení aplikace v sadě Visual Studio for Mac.
2. Klikněte pravým tlačítkem na název řešení v **řešení Pad** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **rozšíření služeb oznámení** a klikněte na **Další** tlačítko: 

    [![](enhanced-user-notifications-images/extension02.png "Vyberte rozšíření služeb oznámení")](enhanced-user-notifications-images/extension02.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **Další** tlačítko: 

    [![](enhanced-user-notifications-images/extension03.png "Zadejte název pro rozšíření")](enhanced-user-notifications-images/extension03.png#lightbox)
5. Upravit **název projektu** nebo **název řešení** dle potřeby **vytvořit** tlačítko: 

    [![](enhanced-user-notifications-images/extension04.png "Nastavte název projektu nebo název řešení")](enhanced-user-notifications-images/extension04.png#lightbox) 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otevřete řešení aplikace v sadě Visual Studio.
2. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **rozšíření služeb oznámení**: 

    [![](enhanced-user-notifications-images/extension01w.png "Vyberte rozšíření služeb oznámení")](enhanced-user-notifications-images/extension01w.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **OK** tlačítko.

-----

> [!IMPORTANT]
> Poznámka: Identifikátor balíku rozšíření služby by měl odpovídat identifikátor balíku hlavní aplikace pomocí `.appnameserviceextension` připojen na konec. Například pokud aplikace hlavní měli identifikátor svazku z `com.xamarin.monkeynotify`, rozšíření služby by měl mít identifikátor svazku z `com.xamarin.monkeynotify.monkeynotifyserviceextension`. Měla by být nastavena automaticky při přidání rozšíření do řešení. 

V rozšíření služby oznámení, který bude potřeba upravit tak, aby požadované funkce, které je jeden hlavní třídy. Příklad:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyChatServiceExtension
{
    [Register ("NotificationService")]
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Computed Properties
        public Action<UNNotificationContent> ContentHandler { get; set; }
        public UNMutableNotificationContent BestAttemptContent { get; set; }
        #endregion

        #region Constructors
        protected NotificationService (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            ContentHandler = contentHandler;
            BestAttemptContent = (UNMutableNotificationContent)request.Content.MutableCopy ();

            // Modify the notification content here...
            BestAttemptContent.Title = $"{BestAttemptContent.Title}[modified]";

            ContentHandler (BestAttemptContent);
        }

        public override void TimeWillExpire ()
        {
            // Called just before the extension will be terminated by the system.
            // Use this as an opportunity to deliver your "best attempt" at modified content, otherwise the original push payload will be used.

            ContentHandler (BestAttemptContent);
        }
        #endregion
    }
}
```

První metoda `DidReceiveNotificationRequest`, se předá identifikátoru oznámení a také oznámení obsahu prostřednictvím `request` objektu. Předaný v `contentHandler` bude muset volat na oznámení pro uživatele.

Druhý metoda `TimeWillExpire`, bude volána těsně před čas je spuštěn rozšíření služby pro zpracování požadavku. Pokud se nepodaří volání rozšíření služby `contentHandler` ve vymezeném množství času, se zobrazí původní obsah pro uživatele.

### <a name="triggering-a-service-extension"></a>Spuštění rozšíření služby

S příponou služba vytvoří a doručit v aplikaci můžete spustit změnou vzdálené datová část oznámení odešle do zařízení. Příklad:

```csharp
{
    aps : {
        alert : "New Message Available",
        mutable-content: 1
    },
    encrypted-content : "#theencryptedcontent"
}
```

Nové `mutable-content` klíč určuje, že rozšíření služby bude nutné spustit pro aktualizaci obsahu vzdáleného oznámení. `encrypted-content` Klíč obsahuje šifrovaná data, která mohou před prezentací uživateli dešifrovat rozšíření služby.

Podívejte se na následující příklad rozšíření služby:

```csharp
using UserNotification;

namespace myApp {
    public class NotificationService : UNNotificationServiceExtension {
    
        public override void DidReceiveNotificationRequest(UNNotificationRequest request, contentHandler) {
            // Decrypt payload
            var decryptedBody = Decrypt(Request.Content.UserInfo["encrypted-content"]);
            
            // Modify Notification body
            var newContent = new UNMutableNotificationContent();
            newContent.Body = decryptedBody;
            
            // Present to user
            contentHandler(newContent);
        }
        
        public override void TimeWillExpire() {
            // Handle out-of-time fallback event
            ...
        }
        
    }
}
```

Tento kód dešifruje šifrovaný obsah z `encrypted-content` klíče, vytvoří novou `UNMutableNotificationContent`, nastaví `Body` vlastnost dešifrovaný obsah a používá `contentHandler` na oznámení pro uživatele.

## <a name="summary"></a>Souhrn

Tento článek má zahrnuté všechny způsoby, jak uživatelé oznámení vylepšily v iOS 10. Zobrazí se nové architektury oznámení pro uživatele a způsobu jeho použití v aplikaci Xamarin.iOS nebo rozšíření aplikace.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework – referenční informace](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Průvodce programováním místních a vzdálených oznámení](https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/)
