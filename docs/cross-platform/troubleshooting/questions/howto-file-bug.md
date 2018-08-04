---
title: Kdy a jak by měl mám ohlásit chybu?
description: Tento dokument popisuje, kdy, kde a jakým způsobem chcete-li ohlásit chybu. Poskytuje také hlášení o chybě, doporučené postupy, které umožňují techniky pro co nejlepší Diagnostikujte problém.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: conceptdev
ms.author: crdun
ms.date: 08/01/2018
ms.openlocfilehash: f20740ff1e16187be3d3703b3da07329f6f52daf
ms.sourcegitcommit: bf05041cc74fb05fd906746b8ca4d1403fc5cc7a
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514335"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kdy a jak by měl mám ohlásit chybu?

> [!TIP]
> Použití **nahlásit problém** položky nabídky v sadě Visual Studio &ndash; tento tok odešle diagnostické informace vaše Sestava chyb pomáhající při řešení problému.
>
> Existují podrobné pokyny pro [Visual Studio 2017](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017) a [Visual Studio for Mac](https://docs.microsoft.com/visualstudio/mac/report-a-problem).
>
> Můžete vyhledat existující sestavy na [komunity vývojářů v aplikaci Visual Studio](https://developercommunity.visualstudio.com/) webu.

## <a name="file-a-bug-if"></a>Oznámit chybu, pokud...

Máte sadu kroků si myslíte, že techniků, kteří budou moct používat k reprodukci problému.

NEBO

Pečlivě popíšete viditelné příznaky problému, zejména v případě, že můžete také popisují některé přesné okolnosti týkající se problému. <sup> [[1]](#note-1)</sup>

## <a name="best-practices-to-help-address-bugs-quickly-and-efficiently"></a>Doporučené postupy pro zlepšení adresa chyby rychle a efektivně vyřešit

1. <a name="ref-1" />Hledání [komunity vývojářů v aplikaci Visual Studio](https://developercommunity.visualstudio.com/) a web pro existující chyby sestavy nebo využití návrhy, které tento problém může vyřešit přímo.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Popište problém jako jasně a výstižně, jako je to možné, včetně popis co se stalo a byla očekávána nestane.

1. <a name="ref-3" />Zahrnout všechny relevantní zásobníku trasování, text chybové zprávy nebo protokoly o chybách (Pokud používáte **nahlásit problém** funkce, může jít o zahrnuty automaticky). <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Zapište si důležité chybové zprávy, které se zobrazují v přílohy snímek obrazovky jako prostý text příliš.

1. <a name="ref-5" />Zahrnout malé, samostatná testovací případ, který reprodukuje chyby s, malým množstvím kódu nejvíce.  Pokud nelze reprodukovat problém s úplně novým projektem (vytvořený pomocí jedné z předdefinovaných šablon), pak prosím zip projektu, který demonstruje daný problém a připojte ji k hlášení o chybě.  Vytvořit příklad projektu co nejjednodušší před jejich připojení. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Popis prostředí, kde byla zjištěna chyba, včetně operačního systému a [verze Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) a všechny závislosti.

## <a name="additional-details"></a>Další podrobnosti

1. <a name="note-1" />[*^*](#ref-1) Popis "viditelné příznaky" v ideálním případě by měl obsahovat dostatek podrobností, aby ostatní zákazníci můžete ověřit, jestli se zobrazují stejný problém (stejné chybové zprávy, stejné snížení výkonu, stejné trasování zásobníku z chybovému ukončení, _atd._ ). Pro "přesné okolností" jeden dobrým příkladem je-li vám můžete říct, že něco jako: "obvykle dosáhnu problém 75 % času, ale když změním tuto jednu věc potom jsem problému se lze vyhnout úplně." Pokud downgradu k předchozí verzi Xamarinu zastaví problému je jiný podobně jako například "přesné závislá na".

1. <a name="note-2" />[*^*](#ref-2) Dle očekávání, fragmenty textu chyby (nebo jakýkoli jiný jednoznačně popisný text) je obvykle nejlepší hledaným výrazům. Pokud stávající Sestava chyb je nekompletní, pak určitě můžete přidat podrobnosti nebo soubor nový, lepší Sestava chyb.

1. <a name="note-3" />[*^*](#ref-3) Další dobrá otázkou je, jestli je stejný problém hlášeno pro libovolný jazyk Java Objective-C a Swift aplikace. Pokud ano, problém je velmi pravděpodobné části Android nebo iOS samotné, nikoli součástí Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Několik příkladů informace, včetně:

    1. Pro chyby, ke kterým dochází při sestavování projektu, uveďte prosím kompletní [výstup diagnostiky sestavení](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) na hlášení o chybě.

    1. Chyby, ke kterým dochází při sestavování nebo ladění projektu pro iOS ze sady Visual Studio, spusťte prosím **Nápověda > Xamarin > Zip protokoly** po dosažení chybu a obsahovat výsledný soubor ZIP na hlášení o chybě.

    1. Výjimky a chyby v aplikacích pro Android nebo iOS, uveďte prosím příslušné [ladit protokoly pro aplikace pro Xamarin.Android a Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps).

1. <a name="note-5" />[*^*](#ref-5) Pokud je to možné pro konkrétního problému, jednou z možností je znovu vytvořit problém tak, že přidáte malý počet soubory z původního řešení do úplně nové řešení. Xamarin tým často bude moct prozkoumat problémy i na větší testovací případy (za předpokladu, že kroky pro reprodukci jsou vysvětleny jasně), ale má největší šanci, že se dá rychle vyřešit chybu jednodušší poskytuje testovací případy.

1. <a name="note-6" />[*^*](#ref-6) Pokud je _není_ možné reprodukovat problém tak, že přidáte malý počet souborů k úplně novému řešení, pak můžete zip a připojit složce celé řešení pro vaše aplikace úplné. Odstraňte prosím `bin`, `obj`, `Components`, a `packages` složek, aby byly menší soubor zip. (Integrované vývojové prostředí a proces sestavení bude obvykle obnovit nebo znovu vytvořit obsah těchto složek podle potřeby.) Můžete také odstranit tolik kódu a zdrojové soubory z projektu, jak budete jako výsledný řešení stále ukazuje původní problém.
