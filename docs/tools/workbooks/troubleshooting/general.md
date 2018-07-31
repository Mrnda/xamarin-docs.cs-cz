---
title: Známé problémy a alternativní řešení
description: Tento dokument popisuje známé problémy a řešení pro Xamarin Workbooks. Popisuje problémy CultureInfo, JSON problémy a další.
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d362698d2844ae6d96bba4929d509f5373742578
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39350992"
---
# <a name="known-issues--workarounds"></a>Známé problémy a alternativní řešení

## <a name="persistence-of-cultureinfo-across-cells"></a>Trvalost CultureInfo v buňkách

Nastavení `System.Threading.CurrentThread.CurrentCulture` nebo `System.Globalization.CultureInfo.CurrentCulture` nezachová do buněk sešitu na základě Mono sešity cíle (Mac, iOS a Android) z důvodu [chyb pro mono `AppContext.SetSwitch` ] [ appcontext-bug] implementace .

### <a name="workarounds"></a>Alternativní řešení

* Nastavit místní domény aplikace `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Nebo aktualizace do sešitů 1.2.1 nebo novější, který se přepíše přiřazení `System.Threading.CurrentThread.CurrentCulture` a `System.Globalization.CultureInfo.CurrentCulture` stanovit požadované chování (obejít Mono chyb).

## <a name="unable-to-use-newtonsoftjson"></a>Nelze použít Newtonsoft.Json

### <a name="workaround"></a>Alternativní řešení

* Aktualizovat sešitů 1.2.1, které nainstaluje Newtonsoft.Json 9.0.1.
  Sešity 1.3, který je aktuálně v alfa kanálu podporuje verze 10 a novější.

### <a name="details"></a>Podrobnosti

Byla vydána Newtonsoft.Json 10, které je závislá na Microsoft.CSharp obsahují třídy, který je v konfliktu s verzí sešity dodává pro podporu sadu `dynamic`. To se zákazníky a vyřešené ve verzi preview sešity 1.3, ale prozatím jsme pracovali vyřešit pomocí Připnutí Newtonsoft.Json speciálně pro verzi 9.0.1.

Balíčky NuGet explicitně v závislosti na Newtonsoft.Json 10 nebo novější se podporují jenom v sešitech 1.3, aktuálně v alfa kanálu.

## <a name="code-tooltips-are-blank"></a>Popisky kódu jsou prázdné

Je [chyb v editoru Monaco] [ monaco-bug] v Safari/WebKit, který se používá v aplikaci sešity k Macu, bude výsledkem vykreslování popisky kódu bez textu.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Alternativní řešení

* Kliknutím na popisek, jakmile se zobrazí vynutí, text, který chcete vykreslit.

* Nebo je aktualizujte na sešity 1.2.1 nebo novější

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>V sešitech 1.3 chybí ve Skiasharpu renderery

Od verze 1.3 sešity, odebrali jsme ve Skiasharpu renderery, jsme dodané v sešitech 0.99.0, a místo toho použití ve Skiasharpu poskytování zobrazovací jednotku, samotného, pomocí našich [SDK](~/tools/workbooks/sdk/index.md).

### <a name="workaround"></a>Alternativní řešení

* Aktualizujte ve Skiasharpu v NuGet na nejnovější verzi. V době psaní jde 1.57.1.

## <a name="related-links"></a>Související odkazy

- [Zasílání zpráv o chybách](~/tools/workbooks/install.md#reporting-bugs)
