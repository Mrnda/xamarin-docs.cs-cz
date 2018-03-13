---
title: "Aktualizaci aplikace na pozadí"
ms.topic: article
ms.prod: xamarin
ms.assetid: A2B2231A-C045-4C11-8176-F9966485197A
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/18/2017
ms.openlocfilehash: f4a18bf8f35d1a6c615c819ea90433d1eb123422
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="updating-an-application-in-the-background"></a>Aktualizaci aplikace na pozadí

Aktualizace na pozadí je proces probuzení aplikaci, která je pozastaven nebo není spuštěn a aktualizace pomocí nového obsahu. iOS poskytuje tři možnosti pro aktualizaci obsahu na pozadí:

1.  *Monitorování oblast* a *významné změny služby umístění* -pozadí aktivační událost umístění rozhraní API se aktualizuje na základě změny v umístění uživatele. Tato rozhraní API umožňuje orientačně aktualizovat obsah v aplikacích nezaložené umístění iOS 6, kde nejsou k dispozici další možnosti.
1.  *Načtení (iOS 7 +) na pozadí* -dočasný přístup k aktualizaci *nekritické* obsah, který aktualizuje *často* .
1.  *Vzdálená oznámení (iOS 7 +)* – aplikace, které přijímat nabízená oznámení můžete použít oznámení k aktivaci obsah aktualizací na pozadí. Tuto metodu můžete použít k aktualizaci s *důležité, dobou* obsah, který aktualizuje *k* .


Následující části se věnují základní informace o těchto možností.

## <a name="region-monitoring-and-significant-location-changes"></a>Oblasti monitorování a umístění významné změny

iOS poskytuje dvě umístění rozhraní API s backgrounding možnosti:

1.  *Oblast monitorování* je proces nastavení oblasti hranic a probuzení zařízení, když uživatel zadá nebo ji opustí oblast. Oblasti jsou cyklické a může mít různou velikost. Když uživatel překračuje hranici oblast, zařízení bude probuzení se obvykle zpracovat událost, která iniciovala oznámení nebo zahájena úloha. Oblast monitorování vyžaduje GPS a zvyšuje baterie a využití dat.
1.  *Významné změny služby umístění* je jednodušší, uspořit power možnost k dispozici pro zařízení s mobilní kombinace. Aplikace, která naslouchá na umístění významné změny budete upozorněni, když zařízení přejde towers buňky. Tato služba slouží k probuzení pozastavená nebo ukončené aplikace a poskytuje možnost zkontrolovat nový obsah na pozadí. Aktivita na pozadí je omezený na asi 10 sekund, pokud byl spárován s [úlohy na pozadí](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/ios-backgrounding-with-tasks.md) .


Aplikace nemusí umístění `UIBackgroundMode` používat tato umístění rozhraní API. Protože iOS nesleduje typy úloh, které může spustit, když je zařízení probuzený změny v umístění uživatele, tato rozhraní API nabízejí řešení pro aktualizaci obsahu na pozadí v systému iOS 6. *Mějte na paměti, že spuštění aktualizace na pozadí na základě polohy rozhraní API obrátí na zdroje informací pro zařízení a může zmást uživatele, kteří nejsou pochopit, proč aplikace vyžaduje přístup k jejich umístění*. Pomocí vlastního rozhodnutí při implementaci monitorování oblasti nebo významné změny umístění pro zpracování v aplikacích, které se již nepoužívají umístění rozhraní API na pozadí.

Aplikace pomocí umístění monitorování pro zpracování na pozadí zpřístupnit Chyba v iOS 6: Nemáte-li aplikace potřebám do pozadí potřeby kategorie, má omezený backgrounding možnosti. Se zavedením dvou nových rozhraní API *pozadí načíst* a *vzdáleného oznámení*, iOS 7 (nebo vyšší) poskytuje backgrounding příležitostí do dalších aplikací. V následujících dvou částech zavést tato nová rozhraní API.

<a name="background_fetch" />

## <a name="background-fetch-ios-7-and-greater"></a>Načítání na pozadí (iOS 7 a vyšší)

V iOS 6 potřeba aplikace zadávání popředí čas načíst nový obsah, uživatelé stručně prezentuje obsah, který jste už viděli. Načítání na pozadí umožňuje aplikacím načíst nová data *před* uživatel spustí aplikaci a poskytnout uživateli nejaktuálnější obsah.

