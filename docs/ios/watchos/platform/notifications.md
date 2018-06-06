---
title: watchOS oznámení v Xamarinu
description: Tento dokument popisuje, jak pracovat s watchOS oznámení v Xamarin. Popisuje vytváření oznámení řadiče, generování oznámení a testování oznámení.
ms.prod: xamarin
ms.assetid: 0BC1306E-0713-4592-996E-7530CCF281E7
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 42b0354f19a9e0c31b7a859d598526fddad726cd
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34791907"
---
# <a name="watchos-notifications-in-xamarin"></a>watchOS oznámení v Xamarinu

Podívejte se, že aplikace může přijímat oznámení, pokud je podporuje obsahující aplikaci pro iOS. Je integrované oznámení zpracování, takže ho použít nechcete *potřebovat* přidání podpory další oznámení popsané dál, ale pokud chcete přizpůsobit oznámení chování a vzhled pak přečte na.

Odkazovat [oznámení iOS](~/ios/platform/user-notifications/deprecated/index.md) doc Další informace o přidání podpory oznámení do aplikace pro iOS ve vašem řešení.

## <a name="creating-notification-controllers"></a>Vytvoření oznámení řadičů

Ve scénáři mají řadiče oznámení o zvláštní typ segue je aktivován. Při přetažení novou **řadič rozhraní oznámení** na scénář bude automaticky mít segue, připojené:

![](notifications-images/notification-storyboard1.png "Rozhraní řadič nové oznámení s segue, připojit")

Když segue oznámení je vybrána můžete upravit jeho vlastnosti:

![](notifications-images/notification-storyboard2.png "Oznámení segue vybrané")

Po přizpůsobení řadičem bude vypadat podobně jako tento příklad z WatchKitCatalog:

![](notifications-images/notifications-segue.png "Vlastnosti oznámení")


Existují dva typy oznámení:

- **Vzhled krátké** -neposouvatelný statické zobrazení definovaná systémem.

- **Long vzhled** – posouvatelného, přizpůsobitelné zobrazení, které jste definovali! Lze zadat jednodušší, statické verze a složitější dynamické verze.

### <a name="short-look-notification-controller"></a>Řadič krátké vzhled oznámení

Vzhled krátké uživatelského rozhraní se skládá z ikonu aplikace, název aplikace a string title oznámení.

Pokud uživatel neignoruje oznámení, systém bude automaticky přepínat dlouho vzhled oznámení, která poskytuje další informace.


### <a name="long-look-notification-controller"></a>Long – vzhled řadiče oznámení

Operačního systému rozhodne, jestli se má zobrazit statickou nebo dynamickou podle počtu faktorů. Musíte poskytnout statické rozhraní a volitelně můžete také zahrnovat dynamická rozhraní pro oznámení.

#### <a name="static"></a>Static

Statické zobrazení musí být jednoduchý a rychle zobrazit.

![](notifications-images/notification-static.png "Statické zobrazení")

#### <a name="dynamic"></a>dynamické

Dynamické zobrazení můžete zobrazit další data a poskytovat další interaktivity.

![](notifications-images/notification-dynamic.png "Dynamické zobrazení")


## <a name="generating-notifications"></a>Generování oznámení

Oznámení mohou pocházet ze vzdáleného serveru ([službu nabízených oznámení Apple](https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html), nebo APNS) nebo může být generována místně v aplikaci pro iOS.

Odkazovat [iOS oznámení návod](~/ios/platform/user-notifications/deprecated/local-notifications-in-ios-walkthrough.md) příklad toho, jak vygenerovat místní oznámení a [WatchNotifications ukázka](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/) například pracovní.

Musí mít místní oznámení `AlertTitle` nastavit zobrazený na Apple Watch - `AlertTitle` řetězec se zobrazí v krátké vzhled rozhraní. Jak `AlertTitle` a `AlertBody` se zobrazí v seznamu oznámení a `AlertBody` se zobrazí v rozhraní vzhled dlouho.

Tento snímek obrazovky ukazuje `AlertTitle` se zobrazí v seznamu oznámení a `AlertBody` zobrazené na rozhraní dlouho vzhled (pomocí [ukázkový kód](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)):

![](notifications-images/watch-notificationslist-sml.png "Tento snímek obrazovky ukazuje AlertTitle se zobrazí v seznamu oznámení") ![ ] (notifications-images/watch-notificationcontroller-sml.png "AlertBody zobrazené na rozhraní dlouho vzhledu")

## <a name="testing-notifications"></a>Testování oznámení

