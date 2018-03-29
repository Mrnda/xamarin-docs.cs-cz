---
title: Ovládací prvky XAML Standard (Preview)
description: Jak začít seznamovat se standardní Preview XAML v Xamarin.Forms
ms.topic: article
ms.prod: xamarin
ms.assetid: 287E6631-D1C5-46C5-8905-AB53D34E365D
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 11/15/2017
ms.openlocfilehash: 084da9cbb06c7ec9bbab6ea4dc6a1a7b15ffe692
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/29/2018
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
|Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo|FontAttributes|FontWeights *Bold, Normal|
|InputView|KeyboardDefault, adresa Url, číslo, telefon, Text, konverzace, e-mailu|InputScopeNameValue * výchozí, adresa Url, číslo, TelephoneNumber, Text, konverzace, EmailNameOrAddress|
|StackPanel|StackOrientation|Orientace *|

> [!IMPORTANT]
> Položky označené * jsou neúplné v aktuální verzi preview

## <a name="related-links"></a>Související odkazy

- [Náhled NuGet](https://aka.ms/xf-xamlstandard-nuget)
