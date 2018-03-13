---
title: Komplikace
description: "watchOS umožňuje vývojářům psát vlastní komplikace pro sledování řezy"
ms.topic: article
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/03/2017
ms.openlocfilehash: affe58d9276bd0b687089fb42a14ca964c570c9c
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="complications"></a>Komplikace

_watchOS umožňuje vývojářům psát vlastní komplikace pro sledování řezy_

Tato stránka vysvětluje různé typy komplikace, které jsou k dispozici a jak přidat komplikace do vaší aplikace watchOS 3.

Všimněte si, každá aplikace watchOS může mít pouze jeden komplikace.

Začněte tím, že čtení [dokumentace společnosti Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) k určení, zda je vhodný pro komplikace vaší aplikace. Existují 5 `CLKComplicationFamily` typy zobrazení můžete vybrat ze:

[![](complications-images/all-complications-sml.png "Dostupné typy 5 CLKComplicationFamily: cyklické malé, modulární malé, modulární velký, využití, a to malé, využití, a to velké")](complications-images/all-complications.png#lightbox)

Aplikace můžete implementovat jenom jeden styl nebo všech pět, v závislosti na dat zobrazených.
Můžete také podporují cestovní čas, zadáním hodnoty pro posledních nebo budoucí časy jako uživatel změní digitální Crown.

<a name="adding" />

## <a name="adding-a-complication"></a>Přidání komplikace

### <a name="configuration"></a>Konfigurace

Komplikace můžete přidat do aplikace pro sledování během vytváření, nebo ručně přidat do existujícího řešení.

### <a name="add-new-project"></a>Přidáte nový projekt...

**Přidat nový projekt...**  Průvodce obsahuje zaškrtávací políčko, bude automaticky vytvoříte třídu kontroleru komplikace a nakonfigurovat **Info.plist** souboru:

![](complications-images/file-new-project-sml.png "Políčko zahrnout komplikace")

### <a name="existing-projects"></a>Existující projekty

Přidání komplikace do existujícího projektu:

1. Vytvořte novou **ComplicationController.cs** soubor třídy a implementujte `CLKComplicationDataSource`.
2. Konfigurace aplikace **Info.plist** vystavit komplikace a identity, které komplikace rodiny jsou podporovány.

Tyto kroky jsou podrobněji popsané v níže.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>CLKComplicationDataSource – třída

Následující šablony C# zahrnuje minimální požadované metody k implementaci `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
    }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
    }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
    }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
    }
}
```

Postupujte podle [zápis komplikace](#writing) pokyny pro přidání kódu do této třídy.

### <a name="infoplist"></a>Info.plist

Rozšíření sledování **Info.plist** soubor by měl určovat název `CLKComplicationDataSource` a které rodiny komplikace, které chcete podporovat:

[![](complications-images/complications-config-sml.png "Typy rodiny komplikace")](complications-images/complications-config.png#lightbox)

**Třída zdroje dat** položka seznamu se zobrazí názvy tříd této podtřídami `CLKComplicationDataSource` podtřídami, která zahrnuje komplikace logiky.

## <a name="clkcomplicationdatasource"></a>CLKComplicationDataSource

Všechny funkce komplikace je implementována do jedné třídy, přepis metod z `CLKComplicationDataSource` abstraktní třídy (které implementuje `ICLKComplicationDataSource` rozhraní).

### <a name="required-methods"></a>Požadované metody

Je nutné implementovat pro komplikace ke spuštění následujících metod:

- `GetPlaceholderTemplate` -Vrátí statické zobrazení použít během konfigurace nebo aplikaci nelze zadat hodnotu.
- `GetCurrentTimelineEntry` -Vypočítejte správné zobrazení, když běží problému.
- `GetSupportedTimeTravelDirections` -Vrátí možnosti z `CLKComplicationTimeTravelDirections` například `None`, `Forward`, `Backward`, nebo `Forward | Backward`.

### <a name="privacy"></a>Ochrana osobních údajů

Komplikací, ke kterým zobrazit osobní data

* `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` Nebo `HideOnLockScreen`

Pokud tato metoda vrátí hodnotu `HideOnLockScreen` pak problému se zobrazí buď ikonu nebo název aplikace (a ne žádná data) Pokud sledovat uzamčené.

### <a name="updates"></a>Aktualizace

- `GetNextRequestedUpdateDate` -Vrátí čas, když operační systém by měl další dotaz aplikace pro aktualizovaný komplikace zobrazí data.

Také můžete vynutit aktualizaci z vaší aplikace pro iOS.

### <a name="supporting-time-travel"></a>Podpora čas cesta

Čas podpora cesta je volitelná a pod kontrolou `GetSupportedTimeTravelDirections` metoda. Vrátí-li `Forward`, `Backward`, nebo `Forward | Backward` pak musí implementovat následující metody

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Zápis komplikace

Komplikace rozsahu od jednoduché datové zobrazení složitá bitové kopii a vykreslování dat s podporou čas cesta. Následující kód ukazuje, jak vytvořit jednoduché, jedním šablony komplikace.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Ukázkový kód

V tomto příkladu se podporuje jenom `UtilitarianLarge` šablony, takže lze vybrat pouze na konkrétní sledovat řezy, které podporují tento typ komplikace. Při *výběr* komplikace na sledovat, zobrazí **Moje KOMPLIKACE** a kdy *systémem* zobrazí text **MINUTU _hodinu_**   (s část času).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```


