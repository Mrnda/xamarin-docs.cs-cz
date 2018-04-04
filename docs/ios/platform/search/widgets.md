---
title: Hledání a vylepšení pomůcky domovskou obrazovku
description: Tento článek se zabývá vylepšení provedena Apple systém pomůcka v iOS 10.
ms.prod: xamarin
ms.assetid: D66FD9E1-9E23-4BB6-825C-ED19B8F72A81
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: e7a64738f29ab2b5c62659d721beb50db7c9adb5
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="search-and-home-screen-widget-enhancements"></a>Hledání a vylepšení pomůcky domovskou obrazovku

_Tento článek se zabývá vylepšení provedena Apple systém pomůcka v iOS 10._


Společnost Apple má několik vylepšení systému pomůcky a ujistěte se, že widgetů hledat skvělé na všechny pozadí, která existuje v nové iOS 10 zamykací obrazovka vydala. Kromě toho nyní obsahují pomůcky [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) vlastnost, která umožňuje vývojáři popisují, kolik obsah je dostupný a umožňuje uživateli rozbalit nebo sbalit obsah.

Pomůcky (také označované jako dnes rozšíření) jsou speciální typ iOS rozšíření, která se malé množství užitečné informace nebo vystavit funkcionalitu konkrétní aplikaci včas. Například pomůcku, která znázorňuje hlavní titulky má zprávy aplikace a aplikace kalendáře poskytuje dvě různé pomůcky: jednu k zobrazení dnešní události a jeden, který zobrazit nadcházející události.

Pomůcky jsou vysoce přizpůsobitelné a může obsahovat prvky uživatelského rozhraní, jako je text, obrázky, tlačítka, atd. Vývojář můžete dále přizpůsobit rozložení jejich pomůcky.