Oznámení (místní i vzdálený) lze pouze správně testovat na zařízení, ale jejich můžete simulated pomocí **.json** souboru v simulátoru iOS.

### <a name="testing-on-apple-watch"></a>Testování v Apple Watch

Při testování oznámení na Apple Watch, nezapomeňte, že [dokumentaci společnosti Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html) následující stavy:

> Pokud jeden z vaší aplikace místního nebo vzdáleného oznámení dorazí na uživatele zařízení iPhone, jestli se mají zobrazovat oznámení na iPhone nebo Apple Watch iOS rozhodne.

To je alluding na skutečnost, že iOS rozhodne, zda se zobrazí oznámení, na iPhone nebo sledován. Pokud spárované iPhone nejsou aktivní při přijetí oznámení, oznámení, je pravděpodobné, který se má zobrazit pro iPhone a *není* směrovány do hodinek.

Zajistit, že se zobrazí oznámení na hodinek vypnout obrazovku iPhone (stisknutím tlačítka napájení jednou), nebo nechte ji přejít do režimu spánku. Pokud spárované sledování je v rozsahu, výkon a je právě použité na zápěstí, existuje, budou směrovány oznámení a zobrazí na sledování (spolu jemně).

### <a name="testing-on-the-ios-simulator"></a>Testování v simulátoru iOS

Můžete *musí* zadejte datovou část JSON test při testování režim oznámení v simulátoru iOS. Nastavení cesty **vlastní provádění argumenty** oken v sadě Visual Studio for Mac.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

Visual Studio pro Mac se zobrazí další možnosti sledování rozšíření jako nastavena **spouštěný projekt**.
Klikněte pravým tlačítkem na projekt rozšíření sledování a zvolte **spustit s > vlastní parametry...** :
    
[![](notifications-images/runwith-customparams-sml.png "Běh vlastní vlastnosti")](notifications-images/runwith-customparams.png#lightbox)
    
Tím se otevře **provádění argumenty** okna, který obsahuje **WatchKit** kartě. Vyberte **oznámení** a zadejte datovou část JSON, stiskněte **Execute** a spusťte aplikaci sledovat v simulátoru:
    
[![](notifications-images/runwith-execargs-sml.png "Vyberte výchozí datová část oznámení")](notifications-images/runwith-execargs.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Nastavit datová část oznámení testů v sadě Visual Studio klikněte pravým tlačítkem na rozšíření sledovat upravit **projektu vlastnosti**. Přejděte na **ladění** a vyberte soubor JSON oznámení ze seznamu (jej bude automaticky všechny soubory JSON zahrnutý v projektu).
    
[![](notifications-images/runwith-execargs-sml-vs.png "Vyberte soubor JSON oznámení")](notifications-images/runwith-execargs-vs.png#lightbox)

Pokud je rozšíření sledovat **spouštěný projekt**, Visual Studio se zobrazí další možnosti, jak je uvedeno níže. Vyberte jednu z **oznámení** spusťte aplikaci sledovat **oznámení** režimu (pomocí souboru JSON, který je vybrán v okně Vlastnosti):
    
![](notifications-images/runwith-vs.png "V nabídce zařízení")

-----

Řadičem výchozí oznámení vypadá takto při testování v simulátoru výchozím souborem datové části JSON:

![](notifications-images/notification-debug-sml.png "Oznámení o příklad")

Je také možné použít [příkazového řádku](~/ios/watchos/troubleshooting.md#command_line) zahájíte simulátoru iOS.

### <a name="example-notification-payload"></a>Datová část oznámení příklad

V [sledovat Kit katalogu](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/) ukázka existuje je soubor příklad datové části JSON **NotificationPayload.json** (uvedené níže).

```csharp
{
    "aps": {
        "alert": "Test message content",
        "title": "Optional title",
        "category": "myCategory"
        },

        "WatchKit Simulator Actions": [
        {
            "title": "First Button",
            "identifier": "firstButtonAction"
        }
        ],

        "customKey": "Use this file to define a testing payload for your notifications. The aps dictionary specifies the category, alert text and title. The WatchKit Simulator Actions array can provide info for one or more action buttons in addition to the standard Dismiss button. Any other top level keys are custom payload. If you have multiple such JSON files in your project, you'll be able to choose between them in when selecting to debug the notification interface of your Watch App."
    }
```



## <a name="related-links"></a>Související odkazy

- [WatchNotifications (místního oznámení) (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchNotifications/)
- [WatchKitCatalog (ukázka)](https://developer.xamarin.com/samples/monotouch/WatchKit/WatchKitCatalog/)
- [Dokumenty sledovat Kit oznámení Apple](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/BasicSupport.html)
