---
title: Oznámení pro pokročilé uživatele
description: Tento článek trvá hlubší pohled na nové architektury uživatelská oznámení a postup plně využít výhod ho v aplikaci pro Xamarin.iOS.
ms.prod: xamarin
ms.assetid: 4E0C60AE-6F54-4098-8FA0-AADF9AC86805
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9958682ce9e356692f451900d7dca0e343b244da
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="advanced-user-notifications"></a>Oznámení pro pokročilé uživatele

_Tento článek trvá hlubší pohled na nové architektury uživatelská oznámení a postup plně využít výhod ho v aplikaci pro Xamarin.iOS._

Nový iOS 10, framework umožňuje doručení a zpracování místní a vzdálené oznámení oznámení pro uživatele. Pomocí toto rozhraní, aplikace nebo rozšíření aplikace můžete naplánovat doručování oznámení místní zadáním sadu podmínek, jako je například umístění nebo denní dobu.

## <a name="about-user-notifications"></a>O upozornění uživatele

Nové architektury oznámení uživateli umožňuje doručení a zpracování místní a vzdálené oznámení. Pomocí toto rozhraní, aplikace nebo rozšíření aplikace můžete naplánovat doručování oznámení místní zadáním sadu podmínek, jako je například umístění nebo denní dobu.

Kromě toho aplikace nebo rozšíření může přijímat (a potenciálně upravit) místních i vzdálených oznámení dodaným do zařízení iOS uživatele.

Nové architektury uživatelského rozhraní oznámení uživatele umožňuje aplikace nebo rozšíření aplikace k přizpůsobení vzhledu místních i vzdálených oznámení poté, co se zobrazí uživateli.

Toto rozhraní nabízí tyto způsoby, které aplikace může poskytovat oznámení pro uživatele:

- **Visual výstrahy** – kde oznámení vrátí dolů z horní části obrazovky v záhlaví.
- **Zvuk a vibrace** -může být přidružen oznámení.
- **Badging ikona aplikace** – kde oznámení "BADGE", zobrazující, že je k dispozici nový obsah, zobrazí se ikona aplikace. Například počet nepřečtená e-mailové zprávy.

Kromě toho v závislosti na kontextu aktuálního uživatele, existují různé způsoby, které se zobrazí oznámení:

- Pokud se zařízení odemkne, oznámení bude vrátit dolů z horní části obrazovky v záhlaví.
- Pokud je zařízení uzamčené, zobrazí se oznámení na zamykací obrazovce uživatele.
- Pokud uživatel byl vynechán oznámení, mohou otevřete Centrum oznámení a zobrazit všechny dostupné, čekání na oznámení existuje.

Aplikace Xamarin.iOS má dva typy oznámení uživateli, aby bylo možné odeslat:

- **Místní oznámení** -tyto odesílá aplikace nainstalované lokálně na zařízení uživatelů.
- **Vzdálená oznámení** – odešlou ze vzdáleného serveru a buď uživateli nebo spustí aktualizaci pozadí obsahu aplikace.

