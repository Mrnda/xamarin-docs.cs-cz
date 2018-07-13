---
title: eXtensible Application Markup Language (XAML)
description: XAML je deklarativní značkovací jazyk, který slouží k definování uživatelských rozhraní. Uživatelské rozhraní je definována v souboru XML pomocí syntaxe XAML, zatímco chování za běhu je definována v souboru samostatné kódu na pozadí.
ms.prod: xamarin
ms.assetid: CD30EECC-8AC1-4CF5-A4FE-348420A6231E
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 06/18/2018
ms.openlocfilehash: f593e5d084d8cd7071d17195663478d430d994b7
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/12/2018
ms.locfileid: "38995479"
---
# <a name="extensible-application-markup-language-xaml"></a>eXtensible Application Markup Language (XAML)

_XAML je deklarativní značkovací jazyk, který slouží k definování uživatelských rozhraní. Uživatelské rozhraní je definována v souboru XML pomocí syntaxe XAML, zatímco chování za běhu je definována v souboru samostatné kódu na pozadí._

> [!VIDEO https://youtube.com/embed/H6UOrSyhTEE]

**Rozvoj 2016: Stávají hlavní XAML**

> [!NOTE]
> Vyzkoušejte si [standardní náhled XAML](standard/index.md)

<a name="xaml" />

## <a name="xaml-basicsxaml-basicsindexmd"></a>[XAML – základy](xaml-basics/index.md)

XAML umožňuje vývojářům definovat uživatelská rozhraní v aplikacích Xamarin.Forms pomocí značek namísto kódu. XAML se nikdy vyžaduje aplikaci Xamarin.Forms, ale je jazyk a často je vizuálně koherentní a stručnější než ekvivalentní kód. XAML je zvlášť vhodné pro použití s oblíbenými aplikace architektury Model-View-ViewModel (MVVM): XAML definuje zobrazení, který je propojený s ViewModel kódu pomocí vazby dat založených na XAML.

## <a name="xaml-compilationxamlcmd"></a>[Kompilace XAML](xamlc.md)

Přímo do jazyka intermediate language (IL) s kompilátorem XAML (XAMLC) můžete volitelně zkompilovat XAML. Tento článek popisuje způsob použití XAMLC a jeho výhody.

## <a name="xaml-previewerxaml-previewermd"></a>[Náhled XAML](xaml-previewer.md)

[Náhled XAML](~/xamarin-forms/xaml/xaml-previewer.md) bylo ohlášená na rozvoj 2016 Xamarin je k dispozici pro testování v kanálu alfa.

## <a name="xaml-namespacesnamespacesmd"></a>[Obory názvů jazyka XAML](namespaces.md)

Používá XAML `xmlns` atribut deklarace oboru názvů XML. Tento článek představuje obor názvů syntaxe XAML a ukazuje, jak k deklarování oboru názvů XAML pro přístup k typu.

## <a name="xaml-markup-extensionsmarkup-extensionsindexmd"></a>[Rozšíření značek XAML](markup-extensions/index.md)

XAML obsahuje rozšíření značek k nastavení atributů na hodnoty nebo objekty rámec toho, co lze vyjádřit pomocí jednoduché řetězce. Patří mezi ně odkazující na konstanty, statické vlastnosti a pole, slovníků prostředků a datové vazby.

## <a name="field-modifiersfield-modifiersmd"></a>[Modifikátory polí](field-modifiers.md)

`x:FieldModifier` Atribut namespace určuje úroveň přístupu pro vygenerované pole pro pojmenované elementy XAML.

## <a name="passing-argumentspassing-argumentsmd"></a>[Předávání argumentů](passing-arguments.md)

XAML je možné předat argumenty k jiné než výchozí konstruktory a metody pro vytváření objektů. Tento článek ukazuje použití atributů XAML, které slouží k předání argumentů konstruktory pro volání metody pro vytváření objektů a určení typu obecný argument.

## <a name="bindable-propertiesbindable-propertiesmd"></a>[Vlastnosti s podporou vazeb](bindable-properties.md)

V Xamarin.Forms funkce common language runtime (CLR) vlastností rozšířit pomocí vlastnosti umožňující vazbu. Vlastnost s vazbou je speciální typ vlastnosti, kde hodnota vlastnosti je sledován pomocí funkce v systému vlastností Xamarin.Forms. Tento článek obsahuje úvod do vlastnosti umožňující vazbu a ukazuje, jak vytvářet a využívat je.

## <a name="attached-propertiesattached-propertiesmd"></a>[Připojené vlastnosti](attached-properties.md)

Připojená vlastnost je speciální typ s možností vazby vlastnosti definované v jedné třídy ale připojený k jiné objekty a rozpoznat v XAML jako atribut, který obsahuje třídu a název vlastnosti oddělené tečkou. Tento článek obsahuje úvod do připojené vlastnosti a ukazuje, jak vytvářet a využívat je.

## <a name="resource-dictionariesresource-dictionariesmd"></a>[Slovníky prostředků](resource-dictionaries.md)

Prostředky XAML jsou definice objektů, které lze použít více než jednou. A [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) umožňuje prostředky definované na jednom místě a opakovaně používat v rámci aplikace Xamarin.Forms. Tento článek ukazuje, jak vytvářet a využívat `ResourceDictionary`a jak sloučení, jeden `ResourceDictionary` do jiné.
