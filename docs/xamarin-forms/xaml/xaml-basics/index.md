---
title: Základy Xamarin.Forms XAML
description: Tato příručka vysvětluje, jak začít pracovat s XAML napříč platformami pro mobilní zařízení. XAML umožňuje vývojářům definovat uživatelská rozhraní v aplikacích Xamarin.Forms pomocí značek namísto kódu.
ms.prod: xamarin
ms.assetid: 67CC2CD6-D10A-4B14-9696-1D3A410EFFBF
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/25/2017
ms.openlocfilehash: 901174eb9510eaab670564655f9f6b4bff940bd7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995437"
---
# <a name="xamarinforms-xaml-basics"></a>Základy Xamarin.Forms XAML

XAML – eXtensible Application Markup Language – umožňuje vývojářům definovat uživatelská rozhraní v aplikacích Xamarin.Forms pomocí značek namísto kódu. XAML se nikdy vyžaduje aplikaci Xamarin.Forms, ale často je stručné a vizuálně přeměnit než ekvivalentní kód a potenciálně jazyk. XAML je zvlášť vhodné pro použití s oblíbenými aplikace architektury MVVM (Model-View-ViewModel): XAML definuje zobrazení, který je propojený s ViewModel kódu pomocí vazby dat založených na XAML.

## <a name="xaml-basics-contents"></a>Základní informace o obsahu XAML

* [Přehled](#Overview)
* [Část 1. Začínáme s jazykem XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md)
* [Část 2. Základní syntaxe jazyka XAML](~/xamarin-forms/xaml/xaml-basics/essential-xaml-syntax.md)
* [Část 3. Rozšíření značek XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
* [Část 4. Základy vytváření vazeb dat](~/xamarin-forms/xaml/xaml-basics/data-binding-basics.md)
* [Část 5. Z datové vazby k MVVM](~/xamarin-forms/xaml/xaml-basics/data-bindings-to-mvvm.md)

Kromě těchto článcích XAML základy si můžete stáhnout kapitoly knihy [vytváření mobilních aplikací pomocí Xamarin.Forms](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md):

[![](images/cover-sml.png "Titulní knihy")](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)

XAML jsou témata v části mnoho kapitoly knihy, včetně:

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
    <h4>Kapitola 8. Kód a XAML v souladu</h4>
  </td>
  <td style="border:0px;" align="right"><a href="https://download.xamarin.com/developer/xamarin-forms-book/XamarinFormsBook-Ch08-Apr2016.pdf">Stáhnout PDF</a> </td>
  <td style="border:0px;" align="right"><a href="~/xamarin-forms/creating-mobile-apps-xamarin-forms/summaries/chapter08.md">Shrnutí</a></td>
</tr>
<tr style="background:#f8f9fa">
  <td style="border:0px;">
    <h4>Kapitola 10. Rozšíření značek XAML</h4>
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

Může být tyto kapitoly [stáhnout zdarma](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md).

<a name="Overview" />

## <a name="overview"></a>Přehled

XAML je jazyk založený na formátu XML vytvořené microsoftem jako alternativu ke kódu pro vytváření instancí a inicializace objektů a uspořádání těchto objektů v hierarchie nadřazený podřízený. XAML byla přizpůsobena několik technologií rozhraní .NET Framework, ale bylo zjištěno jeho nejlepší nástroj, při definování rozložení uživatelského rozhraní v rámci Windows Presentation Foundation (WPF), Silverlight, Windows Runtime a Universal Windows Platformy (UPW).

XAML je také součástí Xamarin.Forms – multiplatformní nativně založené na rozhraní pro iOS, Android a UPW mobilní zařízení. V souboru XAML Xamarin.Forms pro vývojáře můžete definovat uživatelských rozhraní pomocí všech Xamarin.Forms zobrazení, rozložení a stránky, jako třídy a taky s vlastními. Soubor XAML můžete zkompilovat nebo vložený ve spustitelném souboru. V obou případech informace XAML je analyzován, v okamžiku sestavení s názvem objekty a znovu za běhu konkretizovat a inicializovat objekty a stanovit propojení mezi tyto objekty a programového kódu.

XAML má několik výhod oproti ekvivalentní kód:

-  XAML je často stručné a čitelnější než ekvivalentní kód.
-  Hierarchie nadřazený podřízený vyplývajících z XML umožňuje XAML tak, aby napodoboval visual itsc hierarchie nadřazenosti a podřízenosti objektů uživatelského rozhraní.
-  XAML lze snadno ručně psanou programátory, ale také slouží jako jazyk a vygenerovaný pomocí nástrojů pro vizuální návrh.

Samozřejmě existují také nevýhody, většinou související s omezením, které jsou přirozené pro značkovacích jazycích:

-  XAML nemůže obsahovat kód. Všechny obslužné rutiny události musí být definován v souboru kódu.
-  XAML nemůže obsahovat smyčky pro opakované zpracování. (Ale několik vizuální objekty Xamarin.Forms – zejména [ `ListView` ](xref:Xamarin.Forms.ListView) – můžete generovat více podřízených položek na základě objektů v jeho `ItemsSource` kolekce.)
-  XAML nemůže obsahovat podmíněného zpracování (ale datové vazby můžete odkazovat konvertor vazba založená na kódu, který umožňuje efektivně některé podmíněného zpracování.)
-  XAML obecně nelze vytvořit instanci třídy, které nemá definován konstruktor bez parametrů. (Existuje ale někdy ovládat toto omezení.)
-  XAML obecně nelze volat metody. (Opět, toto omezení v některých případech je možné překonat.)

Není dosud vizuálního návrháře pro vytváření v aplikacích Xamarin.Forms XAML. Všechny XAML musí být ručně psanou, ale neexistuje [náhled XAML](~/xamarin-forms/xaml/xaml-previewer.md). Programátorům nové XAML může být vhodné často vytváření a spouštění svých aplikací, zejména po cokoli, co nemusí být samozřejmě správná. S velkým množstvím prostředí v XAML dokonce vývojáři vědět, že je odměny pro experimentování ve službě.

XAML je v podstatě XML, ale XAML obsahuje některé funkce syntaxe jedinečný. Nejdůležitější jsou:

- Vlastnosti elementů
- Připojené vlastnosti
- Rozšíření značek

Tyto funkce jsou *není* rozšíření XML. XAML je zcela právní XML. Ale tyto funkce syntaxe XAML použít XML jedinečným způsobem. Tyto jsou podrobně popsány v v článcích uvedených dole, které uzavřou obsahuje úvod do používání XAML pro implementaci MVVM.

## <a name="requirements"></a>Požadavky

Tento článek předpokládá pracovní znalost Xamarin.Forms. Čtení [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md) se důrazně doporučuje.

Tento článek také předpokládá některé znalost jazyka XML, včetně vysvětlení použití deklarace oboru názvů XML a podmínky *element*, *značky*, a *atribut*.

Pokud jste obeznámeni s Xamarin.Forms a jazyka XML, začít se čtením [část 1. Začínáme s XAML](~/xamarin-forms/xaml/xaml-basics/get-started-with-xaml.md).



## <a name="related-links"></a>Související odkazy

- [XamlSamples](https://developer.xamarin.com/samples/xamarin-forms/XamlSamples/)
- [Úvod do Xamarin.Forms](~/xamarin-forms/get-started/introduction-to-xamarin-forms.md)
- [Vytváření kniha Mobile Apps](~/xamarin-forms/creating-mobile-apps-xamarin-forms/index.md)
- [Ukázky Xamarin.Forms](https://developer.xamarin.com/samples/xamarin-forms/all/)