Další informace najdete v tématu naše [rozšířené oznámení uživateli](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentaci.

## <a name="the-new-user-notification-interface"></a>Nové uživatelské rozhraní oznámení

Nový design uživatelského rozhraní, která poskytuje další obsah, jako jsou název, Subtitle a volitelné přílohy média, kterou lze zobrazit na zamykací obrazovce jako informační zpráva v horní části zařízení nebo v centru oznámení, můžou uživatelská oznámení v iOS 10.

Bez ohledu na to, kde se zobrazí oznámení pro uživatele v iOS 10 zobrazí se s stejné vzhledu a chování a stejné funkce a funkce.

Společnost Apple vydala v iOS 8, nastavení řešitelné oznámení, kde může vývojář přiřadit vlastní akce oznámení a umožní uživateli provést akci pro oznámení bez nutnosti spustit aplikaci. V systému iOS 9 rozšířeného Apple řešitelné oznámení s rychle reagovat, což mu umožní reagovat na oznámení s zadávání textu.

Protože oznámení uživateli více nedílnou součástí činnost koncového uživatele v iOS 10, Apple dále rozšířit řešitelné oznámení pro podporu 3D dotykového ovládání, kde uživatel stiskne na oznámení a vlastní uživatelské rozhraní je zobrazení zajistit bohaté interakce oznámení.

Při vlastní uživatelské rozhraní uživatele oznámení se zobrazí, pokud uživatel pracuje s akcemi, které jsou připojené k oznámení, vlastní uživatelské rozhraní můžete okamžitě aktualizovat k poskytnutí zpětné vazby, co se změnilo.

Nové iOS 10, rozhraní API uživatelského rozhraní oznámení uživatele povoluje aplikace Xamarin.iOS snadno využít těchto nových funkcí uživatelského rozhraní oznámení uživatele.

## <a name="adding-media-attachments"></a>Přidávání příloh média

Jedním z běžnějších položky, které získat sdílet mezi uživateli je fotografie, takže iOS 10 přidat možnost připojit položku média (jako jsou fotografie) přímo oznámení, kde bude vidění a snadno dostupné pro uživatele spolu s ostatními konte na oznámení NT.

Kvůli velikosti zahrnutých v odesílání stane však nepraktické i malý obrázek, připojení k vzdálené datová část oznámení. Ke zpracování této situace, vývojáři použít nové rozšíření služby v iOS 10 stáhnout bitovou kopii z jiného zdroje (například CloudKit úložiště) a jeho připojení k obsahu na oznámení, než se zobrazí uživateli.

Pro vzdálené oznámení upravit rozšíření služby musí být označen jeho datové části jako měnitelný. Příklad:

```csharp
{
    aps : {
        alert : "New Photo Available",
        mutable-content: 1
    },
    my-attachment : "https://example.com/photo.jpg"
}
```

Podívejte se na následující přehled procesu:

[![](advanced-user-notifications-images/extension02.png "Přidávání příloh média procesu")](advanced-user-notifications-images/extension02.png#lightbox)

Jakmile vzdáleného oznámení je doručit do zařízení (přes APNs), rozšíření služby můžete pak stáhnout požadované bitovou kopii prostřednictvím jakýmkoli způsobem potřeby (například `NSURLSession`) a po přijetí bitovou kopii, můžete upravovat obsah oznámení a zobrazení je pro uživatele.

Následuje příklad, jak tento proces může být zpracována v kódu:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;

namespace MonkeyNotification
{
    public class NotificationService : UNNotificationServiceExtension
    {
        #region Constructors
        public NotificationService ()
        {
        }
        #endregion

        #region Override Methods
        public override void DidReceiveNotificationRequest (UNNotificationRequest request, Action<UNNotificationContent> contentHandler)
        {
            // Get file URL
            var attachementPath = request.Content.UserInfo.ObjectForKey (new NSString ("my-attachment"));
            var url = new NSUrl (attachementPath.ToString ());

            // Download the file
            var localURL = new NSUrl ("PathToLocalCopy");

            // Create attachment
            var attachmentID = "image";
            var options = new UNNotificationAttachmentOptions ();
            NSError err;
            var attachment = UNNotificationAttachment.FromIdentifier (attachmentID, localURL, options , out err);

            // Modify contents
            var content = request.Content.MutableCopy() as UNMutableNotificationContent;
            content.Attachments = new UNNotificationAttachment [] { attachment };

            // Display notification
            contentHandler (content);
        }

        public override void TimeWillExpire ()
        {
            // Handle service timing out
        }
        #endregion
    }
}
```

Po přijetí oznámení ze služby APN vlastní adresu bitové kopie je pro čtení z obsahu a soubor se stáhne ze serveru. Pak `UNNotificationAttachement` je vytvořen s jedinečným ID a místní umístění bitové kopie (jako `NSUrl`). Vytvoří měnitelný kopie obsahu oznámení a přidávání příloh média. Nakonec se zobrazí oznámení uživateli při volání `contentHandler`.

Jakmile přílohu byl přidán k oznámení, systém má přesouvání a správy souboru.

Kromě vzdáleného oznámení popsané výše, média přílohy jsou podporovány také z místního oznámení, kde `UNNotificationAttachement` se vytvoří a připojené k oznámení spolu s jeho obsahem.

Oznámení v iOS 10 podporují média přílohy bitových kopií (statické a obrázky GIF), zvuk nebo video a systém se automaticky zobrazí správný vlastní uživatelské rozhraní pro každou z těchto typů přílohy při oznámení pro uživatele.

> [!NOTE]
> Byste měli věnovat k optimalizaci velikosti média a době potřebné ke stažení média ze vzdáleného serveru (nebo ke kompilaci média pro místního oznámení) jako systém ukládá striktní limity pro obě při spuštění rozšíření aplikace služby. Představte si třeba odesílání škálovat dolů verze bitové kopie nebo jen nepatrnou klip videa prezentovány v oznámení.

## <a name="creating-custom-user-interfaces"></a>Vytvoření vlastního uživatelského rozhraní

Pokud chcete vytvořit vlastní uživatelské rozhraní pro jeho oznámení uživateli, musí vývojář přidat (nový iOS 10) obsahu rozšíření oznámení do aplikace řešení.

Rozšíření obsah oznámení umožňuje vývojáři přidávat své vlastní zobrazení uživatelského rozhraní oznámení a kreslení na žádný obsah, který jim vyhovuje. Interaktivní uživatelské rozhraní pomůcek (například tlačítka nebo textová pole) jsou však **není** nepodporuje rozhraní oznámení a nikdy musí být přidaní do vlastní návrh uživatelského rozhraní.

Pro podporu interakce uživatele s oznámení pro uživatele, vlastní akce by měl být vytvořen, zaregistrována v systému a připojené k oznámení před plánovaným systémem. Rozšíření obsahu oznámení bude volána pro zpracování procesů z těchto akcí. Najdete v článku [práce s akcí oznámení](~/ios/platform/user-notifications/enhanced-user-notifications.md) části [rozšířené oznámení uživateli](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentu další podrobnosti o vlastní akce.

Když se oznámení pro uživatele s vlastní uživatelské rozhraní pro uživatele, bude mít následující prvky:

[![](advanced-user-notifications-images/customui01.png "Oznámení pro uživatele pomocí uživatelského rozhraní pro vlastní elementy")](advanced-user-notifications-images/customui01.png#lightbox)

Pokud uživatel pracuje s vlastní akcí (uvedené níže oznámení), uživatelské rozhraní můžete aktualizovat tak, aby zpětnou vazbu od uživatelů jako co se stalo při jejich vyvolání danou akci.

### <a name="adding-a-notification-content-extension"></a>Přidání obsahu příponu oznámení

Pokud chcete implementovat uživatelského rozhraní pro oznámení uživatele v aplikaci Xamarin.iOS, postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Otevřít řešení aplikace v sadě Visual Studio for Mac.
2. Klikněte pravým tlačítkem na název řešení v **řešení Pad** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **obsahu rozšíření oznámení** a klikněte na **Další** tlačítko: 

    [![](advanced-user-notifications-images/notify01.png "Vyberte oznámení obsahu rozšíření")](advanced-user-notifications-images/notify01.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **Další** tlačítko: 

    [![](advanced-user-notifications-images/notify02.png "Zadejte název pro rozšíření")](advanced-user-notifications-images/notify02.png#lightbox)
5. Upravit **název projektu** nebo **název řešení** dle potřeby **vytvořit** tlačítko: 

    [![](advanced-user-notifications-images/notify03.png "Nastavte název projektu nebo název řešení")](advanced-user-notifications-images/notify03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Otevřít řešení aplikace v sadě Visual Studio for Mac.
2. Klikněte pravým tlačítkem na název řešení v **Průzkumníku řešení** a vyberte **přidat** > **přidat nový projekt**.
3. Vyberte **iOS** > **rozšíření** > **obsahu rozšíření oznámení**: 

    [![](advanced-user-notifications-images/notify01w.png "Vyberte oznámení obsahu rozšíření")](advanced-user-notifications-images/notify01w.png#lightbox)
4. Zadejte **název** pro rozšíření a klikněte na **OK** tlačítko.

-----

Když rozšíření obsahu oznámení se přidá k řešení, budou vytvořeny tři soubory v rozšíření projektu:

1. `NotificationViewController.cs` -Toto je hlavní zobrazení řadič pro rozšíření obsahu oznámení.
2. `MainInterface.storyboard` -Kde vývojář rozložen viditelné uživatelského rozhraní pro rozšíření obsahu oznámení v iOS Designer.
3. `Info.plist` – Ovládací prvky konfigurace rozšíření obsahu oznámení.

Výchozí hodnota `NotificationViewController.cs` souboru vypadá takto:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

        }
        #endregion
    }
}
```

`DidReceiveNotification` Metoda je volána, když oznámení rozšíří uživatelem tak, že rozšíření obsahu oznámení může vyplnit vlastní uživatelské rozhraní s obsahem `UNNotification`. Výše uvedené například štítek přidala do zobrazení, zpřístupněna kódu s názvem `label` a slouží k zobrazení textu oznámení.

### <a name="setting-the-notification-content-extensions-categories"></a>Nastavení oznámení obsahu rozšíření kategorií

V systému musí být informováni o tom, jak najít rozšíření obsahu oznámení aplikace podle specifických kategorií, které odpovídá na. Postupujte takto:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. Dvakrát klikněte na rozšíření `Info.plist` v soubor **řešení Pad** otevřete pro úpravy.
2. Přepnout **zdroj** zobrazení.
3. Rozbalte `NSExtension` klíč.
4. Přidat `UNNotificationExtensionCategory` klíče jako typ **řetězec** s hodnotou rozšíření patří do kategorie (v tomto příkladu se události pozvání): 

    [![](advanced-user-notifications-images/customui02.png "Přidejte klíč UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02.png#lightbox)
5. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Dvakrát klikněte na rozšíření `Info.plist` v soubor **Průzkumníku řešení** otevřete pro úpravy.
3. Rozbalte `NSExtension` klíč.
4. Přidat `UNNotificationExtensionCategory` klíče jako typ **řetězec** s hodnotou rozšíření patří do kategorie (v tomto příkladu se události pozvání): 

    [![](advanced-user-notifications-images/customui02w.png "Přidejte klíč UNNotificationExtensionCategory")](advanced-user-notifications-images/customui02w.png#lightbox)
5. Uložte provedené změny.

-----

Oznámení obsahu rozšíření kategorií (`UNNotificationExtensionCategory`) použít stejné hodnoty kategorie, které se používají k registraci akcí oznámení. V situaci, kdy aplikace bude používat stejné uživatelské rozhraní pro více kategorií, přepněte `UNNotificationExtensionCategory` typu **pole** a zadejte všechny požadované kategorie. Příklad:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui03.png "Oznámení obsahu rozšíření kategorií")](advanced-user-notifications-images/customui03.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui03w.png "Oznámení obsahu rozšíření kategorií")](advanced-user-notifications-images/customui03w.png#lightbox)

-----

### <a name="hiding-the-default-notification-content"></a>Skrytí výchozí obsah oznámení

V situaci, kdy rozhraní vlastní oznámení zobrazovat stejný obsah jako výchozí oznámení (název, Subtitle a text v dolní části rozhraní oznámení zobrazí automaticky), můžete tyto informace výchozí skryté přidáním `UNNotificationExtensionDefaultContentHidden`klíče k `NSExtensionAttributes` klíče jako typ **Boolean** s hodnotou `YES` v rozšíření `Info.plist` souboru:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui04.png "Výchozí informace o hledání")](advanced-user-notifications-images/customui04.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui04w.png "Výchozí informace o hledání")](advanced-user-notifications-images/customui04w.png#lightbox)

-----

### <a name="designing-the-custom-ui"></a>Návrh vlastní uživatelské rozhraní

K návrhu obsahu rozšíření oznámení vlastní uživatelské rozhraní, dvakrát klikněte na `MainInterface.storyboard` soubor otevřete pro úpravy v iOS Designer, přetáhněte v elementech, které potřebujete k vytvoření požadované rozhraní (například `UILabels` a `UIImageViews`).

> [!NOTE]
> Rozhraní oznámení nemá _není_ podporu interaktivní ovládací prvky, jako je například textových polí nebo tlačítek v obsahu rozšíření oznámení. Při mohou být přidány do scénáře, uživatel nebude moci komunikovat s nimi. Chcete-li přidat do vlastní uživatelské rozhraní oznámení interakci s uživatelem, použijte vlastní akce.

Jakmile byl nastíněny uživatelského rozhraní a příslušných ovládacích prvků viditelné na kód C#, otevřete `NotificationViewController.cs` pro úpravy a upravovat `DidReceiveNotification` metoda k vyplnění uživatelského rozhraní, když uživatel rozšíří oznámení. Příklad:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

        }
        #endregion
    }
}
```

### <a name="setting-the-content-area-size"></a>Nastavení velikosti oblasti obsahu

Upravit velikost oblasti obsahu, zobrazí se uživateli, je nastavení následující kód `PreferredContentSize` vlastnost `ViewDidLoad` metoda na požadovanou velikost. Tato velikost může být také upravena použitím omezení zobrazení v Návrháři iOS, je ponechán na vývojáře a vyberte metodu, která je nejlepší pro ně.

Vzhledem k tomu, že bude zpočátku oznámení systému je již spuštěna před oznámení vyvolání obsahu rozšíření oblast obsahu úplné velikosti a být animovaný dolů na požadovanou velikost, když uživateli.

Chcete-li odstranit tento vliv, upravte `Info.plist` soubor pro rozšíření a sadu `UNNotificationExtensionInitialContentSizeRatio` klíče z `NSExtensionAttributes` klíč na typ **číslo** s hodnotu představující požadované poměr. Příklad:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

[![](advanced-user-notifications-images/customui05.png "Klíč UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](advanced-user-notifications-images/customui05w.png "Klíč UNNotificationExtensionInitialContentSizeRatio")](advanced-user-notifications-images/customui05w.png#lightbox)

-----

### <a name="using-media-attachments-in-custom-ui"></a>Pomocí médií přílohy v vlastní uživatelského rozhraní

Protože přílohy média (jak je vidět v [přidávání příloh média](#Adding-Media-Attachments) část výše) jsou součástí datová část oznámení, mohou být přístup a zobrazí v obsahu rozšíření oznámení stejně, jako by byly ve výchozí Oznámení uživatelského rozhraní.

Například, pokud rozhraní vlastní výše uvedené zahrnuté `UIImageView` který byl vystaven kód C#, může následující kód k vyplnění z s přílohou média:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;

namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }
        #endregion
    }
}
```

