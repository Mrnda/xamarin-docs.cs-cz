---
title: Práce s tvOS Segmentovaným ovládacích prvků v Xamarinu
description: Tento dokument popisuje, jak pracovat s tvOS segmentovaným ovládací prvky v aplikaci vytvořené s nástroji Xamarin. Popisuje ikony segmentu a text, události, změna vzhledu ovládacího prvku a další.
ms.prod: xamarin
ms.assetid: 23AD94CC-E93A-40B1-8E2B-ECD21FA355BE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 8980f9fbf6996217dbcaec869e4c81598ac36552
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34789236"
---
# <a name="working-with-tvos-segmented-controls-in-xamarin"></a>Práce s tvOS Segmentovaným ovládacích prvků v Xamarinu

Ovládacího prvku Segmentovaným poskytuje sadu lineární prvky, z nichž každá může obsahovat ikony nebo text a slouží k poskytování souvisejících voleb pro uživatele.

[![](segmented-controls-images/segment01.png "Ukázka segment ovládací prvky")](segmented-controls-images/segment01.png#lightbox)

Společnost Apple má následující návrhy pro práci s Segmentovaným ovládací prvky:

- **Počítat s dostatečným místem** -pozor by měla být proveďte zajistit dostatek prostoru mezi dalšími [může získat fokus položky](~/ios/tvos/app-fundamentals/navigation-focus.md) a Segmentovaným ovládacího prvku. Segment jednotlivých stane vybrat, když je vybraný (není po klepnutí) a uživatel může nechtěně změnit segmentů, když chtějí ve skutečnosti vyberte jinou položku může získat fokus na aktuální segment.
- **Pomocí rozdělení zobrazení pro filtrování obsahu** -Segmentovaným ovládací prvky nevytvářejte velmi dobře hodí obsah filtrování podle rozdělení zobrazení byly navrženy pro snadné navigace mezi obsahem a filtry.
- **Omezení sedm maximální segmenty** -snažte se zachovat maximálního počtu segmentů níže osm (8) jako tento postup je jednodušší analyzovat z napříč místnosti na pohovce a usnadňuje navigaci.
- **Použít konzistentní velikost obsahu segmentu** – všechny segmenty mají stejné šířky, a pokud je to možné, že byste měli zkusit zachovat obsah v každém segmentu stejnou velikost. Pouze to umožňuje vizuálně nedeformuje ovládací prvky segmentu, ale usnadňuje čtení na první pohled.
- **Vyhněte se kombinování ikony a Text** -každé jednotlivé Segment může obsahovat ikonu nebo text, ale ne obojí. I když je možné kombinovat ikony i text v ovládacím prvku stejné Segmentovaným, to je nutno.

<a name="About-Segment-Icons" />

## <a name="about-segment-icons"></a>O segmentu ikony

Apple navrhuje pomocí jednoduché, rozpoznatelném obrázků pro ikony segmentu, jako je například Lupa pro vyhledávání. Jsou příliš složité ikony pevného rozpoznat na obrazovce TV napříč místnosti, takže je vhodné omezit ikony pro vyjádření jednoduché.

Není možné kombinovat text a ikony na daný Segment a není vhodné kombinovat ikony a text v jedné Segmentovaným ovládacího prvku. To by měl být všechny ikony nebo celý text.

<a name="Summary" />

## <a name="segment-text"></a>Text segmentu

Následující návrhy pro práci s textem segmentu, bude společnosti Apple:

- **Použít krátké smysluplný podstatná jména** -title segmentu by měl vyžadují, jasně uvádějí typ obsahu, který uživatel by měl očekávat, když vyberete daný Segment. Příklad: Hudba nebo videa.
- **Použít první písmeno velké malá a velká písmena** -každého slova segmenty název by měl zadat velkými písmeny, s výjimkou články, spojky a předložky menší než čtyři (4) znaky.
- **Použít krátké názvy zaměřuje** -zachovat názvy krátký a zaměřují se na typ obsahu mohou očekávat, když je vybrána segmentu.

Znovu není možné kombinovat text a ikony na daný Segment a není vhodné kombinovat ikony a text v jedné Segmentovaným ovládacího prvku.

<a name="Segment-Controls-and-Storyboards" />

## <a name="segment-controls-and-storyboards"></a>Ovládací prvky segmentu a scénářů

Nejjednodušší způsob, jak pracovat s ovládacími prvky segmentu v aplikaci Xamarin.tvOS je chcete přidat do aplikace uživatelského rozhraní pomocí návrháře iOS.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio for Mac](#tab/vsmac)

1. V **řešení Pad**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **Segment řízení** z **sada nástrojů** na zobrazení: 

    [![](segmented-controls-images/segment02.png "Segment ovládacího prvku")](segmented-controls-images/segment02.png#lightbox)
1. V **pomůcky karta** z **vlastnost Pad**, můžete upravit několik vlastností ovládacího prvku Segment jeho **styl** a **stavu**: 

    [![](segmented-controls-images/segment03.png "Na kartě pomůcky")](segmented-controls-images/segment03.png#lightbox)
1. Použití **segmenty** pole k řízení počet segmentů v kontroleru.
1. Vyberte daný Segment z **Segment rozevírací** upravit jeho jednotlivé vlastnosti, jako **název** nebo **Image** a kontrolu, pokud je daný Segment  **Povolit** nebo **vybrané** při zobrazení ovládacího prvku.
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](segmented-controls-images/segment04.png "Přiřadit název")](segmented-controls-images/segment04.png#lightbox)
1. Uložte provedené změny.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)
    
1. V **Průzkumníku řešení**, dvakrát klikněte `Main.storyboard` souborů a otevřete pro úpravy.
1. Přetáhněte **Segment řízení** z **sada nástrojů** na zobrazení: 

    [![](segmented-controls-images/segment02-vs.png "Segment ovládacího prvku")](segmented-controls-images/segment02-vs.png#lightbox)
1. V **pomůcky karta** z **vlastnost Explorer**, můžete upravit několik vlastností ovládacího prvku Segment jeho **styl** a **stavu**: 

    [![](segmented-controls-images/segment03-vs.png "Na kartě pomůcky")](segmented-controls-images/segment03-vs.png#lightbox)
1. Použití **segmenty** pole k řízení počet segmentů v kontroleru.
1. Vyberte daný Segment z **Segment rozevírací** upravit jeho jednotlivé vlastnosti, jako **název** nebo **Image** a kontrolu, pokud je daný Segment  **Povolit** nebo **vybrané** při zobrazení ovládacího prvku.
1. Nakonec přiřadit **názvy** pro ovládací prvky, aby mohli odpovídat na ně v kódu jazyka C#. Příklad: 

    [![](segmented-controls-images/segment04-vs.png "Přiřadit název")](segmented-controls-images/segment04-vs.png#lightbox)
1. Uložte provedené změny.
    
-----

Další informace o práci s scénářů, najdete v tématu naše [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md). 

<a name="Working-with-Segmented-Controls" />

## <a name="working-with-segmented-controls"></a>Práce s Segmentovaným ovládací prvky

Jak jsme uvedli výše, s Segmentovaným řízení poskytuje sadu elementů lineární, z nichž každá může obsahovat ikony nebo text a slouží k poskytování souvisejících voleb pro uživatele.

Existují několika různými způsoby, kterými můžete pracovat s Segmentovaným ovládacími prvky v aplikaci Xamarin.tvOS.

<a name="Exposed-as-Outlets-and-Actions" />

## <a name="exposed-as-names-and-events"></a>K dispozici jako názvy a události

Pokud jste vytvořili vlastní ovládací prvek segmentu v Návrháři rozhraní a zveřejněné jako ovládací prvek s názvem a událost můžete reagovat na měnící segment následující kód:

```csharp
partial void PlayerCountChanged (Foundation.NSObject sender) {

    // Take action based on the number of players selected
    switch(PlayerCount.SelectedSegment) {
    case 0:
        // Do something if the segment is selected
        ...
        break;
    case 1:
        // Do something if the segment is selected
        ...
        break;
    case 2:
        // Do something if the segment is selected
        ...
        break;
    }
}
```

V případě výše uvedeném příkladu byla ovládacího prvku Segment zveřejněné jako `PlayerCount` název a `PlayerCountChanged` akce události. Další informace o práci s akcemi a výstupy, najdete v tématu [psaní kódu s výstupy a akce](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) části našich [Hello, tvOS úvodní příručce](~/ios/tvos/get-started/hello-tvos.md).

`SelectedSegment` Vlastnost získá nebo nastaví index založený na aktuálně vybraný segment nula (0) na základě. Takže pokud máte pět (5) segmenty, první Segment bude mít index nula (0) a poslední indexu čtyři (4).

<a name="Modifying-Segments" />

## <a name="modifying-segments"></a>Úprava segmenty

Kdykoli můžete upravit počet a obsah Segmentovaným ovládacích prvků. Chcete-li vložit novou ikonu Segment použijte následující kód:

```csharp
// Icon Segment
SegmentedControl.InsertSegment(UIImage.FromFile("icon.png"), 0, true);

// Text Segment
SegmentedControl.InsertSegment("New Segment", 0, true);
```

Druhý parametr definuje, kde budou segmentu vložit pomocí nula (0) na základě indexu. Pokud je poslední parametr `true` bude mít animovaný úlohy insert.

Chcete-li odebrat daný Segment, použijte tento příkaz:

```csharp
SegmentedControl.RemoveSegmentAtIndex(0, true);
```

Nebo odebrat všechny segmenty následující:

```csharp
SegmentedControl.RemoveAllSegments();
```

Znovu Pokud je poslední parametr `true`, bude mít animovaný odebrání. Použití `NumberOfSegments` vlastnost vrátí aktuální počet segmentů.

Chcete-li získat **název** nebo **ikonu** pro daný segment, použijte tento příkaz:

```csharp
// Get title
var title = SegmentedControl.TitleAt(0);

// Get icon
var icon = SegmentedControl.ImageAt(0);
```

A chcete-li změnit **název** nebo **ikonu**, použijte tento příkaz:

```csharp
// Set title
SegmentedControl.SetTitle("New Title", 0);

// Set icon
SegmentedControl.SetImage(UIImage.FromFile("icon.png"), 0);
```

Zda je daný Segment **povoleno**, použijte tento příkaz:

```csharp
if (SegmentedControl.IsEnabled(0)) {
    // Do something
    ...
}
```

A **povolit nebo zakázat** základě segmentu, použijte následující:

```csharp
SegmentedControl.SetEnabled(false, 0);
```

<a name="Modifying-the-Segmented-Controls-Appearance" />

## <a name="modifying-the-segmented-controls-appearance"></a>Úprava vzhled Segmentovaným ovládacího prvku

Chcete-li změnit na pozadí daný Segment do bitové kopie můžete následující kód:

```csharp
SegmentedControl.SetBackgroundImage (UIImage.FromFile("background.png"), UIControlState.Normal, UIBarMetrics.Default);
```

Kde `UIControlState` Určuje stav ovládací prvek, který nastavíte pro bitovou kopii jako:

- Normální
- Zvýrazněná
- zakázáno
- Vybrané
- Zaměřuje

A `UIBarMetrics` určuje podle metrik, které chcete použít jako:

- Výchozí
- Compact
- DefaultPrompt
- CompactPrompt

Kromě toho můžete nastavit na oddělovač mezi segmenty pomocí:

```csharp
SegmentedControl.SetDividerImage (UIImage.FromFile("divider.png"), UIControlState.Normal, UIControlState.Normal, UIBarMetrics.Default);
```

Kde první `UIControlState` Určuje stav Segment nalevo od dělicí a druhý `UIControlState` Určuje stav Segment, který má práva.

<a name="Summary" />

## <a name="summary"></a>Souhrn

Tento článek má zahrnutých navrhování a práce s Segmentovaným řízení uvnitř Xamarin.tvOS aplikace.



## <a name="related-links"></a>Související odkazy

- [Ukázky pro tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS lidské rozhraní příručky](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Průvodce programováním aplikace pro tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
