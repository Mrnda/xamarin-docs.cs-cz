---
title: Rozšíření značek XAML
description: Tento článek vysvětluje způsob používání rozšíření značek XAML Xamarin.Forms rozšířit výkonná a flexibilní XAML, tím, že element atributů, které mají být v rozsahu od zdrojů než literál řetězce.
ms.prod: xamarin
ms.assetid: EB06C8B7-3FD5-47B7-A09C-A13063BD110F
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/05/2018
ms.openlocfilehash: d507ff3c74de6bb4ea36c1a7b7dc2cd5dd60823b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38996737"
---
# <a name="xaml-markup-extensions"></a>Rozšíření značek XAML

Rozšíření značek XAML pomáhají rozšířit výkonná a flexibilní XAML, tím, že element atributů, které mají být v rozsahu od zdrojů než literál řetězce.

Například obvykle nastaveno `Color` vlastnost `BoxView` tímto způsobem:

```xaml
<BoxView Color="Blue" />
```

Nebo můžete ho nastavit na šestnáctkovou hodnotu barva RGB:

```xaml
<BoxView Color="#FF0080" />
```

V obou případech se textový řetězec nastaven na `Color` atribut je převedena na `Color` hodnota [ `ColorTypeConverter` ](xref:Xamarin.Forms.ColorTypeConverter) třídy.

Pravděpodobně budete chtít místo toho nastavit `Color` atribut z hodnoty uloženy ve slovníku prostředků, nebo hodnotu statická vlastnost třídy, kterou jste vytvořili nebo vlastnost typu `Color` jiného elementu na stránce, nebo vytvořený z oddělte hodnoty odstín, sytost a světelnost.

Všechny tyto možnosti jsou přes rozšíření značek XAML. Ale Nenechte frázi "přípony označení" vystrašení vám: rozšíření značek XAML jsou *není* rozšíření XML. Dokonce i rozšíření značek XAML, XAML jsou vždy právní XML.

Rozšíření značek je vlastně jenom jiný způsob, jak vyjádřit atribut prvku. Rozšíření značek XAML jsou obvykle poznáte ho podle nastavení atributu, který je uzavřen ve složených závorkách:

```xaml
<BoxView Color="{StaticResource themeColor}" />
```

Jakékoli nastavení atributu ve složených závorkách je *vždy* rozšíření značek XAML. Ale jak uvidíte, rozšíření značek XAML lze také odkazovat bez použití složených závorek.

Tento článek je rozdělený do dvou částí:

## <a name="consuming-xaml-markup-extensionsconsumingmd"></a>[Používání rozšíření značek XAML](consuming.md)  

Použití rozšíření značek XAML, který je definován v Xamarin.Forms.

## <a name="creating-xaml-markup-extensionscreatingmd"></a>[Vytváření rozšíření značek XAML](creating.md)

Napište vlastní vlastní rozšíření značek XAML.



## <a name="related-links"></a>Související odkazy

- [Rozšíření značek (ukázka)](https://developer.xamarin.com/samples/xamarin-forms/XAML/MarkupExtensions/)
- [Kapitola rozšíření značek XAML Xamarin.Forms knihy](~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md)
- [Slovníky prostředků](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Dynamické styly](~/xamarin-forms/user-interface/styles/dynamic.md)
- [Datová vazba](~/xamarin-forms/app-fundamentals/data-binding/index.md)
