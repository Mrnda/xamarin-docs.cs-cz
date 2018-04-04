---
title: XAML – rozšíření značek
description: Zvětšete rozsah zdrojů, ze které XAML atributy se nastavují
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: b81bc4b31edd1d8b8f5f43f97885c38e889dd32c
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-markup-extensions"></a>XAML – rozšíření značek

XAML – rozšíření značek pomáhají prodloužit výkon a flexibilitu XAML tím, že atributy prvků nastavit z jiných zdrojů než literálu textové řetězce.

Například obvykle nastaveno `Color` vlastnost `BoxView` podobné výjimky:

```xaml
<BoxView Color="Blue" />
```

Nebo, můžete ho nastavit na hodnotu hexadecimální barva RGB:

```xaml
<BoxView Color="#FF0080" />
```

V obou případech textový řetězec nastavena `Color` atribut je převést na `Color` hodnota ve [ `ColorTypeConverter` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ColorTypeConverter/) – třída.

Může místo toho raději nastavit `Color` atribut z hodnoty uloženy ve slovníku prostředků, nebo hodnotu pomocí statické vlastnosti třídy, kterou jste vytvořili nebo vlastnost typu `Color` jiného elementu na stránce nebo vytvořený z oddělte hodnoty hue, sytost a Světlost.

Je možné pomocí rozšíření značek v jazyce XAML, všechny tyto možnosti. Ale Nenechte fráze "rozšíření značek" vystrašení jste: XAML – rozšíření značek jsou *není* rozšíření do formátu XML. I když rozšíření značek v jazyce XAML XAML je vždy právní XML. 

Rozšíření značek je skutečně právě jiný způsob, jak express atribut elementu. XAML – rozšíření značek jsou obvykle osobní nastavením atributu, který je uzavřen do složených závorek:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Všechna nastavení atributu do složených závorek je *vždy* rozšíření značek XAML. Ale jak zjistíte, XAML – rozšíření značek lze také odkazovat bez použití složené závorky.

Tento článek je rozdělené do dvou částí:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Používání rozšíření značek XAML](consuming.md)  

Pomocí rozšíření značek XAML, který je definován v Xamarin.Forms.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Vytváření rozšíření značek XAML](creating.md) 

Zápis vlastní vlastní rozšíření značek v jazyce XAML.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML z adresáře Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Slovníky prostředků](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamické styly](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datová vazba](~/xamarin-forms/app-fundamentals/data-binding/index.md)