Protože přílohu média je spravován systémem, je mimo izolovaného prostoru aplikace. Rozšíření potřebuje k informování systému požaduje přístup k souboru voláním `StartAccessingSecurityScopedResource` metoda. Pokud rozšíření se provádí u souboru, je nutné volat `StopAccessingSecurityScopedResource` k uvolnění připojení.

### <a name="adding-custom-actions-to-a-custom-ui"></a>Přidání vlastních akcí, které vlastní uživatelského rozhraní

Jak jsme uvedli výše, rozhraní oznámení nepodporuje interakci s uživatelem, například textová pole nebo tlačítka – ovládací prvky nejsou podporovány v návrhu uživatelského rozhraní.

Místo toho mechanismus standardní vlastní akce se používá k přidání interaktivity na vlastní uživatelské rozhraní oznámení. Najdete v článku [práce s akcí oznámení](~/ios/platform/user-notifications/enhanced-user-notifications.md) části [rozšířené oznámení uživateli](~/ios/platform/user-notifications/enhanced-user-notifications.md) dokumentu další podrobnosti o vlastní akce.

Kromě vlastní akce obsahu rozšíření oznámení může reagovat také následující vestavěné akce:

- **Výchozí akce** – to je, když uživatel klepnutím oznámení otevřete aplikaci a zobrazit podrobnosti o daném oznámení.
- **Zrušit akci** – tato akce je odeslána do aplikace, pokud uživatel zavře daného oznámení.

