---
title: Ovládací prvky XAML Standard (Preview)
description: Tento článek se věnuje XAML standardní ovládací prvky k dispozici v Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/11/2018
ms.locfileid: "38843940"
---
# <a name="xaml-standard-preview-controls"></a>Ovládací prvky XAML Standard (Preview)

![Náhled](~/media/shared/preview.png)

Tato stránka obsahuje seznam dostupná ve verzi Preview, spolu s jejich ekvivalentní ovládací prvek Xamarin.Forms XAML běžným ovládacím prvkům.

Je také seznam ovládacích prvků, které mají novou vlastnost a výčet názvů v XAML Standard.

## <a name="controls"></a>Ovládací prvky

|Xamarin.Forms|XAML Standard|
|--- |--- |
|Rámec|Ohraničení|
|Výběr|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Popisek|TextBlock|
|Položka|TextBox|
|Přepínač|ToggleSwitch|
|ContentView|Uživatelský ovládací prvek|


## <a name="properties-and-enumerations"></a>Vlastnosti a výčty

|Ovládací prvky Xamarin.Forms s aktualizované vlastnosti|Vlastnost Xamarin.Forms nebo výčet.|Ekvivalentní standardní XAML|
|--- |--- |--- |
|Tlačítko, položka, popisek, ovládací prvek DatePicker, Editor, SearchBar, TimePicker|TextColor|Popředí|
|VisualElement|BackgroundColor|Pozadí *|
|Výběr tlačítka|BorderColor, OutlineColor|BorderBrush|
|Tlačítko|Šířka okraje|BorderThickness|
|ProgressBar|Průběh|Hodnota|
|Tlačítko, položka, popisek, Editor, SearchBar, značka Span, písmo|FontAttributesBold, kurzíva, None|FontStyleItalic normální|
|Tlačítko, položka, popisek, Editor, SearchBar, značka Span, písmo|FontAttributes|FontWeights * Bold, normální|
|InputView|KeyboardDefault, adresa Url, číslo, telefon, Text, konverzace, e-mailu|InputScopeNameValue * výchozí, adresa Url, čísla, TelephoneNumber, Text, konverzace, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientace *|

> [!IMPORTANT]
> Položky označené * jsou neúplné v aktuální verzi preview

## <a name="related-links"></a>Související odkazy

- [NuGet ve verzi Preview](https://aka.ms/xf-xamlstandard-nuget)
