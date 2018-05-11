---
title: Známé problémy a řešení
ms.prod: xamarin
ms.assetid: 495958BA-C9C2-4910-9BAD-F48A425208CF
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 186faf3fc4f93d1c9a4af9e3e9f72afd569fed8b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 05/09/2018
---
# <a name="known-issues--workarounds"></a>Známé problémy a řešení

## <a name="persistence-of-cultureinfo-across-cells"></a>Trvalost CultureInfo v buňkách

Nastavení `System.Threading.CurrentThread.CurrentCulture` nebo `System.Globalization.CultureInfo.CurrentCulture` není zachována napříč buněk sešitu na základě Mono sešity cíle (Mac, iOS a Android) z důvodu [chyb na mono `AppContext.SetSwitch` ] [ appcontext-bug] implementace .

### <a name="workarounds"></a>Alternativní řešení

* Nastavit místní domény aplikace `DefaultThreadCurrentCulture`:
```csharp
using System.Globalization;
CultureInfo.DefaultThreadCurrentCulture = new CultureInfo("de-DE")
```

* Nebo aktualizace pro sešity 1.2.1 nebo novější, který bude přepisování přiřazení `System.Threading.CurrentThread.CurrentCulture` a `System.Globalization.CultureInfo.CurrentCulture` zajistit pro toto chování žádoucí (práce kolem Mono chyb).

## <a name="unable-to-use-newtonsoftjson"></a>Nelze použít Newtonsoft.Json

### <a name="workaround"></a>Alternativní řešení

* Aktualizace pro sešity 1.2.1, které se nainstalují Newtonsoft.Json 9.0.1.
  Sešity 1.3, právě alfa kanálu podporuje verze 10 a novější.

### <a name="details"></a>Podrobnosti

Byl vydán Newtonsoft.Json 10, které je závislá na Microsoft.CSharp, který je v konfliktu s sešity verze dodává pro podporu bumped `dynamic`. To je ve verzi preview sešity 1.3 řešit, ale prozatím jsme již dříve pracovali řešení podle Připnutí Newtonsoft.Json konkrétně na verzi 9.0.1.

Balíčky NuGet explicitně v závislosti na Newtonsoft.Json 10 nebo novější jsou podporovány pouze v 1.3 sešity aktuálně v alfa kanálu.

## <a name="code-tooltips-are-blank"></a>Popisy tlačítek kódu jsou prázdné

Je [chyb v editoru Monaco] [ monaco-bug] v prohlížeči Safari nebo WebKit, který se používá v aplikaci Mac sešity, bude výsledkem vykreslování popisy tlačítek kód bez textu.

![](general-images/monaco-signature-help-bug.png)

### <a name="workaround"></a>Alternativní řešení

* Kliknutím na popisek, jakmile se objeví vynutí text k vykreslení.

* Nebo aktualizovat do sešitů 1.2.1 nebo novější

[appcontext-bug]: https://bugzilla.xamarin.com/show_bug.cgi?id=54448
[monaco-bug]: https://github.com/Microsoft/monaco-editor/issues/408

## <a name="skiasharp-renderers-are-missing-in-workbooks-13"></a>Nástroji pro vykreslování SkiaSharp chybí v 1.3 sešity

Počínaje 1.3 sešity, odebrali jsme nástroji pro vykreslování SkiaSharp, které jsme součástí v sešitech 0.99.0, považuje SkiaSharp poskytování nástroji pro vykreslování sám sebe, pomocí našich [SDK] [/ příručky/cross platformy nebo sešity nebo sdk nebo].

### <a name="workaround"></a>Alternativní řešení

* Aktualizujte SkiaSharp v NuGet na nejnovější verzi. V době psaní to je 1.57.1.

## <a name="related-links"></a>Související odkazy

- [Zasílání zpráv o chybách](~/tools/workbooks/install.md#reporting-bugs)
