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
ms.openlocfilehash: 3f30a77975a9f42380ecf7efd73426763ec83ef0
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 02/27/2018
---
# <a name="xaml-standard-preview-controls"></a>Ovládací prvky XAML Standard (Preview)

![Náhled](~/media/shared/preview.png)

Tato stránka obsahuje seznam XAML standardní ovládací prvky dostupné ve verzi Preview, spolu s jejich ekvivalentní Xamarin.Forms řízení.

Je také seznam ovládacích prvků, které mají nové vlastnosti a výčet názvy v jazyce XAML standardní.

## <a name="controls"></a>Ovládací prvky

<table style="width:300px">
  <tr><th>Xamarin.Forms</th><th>Standardní XAML</th></tr>
  <tr><td>Rámec</td><td>Ohraničení</td></tr>
  <tr><td>Výběr.</td><td>ComboBox</td></tr>
  <tr><td>ActivityIndicator</td><td>ProgressRing</td></tr>
  <tr><td>StackLayout</td><td>StackPanel</td></tr>
  <tr><td>Popisek</td><td>TextBlock</td></tr>
  <tr><td>Položka</td><td>TextBox</td></tr>
  <tr><td>přepínače</td><td>ToggleSwitch</td></tr>
  <tr><td>ContentView</td><td>UserControl</td></tr>
</table>

## <a name="properties-and-enumerations"></a>Vlastnosti a výčty

<table>
  <tr><th>Xamarin.Forms<br/>Ovládací prvky s aktualizované vlastnosti</th><th>Xamarin.Forms<br/>Vlastnost nebo výčtu</th><th>Standardní XAML<br/>Ekvivalent</th></tr>
  <tr><td>Tlačítko, záznamu, popisek, ovládací prvek DatePicker, Editor, SearchBar, TimePicker</td><td>TextColor</td><td>Popředí</td></tr>
  <tr><td>VisualElement</td><td>BackgroundColor</td><td><i>Pozadí *</i></td></tr>
  <tr><td>Výběr tlačítka</td><td>Barva okraje, OutlineColor</td><td>BorderBrush</td></tr>
  <tr><td>Tlačítko</td><td>Hodnota BorderWidth</td><td>BorderThickness</td></tr>
  <tr><td>ProgressBar</td><td>Průběh</td><td>Hodnota</td></tr>
  <tr><td>Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo</td><td>FontAttributes<br/>Tučné, kurzíva, None</td><td>FontStyle<br/>Kurzíva, normální</td></tr>
  <tr><td>Tlačítko, záznamu, popisek, Editor, SearchBar, značka Span, písmo</td><td>FontAttributes</td><td><i>FontWeights *</i><br/>Tučné písmo, normální</td></tr>
  <tr><td>InputView</td><td>Klávesnice<br/>Výchozí adresa Url, číslo, telefon, Text, konverzace, e-mailu</td><td><i>InputScopeNameValue *</i><br/>Výchozí adresa Url, číslo, TelephoneNumber, Text, konverzace, EmailNameOrAddress</td></tr>
  <tr><td>StackPanel</td><td>StackOrientation</td><td><i>Orientace *</i></td></tr>
</table>

> [!IMPORTANT]
> Položky označené * jsou neúplné v aktuální verzi preview


## <a name="related-links"></a>Související odkazy

- [Náhled NuGet](https://aka.ms/xf-xamlstandard-nuget)
