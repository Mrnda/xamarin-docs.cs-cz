---
title: Ovládací prvky XAML Standard (Preview)
description: Tento článek popisuje XAML standardní ovládací prvky v Xamarin.Forms k dispozici.
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 1b01d0773f0c2150db575875b770957eb6452f41
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245564"
---
# <a name="xaml-standard-preview-controls"></a>Ovládací prvky XAML Standard (Preview)

![Náhled](~/media/shared/preview.png)

Tato stránka obsahuje seznam XAML standardní ovládací prvky dostupné ve verzi Preview, spolu s jejich ekvivalentní Xamarin.Forms řízení.

Je také seznam ovládacích prvků, které mají nové vlastnosti a výčet názvy v jazyce XAML standardní.

## <a name="controls"></a>Ovládací prvky

|Xamarin.Forms|Standardní XAML|
|--- |--- |
|Rámec|Ohraničení|
|Výběr.|ComboBox|
|ActivityIndicator|ProgressRing|
|StackLayout|StackPanel|
|Popisek|TextBlock|
|Položka|TextBox|
|přepínače|ToggleSwitch|
|ContentView|UserControl|


## <a name="properties-and-enumerations"></a>Vlastnosti a výčty

|Ovládací prvky Xamarin.Forms aktualizované vlastnostmi|Vlastnost Xamarin.Forms nebo výčtu|Ekvivalentní standardní XAML|
|--- |--- |--- |
|Tlačítko, záznamu, popisek, ovládací prvek DatePicker, Editor, SearchBar, TimePicker|TextColor|Popředí|
|VisualElement|BackgroundColor|Pozadí *|
|Výběr tlačítka|Barva okraje, OutlineColor|BorderBrush|
|Tlačítko|Hodnota BorderWidth|BorderThickness|
|ProgressBar|Průběh|Hodnota|
|Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo|FontAttributesBold, kurzíva, None|FontStyleItalic, normální|
|Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo|FontAttributes|FontWeights * Bold normální|
|InputView|KeyboardDefault, adresa Url, číslo, telefon, Text, konverzace, e-mailu|InputScopeNameValue * výchozí, adresa Url, číslo, TelephoneNumber, Text, konverzace, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientace *|

> [!IMPORTANT]
> Položky označené * jsou neúplné v aktuální verzi preview

## <a name="related-links"></a>Související odkazy

- [Náhled NuGet](https://aka.ms/xf-xamlstandard-nuget)