<a name="templates" />

## <a name="complication-templates"></a>Komplikace šablony

Nejsou k dispozici pro každý styl komplikace několik různých šablon.
**Prstenec** šablony umožňují zobrazit průběh stylu prstenec kolem komplikace, které může být použito k grafické zobrazení průběhu nebo s jinou hodnotou.

[Dokumentace CLKComplicationTemplate společnosti Apple](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Cyklické malá

Názvy tříd tyto šablony jsou všechny s předponou `CLKComplicationTemplateCircularSmall`:

- **RingImage** -zobrazení jedné bitové kopie, průběh prstenec kolem něj.
- **RingText** -zobrazí jeden řádek textu, průběh prstenec kolem něj.
- **SimpleImage** – stačí zobrazit malé jedné image.
- **SimpleText** – stačí zobrazit malé fragment textu.
- **StackImage** -zobrazení bitovou kopii a na řádku textu, nad sebou
- **StackText** -zobrazí dva řádky textu.

### <a name="modular-small"></a>Modular Small

Názvy tříd tyto šablony jsou všechny s předponou `CLKComplicationTemplateModularSmall`:

- **ColumnsText** -zobrazí mřížku malé textové hodnoty (2 řádků a sloupců 2).
- **RingImage** -zobrazení jedné bitové kopie, průběh prstenec kolem něj.
- **RingText** -zobrazí jeden řádek textu, průběh prstenec kolem něj.
- **SimpleImage** – stačí zobrazit malé jedné image.
- **SimpleText** – stačí zobrazit malé fragment textu.
- **StackImage** -zobrazení bitovou kopii a na řádku textu, nad sebou
- **StackText** -zobrazí dva řádky textu.

### <a name="modular-large"></a>Modulární velké

Názvy tříd tyto šablony jsou všechny s předponou `CLKComplicationTemplateModularLarge`:

- **Sloupce** -zobrazí mřížku 3 řádky s 2 sloupce, volitelně včetně obrázek vlevo od jednotlivých řádků.
- **StandardBody** -zobrazí řetězec tučné záhlaví s dva řádky prostý text. Záhlaví může volitelně zobrazit bitovou kopii na levé straně.
- **Tabulka** -zobrazí řetězec tučné záhlaví s 2 x 2 mřížky textu pod ní. Záhlaví může volitelně zobrazit bitovou kopii na levé straně.
- **TallBody** -zobrazí řetězec tučné záhlaví s větší jeden řádek písmo textu pod.

### <a name="utilitarian-small"></a>Využití, a to malé

Názvy tříd tyto šablony jsou všechny s předponou `CLKComplicationTemplateUtilitarianSmall`:

- **Ploché** -zobrazí bitovou kopii a text na jeden řádek (text by měl být krátký).
- **RingImage** -zobrazení jedné bitové kopie, průběh prstenec kolem něj.
- **RingText** -zobrazí jeden řádek textu, průběh prstenec kolem něj.
- **Hranaté** -zobrazí čtvereček bitové kopie (40px nebo 44px odmocnina pro 38 mm nebo 42 mm Apple Watch v uvedeném pořadí).

### <a name="utilitarian-large"></a>Využití, a to velké

Jenom jedna šablona pro tento styl komplikace: `CLKComplicationTemplateUtilitarianLargeFlat`.
Zobrazí jednu bitovou kopii a text, všechny na jeden řádek.



## <a name="related-links"></a>Související odkazy

- [Dokumentace společnosti Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [WWDC video](https://developer.apple.com/videos/play/wwdc2015-209/)
