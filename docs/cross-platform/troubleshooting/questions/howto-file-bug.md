---
title: Kdy a jak by měl mám ohlásit chybu?
description: Tento dokument popisuje, kdy, kde a jakým způsobem chcete-li ohlásit chybu. Poskytuje také hlášení o chybě, doporučené postupy, které umožňují techniky pro co nejlepší Diagnostikujte problém.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 8AD9CFBF-282A-4C1F-95E9-25F21141B052
author: asb3993
ms.author: amburns
ms.date: 06/05/2018
ms.openlocfilehash: b70fe29a79e099f1141c1295d907b48afaa2c3c7
ms.sourcegitcommit: aa9b9b203ab4cd6a6b4fd51e27d865e2abf582c1
ms.translationtype: MT
ms.contentlocale: cs-CZ
ms.lasthandoff: 07/30/2018
ms.locfileid: "39351603"
---
# <a name="when-and-how-should-i-file-a-bug-report"></a>Kdy a jak by měl mám ohlásit chybu?

Hlášení chyb v Xamarinu pro Bugzilla sledování chyb tady: [ https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all ](https://bugzilla.xamarin.com/enter_bug.cgi?classification=__all).

## <a name="file-a-bug-if"></a>Oznámit chybu, pokud...

Máte sadu kroků, které si myslíte, že technici Xamarinu budete moct použít reprodukovat problém, který je způsobeno Xamarin.

NEBO

Pečlivě popíšete viditelné příznaky problému, zejména v případě, že můžete také popisují některé přesné okolnosti týkající se problému. <sup> [[1]](#note-1)</sup>


## <a name="best-practices-to-help-xamarin-address-bugs-quickly-and-efficiently"></a>Doporučené postupy pro zlepšení Xamarin adresa chyby rychle a efektivně vyřešit


1. <a name="ref-1" />Hledání [Bugzilla](https://bugzilla.xamarin.com/query.cgi?format=specific&amp;bug_status=__all__) a web pro existující chyby sestavy nebo využití návrhy, které tento problém může vyřešit přímo.<sup> [[2]](#note-2)</sup><sup>[[3]](#note-3)</sup>

1. <a name="ref-2" />Postupujte podle [chyby zápisu pokyny](https://bugzilla.xamarin.com/page.cgi?id=bug-writing.html) k popisu problému očekávaným jasně a výstižně nejvíce, včetně popis co se stalo a se nestane.

1. <a name="ref-3" />Zahrnout všechny relevantní zásobníku trasování, text chybové zprávy nebo protokoly o chybách. <sup>[[4]](#note-4)</sup>

1. <a name="ref-4" />Zapište si důležité chybové zprávy, které se zobrazují v přílohy snímek obrazovky jako prostý text příliš.

1. <a name="ref-5" />Zahrnout malé, samostatná testovací případ, který reprodukuje chyby s, malým množstvím kódu nejvíce.  Pokud nelze reprodukovat problém s úplně novým projektem (vytvořený pomocí jedné z předdefinovaných šablon), pak prosím zip projektu, který demonstruje daný problém a připojte ji k hlášení o chybě.  Vytvořit příklad projektu co nejjednodušší před jejich připojení. <sup> [[5]](#note-5)</sup><sup>[[6]](#note-6)</sup>

1. <a name="ref-6" />Popis prostředí, kde byla zjištěna chyba, včetně operačního systému a [verze Xamarin](~/cross-platform/troubleshooting/questions/version-logs.md) a všechny závislosti.

---

## <a name="additional-details"></a>Další podrobnosti

1. <a name="note-1" />[*^*](#ref-1) Popis "viditelné příznaky" v ideálním případě by měl obsahovat dostatek podrobností, aby ostatní zákazníci můžete ověřit, jestli se zobrazují stejný problém (stejné chybové zprávy, stejné snížení výkonu, stejné trasování zásobníku z chybovému ukončení, _atd._ ). Pro "přesné okolností" jeden dobrým příkladem je-li vám můžete říct, že něco jako: "obvykle dosáhnu problém 75 % času, ale když změním tuto jednu věc potom jsem problému se lze vyhnout úplně." Pokud downgradu k předchozí verzi Xamarinu zastaví problému je jiný podobně jako například "přesné závislá na".

1. <a name="note-2" />[*^*](#ref-2) Dle očekávání, fragmenty textu chyby (nebo jakýkoli jiný jednoznačně popisný text) je obvykle nejlepší hledaným výrazům. Pokud stávající Sestava chyb je nekompletní, pak určitě můžete přidat podrobnosti nebo soubor nový, lepší Sestava chyb.

1. <a name="note-3" />[*^*](#ref-3) Další dobrá otázkou je, jestli je stejný problém hlášeno pro libovolný jazyk Java Objective-C a Swift aplikace. Pokud ano, problém je velmi pravděpodobné části Android nebo iOS samotné, nikoli součástí Xamarin.

1. <a name="note-4" />[*^*](#ref-4) Několik příkladů informace, včetně:

    1. Pro chyby, ke kterým dochází při sestavování projektu, uveďte prosím kompletní [výstup diagnostiky sestavení](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output) na hlášení o chybě.
    
    1. Chyby, ke kterým dochází při sestavování nebo ladění projektu pro iOS ze sady Visual Studio, spusťte prosím _Nápověda > Xamarin > Zip protokoly_ po dosažení chybu a obsahovat výsledný soubor ZIP na hlášení o chybě.
    
    1. Výjimky a chyby v aplikacích pro Android nebo iOS, uveďte prosím příslušné "[ladit protokoly pro aplikace pro Xamarin.Android a Xamarin.iOS](~/cross-platform/troubleshooting/questions/version-logs.md#debug-logs-for-xamarin-apps)."

1. <a name="note-5" />[*^*](#ref-5) Pokud je to možné pro konkrétního problému, jeden skvělou možností je znovu vytvořit problém tak, že přidáte malý počet soubory z původního řešení do úplně nové řešení. Xamarin tým často bude moct prozkoumat problémy i na větší testovací případy (za předpokladu, že kroky pro reprodukci jsou vysvětleny jasně), ale má největší šanci, že se dá rychle vyřešit chybu jednodušší poskytuje testovací případy.


1. <a name="note-6" />[*^*](#ref-6) Pokud je _není_ možné reprodukovat problém tak, že přidáte malý počet souborů k úplně novému řešení, pak můžete zip a připojit složce celé řešení pro vaše aplikace úplné. Odstraňte prosím `bin`, `obj`, `Components`, a `packages` složek, aby byly menší soubor zip. (Integrované vývojové prostředí a proces sestavení bude obvykle obnovit nebo znovu vytvořit obsah těchto složek podle potřeby.) Můžete také odstranit tolik kódu a zdrojové soubory z projektu, jak budete jako výsledný řešení stále ukazuje původní problém.