[![](widgets-images/widgets01.png "Příklad pomůcky")](widgets-images/widgets01.png#lightbox)

Existují dvě hlavní míst, na které může uživatel zobrazení a interakci s aplikace pomůcky:

- **Na obrazovce vyhledávání** -uživatele můžete přidat pomůcky najdou nejužitečnější na obrazovku jejich vyhledávání. Na obrazovce vyhledávání přistupuje potáhnutím pravé na domovské i zámku obrazovky.
- **Na obrazovce Domů** -z Domů obrazovky, uživatel může použít 3D Touch otevřete seznam rychlé akce použitím tlak na ikonu aplikace. Pomůcky aplikace se zobrazí nad seznamu Rychlé akce. Najdete v tématu naše [Úvod do 3D Touch](~/ios/platform/3d-touch.md) Další informace naleznete v dokumentaci.

## <a name="widgets-developer-suggestions"></a>Návrhy vývojáře pomůcky

V ideálním případě by měl vývojář vždy zkuste a navrhněte pomůcky, které uživatel bude chcete přidat do svých vyhledávání obrazovkách. Za tímto účelem Společnost Apple má následující návrhy:

- **Vytvoření dobře, přehledné prostředí** -uživatele chcete pomůcky, které poskytují stručný, přehledné informace o počtu aktualizací stavu nebo mohly rychle provádět jednoduché úlohy. Díky poskytování správného množství informací a interakce nezbytné. Pokud je to možné, povolí uživateli provést danou úlohu s jedním klepnutím. Navíc vzhledem k tomu, že pomůcky nepodporuje posouvání nebo posouvání, to muset vzít v úvahu při návrhu ovládacího prvku.
- **Rychle zobrazit obsah** -pomůcky jsou navržené tak, aby přehledné, uživatel nikdy nemusí čekat na obsah načíst, jakmile se zobrazí Widget. Pomůcky by jejich ukládat obsah do mezipaměti místně, tak při obsah načítání na pozadí se může zobrazit poslední obsah.
- **Zadejte odpovídající odsazení a okraje** -pomůcky nikdy by měl vypadat frekventované, tak vyhnout rozšíření obsah k okrajům zobrazení ovládacího prvku. Vždy by měl být několik pixelů široké okraje mezi okrajů a obsah. Apple najdete zde také doporučení pomocí ikony aplikace, zobrazí v horní části widgetu jako vodítko zarovnání. Pokud widgetu uvede rozložení mřížky, zkontrolujte, zda je správný odsazení mezi položky v mřížce a zkuste omezit počet položek má čtyři max.
- **Použít přizpůsobivé rozložení** – šířka pomůcky A budou lišit v závislosti na zařízení se systémem a orientace zařízení. Výška pomůcky mohou lišit na základě Pokud se zobrazily v sbalená (výchozí) nebo rozšířené (nepodporuje všechny pomůcky) stavu. Widget sbalené má výšku řádky tabulky zhruba dvě a půl standardní iOS. Vývojář může požádat o velikosti rozšířit pomůcky, ale v ideálním případě ji musí být menší než výška obrazovky. Ve stavu sbalená widgetu by měl zobrazit jenom základní, samostatné informace. Pokud rozšířené, widgetu by měl zobrazit dodatečné informace, které rozšiřuje primární obsah ukazuje stav sbalená. Pomůcky uvedené v seznamu Rychlé akce bude pouze ve stavu sbalená.
- **Nemáte přizpůsobit pozadí ovládacího prvku** -pomůcky se zobrazují na pozadí světla, rozostřených poskytované systémem. To slouží k převedení konzistenci mezi pomůcky a zlepšení čitelnosti obsahu. Vyhněte se používání bitovou kopii jako pozadí pomůcky, protože může kolidovat s tapety uzamčení a domů obrazovky uživatele.
- **Použít písmo systému černá nebo tmavý šedá** – při zobrazení textu v Widget doporučené písmo systému funguje. Písma musí být ve použitelný pozadí pomůcky světla, rozostřených černé nebo tmavý šedou barvu.
- **Poskytují aplikace přístup po odpovídající** -ovládacího prvku vždy samostatně pracovat ze své aplikace, ale pokud hlubší funkčnost je potřeba, widgetu by měly být moct spustit aplikaci zobrazte nebo upravte konkrétní informace. Nikdy nezahrne tlačítko "otevřete aplikaci", jednoduše umožnit uživatelům klepněte na samotný obsah a nikdy otevřete 3. stran aplikace.
- **Vyberte zaškrtnutí, stručným název pomůcky** – ikona aplikace a název ovládacího prvku se zobrazují vždy přes obsah ovládacího prvku. Apple navrhuje pomocí názvu aplikace jeho primární pomůcky a jasným a přesným metrikám název všech ostatních, které poskytuje. Pokud poskytuje vlastní název pomůcky, by měla obsahovat předponu s názvem aplikace (například mapy blízkým restaurace mapy, atd.).
- **Informovat o tom, kdy ověřování přidá hodnotu** – v případě dalších funkcí nebo informace o je k dispozici, pouze když je uživatel ověřený a to přihlášeni, k dispozici pro uživatele. Například pravé aplikace pro sdílení Moje indikované "Přihlášení do adresáře pravé".
- **Vyberte Widget seznamu Rychlé akce** – Pokud aplikace obsahuje více než jeden pomůcky, vývojáři měli vybrat tu, k dispozici, když uživatel otevře seznamu Rychlé akce použitím tlak na ikonu aplikace pomocí 3D Touch.

Další informace o práci s pomůcky, najdete v tématu naše [Úvod do rozšíření](~/ios/platform/extensions.md), [Úvod do 3D Touch](~/ios/platform/3d-touch.md) dokumentace a společnosti Apple [aplikace Průvodce programováním rozšíření](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html).

## <a name="working-with-vibrancy"></a>Práce s sytější

Sytější zajistí, že text pomůcky zůstává čitelná při na light ovládacího prvku, rozostřeného pozadí (zadaný v systému). Před iOS 10, využije Vývojář [NotificationCenterVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1613917-notificationcentervibrancyeffect) pro sytější ovládacího prvku. Příklad:

```csharp
// DEPRECATED: Get Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreateForNotificationCenter ();
```

To se v iOS 10 nepoužívá a měl by být nahrazen buď [WidgetPrimaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771278-widgetprimaryvibrancyeffect) nebo [WidgetSecondaryVibrancyEffect](https://developer.apple.com/reference/uikit/uivibrancyeffect/1771277-widgetsecondaryvibrancyeffect). Příklad:

```csharp
// Get Primary Widget Vibrancy Effect
var vibrancy = UIVibrancyEffect.CreatePrimaryVibrancyEffectForNotificationCenter ();

// Get Secondary Widget Vibrancy Effect
var vibrancy2 = UIVibrancyEffect.CreateSecondaryVibrancyEffectForNotificationCenter ();
```

## <a name="working-with-collapsed-and-expanded-widgets"></a>Práce s sbalené a rozšířená pomůcky

Nové na iOS 10 pomůcky teď obsahují [NCWidgetDisplayMode](https://developer.apple.com/reference/notificationcenter/ncwidgetdisplaymode) vlastnost, která umožňuje vývojáři popisují, kolik obsah je dostupný a umožňuje uživateli rozbalit nebo sbalit obsah.

Widget zpočátku zobrazen, je ve stavu sbalená. Widget sbalené má výšku řádky tabulky zhruba dvě a půl standardní iOS. Vývojář může požádat o velikosti rozšířit pomůcky, ale v ideálním případě ji musí být menší než výška obrazovky. 

Ve stavu sbalená widgetu by měl zobrazit jenom základní, samostatné informace. Pokud rozšířené, widgetu by měl zobrazit dodatečné informace, které rozšiřuje primární obsah ukazuje stav sbalená. Například aplikace počasí zobrazuje aktuální počasí při sbalení a přidá každou hodinu prognózy Pokud rozbalen.

Pomůcky uvedené v seznamu Rychlé akce bude pouze ve stavu sbalená. Pokud aplikace obsahuje více než jeden pomůcky, musí vývojář vybrat tu, k dispozici, když uživatel otevře seznamu Rychlé akce použitím tlak na ikonu aplikace pomocí 3D Touch.

V následujícím příkladu je jednoduchý dnes příponou (pomůcka) zpracovává sbalená a rozšířené stavy:

```csharp
using System;
using NotificationCenter;
using Foundation;
using UIKit;
using CoreGraphics;

namespace MonkeyAbout
{
    public partial class TodayViewController : UIViewController, INCWidgetProviding
    {
        protected TodayViewController (IntPtr handle) : base (handle)
        {
            // Note: this .ctor should not contain any initialization logic.
        }

        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();

            // Tell widget it can be expanded
            ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);

            // Get the maximum size
            var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
        }

        [Export ("widgetPerformUpdateWithCompletionHandler:")]
        public void WidgetPerformUpdate (Action<NCUpdateResult> completionHandler)
        {
            // Take action based on the display mode
            switch (ExtensionContext.GetWidgetActiveDisplayMode()) {
            case NCWidgetDisplayMode.Compact:
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                Content.Text = "Gorilla!!!!";
                break;
            }

            // Report results
            // If an error is encoutered, use NCUpdateResultFailed
            // If there's no update required, use NCUpdateResultNoData
            // If there's an update, use NCUpdateResultNewData
            completionHandler (NCUpdateResult.NewData);
        }

        [Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
        public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
        {
            // Take action based on the display mode
            switch (activeDisplayMode) {
            case NCWidgetDisplayMode.Compact:
                PreferredContentSize = maxSize;
                Content.Text = "Let's Monkey About!";
                break;
            case NCWidgetDisplayMode.Expanded:
                PreferredContentSize = new CGSize (0, 200);
                Content.Text = "Gorilla!!!!";
                break;
            }
        }

    }
}
```

Podívejte se na konkrétní kód režim zobrazení pomůcky podrobně. K informování systému, že tato pomůcka podporuje rozšířené stavu, používá:

```csharp
// Tell widget it can be expanded
ExtensionContext.SetWidgetLargestAvailableDisplayMode (NCWidgetDisplayMode.Expanded);
```

Chcete-li získat aktuální režim zobrazení ovládacího prvku, používá:

```csharp
ExtensionContext.GetWidgetActiveDisplayMode()
```

Chcete-li získat maximální velikost pro buď sbalená nebo rozbalené stav, používá:

```csharp
// Get the maximum size
var maxSize = ExtensionContext.GetWidgetMaximumSize (NCWidgetDisplayMode.Expanded);
```

A pro zpracování stavu (režim zobrazení), změna, používá:

```csharp
[Export ("widgetActiveDisplayModeDidChange:withMaximumSize:")]
public void WidgetActiveDisplayModeDidChange (NCWidgetDisplayMode activeDisplayMode, CGSize maxSize)
{
    // Take action based on the display mode
    switch (activeDisplayMode) {
    case NCWidgetDisplayMode.Compact:
        PreferredContentSize = maxSize;
        Content.Text = "Let's Monkey About!";
        break;
    case NCWidgetDisplayMode.Expanded:
        PreferredContentSize = new CGSize (0, 200);
        Content.Text = "Gorilla!!!!";
        break;
    }
}
```

Kromě nastavení požadovaná velikost pro každý stav (minimalizované nebo rozbalené), aktualizuje také obsah se zobrazuje na novou velikost.

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých vylepšení provedena Apple systém pomůcka v iOS 10 a zobrazí postup, jak implementovat v Xamarin.iOS.



## <a name="related-links"></a>Související odkazy

- [iOS 10 – ukázky](https://developer.xamarin.com/samples/ios/iOS10/)
- [Úvod do rozšíření](~/ios/platform/extensions.md)
- [Úvod do 3D dotykového ovládání](~/ios/platform/3d-touch.md)
- [Průvodce programováním v rozšíření aplikace](https://developer.apple.com/library/prerelease/content/documentation/General/Conceptual/ExtensibilityPG/index.html)