Chcete-li implementovat načítání na pozadí, upravte *Info.plist* a zkontrolujte **povolit režimy pozadí** a **načítání na pozadí** zaškrtávací políčka:

 [![](updating-an-application-in-the-background-images/fetch.png "Upravit Info.plist a zaškrtněte políčko Povolit režimy pozadí a načíst pozadí")](updating-an-application-in-the-background-images/fetch.png#lightbox)

Vedle `AppDelegate`, přepsat `FinishedLaunching` metodu a nastavit minimální načítání intervalu. V tomto příkladu jsme mohli rozhodnout, jak často se načíst nový obsah operačního systému:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
  UIApplication.SharedApplication.SetMinimumBackgroundFetchInterval (UIApplication.BackgroundFetchIntervalMinimum);
  return true;
}
```

Nakonec načítání provést přepsání `PerformFetch` metoda v `AppDelegate`a předejte *obslužná rutina dokončení*. Obslužná rutina dokončení je delegáta, který přebírá `UIBackgroundFetchResult`:

```csharp
public override void PerformFetch (UIApplication application, Action<UIBackgroundFetchResult> completionHandler)
{
  // Check for new data, and display it
  ...
  
  // Inform system of fetch results
  completionHandler (UIBackgroundFetchResult.NewData);
}
```

Když máme Hotovo aktualizace obsahu, nám umožní vědět voláním dokončení obslužná rutina s příslušnou stav operačního systému. iOS nabízí tři možnosti, stav dokončení obslužné rutiny:

1.  `UIBackgroundFetchResult.NewData` – Voláno, pokud získána nový obsah a aplikace se aktualizovalo.
1.  `UIBackgroundFetchResult.NoData` – Voláno, pokud došlo k načtení pro nový obsah, ale je k dispozici žádný obsah.
1.  `UIBackgroundFetchResult.Failed` -Užitečné pro zpracování chyb, to je volána, když načítání se nepodařilo projít.


Aplikace, které používají načíst pozadí můžete provádět volání aktualizace uživatelského rozhraní z na pozadí. Když uživatel otevře aplikaci, uživatelské rozhraní bude až datum a zobrazení nového obsahu. Tím dojde také k aktualizaci aplikace aplikace přepínači snímek, uživatel uvidí, když aplikace má nový obsah.

> [!IMPORTANT]
> **Poznámka:**: jednou `PerformFetch` je volána, aplikace má ji stáhnout nový obsah a volání obslužné rutiny blok dokončení přibližně 30 sekund. Pokud to trvá příliš dlouho, aplikace se ukončí. Zvažte použití pozadí načíst pomocí _služba přenosu na pozadí_ při stahování média nebo další velké soubory.


### <a name="backgroundfetchinterval"></a>BackgroundFetchInterval

Ve výše uvedeném ukázkovém kódu, nám umožní operačního systému určete, jak často chcete načíst nový obsah nastavením minimální načítání interval na `BackgroundFetchIntervalMinimum`. iOS nabízí tři možnosti pro načtení interval:

1.  `BackgroundFetchIntervalNever` -Oznamují systému nikdy načíst nový obsah. Použijte tento vypnout načítání v určitých situacích, například když není přihlášený uživatel. Toto je výchozí hodnota pro interval načtení. 
1.  `BackgroundFetchIntervalMinimum` – Umožňuje systému rozhodnout, jak často se načíst na základě uživatelské vzory, z baterie, využití dat a potřeby jiné aplikace.
1.  `BackgroundFetchIntervalCustom` – Pokud víte, jak často aktualizuje obsah aplikace, můžete zadat interval "spánku" po každé načítání, během kterého aplikace nebudou moci načítání nový obsah. Po tomto intervalu až systému kdy načíst obsah.


Obě `BackgroundFetchIntervalMinimum` a `BackgroundFetchIntervalCustom` spoléhají na systém a naplánovat načtení. Tento interval je dynamický, přizpůsobení musí zařízení a také zvyklosti jednotlivé uživatele. Například pokud jeden uživatel ověří aplikace každé ráno, a další kontroly každou hodinu iOS zajistí obsah je aktuální pro oba uživatele při každém otevření aplikace.

Pro aplikace, které často aktualizovat nekritické obsahu je třeba použít načítání na pozadí. Pro aplikace s důležitými aktualizacemi je třeba použít vzdálené oznámení. Vzdálená oznámení jsou založené na pozadí načíst a sdílet stejnou obslužná rutina dokončení. Jsme budete ponořte do vzdáleného oznámení dále.

 <a name="remote_notifications" />


## <a name="remote-notifications-ios-7-and-greater"></a>Vzdálená oznámení (iOS 7 a vyšší)

Nabízená oznámení jsou JSON zpráv odeslaných z poskytovatele na zařízení prostřednictvím *služby Apple Push Notification (APNs)*.

Příchozí nabízená oznámení v iOS 6, říká systému upozornit uživatele, který něčeho zajímavého došlo v aplikaci. Kliknutím na oznámení vrátí aplikaci ze stavu pozastavená nebo ukončené a aplikace by zahájení aktualizace obsahu. iOS 7 (nebo vyšší) rozšiřuje obyčejnou nabízená oznámení tím, že aplikace šance aktualizovat obsah na pozadí *před* upozornění uživatele, aby uživatel může otevřít aplikaci a bude nabídnuta nový obsah okamžitě.

Chcete-li implementovat vzdáleného oznámení, upravte *Info.plist* a zkontrolujte **povolit režimy pozadí** a **vzdáleného oznámení** zaškrtávací políčka:

 [![](updating-an-application-in-the-background-images/remote.png "Režim pozadí nastaven na možnost povolit režimy pozadí a vzdáleného oznámení")](updating-an-application-in-the-background-images/remote.png#lightbox)

Dále nastavte `content-available` příznak na nabízené oznámení sám sebe na 1. To umožní aplikaci vědět, abyste mohli načíst nový obsah před zobrazením výstrahy:

```csharp
'aps' {
  'content-available': 1,
  'alert': 'Something new has happened in your app!''
}
```

V *AppDelegate*, přepsat `DidReceiveRemoteNotification` metoda kontrolovat datová část oznámení pro dostupný obsah, a volání bloku odpovídající dokončení obslužné rutiny:

```csharp
public override void DidReceiveRemoteNotification (UIApplication application, NSDictionary userInfo, Action<UIBackgroundFetchResult> completionHandler)
{
  if([content-available]) {
    // fetch content
    completionHandler (UIBackgroundFetchResult.NewData);
  }
}
```

Vzdálená oznámení je třeba použít jen zřídka aktualizací s obsahem, který je zásadní význam pro funkce aplikace. Další informace o vzdálené oznámení najdete v tématu Xamarin [nabízených oznámení v iOS](~/ios/platform/user-notifications/deprecated/remote-notifications-in-ios.md) průvodce.

> [!IMPORTANT]
> **Poznámka:**: protože mechanismus aktualizace v vzdáleného oznámení je založen na pozadí načíst, musíte ji stáhnout nový obsah a volání obslužné rutiny blok dokončení během 30 sekund pro příjem oznámení aplikace nebo iOS bude Ukončete aplikaci. Vezměte v úvahu párování vzdáleného oznámení s _služba přenosu na pozadí_ při stahování média nebo další velké soubory na pozadí.


### <a name="silent-remote-notifications"></a>Tichou vzdáleného oznámení

Vzdálená oznámení jsou jednoduchý způsob, jak upozornit aplikace aktualizací a ji načítání nový obsah, ale existují případy, kde společnost Microsoft nepotřebuje upozornění uživatele, že došlo ke změně. Například pokud uživatel flags soubor pro synchronizace, jsme nemusíte je upozornění pokaždé, když se soubor aktualizace. Synchronizace souboru není překvapivé událostí, ani nevyžaduje okamžitou pozornost uživatele. Uživatelé očekávají právě, že byl soubor aktuální po jeho otevření.

Případech podobný jako výše iOS povoluje nabízená oznámení k odeslání bezobslužně – to znamená, bez upozornění. Chcete-li regulární oznámení do tichou jeden, jednoduše odebrání výstrahy ze datová část oznámení:

```csharp
'aps' {
  'content-available': 1
}
```

#### <a name="rate-limits"></a>Omezení přenosové rychlosti

Největší rozdíl mezi běžné a tichou oznámení z hlediska vývojáře je, že tichou nabízených oznámení míra omezené. APNs bude zpoždění doručení tichou nabízených oznámení do zařízení, pokud rychlost nabízených získá příliš vysoká. To je potřeba zajistit, že aplikace není vyprazdňování zařízení prostředky s příliš mnoha tichou oznámení.

APNs však vám umožní tichou oznámení "přilepí" spolu s normální vzdáleného oznámení nebo udržování odpovědi. Protože pravidelných oznámení není míra omezená, můžete používají pro nabízená oznámení uložené až tichou z APNs na zařízení, které jsou popsány v následujícím diagramu:

 [![](updating-an-application-in-the-background-images/silent.png "Pravidelných oznámení lze použít k replikaci uložené tichou oznámení z APNs na zařízení, které jsou popsány v tomto diagramu")](updating-an-application-in-the-background-images/silent.png#lightbox)

> [!IMPORTANT]
> **Poznámka:**: Apple umožňuje vývojářům odesílat tichou nabízená oznámení vždy, když aplikace vyžaduje, a umožňují APNs naplánovat jejich doručování.


V této části jsme jste popsané různé možnosti pro aktualizaci obsahu na pozadí ke spuštění úlohy, které se nehodí do pozadí potřeby kategorie. Nyní se podívejme se některé z těchto rozhraní API v akci.

 [Další krok: Část 4 - iOS Backgrounding návody](~/ios/app-fundamentals/backgrounding/ios-backgrounding-walkthroughs/index.md)