Oznámení obsahu rozšíření také mít možnost aktualizovat své uživatelské rozhraní, když uživatel vyvolá jednu z vlastních akcí, jako je například zobrazující datum přijetí, kdy uživatel klepnutím **přijmout** tlačítko Vlastní akce. Kromě toho obsahu rozšíření oznámení poznáte systému zpoždění propouštění UI oznámení, uživatel uvidí účinku akci před oznámení je uzavřený.

K tomu je potřeba implementace z druhé verze `DidReceiveNotification` metoda, která zahrnuje obslužnou rutinu dokončení. Příklad:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;
using CoreGraphics;

namespace myApp {
    public class NotificationViewController : UIViewController, UNNotificationContentExtension {

        public override void ViewDidLoad() {
            base.ViewDidLoad();

            // Adjust the size of the content area
            var size = View.Bounds.Size
            PreferredContentSize = new CGSize(size.Width, size.Width/2);
        }

        public void DidReceiveNotification(UNNotification notification) {

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = Content.UserInfo["location"] as string;
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments[0];
                if (attachment.Url.StartAccessingSecurityScopedResource()) {
                    EventImage.Image = UIImage.FromFile(attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch(response.ActionIdentifier){
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
    }
}
```

Přidáním `Server.PostEventResponse` obslužná rutina `DidReceiveNotification` metoda oznámení obsahu rozšíření rozšíření *musí* zpracovat všechny vlastní akce. Rozšíření můžete dál vlastní akce k aplikaci obsahující změnou `UNNotificationContentExtensionResponseOption`. Příklad:

```csharp
// Close Notification
completionHandler (UNNotificationContentExtensionResponseOption.DismissAndForwardAction);
```

### <a name="working-with-the-text-input-action-in-custom-ui"></a>Práce s akce vstupní Text ve vlastní uživatelského rozhraní

V závislosti na návrh aplikace a oznámení může být pokusů, které vyžadují, aby uživatel zadal text do oznámení (například odpovědi na zprávu). Příponu obsahu oznámení má přístup k vstupní akce integrovaný text, stejně jako standardní oznámení nepodporuje.

Příklad:

```csharp
using System;
using Foundation;
using UIKit;
using UserNotifications;
using UserNotificationsUI;


namespace MonkeyChatNotifyExtension
{
    public partial class NotificationViewController : UIViewController, IUNNotificationContentExtension
    {
        #region Computed Properties
        // Allow to take input
        public override bool CanBecomeFirstResponder {
            get { return true; }
        }

        // Return the custom created text input view with the
        // required buttons and return here
        public override UIView InputAccessoryView {
            get { return InputView; }
        }
        #endregion

        #region Constructors
        protected NotificationViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Do any required interface initialization here.
        }
        #endregion

        #region Private Methods
        private UNNotificationCategory MakeExtensionCategory ()
        {

            // Create Accept Action
            ...

            // Create decline Action
            ...

            // Create Text Input Action
            var commentID = "comment";
            var commentTitle = "Comment";
            var textInputButtonTitle = "Send";
            var textInputPlaceholder = "Enter comment here...";
            var commentAction = UNTextInputNotificationAction.FromIdentifier (commentID, commentTitle, UNNotificationActionOptions.None, textInputButtonTitle, textInputPlaceholder);

            // Create category
            var categoryID = "event-invite";
            var actions = new UNNotificationAction [] { acceptAction, declineAction, commentAction };
            var intentIDs = new string [] { };
            var category = UNNotificationCategory.FromIdentifier (categoryID, actions, intentIDs, UNNotificationCategoryOptions.None);

            // Return new category
            return category;

        }
        #endregion

        #region Public Methods
        [Export ("didReceiveNotification:")]
        public void DidReceiveNotification (UNNotification notification)
        {
            label.Text = notification.Request.Content.Body;

            // Grab content
            var content = notification.Request.Content;

            // Display content in the UILabels
            EventTitle.Text = content.Title;
            EventDate.Text = content.Subtitle;
            EventMessage.Text = content.Body;

            // Get location and display
            var location = content.UserInfo ["location"].ToString ();
            if (location != null) {
                Event.Location.Text = location;
            }

            // Get Media Attachment
            if (content.Attachements.Length > 1) {
                var attachment = content.Attachments [0];
                if (attachment.Url.StartAccessingSecurityScopedResource ()) {
                    EventImage.Image = UIImage.FromFile (attachment.Url.Path);
                    attachment.Url.StopAccessingSecurityScopedResource ();
                }
            }
        }

        [Export ("didReceiveNotificationResponse:completionHandler:")]
        public void DidReceiveNotification (UNNotificationResponse response, Action<UNNotificationContentExtensionResponseOption> completionHandler)
        {

            // Is text input?
            if (response is UNTextInputNotificationResponse) {
                var textResponse = response as UNTextInputNotificationResponse;
                Server.Send (textResponse.UserText, () => {
                    // Close Notification
                    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
                });
            }

            // Update UI when the user interacts with the
            // Notification
            Server.PostEventResponse += (response) {
                // Take action based on the response
                switch (response.ActionIdentifier) {
                case "accept":
                    EventResponse.Text = "Going!";
                    EventResponse.TextColor = UIColor.Green;
                    break;
                case "decline":
                    EventResponse.Text = "Not Going.";
                    EventResponse.TextColor = UIColor.Red;
                    break;
                }

                // Close Notification
                completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
            };
        }
        #endregion
    }
}
```

Tento kód vytvoří novou akci vstupní text a přidává ji k rozšíření kategorie (v `MakeExtensionCategory`) metoda. V `DidReceive` potlačí metodu, zpracovává uživatele při zadávání textu s následujícím kódem:

```csharp
// Is text input?
if (response is UNTextInputNotificationResponse) {
    var textResponse = response as UNTextInputNotificationResponse;
    Server.Send (textResponse.UserText, () => {
        // Close Notification
        completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);
    });
}
```

Volá-li návrh pro přidání do vstupní textové pole vlastních tlačítek, přidejte následující kód a zahrnout je do:

```csharp
// Allow to take input
public override bool CanBecomeFirstResponder {
    get {return true;}
}

// Return the custom created text input view with the
// required buttons and return here
public override UIView InputAccessoryView {
    get {return InputView;}
}
```

Při aktivaci akce komentář uživatelem řadiče zobrazení a vstupní pole vlastní text potřeba aktivovat:

```csharp
// Update UI when the user interacts with the
// Notification
Server.PostEventResponse += (response) {
    // Take action based on the response
    switch(response.ActionIdentifier){
    ...
    case "comment":
        BecomeFirstResponder();
        TextField.BecomeFirstResponder();
        break;
    }

    // Close Notification
    completionHandler (UNNotificationContentExtensionResponseOption.Dismiss);

};
```

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek provedlo v návaznosti pokročilé pohled na použití nové architektury oznámení pro uživatele v aplikaci Xamarin.iOS. O něm zmínka přidávání příloh média pro místní i vzdálené oznámení a o něm zmínka pomocí uživatelského rozhraní nové oznámení uživatele Chcete-li vytvořit vlastní uživatelská oznámení.


## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [UserNotifications Framework – referenční informace](https://developer.apple.com/reference/usernotifications)
- [UserNotificationsUI](https://developer.apple.com/reference/usernotificationsui)
- [Průvodce programováním místních a vzdálených oznámení](https://developer.apple.com/library/prerelease/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/Introduction.html)
