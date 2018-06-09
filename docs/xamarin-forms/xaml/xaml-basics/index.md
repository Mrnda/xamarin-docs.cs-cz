---
title: Základy Xamarin.Forms XAML
description: Tato příručka vysvětluje, jak začít pracovat s XAML a platformy pro mobilní zařízení. XAML umožňuje vývojářům definování uživatelských rozhraní v aplikacích Xamarin.Forms pomocí značek, nikoli kódu.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 627267b95bb2d810a60f84c51e38bf5387fe1f99
ms.sourcegitcommit: 66682dd8e93c0e4f5dee69f32b5fc5a96443e307
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/08/2018
ms.locfileid: "35245960"
---
# <a name="xamarinforms-xaml-basics"></a>Základy Xamarin.Forms XAML

XAML – eXtensible Application Markup Language – umožňuje vývojářům definovat uživatelská rozhraní v aplikacích Xamarin.Forms pomocí značek namísto kódu. XAML v programu Xamarin.Forms nikdy požaduje, ale je často stručného a vizuálně souvislý než ekvivalentní kódu a potenciálně jazyk. XAML je zvlášť vhodné pro použití pomocí Oblíbené Architektura aplikace rozhraní MVVM (Model-View-ViewModel): XAML definuje zobrazení, které je spojené s kódem ViewModel prostřednictvím vazby dat založených na XAML.

## <a name="xaml-basics-contents"></a>Základní informace o obsahu XAML

* [Přehled](#Overview)
* [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Část 3. Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Část 5. Z datové vazby k rozhraní MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Kromě těchto článcích XAML základy si můžete stáhnout kapitolám knihy [vytváření mobilních aplikací s Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Obálky knihy")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML témata do větší hloubky v mnoha kapitolách knihy, včetně:

<table style="border:0px; box-shadow:0 0px 0px" cellpadding="0" cellspacing="2" border="0" width="85%">
<tr style="background:#ecf0f1">
  <td style="border:0px;">
    <h4>Kapitola 7. XAML vs. Kód</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch07-Apr2016.pdf">Stáhnout PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter07.md">Shrnutí</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitoly 8. Kód a XAML v souladu</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Stáhnout PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Shrnutí</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitola 10. XAML – rozšíření značek</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch10-Apr2016.pdf">Stáhnout PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter10.md">Shrnutí</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitola 18. MVVM</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch18-Apr2016.pdf">Stáhnout PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter18.md">Shrnutí</a></td></tr>
</table>

Může být tyto kapitolám [stáhnout zdarma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Přehled

XAML je založený na jazyce XML jazyk vytvořená microsoftem, jako alternativu k programování kód pro vytvoření instance a inicializace objektů a uspořádání těchto objektů v nadřazený-podřízený. XAML byl přizpůsoben několik technologií v rámci rozhraní .NET framework, ale jeho největší nástroj má najít v definování rozložení uživatelského rozhraní v rámci Windows Presentation Foundation (WPF), Silverlight, prostředí Windows Runtime a Universal Windows Platforma (UWP).

XAML je také součástí Xamarin.Forms, na platformě nezávislý nativně programovací rozhraní pro iOS, Android a UWP mobilní zařízení. V souboru XAML můžete definovat vývojáře Xamarin.Forms uživatelského rozhraní pomocí všechny Xamarin.Forms zobrazení, rozložení a stránky, jako i vlastní třídy. Soubor XAML můžete zkompilovat nebo vložené do spustitelného souboru. V obou případech informace XAML je analyzována v okamžiku sestavení s názvem objekty a znovu za běhu k vytváření instancí a inicializace objektů a k vytvoření propojení mezi tyto objekty a programového kódu.

XAML má několik výhod oproti ekvivalentní kódu:

-  XAML je často stručného a čitelná než ekvivalentní kódu.
-  Vyplývajících z XML nadřazenou podřízenou hierarchii umožňuje XAML tak, aby napodoboval s větší přehlednost hierarchii nadřazených a podřízených objektů uživatelského rozhraní.
-  XAML lze snadno ručně psané programátory, ale také poskytuje vlastní jazyk a vygenerované pomocí nástrojů visual návrhu.

Samozřejmě existují také nevýhody, většinou související s omezeními, které jsou vlastní kód jazyky:

-  Kód nemůže obsahovat XAML. Všechny obslužné rutiny události musí být definovány v souboru kódu.
-  XAML nemůže obsahovat smyčky pro opakované zpracování. (Však několik vizuální objekty Xamarin.Forms – zejména [ `ListView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ListView/) – může generovat více podřízených položek na základě u objektů v jeho `ItemsSource` kolekce.)
-  XAML nemůže obsahovat podmíněného zpracování (však vazby dat můžete odkazovat převaděč založené na kódu vazby, který umožňuje efektivně některé podmíněného zpracování.)
-  XAML obecně nelze vytvořit instanci třídy, které nedefinuje konstruktor bez parametrů. (Však se někdy způsobem řešení toto omezení.)
-  XAML obecně nelze volat metody. (Znovu, toto omezení můžete někdy překonat.)

Není ještě vizuálního návrháře pro generování XAML v aplikacích Xamarin.Forms. Všechny XAML musí být rukou, ale je [XAML Náhled](~/xamarin-forms/xaml/xaml-previewer.md). Programátorům nové XAML chtít často sestavit a spustit jejich vlastních aplikací, zejména po všechno, co nemusí být samozřejmě správné. I vývojáři s mnoha prostředí v jazyce XAML vědět, že je odměny experimenty.

XAML je v podstatě XML, ale některé funkce jedinečný syntaxe má XAML. Nejdůležitější jsou:

- Vlastnosti elementů
- Přidružené vlastnosti
- Rozšíření značek

Tyto funkce jsou *není* rozšíření XML. XAML je zcela právní XML. Ale tyto funkce jazyka XAML syntaxe používat XML způsoby jedinečný. Že jsou podrobněji v článcích níže, které uzavřou obsahuje úvod do používání XAML pro implementaci rozhraní MVVM.

## <a name="requirements"></a>Požadavky

Tento článek předpokládá znalost Xamarin.Forms funkční. Čtení [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) důrazně doporučujeme.

Tento článek také předpokládá některé znalost XML, včetně vysvětlení použití deklarace oboru názvů XML a podmínky *element*, *značka*, a *atribut*.

Pokud jste obeznámeni s Xamarin.Forms a XML, začít číst [část 1. Začínáme s XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Vytvoření adresáře Mobile Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
