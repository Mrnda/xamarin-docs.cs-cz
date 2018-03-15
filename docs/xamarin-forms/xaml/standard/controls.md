---
title: "Ovládací prvky XAML Standard (Preview)"
description: "Jak začít seznamovat se standardní Preview XAML v Xamarin.Forms"
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: b044cb849f9a8e591a8db5907211a55f77d6e45f
ms.sourcegitcommit: 8e722d72c5d1384889f70adb26c5675544897b1f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/15/2018
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

|Xamarin.FormsControls aktualizované vlastnostmi|Xamarin.FormsProperty nebo výčtu|XAML StandardEquivalent|
|--- |--- |--- |
|Tlačítko, záznamu, popisek, ovládací prvek DatePicker, Editor, SearchBar, TimePicker|TextColor|Popředí|
|VisualElement|BackgroundColor|Pozadí *|
|Výběr tlačítka|Barva okraje, OutlineColor|BorderBrush|
|Tlačítko|Hodnota BorderWidth|BorderThickness|
|ProgressBar|Průběh|Hodnota|
|Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo|FontAttributesBold, kurzíva, None|FontStyleItalic, normální|
|Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo|FontAttributes|FontWeights *Bold, Normal|
|InputView|KeyboardDefault, adresa Url, číslo, telefon, Text, konverzace, e-mailu|InputScopeNameValue * výchozí, adresa Url, číslo, TelephoneNumber, Text, konverzace, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientace *|

> [!IMPORTANT]
> Položky označené * jsou neúplné v aktuální verzi preview

## <a name="related-links"></a>Související odkazy

- [Náhled NuGet](https://aka.ms/xf-xamlstandard-nuget)
