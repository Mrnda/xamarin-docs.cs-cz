---
title: Rozšiřitelné aplikace Markup Language (XAML)
description: XAML je deklarativní jazyk, který slouží k definování uživatelských rozhraní. Uživatelské rozhraní je definována v souboru XML pomocí syntaxe jazyka XAML, když modul runtime chování je definována v souboru samostatné kódu.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 10/24/2016
ms.openlocfilehash: bb3b4c4f80171f676e8b5f9a7464f4da890a4643
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 04/04/2018
---
# <a name="extensible-application-markup-language-xaml"></a>Rozšiřitelné aplikace Markup Language (XAML)

_XAML je deklarativní jazyk, který slouží k definování uživatelských rozhraní. Uživatelské rozhraní je definována v souboru XML pomocí syntaxe jazyka XAML, když modul runtime chování je definována v souboru samostatné kódu._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Momentální 2016: Stal hlavní XAML**

> [!NOTE]
> Vyzkoušení [standardní Preview XAML](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML – základy](xaml-basics/index.md)

XAML umožňuje vývojářům definování uživatelských rozhraní v aplikacích Xamarin.Forms pomocí značek, nikoli kódu. Nikdy požaduje XAML v programu Xamarin.Forms ale jedná jazyk který je často vizuálně souvislé a více stručného než ekvivalentní kódu. XAML je zvlášť vhodné pro použití pomocí Oblíbené Architektura aplikace Model-View-ViewModel (modelem MVVM): XAML definuje zobrazení, které je spojené s kódem ViewModel prostřednictvím vazby dat založených na XAML.

## <a name="xaml-compilationxamlcmd"></a>[Kompilace XAML](xamlc.md)

XAML můžete volitelně zkompilovat přímo do převodní jazyk (IL) s kompilátoru jazyka XAML (XAMLC). Tento článek popisuje, jak používat XAMLC a jeho výhody.

## <a name="xaml-previewerxaml-previewermd"></a>[Náhled XAML](xaml-previewer.md)

[XAML Náhled](~/xamarin-forms/xaml/xaml-previewer.md) oznamují na Xamarin momentální 2016 je k dispozici pro testování v kanálu alfa.

## <a name="xaml-namespacesnamespacesmd"></a>[Obory názvů jazyka XAML](namespaces.md)

Používá XAML `xmlns` atribut XML pro deklarace oboru názvů. Tento článek představuje syntaxe oboru názvů jazyka XAML a ukazuje, jak deklarace oboru názvů jazyka XAML pro přístup k typu.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Rozšíření značek XAML](markup-extensions/index.md)

XAML zahrnuje rozšíření značek pro nastavení atributy hodnoty nebo objekty nad rámec co lze vyjádřit pomocí jednoduchého řetězce. Patří mezi ně odkazující na konstanty, statické vlastnosti a pole, slovnících prostředků a datové vazby.

## <a name="passing-argumentspassing-argumentsmd"></a>[Předávání argumentů](passing-arguments.md)

XAML slouží k předání argumentů do jiné než výchozí konstruktory nebo do metod vytváření. Tento článek ukazuje použití XAML atributy, které slouží k předání argumentů konstruktory, volání metod vytváření a k určení typu Obecné argumentu.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Vlastnosti s podporou vazeb](bindable-properties.md)

V Xamarin.Forms je funkce společných vlastností language runtime (CLR) rozšířit pomocí vazbu vlastnosti. Vazbu vlastnosti je speciální typ vlastnosti, kde je vlastnost systémem Xamarin.Forms sledovat hodnotu vlastnosti. Tento článek obsahuje úvod do vlastnosti vazbu a ukazuje, jak vytvářet a využívat je.

## <a name="attached-propertiesattached-propertiesmd"></a>[Připojené vlastnosti](attached-properties.md)

– Přidružená vlastnost je zvláštním typem vazbu vlastnosti definované v jedné třídy ale připojené k jiné objekty a rozpoznat v jazyce XAML jako atribut, který obsahuje třídu a název vlastnosti, které jsou odděleny tečkou. Tento článek obsahuje úvod do přidružené vlastnosti a ukazuje, jak vytvářet a využívat je.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Slovníky prostředků](resource-dictionaries.md)

XAML prostředky jsou definice objektů, které může být použit více než jednou. A [ `ResourceDictionary` ](https://developer.xamarin.com/api/type/Xamarin.Forms.ResourceDictionary/) umožňuje prostředky definované na jednom místě, a znovu použít v celé aplikaci Xamarin.Forms. Tento článek ukazuje, jak vytvářet a využívat `ResourceDictionary`a jak sloučení, jeden `ResourceDictionary` do jiné.
