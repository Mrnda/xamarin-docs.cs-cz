---
title: "Cílová architektura"
description: "Tento článek se zabývá cílové rozhraní (základní knihovny tříd) k dispozici pro Xamarin.Mac a jejich používání v projektu Xamarin.Mac důsledky."
ms.topic: article
ms.prod: xamarin
ms.assetid: AF21BE16-3F92-4121-AB4C-D51AC863D92D
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 11/10/2017
ms.openlocfilehash: f657fc3dd87d5c39d442a863e4acc00ac320b00d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="target-framework"></a>Cílová architektura

_Tento článek se zabývá cílové rozhraní (základní knihovny tříd) k dispozici pro Xamarin.Mac a jejich používání v projektu Xamarin.Mac důsledky._

![Target – možnosti framework pro Xamarin.Mac](target-framework-images/select-target.png "Target – možnosti framework pro Xamarin.Mac")

## <a name="background"></a>Pozadí

Každý program rozhraní .NET nebo knihovna závisí na funkce poskytované základní knihovny pro třídu (BCL). Tato BCL zahrnuje sestavení například mscorlib, systému, System.Net.Http a System.Xml, které poskytují běžné funkce integrované na všechny jazyky rozhraní .NET.

V průběhu let si vytvořili více různých verzích této BCL optimalizovaný pro použití v odlišných situacích. "Plocha" BCL zahrnuje Bohatší sada knihovny, které může být příliš těžký pro jiné případy použití, když mobile se zaměřuje na zajištění, že rozhraní API jsou bezpečné pro propojení, který odebere nepoužívané kódu ke snížení nároků aplikace.

Jeden z důležitější má tyto různé cílové rozhraní, je, že všechna sestavení v daném programu *musí* cíle kompatibilní BCL sestavení. Pokud to není tomu, můžete mít dvě sestavení propojené s různými verzemi **System.dll** disagreeing o podpisu daného typu. Sdílené knihovny můžete buď cílový [.NET standardní 2](https://blog.xamarin.com/share-code-net-standard-2-0/), což je běžný podmnožinu cílové rozhraní, nebo konkrétní cílové rozhraní.

K dispozici pro Xamarin.Mac, každý s různé výhody a nevýhody jsou tři možnosti cílové rozhraní:

- **Moderní** (nazývané Mobile v starší dokumentaci) – podmnožinu velmi podobně jako na jaké mocniny Xamarin.iOS, vysoce vyladěných výkonu a velikost. Této cílové rozhraní je linkeru bezpečné, takže tyto projekty může mít jejich poslední nároků výrazně sníženy odebráním nepoužívané kódu.

- **Úplné** (nazývané XM 4.5 v starší dokumentaci) – podmnožinu velmi podobně jako na "plocha" BCL, s pár malých odstraňování. Jako cílové rozhraní je téměř identický net45 (a novější), ho můžete snadno využívat mnoho nugets, které neposkytují buď netstandard2 nebo konkrétní Xamarin.Mac sestavení. Z důvodu System.Configuration využití je kompatibilní s propojení.

- **Nepodporované** (označovaný jako systém starší dokumentace) – místo propojení BCL poskytované Xamarin.Mac, použijte aktuální mono nainstalován systém. To poskytuje maximálním možném sadu sestavení, včetně některých známé jako problematické (například System.Drawing). Tato možnost existuje pouze má "poslední možnost" a důrazně doporučujeme před jeho použitím vyčerpat další možnosti. Jak již název napovídá, využití nepodporuje oficiální podporu kanály.

## <a name="setting-the-target-framework"></a>Nastavení cílového prostředí

Chcete-li změnit typ cílové rozhraní pro projekt Xamarin.Mac, postupujte takto:

1. Otevřete projekt Xamarin.Mac v sadě Visual Studio for Mac.
2. V **Průzkumníku řešení**, poklikejte na soubor projektu otevřete **možnosti projektu** dialogové okno.
3. Z **Obecné** , vyberte typ **cílové rozhraní** která vyhovuje potřebám vaší aplikace:

  [![Používání okna Možnosti projektu zvolte cílové rozhraní](target-framework-images/select-target-full.png "pomocí okna Možnosti projektu zvolte cílové rozhraní")](target-framework-images/select-target-full-large.png#lightbox)

4. Klikněte **OK** tlačítko uložte provedené změny.

Měli byste **Vyčistit** a potom **znovu sestavit** projektu Xamarin.Mac po přepnutí typ cílové rozhraní.

## <a name="summary"></a>Souhrn

Tento článek má stručně popsané různé typy cílové rozhraní (základní knihovny tříd) k dispozici na aplikaci Xamarin.Mac a každý typ framework má být použit.


## <a name="related-links"></a>Související odkazy

- [iOS a Mac sdílení kódu](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [Přenosné knihovny tříd](~/cross-platform/app-fundamentals/pcl.md)
- [Sestavení](~/cross-platform/internals/available-assemblies.md)
- [Aktualizace stávající aplikace pro Mac](~/cross-platform/macios/unified/updating-mac-apps.md)
