---
title: Podpora programovacích jazyků v Xamarinu
description: 'Tento dokument popisuje různé programovací jazyky nepodporuje Xamarin. Popisuje, C#, F #, přenosné Visual Basic.NET a šablon Razor.'
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: asb3993
ms.author: amburns
ms.date: 02/18/2018
ms.openlocfilehash: 715f63a0be54ba3342bd63c1c76d89656313359a
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 06/05/2018
ms.locfileid: "34781673"
---
# <a name="programming-language-support-in-xamarin"></a>Podpora programovacích jazyků v Xamarinu

## <a name="c"></a>C# 

###  <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Přehled podpory asynchronních operací](~/cross-platform/platform/async.md)

Verze 5 jazyka C# zavedená dvě nová klíčová slova a express asynchronních operací: async a operátoru await. Tato klíčová slova umožňuje psát jednoduché kód, který využívá Task Parallel Library provádět dlouhotrvající operace (například přístup k síti) v jiné vlákno a snadný přístup k výsledky při dokončení. Nejnovější verze Xamarin.iOS a Xamarin.Android podporu async a operátoru await – tento dokument obsahuje vysvětlení a příklady pomocí nové syntaxe s funkcí Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[Funkce jazyka C# 6](~/cross-platform/platform/csharp-six.md)

Nejnovější verzi jazyka C# – verze 6 – stále vyvíjí jazyka, který má mít méně často používaný, lepší přehlednost a další konzistence. Čisticí inicializace syntaxe, možnost používat `await` v `catch/finally` bloky a null podmíněné `?` operátor jsou obzvláště užitečná.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Vytváření mobilních aplikací s F # a Xamarin.

##  <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Přenosné Visual Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio podporuje vytvoření přenosné knihovny tříd pomocí Visual Basic.NET, který lze potom ji vložit do aplikace Xamarin. Tento článek ukazuje, jak vytvořit nový PCL Visual Basic a použít ho v ukázkové aplikaci Xamarin.iOS, Xamarin.Android a Windows Phone.

##  <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Vytváření zobrazení HTML pomocí šablon Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin vývojářům umožňuje využít modul ukázka Razor poprvé zavedlo s architekturou ASP.NET MVC, společně s C# k snadno kombinovat data s jazyky HTML, Javascript a CSS bez starostí ručně vytváření HTML řetězců v kódu.
Tento článek ukazuje, jak používat šablony Razor Xamarin pro Android a iOS.
