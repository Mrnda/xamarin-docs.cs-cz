---
title: "Pravidel analýzy Xamarin.iOS"
ms.topic: article
ms.prod: xamarin
ms.assetid: C29B69F5-08E4-4DCC-831E-7FD692AB0886
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/06/2018
ms.openlocfilehash: c7dc63cbed0dbdc13dfd2d32a0859c0fe7a29196
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 03/09/2018
---
# <a name="xamarinios-analysis-rules"></a>Pravidel analýzy Xamarin.iOS

Analýza Xamarin.iOS je sada pravidel, které kontrolují nastavení projektu můžete zjistit, jestli jsou k dispozici lepší nebo další optimalizované nastavení.

Co nejvíce již v rané fázi na Najít možná zlepšení a ušetřit čas při vývoji spusťte pravidel analýzy.

Pro spouštění pravidel, v sadě Visual Studio pro Mac na nabídky, **Projekt > spuštění analýzy kódu**.

> [!NOTE]
> Analýza Xamarin.iOS lze spustit pouze na aktuálně vybrané konfiguraci. Důrazně doporučujeme spustit nástroj pro ladění **a** konfigurace verze.

## <a name="a-namexia0001xia0001-disabledlinkerrule"></a><a name="XIA0001"/>XIA0001: DisabledLinkerRule

- **Problém:** linkeru je zakázána na zařízení pro režim ladění.
- **Oprava:** pokuste se spustit kód s linkeru předejdete žádné výskyt nečekaných událostí.
Chcete-li nastavit tak, přejděte do projektu > iOS sestavení > Linkeru chování.

## <a name="a-namexia0002xia0002-testcloudagentreleaserule"></a><a name="XIA0002"/>XIA0002: TestCloudAgentReleaseRule

- **Problém:** sestavení aplikace, které inicializovat agenta Test Cloud odmítnuta společností Apple při odeslání, které používají privátní rozhraní API.
- **Oprava:** přidat nebo opravte nezbytné #if a definuje v kódu.

## <a name="a-namexia0003xia0003-ipadebugbuildsrule"></a><a name="XIA0003"/>XIA0003: IPADebugBuildsRule

- **Problém:** konfiguraci ladění, který používá vývojáře podpisových klíčů by neměla generovat IPA, jako je potřeba jenom pro distribuci, která teď používá v Průvodci publikováním.
- **Oprava:** zakázat IPA sestavení v možnosti projektu pro konfiguraci ladění.

## <a name="a-namexia0004xia0004-missing64bitsupportrule"></a><a name="XIA0004"/>XIA0004: Missing64BitSupportRule

- **Problém:** podporovaná architektura pro "verze | zařízení"není kompatibilní, chybí ARM64 64 bitů. Tento problém je jako Apple nepřijímá 32bitová verze pouze aplikací pro iOS v AppStore.
- **Oprava:** Double klikněte na projekt pro iOS, přejděte na sestavení > iOS sestavení a změnit podporované architektury, má ARM64.

## <a name="a-namexia0005xia0005-float32rule"></a><a name="XIA0005"/>XIA0005: Float32Rule

- **Problém:** není pomocí možnosti float32 (--aot-options =-O = float32) vede k hefty výkonu náklady, zejména na mobilních zařízeních, kde je Dvojitá přesnost matematické měřitelný výkon nižší. Všimněte si, že rozhraní .NET používá Dvojitá přesnost interně i pro float, takže povolením této možnosti ovlivní přesnost a případně i kompatibility.
- **Oprava:** Double klikněte na projekt pro iOS, přejděte na sestavení > iOS sestavení a zrušte zaškrtnutí políčka "Provádět všechny operace 32-bit float jako 64-bit float".

## <a name="a-namexia0006xia0006-httpclientavoidmanaged"></a><a name="XIA0006"/>XIA0006: HttpClientAvoidManaged

- **Problém:** vám doporučujeme používat nativní obslužné rutiny HttpClient místo spravované jednu pro lepší výkon, menší velikost spustitelný soubor a snadná podpora novější standardy.
- **Oprava:** Double klikněte na projekt pro iOS, přejděte na sestavení > iOS sestavení a změnit implementace HttpClient NSUrlSession (iOS 7 +) nebo CFNetwork podporu verze předcházející iOS 7.
